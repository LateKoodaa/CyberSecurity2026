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

Thunderbird does not have a combined view of all feeds by default, so I created one as a saved search (Karvinen 2026). I right-clicked "Blogs & News Feeds", chose Search Messages and set the rule "Age In Days is less than 30". Then I saved the search as a folder named "month".

<img width="751" height="482" alt="image" src="https://github.com/user-attachments/assets/b046f911-a94e-4a9c-bcb7-91800c195ae0" />

<img width="1015" height="730" alt="image" src="https://github.com/user-attachments/assets/cea2b5ef-88e1-4eb2-b817-fe1f6ddbd288" />

### What I learned from following feeds

Unfortunately I could only follow the feeds for about one day. However, the "month" view shows the articles from the previous weeks, so I could still see how often each feed updates.

The most interesting article was "New OpenSSH Vulnerability" on Schneier on Security (10 November 2026). The same vulnerability was discussed in a student presentation in our last class.

Benefits: It is easier to find information about current topics, and I do not need to move from site to site to check what is new.

Downsides: The benefit depends on choosing the feeds well, so it is worth limiting them to sources that really interest me. Compared to social media, feeds have no discussion and do not help me discover new sources.

## f) Port scan

I installed nmap and Apache while the network was still connected and stopped Apache before the first scan:

```
sudo apt-get update
sudo apt-get -y install nmap apache2
sudo systemctl stop apache2
```

Then I disconnected the network adapter from the VirtualBox menu (Devices > Network) and also disconnected my OS PC network capel. Ping showed that the internet was not reachable:

<img width="341" height="207" alt="image" src="https://github.com/user-attachments/assets/b2624740-5220-4cf8-a36d-419e18159665" />

<img width="402" height="53" alt="image" src="https://github.com/user-attachments/assets/91310f20-1250-48ae-8c2c-a8805d599256" />

### First scan

```
sudo nmap -A -v localhost
```

```
Not shown: 999 closed tcp ports (reset)
PORT    STATE SERVICE VERSION
631/tcp open  ipp     CUPS 2.4
|_http-server-header: CUPS/2.4 IPP/2.1
|_http-title: Home - CUPS 2.4.10
```

Nmap scanned 1000 TCP ports. 999 were closed and one was open: port 631, the CUPS printing service.

### Installing a daemon and second scan

```
sudo systemctl start apache2
sudo nmap -A -v localhost
```

```
Not shown: 998 closed tcp ports (reset)
PORT    STATE SERVICE VERSION
80/tcp  open  http    Apache httpd 2.4.71 ((Debian))
|_http-server-header: Apache/2.4.71 (Debian)
|_http-title: Apache2 Debian Default Page: It works
631/tcp open  ipp     CUPS 2.4
```

### How can criminals know what services your employer has?

Port 80 appeared as soon as Apache was running. Nmap found the software name, the exact version (2.4.71), the operating system (Debian) and the fact that the server still has its default page. Criminals can learn what services my employer runs by scanning its public IP addresses. With the version number they can search for known vulnerabilities in exactly that version. I also learned that a machine can run services I did not know about, like CUPS here.



### References

- RSS Advisory Board 2009: RSS 2.0 Specification. https://www.rssboard.org/rss-specification
- Nottingham, M. & Sayre, R. 2005: RFC 4287, The Atom Syndication Format. https://www.rfc-editor.org/rfc/rfc4287
- Supreme Court of Finland 2003: KKO:2003:36. [https://www.finlex.fi/en/case-law/supreme-court/precedents/2003/36](https://www.finlex.fi/fi/oikeuskaytanto/korkein-oikeus/ennakkopaatokset/2003/36#OT2_OT1)
- Schneier, B. 2026: New OpenSSH Vulnerability. Schneier on Security. https://www.schneier.com/blog/archives/2026/11/new-openssh-vulnerability.html

**AI assistance disclosure:** Claude assisted with organizing and editing this report. Commands, settings, and verification results are based on my own work.
