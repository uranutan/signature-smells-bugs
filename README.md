
# Signature Smells of Price/Reward Manipulation Smart Contract Bugs in 2023


A study in price/reward manipulation victim smart contract bugs to isolate their _signature scents_ and _code smells_ in an audit setting.


### The Goal

Deep dive into the smart contracts that are price/reward manipulation victims to seek

- What the bugs look like? 
- What are the bugs natural habitats?
- Where and what to look for in an audit?

Most important of all, 

> **How can we catch these bugs during an audit?**


### The Motivation


**BlockThreat [Week 16](https://newsletter.blockthreat.io/p/blockthreat-week-16-2023)**
>  Price oracle manipulation rings again and again as the primary root cause for most of these hacks. What’s causing protocol designers to miss this vulnerability? Is it deficiencies in the software development process, tools, education? Sounds like a great vulnerability class to focus on for defenders.

**BlockThreat [Week 29](https://newsletter.blockthreat.io/p/blockthreat-week-29-2023)**
> The rest of the compromises continue the trend of _Price Oracle_ and _Reward Manipulation_ exploits making weekly reports blur together. _Auditors, developers please focus on these two attack vectors in any part of your code that touches price data or calculates user payoffs._ 


The insights from the trusted bugs intelligence source, BlockThreat, are very compelling. 
This class of vulnerability appears so often this year that it seems no longer novel or its appearance becomes barely news. 
However behind the facade of familiarity with the term "price/reward manipulation" due to its sheer frequency of appearance, these bugs perhaps come in different shapes and sizes and worth studying further. This is my attempt to answer the call from Peter and lift up the "leaves" to stare at the bugs. 



### A Growing List of Hacks Involving Price/Reward Manipulation. 

These are extracts from the [Blockchain Threat Intelligence](https://newsletter.blockthreat.io/) newsletter (non-premium section) and will be continued until 31 Dec 2023. 

**Week1**

1. On January 3, 2023 GDS lost $187K in a **[reward manipulation](https://medium.com/@numencyberlabs/gds-flash-loan-attack-technical-analysis-ae3ceb7a022c)** exploit.
**Week 3**
2. On January 16, 2023 520 Token lost $11K in a **[price oracle manipulation](https://twitter.com/BeosinAlert/status/1614970065992179712)** attack.    
3. On January 17, 2023 Upswing lost $35K in a **[price oracle manipulation](https://twitter.com/BlockSecTeam/status/1615521051487932418)** attack.
4. On January 18, 2023 Quaternion lost $4K due to a **[reward manipulation](https://twitter.com/BlockSecTeam/status/1615625897671004161)** bug.

**Week 5**

5. On January 30, 2023 Bevo lost $45K in a **[reward manipulation](https://twitter.com/BlockSecTeam/status/1620030206571593731)** exploit.
6. On February 1, 2023 Bonq lost $120M in a **[price oracle manipulation exploit](https://rekt.news/bonq-rekt/)** where an attack was simply able to **[lie about the price to the oracle](https://akshaysrivastav.hashnode.dev/culprit-behind-the-120-million-bonq-attack)**.

**Week 11**

7. On March 13, 2023 Euler lost $197M due to a **[liquidation reward manipulation](https://medium.com/@omniscia.io/euler-finance-incident-post-mortem-1ce077c28454)** vulnerability. The exploit transaction was initially **[front-run by an MEV bot](https://etherscan.io/tx/0x44b559c86ca8ccac5c16df507516ef19c06042a48cdbd865cb9db4c52d15ea17)**, but the hard-coded address only benefitted the attacker. Things got really wild when the attacker first sent **[100 ETH to a random on-chain beggar](https://etherscan.io/tx/0xcc8edbe70d22176e90027bb07047b5cc7541b169ef9ef71ae6d6793f344b8bc5)** and later **[sent 100 ETH to the Ronin Bridge exploiter](https://etherscan.io/tx/0x202a67d3a1d52e4dd5e1eebe49da511164b6e4a1ebe717dcf4674dd83a2bd457)** address which in turn **[replied with an encrypted message](https://etherscan.io/tx/0xcf0b3487dc443f1ef92b4fe27ff7f89e07588cdc0e2b37d50adb8158c697cea6)** which some suspect was to phish but more likely to just thank or even recruit the bad actor, **[Cuckoo’s Egg](https://archive.org/details/The_KGB_The_Computer_and_Me_1990)** style. Euler attacker has since returned **[3000 ETH back to Euler](https://etherscan.io/tx/0x20f89e9f029c1552ac1b1e2346c8305924ac76d9252a84c91a2b3157c669ab6a)** and continues communicating with Euler to send back **[what is not theirs to keep](https://etherscan.io/tx/0xcc73d182db1f36dbadf14205de7d543cfd1343396b50d34c768529aaab46a1c0)**.


8. On March 16, 2023 ParaSpace was unsuccessfully targeted with a **[reward manipulation exploit](https://medium.com/@Ancilia/thunderstorm-come-to-para-space-68f1dd6995b9)**. BlockSec was able to detect attacker’s attempts and **[performed the attack themselves](https://twitter.com/BlockSecTeam/status/1636650252844294144)** rescuing $5M in assets. On the fun side, the attacker **[reached out to BlockSec trying to recoup their gas expenses](https://etherscan.io/tx/0x8eb65ef100eb65273e42f227fb4b4b639531c2c892f4aa60c118c84dc677f98b)**.


**Week 12**

9. On March 17, 2023 Anji Eco lost $37k in a **[price oracle manipulation](https://blog.solidityscan.com/anji-eco-hack-analysis-improper-upgrades-2cf6922d47d7)** exploit.
10. On March 22, 2023 Nuwa lost $110k in a **[price oracle manipulation](https://twitter.com/NumenAlert/status/1638563104668925952)** exploit.

**Week 13**

11. On March 29, 2023 UNMS lost $100k in a **[price oracle manipulation](https://twitter.com/BeosinAlert/status/1641018602878042113)** attack.
12. On April 1, 2023 Allbridge was targeting by two attackers using a **[price oracle manipulation](https://blog.solidityscan.com/allbridge-hack-analysis-improper-business-logic-564fbadf38b2)** attack. $570k were lost and about $470k were returned after one of the attackers turned “whitehat” following **[doxing](https://twitter.com/bbbb/status/1642486972697526274)**.

**Week 15**

13. On April 15, 2023 Hundred Finance lost $7m due to a **[price oracle manipulation](https://www.numencyber.com/hundred-finance-exploit-7-million/)** unique to empty Compound-like protocols.
14. On April 15, 2023 0x0 Audits lost 18k in a **[price oracle manipulation](https://twitter.com/pcaversaccio/status/1647370508751577089)** exploit.

**Week 16**

15. On April 17, 2023 DeFiGeek Japan was hit with a **[price oracle manipulation](https://docs.google.com/document/d/1cdl86tgbPbVRp7KvBXCGG9vfHUpF2yQRfIyPeLr8nlo/edit)** exploit resulting in the loss of $20k.
16. On April 19, 2023 OceanLife lost $11k in a **[price oracle manipulation](https://blog.solidityscan.com/ocean-life-token-hack-analysis-flash-loan-attack-ded51d0ee574)** exploit.
17. On April 20, 2023 Elastic BNB lost $10k due to a **[price oracle manipulation](https://twitter.com/BeosinAlert/status/1648970953307877377)** exploit.
18. On April 23, 2023 FilDA lost $700k due to a **[price oracle manipulation exploit](https://fildafinance.medium.com/filda-exploit-statement-49ec69e34c53)**. About $400k have since been **[returned and/or recovered](https://fildafinance.medium.com/filda-exploit-update-2-67ea532844)**.

**Week 17**

19.  On April 28, 2023 0vix Protocol **[lost $2m](https://0vixprotocol.medium.com/0vix-exploit-post-mortem-15c882dcf479)** in a **[price oracle manipulation exploit](https://quillaudits.medium.com/decoding-ovix-protocols-2-million-exploit-quillaudits-92befc250e7c)**.
20. On April 28, 2023 ForTube lost $80k in a **[price oracle manipulation exploit](https://twitter.com/AnciliaInc/status/1651984219990810624)**.

**Week 18**

21. On May 2, 2023 NeverFall lost $74k due to a **[price oracle manipulation](https://medium.com/neptune-mutual/how-was-neverfall-project-exploited-fc5240160427)** exploit.
22. On May 6, 2023 Block Forest lost $260k in a **[price oracle manipulation](https://twitter.com/AnciliaInc/status/1654906431534153728)** exploit.

**Week 19**

23. On May 9, 2023 Floki Inu lost $60 in a **[reward manipulation](https://twitter.com/AnciliaInc/status/1655971355790286849)** exploit.
24. On May 9, 2023 Weeb lost $30k due to a **[price oracle manipulation](https://twitter.com/Alchemyst0x/status/1656034647808065538)** exploit.
25. On May 10, 2023 Snooker lost $200k in a **[reward manipulation](https://blog.solidityscan.com/snooker-token-hack-analysis-acd89cce6311)** exploit.
26. On May 12, 2023 LW Token lost $48k in a **[price oracle manipulation](https://twitter.com/PeckShieldAlert/status/1656850634312925184)** exploit. 
27. On May 13, 2023 Bitpaid lost $1k in a **[reward manipulation](https://twitter.com/BlockSecTeam/status/1657411284076478465)** exploit that took 6 months to execute.

**Week 21**

28. On May 22, 2023 LunaFi lost $35k in a **[reward manipulation](https://medium.com/neptune-mutual/how-was-lunafi-exploited-80d661e3a08a)** exploit.
29. On May 23, 2023 CS Token lost $714k in a **[price oracle manipulation](https://twitter.com/BeosinAlert/status/1661202338290487296)** exploit.
30. On May 24, 2023 GPT Token lost $155k in a **[reward manipulation](https://twitter.com/Phalcon_xyz/status/1661424685320634368)** exploit.
31. On May 28, 2023 Jimbos Protocol lost $7.5m in a **[price oracle manipulation](https://medium.com/numen-cyber-labs/a-detailed-analysis-of-arbitrum-based-jimbos-protocol-7-5-million-hack-36af84faee2)** exploit.

**Week 22**

32. On May 28, 2023 Baby Doge lost $137k in a **[price oracle manipulation](https://twitter.com/pennysplayer/status/1662737500870414341)** exploit.
33. On May 29 2023 El Dorado Exchange (EDE) lost $520k in a **[price oracle manipulation](https://medium.com/numen-cyber-labs/a-detailed-analysis-on-ede-finances-520k-hack-1187a5f274db)** exploit. Curiously the attacker returned most of the assets after **[calling out a potential backdoor](https://cryptoslate.com/ede-finance-attacker-returns-over-400k-after-team-admits-price-manipulation/)** in the project.
34. On May 31, 2023 ERC20Token lost $115k in a **[price oracle manipulation](https://twitter.com/BlockSecTeam/status/1663810037788311561)** exploit.
35. On June 1, 2023 Cellframe lost $74k in a **[price oracle manipulation](https://slowmist.medium.com/a-brief-analysis-on-the-cellframe-hack-b74b72b8e2e6)** exploit.
36. On June 1, 2023 DD Coin lost $126k in a **[price oracle manipulation](https://twitter.com/PeckShieldAlert/status/1664167119091810304)** exploit.


**Week 23**

37. On June 6, 2023 Compounder Finance lost $30K in a [price oracle manipulation](https://twitter.com/HypernativeLabs/status/1666330194708144129) exploit.

38. On June 6, 2023 UN Token lost $26K in a [reward manipulation](https://twitter.com/BeosinAlert/status/1666099182032265216) exploit.


39. On June 11, 2023 Trust the Trident (SELLC) lost $100K in a [price oracle manipulation](https://twitter.com/PeckShieldAlert/status/1668151112569065472) exploit.


40. Silo Protocol fixed interest manipulation vulnerability for markets with $0 deposits. The vulnerability was responsibly disclosed by **[konkodu](https://twitter.com/kankodu)** through Immunefi platform and the patch verified by Certora.
    - [https://medium.com/silo-protocol/vulnerability-disclosure-2023-06-06-c1dfd4c4dbb8](https://medium.com/silo-protocol/vulnerability-disclosure-2023-06-06-c1dfd4c4dbb8)
    - [https://medium.com/immunefi/silo-finance-logic-error-bugfix-review-35de29bd934a](https://medium.com/immunefi/silo-finance-logic-error-bugfix-review-35de29bd934a)
    - [https://medium.com/certora/silo-finance-post-mortem-3b690fffeb08](https://medium.com/certora/silo-finance-post-mortem-3b690fffeb08)


**Week 25**

Other protocols like Baby Doge (again), Shido, and others suffered from the more traditional price oracle and reward manipulation classes of attacks.

**Week 26**

Other compromises like Biswap, Themis were the **usual logic error/price oracle manipulation** exploits while Unagi had to self-hack after discovering an access control exploit.

**Week 27**

Other protocols such as Bao Community, Bamboo AI, LUSD experienced the more **traditional price oracle manipulation** attacks while Arcadia Finance got hit with reentrancy.

**Week 29**

Conic was hit multiple time in a single day losing $3.6m with Read-only Reentrancy and **Price Oracle Manipulation** exploits while multiple projects on BSC reused the same vulnerable airdrop randomness generator and paid $230k for the mistake.

**Week 32**

Almost $5.5m were stolen from various DeFi projects this week with the **usual arsenal of price oracle manipulation** (Zunami), **reward manipulation** (Cypher), reentrancy (Earning Farm), and other attack vectors.
