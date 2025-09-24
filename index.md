# Web3 Security Vulnerabilities Blog

Welcome to my lab notebook and blog archive for Web3 security.  
Here you’ll find detailed breakdowns of smart contract exploits, vulnerability research, and secure coding practices — all documented as part of my ongoing journey from finance into smart contract auditing.

---

## 🔍 What You'll Find Here

- **Exploit Analysis** — Deep dives into real-world Web3 attacks and how they happened  
- **Vulnerability Research** — Technical analysis of common smart contract weaknesses  
- **Secure Coding Practices** — Patterns and techniques for building robust, auditable contracts  
- **Audit Insights** — Lessons learned from security reviews and personal audit exercises  
- **Personal Growth Notes** — Reflections on my progress as a Web3 developer and auditor  

---

## 📚 Latest Posts

Below are my most recent write-ups, published weekly in the `/_posts/` folder:

{% if site.posts.size > 0 %}
{% for post in site.posts %}
### [{{ post.title }}]({{ post.url | relative_url }})
*{{ post.date | date: "%B %-d, %Y" }}*

{{ post.excerpt | strip_html | truncate: 200 }}

---
{% endfor %}
{% else %}
### Coming Soon!

I'm currently working on detailed analysis of recent Web3 exploits and vulnerabilities. Check back soon for in-depth technical content covering:

- Reentrancy attacks and prevention
- Flash loan exploits 
- Bridge vulnerabilities
- MEV and front-running issues
- Access control failures

{% endif %}
