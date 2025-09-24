---
title: "Morpho Governance Exploit: A Deep Dive into DelegateKey Vulnerabilities"
date: 2025-04-21
categories: [DeFi, Morpho, Threat Intelligence, Infrastructure]
---

## Incident Overview
- **Date:** March 2025  
- **Protocol:** Morpho  
- **Attack Type:** Governance Exploit  
- **Funds Lost:** ~$450K in MORPHO tokens  

A governance vulnerability in Morpho’s **DelegateKey mechanism** was exploited, allowing the attacker to bypass proper governance processes and seize tokens directly from protocol-controlled addresses.

---

## Technical Breakdown

### Governance Flow
Morpho uses a **delegate key model**, where:
1. Users can delegate voting rights via `DelegateKey`.
2. Delegate keys interact with the governance contract.
3. Proposals execute through the DAO treasury.

### Vulnerability
The `DelegateKey` implementation failed to **validate execution contexts**. This allowed:
- A delegate key to call governance functions directly.  
- Execution of malicious payloads without quorum validation.  
- Seizure of treasury-controlled tokens.  

**Exploit Path (Simplified):**

```solidity
function delegateVote(address proposal) external {
    require(msg.sender == delegateKey, "Not delegate");
    proposal.execute(); // Missing guard!
}
```

**Exploit Steps**
1. Attacker acquired small voting power.
2. Registered a malicious delegate key.
3. Called governance execution functions directly.
4. Drained ~450K MORPHO tokens.

**On-Chain Evidence**
- Attacker Address: 0x41c8...b6f2
- Exploit Tx Hash: 0x5ce2cf51f0e6e099c33eabb84561cb01a1acb2c9fc26c5388cedec0e4292ce99
- Funds Laundered via: Tornado Cash + secondary mixers


**Recommended Fix** 

```solidity

require(msg.sender == governanceContract, "Unauthorised");
```

**Critical Insights**
- `DelegateKey` designs must enforce context validation.
- Governance attack surface grows with complex delegation mechanics.
- Exploit shows similarity to Compound governance bug (2020).
