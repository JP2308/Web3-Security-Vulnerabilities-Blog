---
title: "New Year, New Exploits"
date: 2025-01-09
categories: [DeFi, Sora, Web3 Security, Exploits, Analysis]
---


## Fx Protocol – $125K wstETH Reward Miscalculation

**Incident Overview:**  
Fx Protocol miscalculated rewards for the ever-increasing wstETH asset, leading to a $125,000 loss. While not catastrophic, this highlights how seemingly minor reward misconfigurations can become exploitable if left unchecked.

**Analysis:**  
- Reward calculation logic did not properly account for continuously compounding wstETH balances.  
- Attackers or opportunistic users could exploit the miscalculation to claim disproportionate rewards.  
- Emphasises the importance of rigorous testing for all token reward logic, especially for dynamic assets.

---

## PumpTokenFactory – Multi-Token Price Oracle Exploit

**Incident Overview:**  
PumpTokenFactory deployed flawed token template code, which triggered a series of price oracle exploits impacting tokens like Laura and Luke. This bears similarity to the GemPad reentrancy attacks, which previously led to $2M stolen.

**Analysis:**  
- Vulnerability stems from shared factory templates, making multiple tokens susceptible simultaneously.  
- Attackers leveraged oracle manipulation to drain token value or extract funds.  
- Highlights systemic risk in token factories: a single flaw can cascade across many deployments.

**Takeaways:**  
- Audit template contracts before deployment.  
- Ensure oracle inputs are tamper-resistant and monitored for unusual price movements.  
- Introduce per-token safety checks instead of relying solely on factory-level logic.

---

## Tangem / StakeOM / BNPL – Miscellaneous Early-2025 Incidents

**Incident Overview:**  
Several smaller incidents occurred across Tangem, StakeOM, and BNPL, each resulting in losses under $300K. While individually minor, they illustrate persistent misconfigurations and smart contract risks at the start of 2025.

**Analysis:**  
- Tangem’s wallet integration experienced minor mismanagement of transaction logic.  
- StakeOM revealed staking reward miscalculations similar to Fx Protocol’s issue.  
- BNPL incidents involved operational missteps during token bridging or reward issuance.

**Takeaways:**  
- Even small misconfigurations can accumulate to significant risk.  
- Continuous monitoring and early audits help prevent cascading losses.

---

### 🔑 Overall Lessons for Web3 Security
My takeaways are as follows:
1. **Reward Logic** – Dynamic assets need **stress-tested reward calculations**.  
2. **Continuous Monitoring** – Small early-year incidents can reveal systemic weaknesses before they scale.  
3. **Oracle Security** – Price manipulation remains a primary attack vector; decentralised, reliable oracles are essential.

---

### 📚 Sources

- [BlockThreat – Week 1, 2025](https://newsletter.blockthreat.io/p/blockthreat-week-1-2025?utm_source=substack&utm_medium=email)
