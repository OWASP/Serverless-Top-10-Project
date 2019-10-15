# Other Risks to Consider

OWASP Top 10 is based on years and years of data and experience, leaving this project with a lot of doubt due to the early stage of serverless adoption in the market. However, some research was already done in the field. As a result, the following issues should be considered for the official OWASP Serverless Top 10 project.

## X: Denial of Service (DoS)

The fact that each event is handled on a separated environment means that the traditional DoS attacks are not so relevant in their current form. Even if the attacker has managed to make his container unreliable, it will only affect the event coming into this environment and will not affect the next coming event.

However, there are some cases in which the attacker can achieve DoS across the account:

- Function concurrent limit (e.g. triggering a function until the pre-defined concurrency is achieved)
- Environment disk capacity (e.g. filling the `/tmp` folder)
- Account writing/reading capacity (e.g. triggering max allowed DynamoDB table scans)

The infrastructure helps managing such attacks and even provides some solutions (e.g. AWS Shield). The total risk in serverless therefore, should be lower.

## X: Denial of Wallet (DoW)

The automated scalability and availability is one of the reasons to use serverless. This allows application developers to pay only for what you use and to transfer the responsibility for scaling up the application to the infrastructure provider.

Nevertheless, it comes with a cost that has no bullet-proof protection. Attackers can trigger resources (e.g. external APIs, public storage) upon their will and cause financial damage to the organization. To “protect” against such attacks, AWS allows configuring limits for invocations or budget. However, if the attacker can achieve that limit, he can cause DoS to the account availability.

There is no actual protection that is not resulting in DoS. The attack is not as straightforward in traditional architecture as in serverless. Therefore, the risk should be high.

## X: Insecure Secret Management

It is always hard to securely manage all our secrets. However, usually secrets could be managed on a protected location in our backend. In serverless, they are shared across resources in the account.

Secrets like cryptography keys, API tokens, storage credentials and other sensitive settings are now shared more easily between functions and code, which could lead to sensitive data leakage that could be hard to mitigate.

Additionally, if a secret is stored as an environment variable for every function that is deployed, rather than a traditional configuration file, it would be much harder to go and change that for all functions if compromised.

On the other hand, it is easier to change a compromised key on a cloud-native application, than on an on-site compiled version. The total risk should be equal to the risk in traditional applications.

## X: Insecure Shared Space

Serverless environments share space between invocations if the container was not destroyed. That means that if the application wrote some data into the user-space (e.g. `/tmp`) and did not manually delete it after use, thinking that the container will die, an attacker could leverage that into stealing data of other users.

This would probably require another vulnerability that would provide attackers access to the environment. If the application is vulnerable to code or command injection, an attacker could simply access the `/tmp` folder and steal sensitive data.

On a traditional application this is usually achieved when the application is vulnerable to traversal attacks. On serverless (AWS) the only space available for writing is `/tmp`. The fact that it is only temporary (or limited to the container) however, makes the risk slightly lower.

## X: Business Logic / Flow manipulation

Business logic attacks​ may be the most complicated attacks to detect and usually have high business impact. Attacks like identify, constraint and flow manipulations may not be unique for serverless, but the fact that use of microservices is mostly stateless means that a careful design should be considered when relying on events that could or have happened before.

Furthermore, in some cases functions should only be invoked in certain cases and by certain invokers. But the fact that they are stateless means that they might not be able to verify that.

Such behavior could be achieved by:

- Targeting a misconfigured public resource that triggers an internal functionality to bypass the execution flow (refer to ​[S2: Broken Authentication​ attack scenario example](0xS2-broken-authentication.md#example-attack-scenario))
- Targeting resources that do not enforce proper access control and lead into execution flow manipulation
- Accessing unauthorized data by manipulating a parameter that is relied upon by the function, without a way to verify it
- Modifying client-side code to bypass limits

The stateless architecture alone makes logic and flow manipulation an actual risk in serverless applications, which could easily lead into DoS/DoW, invoking internal functionalities, execution-flow bypassing and more.

The overall risk in serverless application should be significantly higher.