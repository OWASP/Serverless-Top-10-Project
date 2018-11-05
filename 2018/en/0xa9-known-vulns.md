# A9:2017 Using Components with Known Vulnerabilities

## Attack Vectors

Serverless functions are usually small and used for micro-services. To be able to execute the desired tasks, they make use of many dependencies and 3rd-party libraries. Vulnerability introduced by the supply chain is one of most common risks these days and attackers will target code that makes use of vulnerable libraries as an entry point to the application. Additionally, in what we refer to as ‘Poisoning the Well,’ attackers aim to gain more long-term persistence in the application by means of an upstream attack. After poisoning the well, they patiently wait as the new version makes its way into cloud applications.

## Security Weakness

This issue is very widespread. Component-heavy development patterns can lead to development teams not even understanding which components they use in their application or API, much less keeping them up to date. Dependency scanners can help in detection, but determining exploitability requires additional effort.

## Impact

Most of the known vulnerabilities contain their full specifications, which helps determining their business impact as well as other information. While most known vulnerabilities have a low impact, or not actually used by the code, some of the [largest breaches to date have relied on exploiting known vulnerabilities](https://snyk.io/blog/owasp-top-10-breaches) in components.

## How to Prevent

Like any facet of cybersecurity, securing serverless applications requires a variety of tactics throughout the entire application development lifecycle and supply chain. However, since vulnerable dependencies are the same risk as in traditional applications, most of the best practices are still relevant:

- Continuously monitor dependencies and their versions throughout the system.
- Only obtain components from official sources over secure links. Prefer signed packages to reduce the chance of including a modified, malicious component.
- Continuously monitor sources like CVE and NVD (e.g. [https://nvd.nist.gov/vuln/](https://nvd.nist.gov/vuln/)) for vulnerabilities, or platform based advisories like [NodeSecurity](https://nodesecurity.io/advisories), [PyUp](https://pyup.io/safety/), [OWASP SafeNuGet](https://www.owasp.org/index.php/OWASP_SafeNuGet), etc.
- It is recommended to scan dependencies for known vulnerabilities using tools such as [OWASP Dependency Check](https://www.owasp.org/index.php/OWASP_Dependency_Check) and [Dependency Track](https://www.owasp.org/index.php/OWASP_Dependency_Track_Project) or commercial solutions.

## Example Attack Scenario

The following function uses the url-parse library. Vulnerable versions of this URL string parsing solution return an incorrect hostname, an issue that leads to multiple flaws such like SSRF (Server Side Request Forgery), Open Redirect, or Bypass Authentication Protocol, leaving users open to exploit.

![Known Vulns](images/0xa9-known-vulns.png)

By passing a malicious data to the apiUrl parameter (e.g. `http://google.com:80%5c%5cyahoo.com//`), the function will return a false host and will result in redirecting the user to a malicious website.