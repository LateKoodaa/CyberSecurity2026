# h2 Follow the Scene

## a) Podcast

**Podcast:** Herrasmieshakkerit
**Episode:** 0x39 "Työkaluvelho", guest Joona Hoikkala
**Published:** 2025-05-16
**Link:** [Spotify](https://open.spotify.com/episode/6GuqlHefGDGoYz7jDkG5jA)

### Summary

* Bit errors in network traffic and memory, known as bitsquatting
* A single random bit flip can change a domain name (for example in a DNS query), so the traffic goes to a different domain than intended
* If an attacker has registered that "one bit off" domain, they receive traffic that was never meant for them
* Joona tested this in practice by registering bit-flipped versions of popular domains
* Results: the bit-flipped domains did receive real traffic. The amount depended on the size of the original domain: variants of big, popular domains got a lot of traffic, while variants of smaller domains got much less.
* Defence: large domain owners such as Amazon have registered bit-flipped versions of their own domains, so attackers cannot use them. This is called defensive registration.
* My idea: a single bit flip is very rare on one device, but popular domains get billions of requests. At that scale even a rare error happens often, which is why big domains are the best targets.

## b) Blog

**Blog:** Traficom NCSC-FI News
**Article:** Cyber Weather September 2026
**Published:** 2026-10-28
**Link:** [Cyber Weather September 2026](https://www.kyberturvallisuuskeskus.fi/en/news/cyber-weather-september-2026)

### Summary

* Cyber Weather is NCSC-FI's monthly overview of the Finnish cyber security situation. In September the main themes were scams, website breaches and critical vulnerabilities in network edge devices.
* Citrix NetScaler: NCSC-FI issued its first severe alert of the year. The vulnerabilities in Citrix NetScaler ADC and Gateway were exploited in several Finnish data breaches. (Patching is not enough)
* Data breach reports rose 81.5 % from August. Ransomware mostly hit small businesses through vulnerabilities or VPN accounts without MFA.
* Scams: CEO fraud attempts were seen almost daily. Criminals also use stolen online banking credentials to take loans in the victim's name.
* Top five threats for the near future: faster exploitation of vulnerabilities, AI risk management, supply chain security, critical infrastructure protection, and ransomware.
* Forecast for October is "worrying": the Citrix situation is not over, and scams and website breaches are expected to continue.
* My idea: "Patching is not enough" if attackers exploit a vulnerability before the update, patching only closes the door, so organisations must also check whether someone is already inside.

## c) Science that

**Theory / article from h1:** Ang, K. W., Chekole, E. G. & Zhou, J. 2025. Unveiling the Covert Vulnerabilities in Multi-Factor Authentication Protocols: A Systematic Review and Security Analysis. ACM Computing Surveys, 57(11), 1–37. https://doi.org/10.1145/3734864
**Connected to:** b) Cyber Weather September 2026 (NCSC-FI)

### The article in brief

* A single factor such as a password is no longer sufficient, and MFA is more secure than single-factor authentication.
* MFA only helps if the protocol is designed correctly. The authors found that almost all analysed protocols assumed a weak adversary, which does not reflect reality.

### How the current event fits

* Cyber Weather September 2026 reports that ransomware attacks mainly targeted small businesses and attackers got in through vulnerabilities or VPN accounts without MFA. This is exactly the situation the review warns about, "one stolen password is enough to get in".

### My own idea

* Small businesses seem to fail at the basics before advanced attacks are even needed. Would it be more effective to make MFA the default in VPN and cloud services, instead of expecting each small business to turn it on?

## d) Download Debian

I downloaded the Debian 13 Trixie Live Xfce image (debian-live-13.7.0-amd64-xfce.iso) from debian.org

<img width="732" height="62" alt="image" src="https://github.com/user-attachments/assets/c7753000-4c46-4367-81fd-912438d5ed60" />


## References

Ang, K. W., Chekole, E. G. & Zhou, J. 2025. Unveiling the Covert Vulnerabilities in Multi-Factor Authentication Protocols: A Systematic Review and Security Analysis. ACM Computing Surveys, 57(11), 1–37. https://doi.org/10.1145/3734864

Herrasmieshakkerit 2025. Työkaluvelho, vieraana Joona Hoikkala | 0x39. Podcast episode, 16 May 2025. https://open.spotify.com/episode/6GuqlHefGDGoYz7jDkG5jA

NCSC-FI 2026. Cyber Weather September 2026. Traficom, 28 October 2026. https://www.kyberturvallisuuskeskus.fi/en/news/cyber-weather-september-2026

NCSC-FI 2026. Cyber Weather, September 2026 (PDF report). https://www.kyberturvallisuuskeskus.fi/files/media/file/EN_Cyber%20weather%2C%20Semptember%202026.pdf
