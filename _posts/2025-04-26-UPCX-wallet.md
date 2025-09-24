---
title: "UPCX Wallet Exploit: Insecure ERC-20 Approvals and the Drain Attack"
date: 2025-04-26
categories: [DeFi, Morpho, Threat Intelligence, Infrastructure]
---

## Incident Overview
- **Date:** March 2025  
- **Protocol:** UPCX Wallet  
- **Attack Type:** Approval Exploit  
- **Funds Lost:** ~$1.2M (USDT, USDC, ETH)  

Attackers exploited a flawed **ERC-20 approval flow** in UPCX’s smart wallet contract, draining user balances by hijacking token allowances.

---

## Technical Breakdown

### Vulnerable Logic
The UPCX wallet contract allowed **unbounded approvals** to third-party addresses without proper revocation logic.

Example flaw:
```solidity
function approveSpender(address spender, uint256 amount) external {
    IERC20(token).approve(spender, amount); 
    // No check for existing allowance!
}
```

**This allowed attackers to:**
- Front-run approvals.
- Reuse stale allowances to drain funds.

**Exploit Steps**
1. Victims used UPCX wallet to interact with DeFi protocols.
2. Approvals were left unrevoked (approve(spender, amount) called multiple times).
3. Attacker monitored mempool → spotted UPCX wallet approvals.
4. Replayed stale approvals to transfer tokens to attacker-controlled wallet.

**Root Cause**
- Failure to follow ERC-20 approval best practices:
- Allowance must first be set to zero before updating.
- UPCX skipped this step, leaving race conditions exploitable.

**Recommended Fix**
```solidity

IERC20(token).approve(spender, 0); 
IERC20(token).approve(spender, amount);
OR use safeApprove() wrappers from OpenZeppelin.
```

**Mitigation & Response**
- UPCX paused wallet approvals.
- Emergency patch applied to enforce zero-reset before new approvals.
- Users instructed to revoke allowances manually via Revoke.cash.

**Critical Insights**
- ERC-20 approval issues are a recurring attack vector in DeFi.
- Similar to Uniswap approval exploit (2019).
- Wallets should abstract approvals away from users via permit signatures (EIP-2612).
