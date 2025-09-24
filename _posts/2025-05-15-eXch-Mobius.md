---
title: "Mobius (MBU) Exploit — Deep Forensic Dive"
date: 2025-05-15
categories: [DeFi, Forensics, Exploits, MBU]
---

# 🔬 Mobius (MBU) Exploit — Technical Deep Dive & On-Chain Forensics

**TL;DR:** 11 May 2025, an attacker exploited a decimal/price-normalisation bug in the Mobius (MBU) token system on BNB Chain (BSC), minting an outsized amount of tokens from a tiny BNB input, dumping them into a Pancake/CakeSwap BUSD pool, and converting proceeds into stablecoins. Net loss ≈ $2.15M. The attacker then mixed proceeds via Tornado Cash (multiple 100 BNB deposits). Key on-chain artifacts (exploit TX, attacker EOA, malicious contract) are included below for reproducible forensic work.

---

## 1) Incident snapshot

- **Date / time (BSC):** 11 May 2025 — exploit tx at `2025-05-11 07:33:46 UTC`. 
- **Protocol / token:** Mobius Token (`MBU`) on BNB Chain. Contract (reported): `0x0dfb6ac3a8ea88d058be219066931db2bee9a581`.   
- **Loss:** ~**$2,157,126** (converted stablecoins / USDT).
- **Exploit tx (primary mint + dump):** `0x2a65254b41b42f39331a0bcc9f893518d6b106e80d9a476b8ca3816325f4a150`.  
- **Attacker EOA (collector):** `0xB32A53Af96F7735D47F4b76C525BD5Eb02B42600`. (BscScan label: `Fake_Phishing1105326`) 
- **Malicious contract used as minter/exploit agent:** `0x631adFF068D484Ce531Fb519Cda4042805521641`.   
- **Observed minted amount:** ~**9.73 × 10^15** (9,731,099,570,720,980.66 MBU — printed in TX logs).

---

## 2) High-level attack flow (what the attacker did)

1. **Deploy malicious contract** (attacker EOA deploys `0x631adF...`), designed to call the Mobius mint/issuance logic or proxy that reads BNB price and mints MBU based on a supplied `amount * price`. The contract was deployed ~2 minutes before the exploit sequence. 
2. **Send tiny deposit (0.001 BNB)** to the exploit contract; due to the price calculation bug, the deposit was valued ~10^18× larger than intended. Contract mints **quadrillions** of MBU to attacker or intermediary. 
3. **Dump MBU** into the Pancake/CakeSwap liquidity pool (MBU/BUSD-T or similar), pulling real stablecoins (BUSD/USDT) out of the pool. The dump drained ~28.5M MBU worth ≈ $2.15M in stablecoins. 
4. **Swap/convert proceeds to BNB/USDT** and route through mixing services (multiple Tornado Cash deposits) to obfuscate the trail. The EOA made ~21 × 100 BNB deposits to Tornado Cash proxy addresses as the exit. 

---

## 3) Root cause (the bug, explained technically)

### The decimal / normalisation mistake
Multiple post-mortems and security shops describe the vulnerability as a double-scaling bug: reserves were normalised to 18 decimals then scaled again by `1e18` when computing the price, producing an astronomically large price figure that broke the minting/collateral accounting.

A canonical vulnerable pattern:

```solidity
// PSEUDOCODE — illustrates the problematic normalisation + extra scaling
function getBNBPriceInUSDT(address bnbToken, address usdtToken) external view returns (uint256 price) {
    (uint112 r0, uint112 r1, ) = IPancakePair(pair).getReserves();
    uint8 d0 = IERC20(token0).decimals();
    uint8 d1 = IERC20(token1).decimals();

    // normalise reserves to 18 decimals
    uint256 norm0 = uint256(r0) * (10 ** (18 - d0));
    uint256 norm1 = uint256(r1) * (10 ** (18 - d1));

    // <-- BUG: extra * 1e18 multiplier here causes 1e18 over-scaling
    if (token0 == bnbToken) {
        price = (norm1 * 1e18) / norm0;
    } else {
        price = (norm0 * 1e18) / norm1;
    }
}
```

### Effect

If price is used to credit freshly minted tokens or compute collateral value, an overinflated price lets the attacker mint vastly more tokens per unit deposit.  
This is exactly what occurred in **Mobius**:  
Microscopic BNB input → enormous token issuance → dump.  

