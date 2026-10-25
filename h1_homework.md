# h1 Cyber Science homework

## a) Science hunt

To find a suitable review article, I used several different searches and followed the guidelines at https://terokarvinen.com/review-article/. The searches I used in Google Scholar were:

- `intitle:review "zero trust"`
- `intitle:review "cloud security"`
- `intitle:review "threat modeling"`
- `intitle:review "Multi-Factor Authentication Protocols"`

The final search was the last one on the list, and with it I found the article:

Ang, K. W., Chekole, E. G. & Zhou, J. 2025. Unveiling the Covert Vulnerabilities in Multi-Factor Authentication Protocols: A Systematic Review and Security Analysis. ACM Computing Surveys, 57(11), 1–37.

This article interested me because I was already somewhat familiar with MFA and wanted to learn more about it. ACM Computing Surveys has a JUFO rating of 3. I verified this from the JUFO portal https://jfp.csc.fi/jufoportal on 25 October 2026.

## b) Review the review

The article is a systematic review of multi-factor authentication (MFA) protocols. In addition to the review, the authors also perform their own security analysis.

### What are the key areas being researched?

- Classification of authentication factors: conventional factors (knowledge, possession, inherence, location) and emerging factors especially suited for machine-to-machine authentication (historical data, PUF, firmware integrity)
- Eight evaluation criteria for the security of MFA protocols, for example mutual authentication, independence of factors and realistic adversary assumptions
- Vulnerability analysis and performance comparison of ten protocols
- Mitigation strategies for the weaknesses found

### What's widely agreed upon?

- A password or any other single factor alone is no longer sufficient
- MFA is more secure than single-factor authentication, but only if the protocol is designed correctly
- The methods for fixing the weaknesses are already known

### What are the open questions?

- How reliable are formal security proofs?
- What kind of adversary should be assumed? Almost all of the analyzed protocols assumed a weak adversary, which according to the authors does not reflect reality
- How will quantum computing affect MFA protocols?

## c) Threat modeling quickrun

**Company:** a local gym, 500 members, 60 €/month, revenue 360 000 €/year.

**Crown jewels:** customer register, reputation, money and business continuity.

**Bad guys (COI):**

- **Capability:** an opportunistic criminal group using ready-made tools.
- **Opportunity:** security has not been considered, and logging in is simple (no MFA, shared accounts).
- **Intent:** money. The gym is under pressure to get operations running again quickly, and the customer register can be used for extortion.

**Risks:**

| Risk | Likelihood | Impact | Estimated value |
|---|---|---|---|
| 1. Admin account takeover and locking of access control | 15 %/year | 65 000 € | 9 750 €/year |
| 2. Theft of member data and extortion by threatening publication | 10 %/year | 52 000 € | 5 200 €/year |

**Mitigate:**

- MFA for admin accounts. In many cloud services it is included in the price, so the cost comes mainly from deployment.
- Short phishing awareness training for staff.
- Personal accounts instead of a shared account, so that former employees' access can be removed.

Estimated cost 500–2 000 €/year. The likelihood could drop, for example, from 15 % to around 3–5 %.

## References

Ang, K. W., Chekole, E. G. & Zhou, J. 2025. Unveiling the Covert Vulnerabilities in Multi-Factor Authentication Protocols: A Systematic Review and Security Analysis. ACM Computing Surveys, 57(11), 1–37. https://doi.org/10.1145/3734864
