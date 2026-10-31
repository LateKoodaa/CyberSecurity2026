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
* Scams: CEO fraud attempts were seen almost daily. Criminals also use stolen online banking credentials to take loans in the victim's name.
* Top five threats for the near future: faster exploitation of vulnerabilities, AI risk management, supply chain security, critical infrastructure protection, and ransomware.
* Forecast for October is "worrying": the Citrix situation is not over, and scams and website breaches are expected to continue.
* My idea: 





## c) Science that