**Source:** [Halborn](https://www.halborn.com)

---

### Why this is so destructive

- **No supply / mint cap checks**: minting logic did not check absolute token supply or enforce sanity bounds vs. expected collateral.  
- **No price sanity gate**: price oracle callers didn't validate that price was within a plausible band (e.g., vs. Chainlink, previous sample, or a minLiquidity threshold).  
- **Atomicity & flash-style deploy**: attacker deployed a specialised contract and executed the entire flow in minutes (deploy → mint → dump → convert), leaving little time for human intervention.  

**Source:** [BNB Smart Chain Explorer](https://bscscan.com)

---

### 4) Forensic trail — concrete on-chain artifacts

These items let you reproduce the timeline and drill into the raw logs.

- **Exploit TX (mint + transfers + dump):**  
  [`0x2a65254b41b42f39331a0bcc9f893518d6b106e80d9a476b8ca3816325f4a150`](https://bscscan.com/tx/0x2a65254b41b42f39331a0bcc9f893518d6b106e80d9a476b8ca3816325f4a150)  

- **Attacker EOA (collector / orchestrator):**  
  [`0xB32A53Af96F7735D47F4b76C525BD5Eb02B42600`](https://bscscan.com/address/0xB32A53Af96F7735D47F4b76C525BD5Eb02B42600)  
  *(BscScan label: Fake_Phishing1105326. Shows multiple 100 BNB deposits to Tornado Cash.)*

- **Malicious contract used in exploit:**  
  [`0x631adFF068D484Ce531Fb519Cda4042805521641`](https://bscscan.com/address/0x631adFF068D484Ce531Fb519Cda4042805521641)  
  *Received 0.001 BNB → minted **9.73T MBU** in the same txn.*

- **Victim / drained contract transfers:**  
  28.5M MBU → [`0xB5252FCe...`](https://bscscan.com/address/0xB5252FCe...) → swapped to stablecoins.  
  Outcome: **$2.15M** in Binance-Peg USDT/USDC.  

- **Laundering pattern (Tornado Cash deposits):**  
  EOA shows multiple 100 BNB OUT deposits (~21 txns).  

**Source:** [BNB Smart Chain Explorer](https://bscscan.com)

---

### 5) Quick reproduction checklist for auditors

- **Token decimals + reserve tests:** unit test varying decimals (6, 8, 18, 24) and assert monotonic price logic.  
- **Fuzz price function calls:** fuzz `reserve0`, `reserve1`, and decimals; assert `0 < price < MAX_PRICE`.  
- **Sanity gates:** compare with Chainlink oracle median; accept only if within ±X%.  
- **Supply & mint limits:** cap per epoch; ratio check (max minted per BNB bounded).  
- **On-chain monitors:** watch Mint events > 1e6× historical mean; auto-pause protocol.  
- **Simulate atomic attacker flows:** deploy + exploit in same block in staging.  

**Example fuzz assertion (pseudocode):**
```python
# assert price within 1e6 factor of naive expectation
assert 1e-6 * (reserve1 / reserve0) < computed_price < 1e6 * (reserve1 / reserve0)
```

### 6) Mitigations & Recommendations

- **Remove hand-rolled pricing** → use vetted oracle networks (Chainlink, Pyth).  
- **Normalise decimals** → avoid multipliers, use a single canonical base (e.g., `wad = 1e18`).  
- **Add sanity checks** → enforce bounding price movement per block/update.  
- **Supply caps & invariants** → require checks like `newSupply <= oldSupply + MAX_MINT_PER_TX`.  
- **Pre-deployment fuzzing** → leverage Echidna, Foundry, Slither, Certora.  
- **Operational controls** → implement a governance circuit breaker.  
- **Monitoring & response** → set up auto-alerts for new contracts calling sensitive functions (`mint()`, `issue()`).  

**Sources:** [Halborn](https://www.halborn.com), [CertiK](https://www.certik.com)

---

### 7) Additional Reporting & Post-Mortem

- [Cointelegraph — Mobius Token exploited, $2.1M drained](https://cointelegraph.com)  
- [Halborn — Explained: The Mobius Hack](https://www.halborn.com)  
- [Rekt News — Exploit summary](https://rekt.news)  
- [CertiK — Incident analysis](https://www.certik.com)  
- [BscScan — Exploit TX + Attacker EOA](https://bscscan.com/tx/0x2a65254b41b42f39331a0bcc9f893518d6b106e80d9a476b8ca3816325f4a150)  

---

### 8) Appendix — Direct On-Chain Links

- [Exploit transaction (BscScan)](https://bscscan.com/tx/0x2a65254b41b42f39331a0bcc9f893518d6b106e80d9a476b8ca3816325f4a150)  
- [Attacker EOA](https://bscscan.com/address/0xB32A53Af96F7735D47F4b76C525BD5Eb02B42600)  
- [Malicious contract](https://bscscan.com/address/0x631adFF068D484Ce531Fb519Cda4042805521641)  
- [MBU token contract](https://bscscan.com/token/0x0dfb6ac3a8ea88d058be219066931db2bee9a581)  

---

### Final Technical Takeaway

This incident is a textbook example of how small **arithmetic / normalisation errors** in collateral pricing lead to **catastrophic economic exploits**:

1. **Numeric correctness bug** — double scaling / decimal mismatch.  
2. **No economic guards** — lack of mint caps or sanity bounds.  
3. **Atomic exploitability** — attacker deploys helper contract and drains in minutes.  
4. **Laundering** — Tornado Cash obfuscation of funds.  

**Action for developers & auditors:**  
Treat **price calculation & decimal handling** as a highest-risk area.

- Fuzz aggressively.  
- Add external oracle cross-checks.  
- Enforce rate limits & supply caps.  

---

### Sources & Further Reading

- [Cointelegraph — Mobius exploit coverage](https://cointelegraph.com)  
- [Halborn — Explained: The Mobius Hack](https://www.halborn.com)  
- [BscScan — Exploit TX & Attacker address](https://bscscan.com)  
- [CertiK — Incident analysis](https://www.certik.com)  
- [Rekt News — Summary & references](https://rekt.news)  
