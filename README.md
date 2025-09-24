# Web3 Security Vulnerabilities Blog

A **lab notebook and blog archive** exploring vulnerabilities, exploits, and security-focused development in Web3.  

This repo continues my journey from **finance professional to smart contract auditor**, first shared in my [OpenZeppelin blog post](https://forum.openzeppelin.com/t/my-coding-journey-from-finance-to-smart-contract-auditor/39251).


---

## 🔍 Purpose

This repository is both a **blog archive** and a **technical lab notebook**.  

I use it to:
- Break down **real-world exploits** and trace their root causes  
- Document **audit findings** and lessons learned  
- Workshop my Tech Stack (see below) to mirror attack vectors or defenses  
- Map vulnerabilities to **secure coding practices** for developers and auditors  
- **Document my learning journey and hold myself accountable as I continue to grow alongside Web3 best practices**
---

## 📚 Structure

When breaking down an incident I follow a consistent format (carried over from my blog series):

1. **Incident Summary** — Key details of the exploit (losses, vectors, impact); 
2. **Root Cause Analysis** — The technical flaw (logic bug, reentrancy, compiler issue, etc.);  
3. **Audit/Mitigation Angle** — How this could have been caught, or patched post-mortem; and  
4. **Code Experiment** — Coding snippets from my Tech Stack that aim to simulate the vulnerability or demonstrate a safer pattern.  

This structure ensures readers see both the *attacker’s perspective* and the *auditor’s defense*.  

I also post more free-form reflections on Web3 security topics, capturing my personal growth and thought process as I develop as an auditor.

---

## 🛠️ Tech Stack

My current Tech Stack reflects the tools I use to analyse, build, and break Web3 systems with a focus on security, auditing, and resilient development practices:

- Python — for fuzzing, simulations, and exploit modelling
- Vyper — for minimal, auditable on-chain contracts
- Solidity — for audit-focused interoperability and DeFi relevance
- Foundry — for contract deployment, fuzzing, and test automation
- Rust — for systems programming, smart contract security research, and Solana/Substrate exploration

---

## 🚀 Why This Matters

I began learning **Python** a little over two years ago, building on my foundation in finance, economics, and law to craft simulations, financial models, and early auditing prototypes. As my interest in smart contract security grew, I took on **Foundry** alongside Solidity — Foundry for rigorous testing and deployment workflows, Solidity for broad Web3/DeFi relevance — and more recently expanded into **Vyper** to experiment with minimal, auditable contracts. My most recent foray into **Rust** has opened up new possibilities in systems programming and security tooling, especially for platforms like Solana or in writing low-level analysis modules.

This repository captures the intersection of financial logic, coding practice, and security research, serving both as a lab notebook and a record of my growth in Web3 development and audit work:

- **Python** → rapid iteration, exploit simulations, fuzzing harnesses, and financial modelling  
- **Foundry** → contract test suites, deployment pipelines, and automated checks for security best practices  
- **Vyper** → secure, minimal contracts designed for clarity, simplicity, and ease of audit  
- **Solidity** → realistic, ecosystem-wide challenges, interoperability, and DeFi relevance  
- **Rust** → systems-level tooling, protocol research, and writing performance-aware components where safety is critical  

Together, these tools create a workflow where **financial models, exploits, and mitigations** live side-by-side — enabling me to test, reflect, and apply learning both in structured audit environments and in open-ended security research.

---

⚡ *This is a living project. Each new vulnerability breakdown or coding experiment will be added here as both a technical record and an educational resource.*
