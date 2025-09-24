---
title: "August's Double Feature: Nomad Hackers Finally Cash Out and PyPI Gets Weaponised"
date: 2024-08-27
categories: [DeFi, Cybersecurity, Malware, Blockchain]
---

August delivered two incidents that perfectly capture the current state of crypto security: old bridge exploiters finally laundering their 2022 haul, and attackers turning Python package management into a wallet-draining operation. Both stories highlight how patient attackers are getting and why trusting community-driven platforms might be naive.

The Nomad Bridge story is particularly interesting because it shows how long sophisticated attackers are willing to wait before cashing out. Two years after stealing $200 million, they're still methodically moving funds through Tornado Cash. Meanwhile, the PyPI incident demonstrates that even developer tools aren't safe from crypto-focused malware.

## 💸 Nomad Bridge: The Long Game Payoff

Two years after the Nomad Bridge exploit drained $200 million, the attackers are finally cashing in their chips. In August, a wallet linked to the original hack moved 14,500 ETH ($35.5 million) through Tornado Cash — the sanctioned mixing service that's become the de facto standard for laundering crypto proceeds.

What strikes me about this timeline (similar to last week's post) is the patience involved. Most crypto exploiters rush to cash out immediately, but these attackers held $35.5 million for two years before moving it. This suggests either exceptional operational security discipline or fear of immediate law enforcement attention.

**The Technical Reality:**
The original Nomad exploit was brutally simple — attackers figured out they could copy successful withdrawal transactions and replay them with their own addresses. What started as a sophisticated attack turned into a crowd-sourced bank run, with hundreds of copycats draining the bridge.

The current laundering operation shows similar methodical thinking. Converting DAI to ETH before mixing suggests they understand that stablecoins are easier to trace and freeze than native assets.

## 🐍 PyPI Weaponisation: Supply Chain Meets Social Engineering

The Python package incident shows how attackers are evolving beyond direct phishing into supply chain manipulation. Someone created a package called "spl-types" on PyPI, promoted it through legitimate StackExchange posts, and waited for developers to install it before pushing malicious updates.

**The Attack Timeline:**
1. **June 2024**: Upload benign "spl-types" package to PyPI
2. **Social Engineering**: Create StackExchange posts recommending the package
3. **Trust Building**: Let developers install and use the harmless version
4. **Malicious Update**: Push wallet-draining code in later version
5. **Mass Compromise**: Automated updates install malware across user systems

```python
# What the malicious package likely did
import subprocess
import requests
import os

def harvest_crypto_data():
    # Scan for wallet files
    wallet_paths = [
        "~/.ethereum/keystore/",
        "~/AppData/Roaming/Exodus/",
        "~/.electrum/wallets/"
    ]
    
    # Exfiltrate sensitive data
    for path in wallet_paths:
        if os.path.exists(path):
            send_to_attacker_server(path)
    
    # Establish backdoor
    download_and_execute_payload()
```

### Why This Actually Worked:
The attackers exploited several assumptions that most developers make:
- PyPI packages from established accounts are trustworthy
- StackExchange recommendations represent community consensus
- Package updates from the same maintainer are safe to install automatically

Unfortunately, this attack highlights a fundamental problem with community-driven package ecosystems. There's no meaningful barrier between "random person uploads code" and "thousands of developers install it automatically." Or better yet, review the code if provided by 3rd parties. 


## The Nexus between the two

Ultiamtely it comes down to the  Uncomfortable Truth that neither incident required novel exploits or zero-day vulnerabilities. The Nomad bridge hack was a logic error, the PyPI attack was basic supply chain compromise. We're not losing to sophisticated attacks — we're losing to basic operational failures. These incidents highlight why both individual users and the broader ecosystem need to assume that popular doesn't mean safe, established doesn't mean secure, and community-recommended doesn't mean trustworthy. The attackers certainly aren't making those assumptions.