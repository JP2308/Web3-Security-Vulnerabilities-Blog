---
title: "How Safe Wallet's Compromise Enabled the Biggest DeFi Heist Yet"
date: 2025-03-07
categories: [DeFi, Smart Contract Security, Exploits, Analysis]
---

### Bybit Cold Storage Exploit – $1.5B Stolen

**Incident Overview:**  
Bybit, a major cryptocurrency exchange, suffered a massive security breach that led to the theft of approximately $1.5 billion in Ethereum and related assets. The attackers exploited a compromised Safe multisig wallet front-end, which tricked Bybit's cold storage operators into signing a malicious contract upgrade. This upgrade allowed the attackers to drain the cold storage vault, including stETH, cmETH, and mETH tokens.

**Attack Timeline:**

- **Stage 1: Safe Compromise**  
  - February 2, 2025: Attacker registers a phishing domain (`getstockprice.com`) on Namecheap.  
  - February 4, 2025: Safe Wallet's developer is compromised with a fake trading software.  
  - February 5–17, 2025: Attacker conducts reconnaissance in Safe's AWS environment.

- **Stage 2: Bybit Preparation**  
  - February 18, 2025: Attacker deploys malicious contracts (`0x9622` and `0xbdd0`) on Bybit.  
  - February 19, 2025: Malicious JavaScript inserted into Safe's frontend targeting Bybit.  
  - February 20, 2025: Final testing of exploit contracts before the hack begins.

- **Stage 3: The Heist**  
  - February 21, 2025: Three cold storage signers unknowingly sign a malicious transaction that upgrades the cold storage vault.  
  - February 21, 2025: Attackers broadcast a new transaction to Bybit's safe, performing the upgrade.  
  - February 21, 2025: The cold storage vault is drained, and the heist is complete.

**Key Observations:**

This attack succeeded because it exploited a fundamental trust assumption in multisig operations: that the interface presenting transactions for signature is honest and accurate. Bybit's security model assumed that Safe's frontend would never present malicious transactions disguised as legitimate operations.

### The Technical Deception:
The malicious JavaScript modified how transactions were displayed in Safe's interface, showing Bybit's signers what appeared to be routine operational transactions while actually presenting contract upgrades that would enable fund drainage. This created a perfect attack where the security controls (multisig requirements) were technically functioning correctly, but the information presented to decision-makers was manipulated by compromised infrastructure.


### The Three-Minute Window:
Once the malicious upgrade was signed and executed, Bybit had less than three minutes to detect and respond before the funds were drained. No reasonable incident response system could prevent losses with such a short detection-to-drainage window. In my view, this demonstrates why transaction simulation and independent verification mechanisms are critical for high-value multisig operations. Relying solely on interface accuracy creates single points of failure in supposedly decentralised security models.

### Supply Chain Attack Evolution
This incident represents a significant evolution in crypto attack sophistication. Instead of targeting individual protocols or exploiting smart contract vulnerabilities, the attackers compromised critical infrastructure that multiple major protocols depend on.


### The Strategic Targeting:
By compromising Safe Wallet's infrastructure, the attackers gained potential access to treasury operations for hundreds of major DeFi protocols and crypto companies. The choice to target Bybit specifically likely reflected their assessment of which victim had the largest accessible funds relative to security monitoring capabilities.

### The Operational Security:
The two-week dwell time in Safe's environment demonstrates professional-level operational security. The attackers conducted thorough reconnaissance, deployed generic exploit contracts to avoid detection, and timed their attack to minimise response opportunities. This level of sophistication suggests either nation-state involvement or criminal organisations with advanced persistent threat capabilities. The $1.5 billion target size would justify significant resource investment in attack development.