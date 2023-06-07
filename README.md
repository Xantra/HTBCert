# HTBCert
Cheatsheet for the Hack The Box certification

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
