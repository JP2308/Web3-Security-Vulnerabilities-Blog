---
title: "Resupply's $9.8M Rounding Error - When Empty Markets Strike Again"
date: 2025-07-06
categories: [Security, Rounding Errors, DeFi Exploits, Access Control, Parameter Validation]
---

## 🔴 Resupply Protocol: $9.8M Rounding Error Catastrophe

**The Crisis:** Resupply lost $9.8 million through exploitation of an empty market rounding error that allowed attackers to mint excessive protocol tokens.

### Technical Analysis: The Empty Market Vulnerability

The attack exploited a fundamental weakness in how DeFi protocols handle markets with minimal liquidity, allowing attackers to manipulate token minting through precision loss in mathematical calculations.

```solidity
// Vulnerable Resupply market implementation
contract VulnerableResupplyMarket {
    mapping(address => uint256) public totalSupply;
    mapping(address => uint256) public totalBorrows;
    
    function mint(uint256 amount) external returns (uint256) {
        // VULNERABILITY: Division by zero or near-zero values
        uint256 exchangeRate = getExchangeRate();
        
        // When totalSupply is 0 or very small, precision loss occurs
        uint256 mintTokens = amount / exchangeRate;
        
        totalSupply[msg.sender] += mintTokens;
        
        return mintTokens;
    }
    
    function getExchangeRate() public view returns (uint256) {
        if (totalSupply[address(this)] == 0) {
            return 1e18; // Initial exchange rate
        }
        
        // CRITICAL FLAW: Can be manipulated when totalSupply is very small
        return (getCash() + totalBorrows[address(this)]) * 1e18 / totalSupply[address(this)];
    }
    
    function redeem(uint256 tokens) external {
        uint256 exchangeRate = getExchangeRate();
        
        // VULNERABILITY: Can result in massive payouts due to inflated exchange rate
        uint256 redeemAmount = tokens * exchangeRate / 1e18;
        
        totalSupply[msg.sender] -= tokens;
        
        // Transfer out more than deposited due to rounding manipulation
        token.transfer(msg.sender, redeemAmount);
    }
}
```

### The Attack Execution

**Phase 1: Market Manipulation Setup**
```solidity
contract ResupplyExploit {
    VulnerableResupplyMarket target;
    IERC20 underlyingToken;
    
    function executeExploit() external {
        // Step 1: Donate small amount to manipulate initial state
        underlyingToken.transfer(address(target), 1);
        
        // Step 2: Mint minimal tokens to establish position
        target.mint(1);
        
        // Step 3: Manipulate the underlying token balance
        // by donating directly to the contract
        underlyingToken.transfer(address(target), 1000000 * 1e18);
        
        // Step 4: The exchange rate is now massively inflated
        // redeem(1) will return way more than initially deposited
        target.redeem(1);
        
        // Result: Drained $9.8M from the protocol
    }
}
```

**Root Cause Analysis:**
Since the 2023 Hundred Finance hack, this vulnerability class has now accounted for over $51 million in losses, as developers continue to learn the painful lesson that newly deployed markets demand extra care, especially around math precision and initial liquidity.

---

## 🔴 printMoney MEV Bot: $2M Access Control Failure

**The Crisis:** An MEV bot called printMoney lost $2 million due to insufficient function access control - one of the most basic security vulnerabilities.

### Technical Breakdown

```solidity
// Vulnerable MEV bot implementation
contract VulnerablePrintMoneyBot {
    address public owner;
    mapping(address => uint256) public funds;
    
    constructor() {
        owner = msg.sender;
    }
    
    // VULNERABILITY: Missing access control modifier
    function withdrawFunds(address token, uint256 amount) external {
        // CRITICAL FLAW: Anyone can call this function
        // require(msg.sender == owner, "Unauthorised");
        
        IERC20(token).transfer(msg.sender, amount);
    }
    
    // VULNERABILITY: Public function without protection
    function executeTrade(
        address targetContract,
        bytes calldata data,
        uint256 value
    ) external payable {
        // MISSING: Access control check
        // require(msg.sender == owner, "Unauthorised");
        
        (bool success, ) = targetContract.call{value: value}(data);
        require(success, "Trade failed");
    }
}
```

**The Exploit:**
Attackers simply called the unprotected withdrawal functions and drained $2 million from the bot's accumulated MEV profits.

---

