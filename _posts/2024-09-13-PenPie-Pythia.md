---
title: "Week 36: The Return of Reentrancy - $27M+ Lost in Classical Smart Contract Attacks"
date: 2024-09-10
categories: [Security, Reentrancy Exploits, Smart Contract Vulnerabilities, Vanity Address, Technical Analysis]
---

This week delivered a painful reminder that the DeFi space still hasn't learned the basics of smart contract security. PenPie Protocol lost $27 million to a textbook reentrancy attack, while MakerDAO discovered their deployer keys were compromised thanks to the Profanity vanity address generator vulnerability that we've known about for years.


## PenPie Protocol: $27M Reentrancy Masterclass

The attack leveraged a re-entrancy vulnerability in the platform's smart contracts, explicitly targeting the `_harvestBatchMarketRewards` function in the Penpie Staking contract.

**The Vulnerable Function:**

```solidity
contract PendleStaking {
    mapping(address => mapping(address => uint256)) public userMarketReward;
    
    function _harvestBatchMarketRewards(
        address[] calldata markets,
        address user
    ) internal {
        for (uint256 i = 0; i < markets.length; i++) {
            address market = markets[i];
            
            // VULNERABILITY: No reentrancy guard here
            uint256 rewardAmount = userMarketReward[user][market];
            
            if (rewardAmount > 0) {
                // Critical flaw: State update happens AFTER external call
                userMarketReward[user][market] = 0;
                
                // External call that enables reentrancy
                _transferReward(user, rewardAmount);
            }
        }
    }
    
    // Missing: nonReentrant modifier
    function harvestBatchMarketRewards(address[] calldata markets) external {
        _harvestBatchMarketRewards(markets, msg.sender);
    }
}
```

**The Critical Flaw:**
This type of vulnerability allows an attacker to call a function multiple times before it is able to make important state updates. Since the vulnerable function lacked proper reentrancy defenses, the attacker was able to inflate the reward balances that would be assigned to them.

In my view, this represents a fundamental failure of development processes. Reentrancy protection should be as automatic as adding semicolons to your code. The fact that a protocol handling millions of dollars deployed without basic guards is inexcusable.


---

## 2. MakerDAO: Profanity Vanity Address Generator Vulnerability Resurgence

The MakerDAO incident is somehow even more frustrating because the Profanity vulnerability has been public knowledge since 2022. The vanity address generator used predictable randomness that reduced private key entropy from 256 bits to just 32 bits, making brute force attacks trivial.



**The Technical Breakdown:**

```python
# Profanity's flawed key generation process
# What Profanity actually did (simplified)
def generate_weak_key():
    seed = random.getrandbits(32)  # Only 4.3 billion possibilities
    private_key = sha256(str(seed))
    return private_key

# What attackers can do
def crack_profanity_key(target_address):
    for seed in range(2**32):  # Brute force 4.3B seeds
        key = sha256(str(seed))
        if derive_address(key) == target_address:
            return key  # Found it
```

With 1,000 GPUs, attackers can crack any 7-character Profanity-generated address in about 50 days. The fact that major protocols are still using compromised deployer addresses two years after this vulnerability was disclosed shows either negligence or ignorance.

---

## 3. Secondary Incidents: The Supporting Cast of Vulnerabilities

Pythia Protocol lost $500K to oracle manipulation, and Ethervista got front-run for $1.4M. Both attacks used techniques that have been known since the early days of DeFi, yet somehow protocols keep implementing the same vulnerable patterns.


### Pythia Protocol: Oracle Manipulation


```solidity
// Simplified oracle manipulation vulnerability
contract VulnerableOracle {
    mapping(address => uint256) public prices;
    
    function updatePrice(address token, uint256 price) external {
        // VULNERABILITY: No access control on price updates
        prices[token] = price;
    }
    
    function getPrice(address token) external view returns (uint256) {
        return prices[token];
    }
}

// Attacker exploitation
contract OracleManipulator {
    VulnerableOracle oracle;
    DeFiProtocol target;
    
    function manipulateAndProfit() external {
        // Step 1: Manipulate oracle price
        oracle.updatePrice(TARGET_TOKEN, INFLATED_PRICE);
        
        // Step 2: Use inflated price for profit
        target.borrowAgainstCollateral(TARGET_TOKEN, MAX_BORROW_AMOUNT);
        
        // Step 3: Restore original price (if needed)
        oracle.updatePrice(TARGET_TOKEN, ORIGINAL_PRICE);
        
        // Step 4: Profit from arbitrage
    }
}
```

---

## 📚 Technical References and Exploitation Resources

I'm conscious that this was a bit more technical and longer form, so I have referred to the resources I used to generate some of these insights, below:

**Code Repositorie**
- [PenPie Exploit PoC](https://github.com/blockapex/penpie-exploit-poc)
- [Reentrancy Guard Implementations](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/ReentrancyGuard.sol)
- [Profanity Vulnerability Scanner](https://github.com/rebryk/profanity-brute-force)

**Research Papers**
- [Formal Analysis of Reentrancy Vulnerabilities](https://arxiv.org/abs/2105.02881)
- [Vanity Address Security Assessment](https://eprint.iacr.org/2022/847.pdf)
- [Oracle Manipulation Attack Vectors](https://arxiv.org/abs/2009.14021)

**Security Tools**
- [Slither Static Analysis](https://github.com/crytic/slither) - Automated vulnerability detection
- [Mythril Symbolic Execution](https://github.com/ConsenSys/mythril) - Deep contract analysis
- [Securify2 Formal Verification](https://github.com/eth-sri/securify2) - Mathematical correctness proofs


