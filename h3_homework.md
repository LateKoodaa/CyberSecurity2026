# h3 Cyber lab

## x) Read / watch / listen and summarize

### Heinonen 2026: How to Install Linux to VirtualBox?
 
- The article explains how to install Debian Linux inside VirtualBox, giving you a "computer inside a computer" (a virtual machine) that you can use for safe testing.
- You create the VM by giving it a name, memory (RAM), disk space, and setting the type to Debian (64-bit). Unattended installation is left off, so you do the install yourself and can see each step.
- After installation the system is updated. The end result is a working Linux desktop for experimenting.
- **My question:** Why is my own virtual machine a safer place to try attacker tools than my real computer?

### Karvinen 2020: Command Line Basics Revisited
 
- The command line is a text-based way to control the computer by typing commands instead of using the mouse. It is fast and easy to automate.
- The most important commands deal with moving around and handling files: `pwd` (where am I), `ls` (list files), `cd` (change directory), `mkdir` (make a directory), `cp` (copy), `rm` (remove).
- Two tricks that make learning easier: the Tab key auto-completes commands, and the Up arrow repeats previous commands. Be careful with `rm`, because deleted files do not go to a recycle bin.
- **My question:** How much do I need to practice and use these commands before they start to feel natural?

### Karvinen 2022: Cracking Passwords with Hashcat
 
- Services do not store your password as-is. They store a "fingerprint" of it called a hash, and you cannot calculate the password back from the hash.
- Cracking works by guessing: the computer tries a huge wordlist (here RockYou, about 14 million words), hashes each word, and checks for a match. In the example the password turned out to be "summer".
- The lesson works both ways. Common dictionary words are cracked in seconds, so your own passwords should be long and unique. The tool is used only on your own hashes or on authorized practice targets.
- **My question:** How long or complex does a password need to be so that it is not found in a wordlist?

## a) Presentation title

I reserved the following presentation title in Laksu:

**Where does SAP keep its keys? Encryption and key management in SAP HANA**

## b) Linux. Install Linux on a virtual machine. Use Debian 13-Trixie.

<img width="1015" height="495" alt="image" src="https://github.com/user-attachments/assets/1074c207-7182-4c1c-a50d-7cd491297f56" />

<img width="1017" height="487" alt="image" src="https://github.com/user-attachments/assets/ed3db2a8-c948-44e2-aee6-01425720a963" />

<img width="822" height="331" alt="image" src="https://github.com/user-attachments/assets/4b9a92ce-07c8-4b14-9fcd-ac050d3a99b6" />

<img width="792" height="700" alt="image" src="https://github.com/user-attachments/assets/d0e8afda-e0cc-49f3-8801-7d38a6634031" />

<img width="1172" height="802" alt="image" src="https://github.com/user-attachments/assets/aa214a34-c525-4417-b4b2-e2d873032c23" />

<img width="1177" height="802" alt="image" src="https://github.com/user-attachments/assets/cb3ae5d8-5bab-4d04-aa10-7cbcd06f5406" />

<img width="1282" height="802" alt="image" src="https://github.com/user-attachments/assets/253847da-0327-4d4e-8a00-b644e457c59b" />

<img width="1095" height="552" alt="image" src="https://github.com/user-attachments/assets/4b1b9c89-00ca-486b-853d-d10d4ec06027" />

<img width="1077" height="362" alt="image" src="https://github.com/user-attachments/assets/afb794f4-e8b2-4732-abcf-af835660e98b" />

<img width="1075" height="147" alt="image" src="https://github.com/user-attachments/assets/80694a25-5e66-4e2a-b36d-5ff8e0b6a692" />

<img width="957" height="640" alt="image" src="https://github.com/user-attachments/assets/a75f496c-2da0-4752-9f99-0372892140df" />

<img width="700" height="190" alt="image" src="https://github.com/user-attachments/assets/590cfd22-d22c-4d40-8db2-127b0b020171" />

```bash
sudo apt-get update
sudo apt-get upgrade
```

<img width="1020" height="767" alt="image" src="https://github.com/user-attachments/assets/21b21621-e0e5-40e7-88d6-22d1cfd6afe8" />

<img width="966" height="381" alt="image" src="https://github.com/user-attachments/assets/551a1320-47c3-4c55-af6b-4ac3656e8714" />

<img width="981" height="592" alt="image" src="https://github.com/user-attachments/assets/1d91627c-8da8-4e4b-9182-e8ddf6a4c542" />

```bash
sudo reboot
```

