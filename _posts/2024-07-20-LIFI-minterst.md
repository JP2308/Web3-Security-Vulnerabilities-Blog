---
title: "Two Ways to Lose Millions: Li.Fi's Approval Nightmare and Minterest's Flash Loan Fumble"
date: 2024-07-20
categories: [DeFi, Smart Contracts, Security, Flash Loans]
---

Mid-July delivered a masterclass in how different DeFi protocols can lose user funds in completely different ways. Li.Fi managed to approve $11.6 million through sloppy facet deployment, while Minterest got schooled by a $1.4 million reentrancy attack that reads like a textbook example of what not to do with flash loans.

What's particularly instructive about these incidents is how they showcase two fundamental failure modes in DeFi: trusting user input and failing to anticipate callback manipulation.

## 🔐 Li.Fi: When Facet Deployment Goes Wrong

Li.Fi's July 16th incident was the kind of mistake that makes you wonder about deployment processes. A new smart contract facet was pushed live without properly validating incoming data, allowing attackers to manipulate the approval mechanism and authorise $11.6 million in token transfers.

**The Core Issue:**
```solidity
// Likely what happened (simplified)
contract VulnerableFacet {
    function processSwap(bytes calldata data) external {
        // Missing validation strikes again
        (address token, address spender, uint256 amount) = abi.decode(data, (address, address, uint256));
        
        // The million-dollar mistake
        IERC20(token).approve(spender, amount);
    }
}
```

The attacker essentially crafted calldata that said "approve me to spend all of user X's tokens," and the contract obliged without question. It's the same pattern we've seen repeatedly: external input gets processed as trusted data. In my view, this highlights why facet-based architectures require extra scrutiny. When you can hot-swap contract logic, every deployment becomes a potential attack vector.

## 🧩 Minterest: Flash Loan Reentrancy 101
Minterest's July 14th exploit was more sophisticated but equally preventable. The attacker used a classic reentrancy pattern with flash loans to manipulate exchange rates and extract $1.4 million in mETH and WETH.

### The Attack Flow:

1. Flash loan 4.265M USDY tokens
2. Use borrowed funds to manipulate mUSDY exchange rates
3. Re-enter lendRUSDY function during callback with manipulated rates
4. Profit from the rate discrepancy
5. Bridge stolen funds to Ethereum via Stargate

```solidity
// What should have prevented this
contract SecureMinterest {
    bool private locked;
    
    modifier noReentrant() {
        require(!locked, "Reentrant call");
        locked = true;
        _;
        locked = false;
    }
    
    function lendRUSDY(uint256 amount) external noReentrant {
        // Critical functions need reentrancy protection
        // ... rest of function logic
    }
}
```

What frustrates me about this attack is how textbook it is. Flash loan manipulation combined with reentrancy isn't novel — it's been a known attack pattern for years.

## 🧠 What These Incidents Reveal
- Li.Fi's Lesson: Deployment processes matter as much as code quality. When you're pushing new contract facets, every line of code is a potential vulnerability. The approval mechanism should have had multiple validation layers.
- Minterest's Lesson: If your protocol involves callbacks and external calls, reentrancy protection isn't optional. The fact that this attack succeeded suggests fundamental misunderstanding of callback security.
- Shared Failure: Both protocols fell into the trap of assuming their security model was complete. Li.Fi trusted deployment processes, Minterest trusted callback isolation. Both assumptions proved wrong.

I think these incidents demonstrate why DeFi security requires paranoid thinking. Every external interaction is potentially hostile, every callback is a reentrancy risk, and every deployment is a chance to introduce new attack vectors. The combined $13 million loss from these incidents could have been prevented with established security practices: comprehensive input validation for Li.Fi, reentrancy guards for Minterest. That's not a technology problem — it's a process problem.