## 🔴 Silo Finance: $500K Parameter Validation Failure

**The Crisis:** Silo Finance lost over $500,000 because of poor function parameter validation - another fundamental security issue.

### Technical Analysis

```solidity
// Vulnerable Silo Finance function
contract VulnerableSiloFinance {
    mapping(address => uint256) public userBalances;
    
    // VULNERABILITY: No parameter validation
    function liquidate(
        address user,
        address collateralAsset,
        address debtAsset,
        uint256 amount
    ) external {
        // CRITICAL FLAW: No validation of liquidation parameters
        // Attacker can specify arbitrary amounts and assets
        
        // MISSING VALIDATIONS:
        // - Is user actually liquidatable?
        // - Are the assets valid?
        // - Is the liquidation amount reasonable?
        // - Is msg.sender authorised to liquidate?
        
        // Direct execution without checks
        _transferCollateral(user, msg.sender, collateralAsset, amount);
        _repayDebt(user, debtAsset, amount);
    }
    
    function _transferCollateral(
        address from,
        address to,
        address asset,
        uint256 amount
    ) internal {
        // Transfer collateral to liquidator without proper validation
        IERC20(asset).transferFrom(from, to, amount);
    }
}
```

**Secure Implementation:**
```solidity
// SECURE: Proper parameter validation
function liquidate(
    address user,
    address collateralAsset,
    address debtAsset,
    uint256 amount
) external {
    // Validate liquidation eligibility
    require(isLiquidatable(user), "User not liquidatable");
    
    // Validate assets
    require(isValidAsset(collateralAsset), "Invalid collateral asset");
    require(isValidAsset(debtAsset), "Invalid debt asset");
    
    // Validate liquidation amount
    uint256 maxLiquidatable = getMaxLiquidatableAmount(user, debtAsset);
    require(amount <= maxLiquidatable, "Amount exceeds liquidatable debt");
    
    // Validate liquidator has sufficient funds
    require(IERC20(debtAsset).balanceOf(msg.sender) >= amount, "Insufficient funds");
    
    // Execute liquidation with proper checks
    _executeLiquidation(user, msg.sender, collateralAsset, debtAsset, amount);
}
```

---

## 🔄 The Pattern of Preventable Losses

### Recurring Vulnerability Classes

These are well-known and well-documented issues that continue causing millions in damages:

**1. Rounding Errors in Empty Markets**
- **2023 Hundred Finance:** Initial major incident  
- **2024-2025:** Multiple protocols affected
- **Total Losses:** $51M+ and counting
- **Prevention:** Minimum liquidity requirements, proper precision handling

**2. Insufficient Access Control**
- **Impact:** $2M printMoney bot exploit
- **Pattern:** Missing `onlyOwner` or similar modifiers
- **Prevention:** Comprehensive access control reviews

**3. Parameter Validation Failures** 
- **Impact:** $500K Silo Finance loss
- **Pattern:** Direct execution without input validation
- **Prevention:** Strict parameter validation frameworks

### Code Security Checklist

```solidity
// Essential security patterns
contract SecureProtocol {
    using SafeMath for uint256;
    
    // Access control
    modifier onlyAuthorised() {
        require(hasRole(AUTHORISED_ROLE, msg.sender), "Unauthorised");
        _;
    }
    
    // Parameter validation
    modifier validParameters(address asset, uint256 amount) {
        require(asset != address(0), "Invalid asset");
        require(amount > 0 && amount <= MAX_AMOUNT, "Invalid amount");
        require(isWhitelistedAsset(asset), "Asset not whitelisted");
        _;
    }
    
    // Rounding protection
    function mintTokens(uint256 amount) 
        external 
        onlyAuthorised 
        validParameters(underlyingAsset, amount) 
        returns (uint256) 
    {
        // Ensure minimum liquidity exists
        require(totalSupply() >= MINIMUM_LIQUIDITY, "Insufficient liquidity");
        
        // Use precise math to prevent rounding errors
        uint256 exchangeRate = getExchangeRateStored();
        uint256 mintAmount = amount.mul(1e18).div(exchangeRate);
        
        // Additional safety check
        require(mintAmount > 0, "Mint amount too small");
        require(mintAmount <= MAX_MINT_PER_TX, "Mint amount too large");
        
        _mint(msg.sender, mintAmount);
        return mintAmount;
    }
}
```
