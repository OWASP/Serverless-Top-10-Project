![OWASP Logo](https://www.owasp.org/images/3/32/OWASP_Project_Header.jpg)

# [OWASP Serverless Top 10](https://www.owasp.org/index.php/OWASP_Serverless_Top_10_Project)

## Overview

When adopting a [serverless architecture](https://martinfowler.com/articles/serverless.html), we eliminate the need to develop a server to manage our application. By doing so, we also pass some of the security threats to the infrastructure provider. In addition to the many advantages of serverless application development, such as cost and scalability, some security aspects are also handed to our service provider, which can usually be trusted.

However, even if these applications are running without a provisioning server, they still execute code. If this code is written in an insecure manner, the application can be vulnerable to traditional application-level attacks, like Cross-Site Scripting (XSS), Command/SQL Injection, Denial of Service (DoS), broken authentication and authorization and many more.

The OWASP Top 10 is the de-facto guide for security practitioners to understand the most common application attacks and risks and are selected and prioritized according to this data, in combination with consensus estimates of exploitability, detectability, and impact into providing The Ten Most Critical Web Application Security Risks. The *OWASP Serverless Top 10* project aims at giving the same insight into the top 10 security risks in Serverless Application.

## First Report

The first report is first glance to the serverless security world and will serve as a baseline to the official OWASP Top 10 in Serverless project. The report  examines the differences in attack vectors, security weaknesses, and business impact of successful attacks on applications in the serverless world, and, most importantly, how to prevent them. As we will see, attack prevention is different from the traditional application world. Additional risks, which are not part of the original OWASP Top 10, but might be relevant for the final version, are listed on the Other Risks to Consider page. 

### ToC

* [Release notes](2018/en/0x06-release-notes.md)
* [Intro: Welcome to Serverless Security](2018/en/0x05-introduction.md)
* S1:2017 [Injection](2018/en/0xS1-injection.md)
* S2:2017 [Broken Authentication](2018/en/0xS2-broken-authentication.md)
* S3:2017 [Sensitive Data Exposure](2018/en/0xS3-sensitive-data-disclosure.md)
* S4:2017 [XML External Entities (XXE)](2018/en/0xS4-xxe.md)
* S5:2017 [Broken Access Control](2018/en/0xS5-broken-access-control.md)
* S6:2017 [Security Misconfiguration](2018/en/0xS6-security-misconfiguration.md)
* S7:2017 [Cross-Site Scripting (XSS)](2018/en/0xS7-xss.md)
* S8:2017 [Insecure Deserialization](2018/en/0xS8-insecure-deserialization.md)
* S9:2017 [Using Components with Known Vulnerabilities](2018/en/0xS9-known-vulns.md)
* S10:2017 [Insufficient Logging and Monitoring](2018/en/0xSa-logging-detection-response.md)
* [Other Risks to Consider](2018/en/0xSb-other-risks.md)
* [Summary](2018/en/0xSc-summary.md)
* [Future Work](2018/en/0xSd-future-work.md)
* [Acknowledgements](2018/en/0xSe-acknowledgements.md)

## Open Call

* We are actively looking for organizations and individuals that will provide vulnerability prevalence data.

## Get Involed!

* Translation efforts
* Assisting in the development of related tools (e.g. DVSA)

[Slack](https://owasp.slack.com): [#project-sls-top10](https://owasp.slack.com/messages/project-sls-top10) channel
([invitation link](https://join.slack.com/t/owasp/shared_invite/enQtNDI5MzgxMDQ2MTAwLTEyNzIzYWQ2NDZiMGIwNmJhYzYxZDJiNTM0ZmZiZmJlY2EwZmMwYjAyNmJjNzQxNzMyMWY4OTk3ZTQ0MzFhMDY))

[Official page](https://www.owasp.org/index.php/OWASP_Serverless_Top_10_Project)
