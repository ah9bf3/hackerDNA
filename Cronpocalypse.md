1- rustscan target to identify open port
![[Pasted image 20260607173725.png]]

2- nmap scan for identified ports - enumerating services/version and default script scanning:
![[Pasted image 20260607174153.png]]
after verifying, nothing interesting for now

3- after accessing the webpage on port 80, wi notice a php query
![[Pasted image 20260607174350.png]]

4- LFI : We're able to access /etc/pass.
![[Pasted image 20260607174533.png]]
beside root user, we can see an other uer "ctf" that can have a valid shell on this machine

/etc/shadow is also accessible and we can see "ctf" user hash.
![[Pasted image 20260607174933.png]]
I started by cracking the hash but afer 10 minutes while my laptop started to really getting hot (here the command i tried: ```hashcat  -m 1800 -a 0 hash3 /usr/share/wordlists/rockyou.txt```), I decided to go return and do more manual searching in the home folder.

5- bash history file was the place to find "ctf" password
![[Pasted image 20260607175509.png]]

6- connecting to the machine via ssh anf found the firs flag.
![[Pasted image 20260607175728.png]]

7- lab title was a big hint, so I went locking into cronjobs
![[Pasted image 20260607180021.png]]
![[Pasted image 20260607205308.png]]
We see a cronjob running as root every minute that reads a file we can view. That cronjob executes a script we can write. This is the privilege-escalation vector.

8- added a line into that script to elevate the right it /root folder then for the flag.
![[Pasted image 20260607210104.png]]
![[Pasted image 20260607210137.png]]
It's not very elegant, but it serves the purpose of this challenge. We could also add a line that opens a reverse shell so we'll get a root shell on our listener.


Happy hunting!
