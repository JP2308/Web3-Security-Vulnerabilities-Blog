---
title: "2024 Web3 Security Year in Review: SEAL 911 & BSC Insights"
date: 2024-12-31
categories: [Web3 Security, DeFi, Exploits, Analysis]
---

# 🔐 2024 Web3 Security Review: SEAL 911 and BSC Insights

The year 2024 was a critical period for Web3 security, highlighting both vulnerabilities across protocols and the importance of rapid response and awareness programs. From phishing attacks to wallet drainers and hot wallet compromises, attackers continued to evolve their tactics. This article analyses SEAL 911 activity and BSC 2024 security trends, providing technical insights and lessons learned.

---

## 1️⃣ SEAL 911: Operational Impact and Key Metrics

SEAL 911, a volunteer-driven security response team, tracked and mitigated thousands of incidents in 2024:

- **Tickets handled:** 1,400+  
- **Active war rooms managed:** 75+  
- **Phishing domains blocked:** 150,000+  
- **Estimated funds saved:** ~$75M USD  

### Top 10 Ticket Types

1. Phishing  
2. Private key leaks  
3. Malware / RAT-infected devices  
4. Social media account takeovers  
5. Smart contract hacks  
6. Pig Butchering / Sha Zhu Pan  
7. Vulnerability disclosures  
8. Phishing URL reporting  
9. Frontrunning / white hat wallet rescues  
10. Domain hijacks  

> SEAL 911 is operated by 28 dedicated volunteers ([GitHub](https://github.com/security-alliance/seal-911#members)) who provide rapid, hands-on support for these incidents.

**Key Advice from SEAL 911:**

- Never sign transactions you do not fully understand  
- Triple-check domain authenticity before interacting  
- Avoid downloading unverified software  
- Never enter private keys into unknown platforms  

---

## 2️⃣ 2024 BSC Security Report: Key Findings

HashDit’s BSC report analysed 149 security incidents, totaling $62.9M USD in losses, marking a 61% decrease from 2023.  

### Notable Trends

- **Hot Wallet Compromise:** $27M in 6 incidents  
- **Wallet Drainers:** Spread across EVM and non-EVM chains, including TON and Cardano  
- **DPRK-affiliated Scams:** Targeting high-net-worth individuals and trending protocols  
- **Spoofing/Address Poisoning:** Losses exceeding $80M USD  

### Incident Breakdown

| Type | % of Incidents | Financial Loss |
|------|----------------|----------------|
| Hacks | 82.55% | $50.6M |
| Scams | 16.78% | $12.1M |
| Operational Mismanagement | 0.67% | N/A |

**Observation:** Hot wallets, phishing, and private key compromises remain the largest vectors for financial loss.

### Project Types Targeted

- **DeFi projects:** 48.64% of losses  
- **Individuals:** 14.66%  
- **CEX platforms:** 13.51%  

> DeFi projects remain the primary target due to staking/rewards mechanics and high TVL, combined with limited visibility and security practices.

---

## 3️⃣ Wallet Drainers and Spoofing Attacks

### Wallet Drainers

- **2024 monitored transactions:** 500,000+  
- **Total loss:** $20M+ USD  
- **Largest incident:** $8.3M in SolvBTC tokens (Dec 11, 2024)  

**Technical Note:** Attackers often deploy Angel Drainer contracts that exploit careless signature approvals across multiple addresses, including Create2-generated addresses.

### Spoofing / Address Poisoning

- **Victims targeted across chains:** Ethereum & BNB  
- **Techniques used:**  
  - Sending small amounts to fake addresses resembling target addresses  
  - Monitoring mempool for pending transactions  
  - Exploiting victim behavior for signature approval  

- **Largest single-chain loss:** $68M (Ethereum, May 2024)

---

## 4️⃣ Top 10 Incidents on BSC

| Project | Attack Vector | Damage | Notes |
|---------|---------------|--------|-------|
| Radiant | Hot Wallet Compromise | $17.8M | Multisig compromise, malicious contract deployed |
| User (@0xYuanbo) | Phishing | $8.3M | Angel Drainer via Create2 addresses |
| BingXOfficial | Hot Wallet Compromise | $6.84M | Abnormal network access; emergency measures deployed |
| ALEXLabBTC | Private Key Compromise | $4.3M | Proxy contract upgrades exploited |
| BullcoinBSC | Hot Wallet Compromise | $2.4M | Multisig keys centralised; backdoor installed |
| MEV Bot | Lack of Validation | $2.2M | Fallback function exploited, bridged to ETH |
| NFPrompt | Private Key Compromise | $1.5M | Admin wallets compromised |
| CUT2024CUT | Price Manipulation | $1.45M | Vulnerable token price mechanism |
| Duelbits | Hot Wallet Compromise | $1.1M | Private key compromised |
| CoinsPaid | Hot Wallet Compromise | $1M | Likely DPRK-linked Lazarus attack |

> **Observation:** Hot wallet compromise remains the single largest contributor to financial losses.

---

## 5️⃣ Lessons Learned and Best Practices

1. **Hot Wallet Security:** Enforce multisig, hardware key management, and limited privileges.  
2. **Phishing Awareness:** Educate users, enforce browser/extension-based alerts, and monitor social media channels for impersonations.  
3. **Wallet Drainer Mitigation:** Limit signature approvals, audit Create2 interactions, and monitor smart contracts interacting with user funds.  
4. **Cross-Chain Vigilance:** Non-EVM chains (TON, Cardano) are increasingly targeted.  
5. **Monitoring & Response:** Deploy proactive monitoring tools (e.g., HashDit Chrome Extension, Snaps) for both on-chain and off-chain threats.  
6. **Operational Security:** Avoid post-audit code changes without re-review, and maintain strict patch management.

---

## 6️⃣ Concluding Thoughts

2024 demonstrated that **user behavior, hot wallet security, and continuous monitoring** are critical to mitigating losses in Web3. While overall BSC losses decreased, sophisticated scams and exploit techniques continue to evolve. Volunteer organisations like SEAL 911 remain indispensable for **real-time incident response**, complementing platform-level security improvements.

---

## 📚 Sources

- [SEAL 911 2024 Summary](https://x.com/pcaversaccio/status/1874037752431358442?utm_source=substack&utm_medium=email)  
- [HashDit 2024 BSC Security Report](https://hashdit.github.io/hashdit/blog/bsc-2024-end-of-year-report/?utm_source=substack&utm_medium=email)
