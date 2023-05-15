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
