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

