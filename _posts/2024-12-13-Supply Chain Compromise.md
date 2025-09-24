---
title: "Supply Chain Compromise: Malicious XML-RPC npm Package"
date: 2024-12-13
categories: [Reported Exploits]
---

## Overview
In November 2024, the npm package `@0xengine/xmlrpc` was weaponised into a malicious distribution channel. Initially released as a benign XML-RPC server/client utility for Node.js (Oct 2, 2023), the package was later updated with hidden payloads that enabled data theft, privilege persistence, and cryptocurrency mining.

This is a textbook example of a supply chain compromise: attackers injected malicious code into a trusted dependency, which then propagated across unsuspecting projects.

---

## Timeline
- **October 2, 2023** → `xmlrpc` package published to npm.  
- **Shortly after (v1.3.4)** → Malicious code introduced.  
- **November 2024** → Security researchers and npm discovered abnormal behavior and confirmed compromise.  

---

## Attack Mechanics

### 1. Data Exfiltration
The malicious version exfiltrated sensitive system data every **12 hours**:
- SSH keys  
- Bash history  
- System metadata (OS, hardware)  
- Environment variables (API keys, secrets)  

Data was exfiltrated via external services like **Dropbox** and **file.io**, making detection harder since traffic blended with legitimate HTTPS.

---

### 2. Cryptojacking Payload
Attackers deployed a Monero (XMR) miner via the XMRig binary:
- Installed persistence using systemd (Linux).  
- Consumed CPU resources for attacker profit.  
- Detected ~68 infected systems actively mining Monero.  

**Evasion techniques**:  
- Monitored system processes (`top`, `ps`, `vmstat`, etc.).  
- Suspended mining when monitoring tools were detected.  
- Stayed inactive during certain user activity to avoid raising suspicion.

---

### 3. Propagation Path
Two infection vectors were confirmed:
1. Direct npm install of the malicious `xmlrpc` version.  
2. Indirect install via dependency poisoning — e.g. a project called “yawpp” (Yet Another WordPress Poster) declared `@0xengine/xmlrpc` as a dependency, pulling in the backdoored code automatically.

This made the compromise particularly dangerous: even if developers didn’t install `xmlrpc` directly, they could still be exposed.

---

## Technical Assessment
- **Supply chain compromise**: injected via a popular package ecosystem.  
- **Dual payload**: data exfiltration + crypto mining = attacker monetisation + intelligence gathering.  
- **Persistence**: Linux `systemd` ensured mining restarted after reboots.  
- **Evasion**: mining suspended during monitoring → harder detection by sysadmins or developers.  

---

## Key Takeaways
- Even small, seemingly harmless libraries can escalate into critical supply chain risks.  
- Dependency trust ≠ safety: always inspect dependency trees.  
- Lock versions and audit frequently (`npm audit`, Snyk, OSV-Scanner).  
- Watch out for suspicious resource usage spikes (CPU, network traffic).  
- Treat every new version update as a potential attack vector — malicious code is often introduced post-adoption, once trust is established.

---

## Source
- [The Hacker News — *“XML-RPC npm Library Turns Malicious, Steals Data, Deploys Crypto Miner”*](https://thehackernews.com/2024/11/xmlrpc-npm-library-turns-malicious.html?utm_source=substack&utm_medium=email)

---
