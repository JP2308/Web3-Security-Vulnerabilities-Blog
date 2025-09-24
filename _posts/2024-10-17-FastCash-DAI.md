---
title: "Crypto Threat Landscape 2024: DAI Laundering, Ransomware, and FASTCash Malware"
date: 2024-10-17
categories: [Crypto, Cybersecurity, Malware, Ransomware]
---

# 🧊 Crypto Money Laundering: Why DAI is Favored Over USDT

## 🔍 Introduction

In the evolving landscape of cryptocurrency, stablecoins play a pivotal role in facilitating transactions. However, their inherent characteristics make them susceptible to misuse, particularly in money laundering activities. I found this super intersting and I wanted to share, especially given Tether's often forgettable rep of having large amounts of money being washed through its protocol. 

## 🧩 DAI: A Decentralised and Unfreezable Stablecoin vs USDT: Centralised Control and Regulatory Oversight

DAI operates on the Ethereum blockchain and is governed by MakerDAO, a decentralised autonomous organisation. Key features include:
- Not subject to centralised control  
- Resistant to regulatory actions such as asset freezing or blacklisting  
- Criminals exploit this feature to launder funds without the risk of seisure

USDT is issued by Tether Limited, a centralised entity. Key features include:  
- Complies with regulatory requirements  
- Can freeze assets and blacklist addresses involved in illicit activities  
- Criminals risk having their funds seised if they use USDT

## 📊 Criminal Preferences and Blockchain Analysis

In short, the blockchain analytics firm Elliptic gave a report that noted criminals often convert their stolen proceeds (e.g. rugpulls, whale exploits etc.) into stablised coins given the volatility of the crypto markets. One of the key advantages of DAI is its integration  with DeFi protocols (lending markets like Aave, Maker’s own Spark, DEX liquidity pools). This means criminals can park, stake, or even earn yield on stolen funds without fear of centralised seisure. USDT on the other hand, may be widely used in trading, but its centralisation makes it a liability for long-term parking.


## 🧠 Conclusion

DAI’s role here says less about its “criminal friendliness” and more about the tradeoff between decentralisation and compliance. DAI is permissionless by design, which is exactly why criminals prefer it — and why regulators may eventually target the on/off-ramps around it. Ironically, the same qualities that make DAI attractive to bad actors (no freeze function, decentralised governance, DeFi-native) are also the ones valued by privacy-minded but legitimate users.
---

# 💻 Ransomware in 2024: Latest Trends, Mounting Threats, and Government Response

## 📈 Surge in Ransomware Attacks

In 2024, ransomware attacks surged dramatically in both frequency and sophistication.  
- Targeted sectors: critical infrastructure, healthcare, telecoms, financial services  
- July 2024: 60 publicly disclosed attacks (↑58% vs. 2023)  
- August 2024: 63 attacks (highest on record)  
- 30% of August attacks specifically targeted healthcare

## 🧩 High-Profile Ransomware Incidents

- June 2024: BlackSuit ransomware attacked CDK Global, disrupting thousands of dealerships in North America  
- Demanded 387 BTC (~$25M USD), funds not recovered  
- Highlighted ransomware's capacity to disrupt large-scale operations and supply chains

## 🛡️ Government and Law Enforcement Response

- October 2024: US Treasury OFAC, with UK and Australia, sanctioned seven individuals and two entities linked to Evil Corp
- Evil Corp led by Maksim Yakubets, notorious since 2009 for developing Dridex malware  
- Impacts: >40 countries, financial losses >$100M USD, especially healthcare, finance, and critical infrastructure

## 📉 Decline in Ransomware Payments

- 2024 ransomware payments: $814M (↓35% from $1.25B in 2023)  
- Second half 2024: payments fell $171M vs. first half  
- Decline attributed to:
  - Law enforcement action against AlphV and Lockbit  
  - Newer groups less capable of demanding large ransoms  
  - Increased global awareness, improved defenses, stricter crypto regulations

## 🧠 Conclusion

Ransomware attacks have increased in frequency and sophistication, but financial impact has decreased due to law enforcement and security measures. Continuous vigilance and international cooperation remain critical.

---

# 🐧 FASTCash for Linux: Analysing a New Variant of DPRK-Attributed Malware

The cybersecurity community has long tracked FASTCash, a notorious malware family attributed to DPRK threat actors. Traditionally deployed on Windows and AIX systems, FASTCash is designed to manipulate payment switch infrastructure and enable large-scale, unauthorised ATM withdrawals.  

Now, researchers have uncovered a Linux-based variant, marking a significant evolution in the malware’s development and demonstrating its operators’ ongoing adaptability.

---

## 🧩 What is FASTCash?

At its core, FASTCash is “payment switch” malware. Once installed inside financial networks, it intercepts card transaction messages and manipulates them to appear valid. This allows attackers to force authorisations for fraudulent withdrawals, often across multiple ATMs simultaneously.  

The scale and precision of these attacks have made FASTCash one of the most financially damaging malware families linked to DPRK.

---

## 🐧 The New Linux Variant

Recent analysis has revealed a compiled version of FASTCash tailored for Ubuntu Linux 20.04. Key findings include:

- **Development timeline**: Compiled after April 21, 2022, likely within a VMware VM.  
- **Core functionality retained**: Intercepts declined transactions and converts them into fraudulent approvals, authorising random withdrawal amounts.  
- **Target environment**: Designed specifically for Linux-based financial systems, expanding the malware’s attack surface beyond its earlier Windows and AIX focus.

This discovery underscores the attackers’ efforts to ensure cross-platform reach within global financial infrastructure.

---

## 🔍 Technical Observations and IoCs

While the Linux variant behaves much like its predecessors, a few details stand out:

- It remains focused on the same regional financial networks previously targeted.  
- As of publication, the malware has no recorded detections, suggesting it is both stealthy and under active use.  
- Indicators of Compromise (IoCs) have been released to help defenders detect and mitigate this threat.

---

## 🧠 Final Thoughts

The emergence of a Linux-capable FASTCash varian* highlights the evolving nature of DPRK cyber operations. Financial institutions can no longer assume that legacy systems alone are at risk; even modern Linux-based payment infrastructures may be directly targeted.  

The lesson is clear: organisations operating within the financial sector must invest in layered defenses, conduct regular threat-hunting exercises, and apply rigorous monitoring across all platforms—Windows, AIX, and now Linux.  

FASTCash is not just a relic of past attacks; it continues to adapt, reminding us that nation-state cybercrime is both persistent and innovative.

---

## 📚 References

- [Why DAI is Favored Over USDT for Crypto Money Laundering](https://medium.com/coinmonks/why-dai-is-favored-over-usdt-for-crypto-money-laundering-ec948b14fc7d)  
- [Ransomware in 2024: Latest Trends, Mounting Threats, and the Government Response](https://www.trmlabs.com/resources/blog/ransomware-in-2024-latest-trends-mounting-threats-and-the-government-response?utm_source=substack&utm_medium=email)  
- [FASTCash for Linux](https://haxrob.net/fastcash-for-linux/?utm_source=substack&utm_medium=email)  
- [Ransomware Attacks 2024 – Most Devastating Year Yet?](https://www.sygnia.co/blog/ransomware-attacks-2024/)  
- [New Linux Variant of FASTCash Malware Targets Payment Switches in ATM Heists](https://thehackernews.com/2024/10/new-linux-variant-of-fastcash-malware.html)

