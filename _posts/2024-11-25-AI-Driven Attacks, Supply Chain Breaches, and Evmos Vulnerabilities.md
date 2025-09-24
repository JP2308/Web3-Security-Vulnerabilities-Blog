---
title: "AI-Driven Attacks, Supply Chain Breaches, and Evmos Vulnerabilities"
date: 2024-11-25
categories: [Security, Exploit Analysis, Smart Contracts]
---

# AI-Driven Attacks, Supply Chain Breaches, and Evmos Vulnerabilities

This week, over $60M was siphoned from the Web3 ecosystem through sophisticated exploits, including AI-driven social engineering, supply chain compromises, and protocol-level vulnerabilities. Below is an in-depth technical analysis of the most significant incidents.

---

## 1. Supply Chain Attack: "Shai-Hulud" Infects 187 npm Packages

This one feels especially scary because it shows just how fragile our dependency chains really are.  

- **Vector:** Compromised npm developer account via phishing  
- **Impact:** Over 2 billion weekly downloads of infected packages  
- **Technical Details:**  
  - Attackers used a 2FA reset trick to compromise an npm maintainer.  
  - Injected malware replaced crypto wallet addresses with attacker-controlled ones.  
  - GitHub Actions were abused to quietly exfiltrate secrets.  

What struck me here is how low-tech the initial breach was. We spend so much time thinking about zk-proofs and advanced auditing, but in the end, one phishing email gave attackers access to a package with billions of downloads. It reinforces the idea that Web3 is only as strong as Web2’s weakest link. I think the only real solution to this is MFA enforcement for all developers, and rigorous audits of package maintainers via scripts and manual review (I am conscious that this will be a big task!).  


---

## 2. AI-Powered Voice Cloning Used in Crypto Scams

This is one of those stories where you feel like the “future” just arrived.  

- **Mechanism:** AI-generated voice deepfakes used in scams  
- **Impact:** Losses in the **millions of dollars**  
- **Technical Highlights:**  
  - Attackers cloned voices of trusted individuals.  
  - Used in “pig-butchering” schemes to build trust and drain wallets.  

To me, this is where AI blends with social engineering in a really dangerous way. I’ve always felt that multi-sig wallets were a pain to manage, but reading about these scams, they suddenly feel like table stakes. You simply can’t trust your ears anymore. The scary part? Voice cloning is cheap, accessible, and convincing enough to fool even savvy users. I suppose the only real solution here is to educate users on deepfake risks but I'm not honestly sure what this would look like for the average person. 


---

## 3. Evmos Protocol Vulnerability: $150K Exploit via Module Account Misuse

- **Platform:** Evmos blockchain (built on Cosmos SDK)
- **Vector:** Sending funds to module accounts bypassed ante handler checks
- **Technical Steps:**
  1. `x/bank` module's ante handler did not validate Ethereum transactions properly.
  2. Attackers exploited this by sending transactions targeting precompiles used in Cosmos SDK modules.
  3. Resulted in the ability to halt the blockchain by sending minimal token transfers.
   
For me, the key lesson was that simple documentation-based bugs can have critical financial impact and here's a crazy thing I learnt whilst being a lawyer, you just need to read the docs...Doing so can reveal low-hanging yet high-value vulnerabilities. I think another solution can also be sure to implement automatic invariant checks on fund transfers. What I mean by that is runtime-enforced rules that validate every fund transfer before execution—ensuring things like: account balances never go negative, module accounts can’t be misused, and protocol-specific constraints are always upheld. This removes the risk of developers overlooking manual checks.

---

## Conclusion 
If there’s one lesson I’m drawing from all of this, it’s that attackers are increasingly mixing layers. AI for social engineering, Web2 vectors for supply chain compromises, and Web3 logic flaws for protocol exploits — all in the same week.  

For security researchers like me, it reinforces the need to keep zooming out. Audits and fuzzing are great, but we also need to be testing the human layer, the supply chain layer, and the operational layer. Web3 doesn’t exist in a vacuum, and neither do its risks.  

The bottom line: it’s not enough to harden contracts — we have to harden the entire ecosystem around them. 


---

## 📚 References

- [TechRadar: Shai-Hulud Malware Campaign](https://www.techradar.com/pro/security/self-replicating-shai-hulud-infects-147-npm-packages-with-over-2-million-downloads-per-week)
- [Reuters: ChatGPT Used in Asia Fraud Scheme](https://www.reuters.com/investigations/chatgpt-was-used-help-scammers-do-their-thing-asia-fraud-scheme-2025-09-15/)
- [Medium: $150K Evmos Bug Bounty Case](https://medium.com/@jjordanjjordan/150-000-evmos-vulnerability-through-reading-documentation-d26328590a7a)