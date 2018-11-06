## Summary
After investigating each risk under serverless architecture, we can definitely say the risks were not eliminated
they just changed, for better and for worse.

Interesting application data might lie elsewhere, but it still needs proper protection. While the environment
data might not be as interesting, spreading data into cloud storage requires careful attention of who can
access it and how. Leaving even one function open could lead to a massive data leak.

Serverless has more standardized authentication & authorization models and could be the game-changer.
The fact that apps are built in micro-services provides us with a fine-grained architecture that enables
developers and devops to create a system that follows least privilege in its base, by using a carefully-crafted
IAM permissions for more individual functions. It might be a hard and repeated task, but the opportunity to
give each function its own role makes it all worthwhile.

Injection attacks are now open to code injection more than ever by the common use of languages like Python
and NodeJS in serverless. But would probably be less significant inside the container.

But besides the attacks we know so well, there are serverless-designated attacks; Denial of Service (DoS)
attacks become less of a risk in serverless, due to the ephemeral state of functions. However, Denial of
Wallet (DoW) in which the attacker does not try to prevent the service, but to waste the organization’s
money, is much more of a concern (​ Serverless Top 10 ​ report, ​ Future Work ) ​ .

All that means that hackers would have to come up with a different approach for attacks, which means
different attack vectors. The application developers will not be able to put a single traditional perimeter
protection and would need to change their way of thinking, as almost none of the mitigations suggested for
traditional systems would fit in the serverless world.