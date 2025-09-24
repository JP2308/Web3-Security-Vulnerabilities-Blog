---
title: "Security: Clober Reentrancy ($500k), 1inch Key Compromise, Radiant $50M Update"
date: 2024-12-14
categories: Reported Exploits
---

This week delivered a perfect trifecta of Web3 security failures, showcasing how protocols can lose millions through completely different attack vectors. Radiant Capital lost $50 million to endpoint malware that bypassed all their on-chain security measures, 1inch had their resolver compromised through basic private key mismanagement, and Clober lost $500K to a reentrancy exploit that should have been caught in code review.

What makes these incidents particularly instructive is how they demonstrate that Web3 security failures span every layer of the technology stack. You can have perfect smart contract audits and still lose everything to malware on a developer's laptop, or implement sophisticated on-chain governance that gets bypassed by a single compromised private key.


---

## 1. Radiant Capital – When Malware Beats Multi-Sig

Radiant's $50 million loss represents perhaps the most sophisticated social engineering attack the DeFi space has seen. An attacker impersonating a former contractor approached a Radiant developer on Telegram, sharing what appeared to be a legitimate audit report titled "Penpie_Hacking_Analysis_Report.zip" and asking for feedback.

### What Happened
- A Radiant developer was approached on Telegram by an impersonator posing as a **former contractor**.  
- The attacker shared a **zipped PDF** named `Penpie_Hacking_Analysis_Report.zip`, requesting feedback.  
- The file was actually a **macOS malware loader (INLETDRIFT)**:  
  - It displayed a benign front-end UI (fake audit report).  
  - In the background, it executed **AppleScript-based payloads**.  
  - Used **spoofed domains** to appear legitimate.  
- Once the malware ran, the attacker gained access to the developer’s **signing and execution pipeline**, allowing malicious transactions to be executed as though they came from trusted sources.  

### Technical Vector
- **Endpoint intrusion** → compromised developer signing keys.  
- **Document masquerading** → malicious executable packaged in what looked like a harmless report.  
- **Bypassed on-chain safety nets** (e.g., Tenderly simulations, audits), since compromise occurred *before* transactions hit the chain.  

### Key Lessons
The file wasn't just malware disguised as a document - it was a carefully crafted macOS malware loader called INLETDRIFT that displayed a convincing front-end UI while executing AppleScript payloads in the background. The attacker used spoofed domains to make everything appear legitimate and specifically targeted the developer's signing and execution pipeline.

What makes this attack particularly insidious is how it bypassed every on-chain security measure Radiant had implemented. All their Tenderly simulations, audit processes, and governance mechanisms were useless because the compromise occurred at the developer endpoint level, before transactions ever reached the blockchain.

In my view, this incident highlights a fundamental blind spot in DeFi security thinking. The industry has become obsessed with smart contract audits and on-chain governance while largely ignoring the human infrastructure that actually executes transactions. A malware-infected developer can sign legitimate-looking transactions that drain the entire protocol, and no amount of on-chain security will stop them.

**Source:** [Radiant Capital Incident Update (Medium)](https://medium.com/@RadiantCapital/radiant-capital-incident-update-e56d8c23829e)

---

## 2. 1inch Resolver – Private Key Compromise
The 1inch resolver compromise demonstrates how basic private key mismanagement can undermine even sophisticated protocols. Someone gained unauthorised access to the resolver contract owner's private key, allowing them to modify contract parameters and attempt unauthorised fund movements.

**Attack Flow:**
- Unauthorised access was detected in the resolver contract owner’s private key.  
- Attacker used the key to:  
  - Change resolver contract parameters.  
  - Attempt unauthorised fund movements.  

**Response**
- 1inch immediately revoked the compromised key.  
- Migrated critical contracts to multisignature (multisig) wallets.  
- Conducted comprehensive audit of connected contracts.  
- Community engagement: attacker addresses flagged and shared.  

### Key Lessons
To their credit, 1inch responded quickly by revoking the compromised key and migrating to multisig wallets. But the real question is why they were using single-key control in the first place. A central lesson for me is that key management should incorporate HSMs (Hardware Security Modules), MPC wallets, and key rotation policies. All of these actively mitigate this single point of failure.  

**Source:** [1inch Unauthorized Access Blog](https://blog.1inch.io/1inch-promptly-responds-to-an-unauthorized-access-incident/?utm_source=substack&utm_medium=email)

---

## 3. Clober Dex – Reentrancy Exploit ($500k)
Post-audit code changes introduced a vulnerability in the Liquidity Vault's burn function that allowed attackers to re-enter and drain funds through incomplete state updates.

**Attack Flow**
1. Deploy malicious ERC-20 token contract.  
2. Interact with Clober Liquidity Vault using the malicious token.  
3. Trigger `burnHook` callback during `_burn`.  
4. Re-enter the withdrawal logic multiple times before state updates.  
5. Drain ≈ $500k in vault assets.  

### Key Lessons
Really simple stuff that I have written about constantly:
- Always follow Checks-Effects-Interactions (CEI): update state before external calls;  
- Add reentrancy guards to all externally exposed callbacks; 
- Post-audit changes must be re-audited — audits only cover the version reviewed; and 
- Treat external tokens and strategies as adversarial actors in vault interactions.  

**Source:** [rekt – Clober Dex Exploit](https://rekt.news/cloberdex-rekt?utm_source=substack&utm_medium=email)

---

# 🧩 Closing Thoughts

These three cases show different classes of vulnerabilities:  

- **Radiant Capital**: endpoint intrusion → human layer compromised.  
- **1inch**: key management failure → governance layer compromised.  
- **Clober Dex**: reentrancy exploit → smart contract logic compromised.  

Together, they illustrate that **Web3 security must be holistic**: securing developer devices, strengthening cryptographic key infrastructure, and enforcing rigorous contract design patterns.  

---
