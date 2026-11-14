# h4 Feed me

## x) Why would you want to use a feed reader (and RSS/Atom)? 

A feed is a machine-readable list of a website's latest posts in a standard XML format, RSS (RSS Advisory Board 2009) or Atom (Nottingham & Sayre 2005). A feed reader collects the feeds I subscribe to and shows new posts in one view.

I could visit each site myself, but with many sources that takes time, and most visits would show nothing new. With a reader, new posts come to me:

- All sources are in one list, newest first, and I see what is unread.
- No algorithm decides what I see, unlike on social media.
- Sites that post rarely are not forgotten.
- No account is needed, so the site does not get my email address.

The downsides: many feeds contain only a summary, not every site has a feed, and I have to filter the content myself.

## y) Supreme court of Finland, case Osuuspankkikeskus port scan

The Supreme Court of Finland ruled on 8 April 2003 that port scanning another organisation's network without permission can be a crime (KKO:2003:36).

- A 17-year-old ran a port scan against the network of Osuuspankkikeskus, a Finnish bank group. The scan was stopped by the firewall, so he never got in.
- The District Court convicted him only of interference with communications. The Court of Appeal and the Supreme Court found him guilty of an attempted computer break-in (tietomurron yritys).
- The scan was a suitable first step for breaking in and the law does not require any special motive. Saying "I was only looking" did not help.
- He was fined and had to pay damages to the bank. The Supreme Court did not reduce the damages, even though he was a minor.

Key lesson: a failed scan is still an attempt. I only scan my own machines, and in this homework only localhost with the network disconnected.

## a) Install a feed reader.

I chose Thunderbird, because it is easy to install and it was recommended in the assignment tips (Karvinen 2026). I installed it in my Debian 13 virtual machine.

First I updated the package lists and installed Thunderbird:

```
sudo apt-get update; sudo apt-get -y install thunderbird
```

<img width="817" height="135" alt="image" src="https://github.com/user-attachments/assets/aacd5b52-e4e9-41be-be11-f39d9a535ce0" />

To read feeds, Thunderbird needs a feed account. I pressed Alt to show the menu bar and chose File > New > Feed Account.

<img width="1057" height="796" alt="image" src="https://github.com/user-attachments/assets/c36e8d41-66c9-4615-8c3b-0b18a573ad8e" />

## b) Krebs on Security

I right-clicked "Blogs & News Feeds" in the folder pane and chose Subscribe. I pasted the feed URL https://krebsonsecurity.com/feed/ and clicked Add, then Close.

<img width="1035" height="776" alt="image" src="https://github.com/user-attachments/assets/d6815374-9890-4bd6-8df4-405d592e1db5" />

<img width="1033" height="777" alt="image" src="https://github.com/user-attachments/assets/756cdc42-bdda-4788-b4d5-95f1e263dcae" />

## c) Add a feed for Schneier on Security.

The blog is at https://www.schneier.com. First I searched the page with Ctrl+F for the words "RSS", "Atom" and "Feed", but I did not find a feed link this way.

Then I opened the page source with Ctrl+U and searched for "alternate". This found two link elements:

<img width="1027" height="186" alt="image" src="https://github.com/user-attachments/assets/35840b31-8e22-4c78-a59e-6419eae56201" />

I added the feed to Thunderbird the same way as the Krebs feed.

<img width="1032" height="772" alt="image" src="https://github.com/user-attachments/assets/62d317ec-dba1-472b-81d3-44fe64e3e807" />

## d) Find and add two more security related feeds.

I added the National Cyber Security Centre Finland (NCSC-FI) for an official Finnish view, and Troy Hunt's blog because he focuses on data breaches. I found the NCSC-FI feed on the site's separate RSS page and Troy Hunt's feed link at the bottom of his front page.

<img width="240" height="227" alt="image" src="https://github.com/user-attachments/assets/ed1daac0-39ec-4a07-b2a0-2d3bd307e32b" />

## e) Follow security feeds for a while.



### References

- RSS Advisory Board 2009: RSS 2.0 Specification. https://www.rssboard.org/rss-specification
- Nottingham, M. & Sayre, R. 2005: RFC 4287, The Atom Syndication Format. https://www.rfc-editor.org/rfc/rfc4287
- Supreme Court of Finland 2003: KKO:2003:36. [https://www.finlex.fi/en/case-law/supreme-court/precedents/2003/36](https://www.finlex.fi/fi/oikeuskaytanto/korkein-oikeus/ennakkopaatokset/2003/36#OT2_OT1)
