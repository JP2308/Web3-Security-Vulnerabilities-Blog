---
title: "$67M Lost Across 10 Major Incidents"
date: 2024-09-26
categories: [Security, Exploit Analysis, CEX Hacks, DeFi Vulnerabilities, Telegram Bots]
---


This week will be remembered as one of the most devastating periods in cryptocurrency security history. Everything was hacked this week. Exchanges, smart contracts, telegram bots and some really exotic blockchains all lost almost $67M across 10 incidents. From centralised exchange compromises to sophisticated insider threats, attackers demonstrated an alarming variety of attack vectors. Below is my analysis of the week's most significant security breaches.

---

## BingX Exchange: $52M Hot Wallet Massacre
BingX managed to lose $52 million over five hours of systematic hot wallet drainage, proving that being a major Singapore-based exchange doesn't mean you understand operational security. North Korean attackers methodically emptied wallets across Ethereum, BNB Chain, Base, and Optimism, even bothering to grab the 0.036 ETH left in the BASE chain wallet.

### The Timeline of Failure:
06:00 UTC: Suspicious withdrawals begin
06:00-11:00 UTC: Continuous drainage for 5 hours
11:00 UTC: Exchange finally notices and halts withdrawals

Five hours. It took BingX five hours to notice that $52 million was walking out the door. In modern exchange operations, unusual withdrawal patterns should trigger alerts within minutes, not after your entire hot wallet treasury is gone.

### The Technical Reality:
The attack targeted hot wallets lacking adequate multi-signature controls and real-time monitoring. The systematic nature across multiple chains suggests the attackers had detailed knowledge of BingX's infrastructure — possibly through insider access or comprehensive reconnaissance.

BingX becomes the third major Asian exchange hacked in 2024, following Indodax and WazirX. This pattern suggests either systematic targeting of the region or consistently weaker security practices compared to Western exchanges. The five-hour detection window represents a fundamental failure to understand what exchange security monitoring should look like. $52 million doesn't just slip out unnoticed unless your monitoring systems are decorative.

## DeltaPrime: The $6M Inside Job
DeltaPrime lost $6 million when someone with admin key access upgraded their proxy contracts to malicious implementations and systematically drained the protocol. The twist? DeltaPrime may have hired a North Korean IT worker months earlier, suggesting this was a long-term infiltration rather than an opportunistic hack.

**The Attack Flow:**
1. Compromised admin private key
2. Upgraded five proxy contracts to malicious versions
3. Drained $6M from Arbitrum contracts
4. Deposited $2M into other protocols to earn additional rewards

This represents DeltaPrime's second major security incident in months. In July, they lost $1 million to a "misconfiguration." Two major breaches in a few months suggests systematic security process failures rather than isolated incidents. The North Korean connection, if confirmed, highlights why hiring practices for protocols handling millions in user funds need to be more rigorous than typical tech company screening.

## Telegram Trading Bot Carnage
Three major Telegram trading bots — Banana Gun, Maestro, and Unibot — lost a combined $3.2 million through application-level vulnerabilities. The attacks exploited the fundamental architecture of trading bots, which require users to expose private keys for automated trading.

1. Banana Gun: $3M drained across 11 users through Telegram message oracle exploitation
2. Unibot: Call injection exploit using malicious call data to transfer approved tokens

What's notable about these attacks is they targeted the bot infrastructure itself rather than individual user security failures. This represents an evolution in attack methodology — why target individual users when you can compromise the platform they all trust?

Banana Gun handled the incident well, immediately reimbursing all affected users from treasury reserves. But the fact that multiple bot platforms got hit simultaneously suggests coordinated reconnaissance of the Telegram bot ecosystem.

## The Key Takeaway
This week is interesting to me for the reason that it shows attack vector diversification. That being, attackers demonstrating expertise across exchange infrastructure, DeFi protocols, trading bots, and exotic blockchain features. This suggests either coordination between specialised groups or individual actors with broad technical capabilities. Evidence that further suggests this case is the geographic concentration of Asian exchanges being disproportionately targeted, with BingX joining Indodax and WazirX in 2024's exchange hack hall of fame.



## 📚 Technical References & Further Reading

- [Halborn Security: BingX Hack Analysis](https://www.halborn.com/blog/post/explained-the-bingx-hack-september-2024)
- [Halborn Security: DeltaPrime Exploit Technical Deep-dive](https://www.halborn.com/blog/post/explained-the-deltaprime-hack-september-2024)
- [Rekt News: Banana Gun Incident Report](https://rekt.news/bananagun-rekt)
- [BlockThreat: Week 38 Comprehensive Analysis](https://newsletter.blockthreat.io/p/blockthreat-week-38-2024)

---