<img width="860" height="572" alt="image" src="https://github.com/user-attachments/assets/ce26a2ae-629f-4e63-bd5c-4d74cfaf8d10" />

<img width="1025" height="772" alt="image" src="https://github.com/user-attachments/assets/358891c4-0cd4-4ce8-87ea-58e239e803bb" />

Removing the installation media

<img width="1030" height="770" alt="image" src="https://github.com/user-attachments/assets/fd095c0a-825f-4767-91c6-121404eaf96c" />

### Problems and fixes

During this task I ran into two real problems. I document them here because
they took the most work to solve.

#### Problem 1: the virtual machine hung on boot (graphics)

After installing Debian and rebooting, the VM hung on a black screen. The boot
messages showed a graphics driver error:

vmwgfx [drm] ERROR vmwgfx seems to be running on an unsupported hypervisor.


The default VMSVGA graphics controller loads the Linux vmwgfx driver, which
does not work well under VirtualBox and froze the graphical boot.

Fix: I powered the VM off and changed the display settings in VirtualBox
(Settings > Display):

- Graphics Controller: VBoxSVGA (instead of VMSVGA)
- Video Memory: 128 MB

After this the VM booted to the login screen normally.

#### Problem 2: no internet in the installed system (network)

After installation, apt could not reach the Debian servers:

Temporary failure resolving ‘deb.debian.org’


Checking the interface showed it only had IPv6 addresses and no IPv4 address,
and a ping to an IPv4 address failed:

```bash
ip a
ping -c3 1.1.1.1   # Network is unreachable
```

The VirtualBox NAT DHCP was not giving the guest an IPv4 address, and reboots
and full power cycles did not help. Setting a static IPv4 by hand worked and
gave full connectivity:

```bash
sudo ip addr add 10.0.2.15/24 dev enp0s3
sudo ip route add default via 10.0.2.2
```

Both the NAT gateway (10.0.2.2) and the internet (1.1.1.1) then answered,
which showed the problem was not the NAT itself but how the address was being
assigned.

To make this permanent I set the static address in NetworkManager. At first
NetworkManager reported the connection as activated but still did not apply the
IPv4 address. The root cause was Duplicate Address Detection (DAD):
NetworkManager checks whether the address is already in use, and VirtualBox
reserves 10.0.2.15 for its DHCP, so DAD treated it as a conflict and skipped
the address. Disabling DAD fixed it:

```bash
sudo nmcli con mod "Wired connection 1" ipv4.method manual ipv4.addresses 10.0.2.15/24 ipv4.gateway 10.0.2.2 ipv4.dns 10.0.2.3
sudo nmcli con mod "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli con up "Wired connection 1"
```

After this NetworkManager applied the IPv4 address, and it survived a reboot.
I confirmed it with:

```bash
ip a                     # enp0s3 now has inet 10.0.2.15
ping -c3 deb.debian.org  # name resolution and internet work
```

## c) Crack

The goal of this task is to crack the hash
`6b1628b016dff46e6fa35684be6acc96` using Hashcat, following Karvinen 2022.


```bash
sudo apt-get update
sudo apt-get -y install hashid hashcat wget
```

<img width="942" height="607" alt="image" src="https://github.com/user-attachments/assets/64298d1a-bf1f-4f6c-81ed-efddaf514188" />

<img width="417" height="92" alt="image" src="https://github.com/user-attachments/assets/7caa4ff2-455a-4bf8-857a-72eccdec7a2e" />

<img width="930" height="161" alt="image" src="https://github.com/user-attachments/assets/a4778e88-2096-4199-8d0a-175537a8c6b1" />

<img width="777" height="397" alt="image" src="https://github.com/user-attachments/assets/f23e7596-8978-4bad-a15b-b17d312f519b" />

<img width="932" height="582" alt="image" src="https://github.com/user-attachments/assets/03a286af-7415-4f17-ba52-44b51cfe8358" />

6b1628b016dff46e6fa35684be6acc96:summer

## References
 
- Heinonen, J. (2026). *How to Install Linux to VirtualBox?* https://github.com/johannaheinonen/johanna-test-repo/blob/main/module_1.md
- Karvinen, T. (2020). *Command Line Basics Revisited.* https://terokarvinen.com/2020/command-line-basics-revisited/
- Karvinen, T. (2022). *Cracking Passwords with Hashcat.* https://terokarvinen.com/2022/cracking-passwords-with-hashcat/

**AI assistance disclosure:** Claude assisted with organizing and editing this report. Commands, settings, and verification results are based on my own work.
