## Recon

```bash=
┌──(kali😺dkri3c1)-[~]
└─$ rustscan -a 10.129.1.254 -r 1-65535 --ulimit 5000 -- -sC -sV -Pn
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Open ports, closed hearts.

[~] The config file is expected to be at "/home/kali/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.129.1.254:22
Open 10.129.1.254:80
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} -{{ipversion}} {{ip}} -sC -sV -Pn" on ip 10.129.1.254
Depending on the complexity of the script, results may take some time to appear.
[~] Starting Nmap 7.95 ( https://nmap.org ) at 2026-01-23 03:18 EST
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 03:18
Completed NSE at 03:18, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 03:18
Completed NSE at 03:18, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 03:18
Completed NSE at 03:18, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 03:18
Completed Parallel DNS resolution of 1 host. at 03:18, 0.02s elapsed
DNS resolution of 1 IPs took 0.02s. Mode: Async [#: 3, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 03:18
Scanning 10.129.1.254 [2 ports]
Discovered open port 80/tcp on 10.129.1.254
Discovered open port 22/tcp on 10.129.1.254
Completed SYN Stealth Scan at 03:18, 0.10s elapsed (2 total ports)
Initiating Service scan at 03:18
Scanning 2 services on 10.129.1.254
Completed Service scan at 03:18, 6.26s elapsed (2 services on 1 host)
NSE: Script scanning 10.129.1.254.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 03:18
Completed NSE at 03:19, 2.70s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 03:19
Completed NSE at 03:19, 0.40s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 03:19
Completed NSE at 03:19, 0.00s elapsed
Nmap scan report for 10.129.1.254
Host is up, received user-set (0.067s latency).
Scanned at 2026-01-23 03:18:52 EST for 9s

PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.2 (FreeBSD 20161230; protocol 2.0)
| ssh-hostkey:
|   2048 e3:3b:7d:3c:8f:4b:8c:f9:cd:7f:d2:3a:ce:2d:ff:bb (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDFLpOCLU3rRUdNNbb5u5WlP+JKUpoYw4znHe0n4mRlv5sQ5kkkZSDNMqXtfWUFzevPaLaJboNBOAXjPwd1OV1wL2YFcGsTL5MOXgTeW4ixpxNBsnBj67mPSmQSaWcudPUmhqnT5VhKYLbPk43FsWqGkNhDtbuBVo9/BmN+GjN1v7w54PPtn8wDd7Zap3yStvwRxeq8E0nBE4odsfBhPPC01302RZzkiXymV73WqmI8MeF9W94giTBQS5swH6NgUe4/QV1tOjTct/uzidFx+8bbcwcQ1eUgK5DyRLaEhou7PRlZX6Pg5YgcuQUlYbGjgk6ycMJDuwb2D5mJkAzN4dih
|   256 4c:e8:c6:02:bd:fc:83:ff:c9:80:01:54:7d:22:81:72 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKXh613KF4mJTcOxbIy/3mN/O/wAYht2Vt4m9PUoQBBSao16RI9B3VYod1HSbx3PYsPpKmqjcT7A/fHggPIzDYU=
|   256 0b:8f:d5:71:85:90:13:85:61:8b:eb:34:13:5f:94:3b (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJrg2EBbG5D2maVLhDME5mZwrvlhTXrK7jiEI+MiZ+Am
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.29 ((FreeBSD) PHP/5.6.32)
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
|_http-server-header: Apache/2.4.29 (FreeBSD) PHP/5.6.32
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: FreeBSD; CPE: cpe:/o:freebsd:freebsd

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 03:19
Completed NSE at 03:19, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 03:19
Completed NSE at 03:19, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 03:19
Completed NSE at 03:19, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.21 seconds
           Raw packets sent: 2 (88B) | Rcvd: 2 (88B)
```

## Exploit

連上去會是一個網頁，上面會告訴你有哪些檔案路徑，跟可以讓你執行檔案的頁面

![image](https://hackmd.io/_uploads/SJlOx3xLWx.png)

去到 `info.php` 會發現有告訴你 freeBSD 的 version

![image](https://hackmd.io/_uploads/HJUcl2lIWl.png)

沒有頭緒於是回去最一開始的網頁去試試看錯誤的路徑會如何

![image](https://hackmd.io/_uploads/Skas-hgU-g.png)

發現他有用 include 這個 function，嘗試用 LFI

![image](https://hackmd.io/_uploads/BkiTb2eLbx.png)

成功

接下來去看看 listfiles.php，會發現他給了一坨路徑

![image](https://hackmd.io/_uploads/SJRB7hxUbe.png)

去 `pwdbackup.txt` 撈檔案

![image](https://hackmd.io/_uploads/BkRvQnl8bl.png)

手寫腳本解出來

![image](https://hackmd.io/_uploads/S1hzr2x8be.png)

回頭看 `/etc/passwd` 會發現有個使用者叫做 `charix`，用他去連 ssh 看看

![image](https://hackmd.io/_uploads/HkBSLneLZe.png)

成功

![image](https://hackmd.io/_uploads/HyAIU3e8-x.png)

還看到一個 secret.zip，把他丟去 kali 之後解壓

![image](https://hackmd.io/_uploads/SkMTxpxUbe.png)

不知道能幹嘛

![image](https://hackmd.io/_uploads/By6RgplLZe.png)

提權的部分原本想用 freeBSD 的已知 CVE 去扁 Kernel 的提權，但是都找不到QQ

手足無措下去看ㄌ wp，

先用 netstat 去看機器本身還開了啥 port

```bash=
charix@Poison:~ % netstat -an
Active Internet connections (including servers)
Proto Recv-Q Send-Q Local Address          Foreign Address        (state)
tcp4       0      0 10.129.1.254.22        10.10.14.35.57782      ESTABLISHED
tcp4       0      0 10.129.1.254.22        10.10.14.35.53160      ESTABLISHED
tcp4       0      0 127.0.0.1.25           *.*                    LISTEN
tcp4       0      0 *.80                   *.*                    LISTEN
tcp6       0      0 *.80                   *.*                    LISTEN
tcp4       0      0 *.22                   *.*                    LISTEN
tcp6       0      0 *.22                   *.*                    LISTEN
tcp4       0      0 127.0.0.1.5801         *.*                    LISTEN
tcp4       0      0 127.0.0.1.5901         *.*                    LISTEN
udp4       0      0 *.514                  *.*
udp6       0      0 *.514                  *.*
Active UNIX domain sockets
Address          Type   Recv-Q Send-Q            Inode             Conn             Refs          Nextref Addr
fffff80003b6ce10 stream      0      0                0 fffff80003b6c960                0                0
fffff80003b6c960 stream      0      0                0 fffff80003b6ce10                0                0
fffff80003b6cb40 stream      0      0                0 fffff80003b6ca50                0                0
fffff80003b6ca50 stream      0      0                0 fffff80003b6cb40                0                0
fffff80003b6d3c0 stream      0      0                0 fffff80003b6d4b0                0                0 /tmp/.X11-unix/X1
fffff80003b6d4b0 stream      0      0                0 fffff80003b6d3c0                0                0
fffff80003b6d690 stream      0      0                0 fffff80003b6d5a0                0                0 /tmp/.X11-unix/X1
fffff80003b6d5a0 stream      0      0                0 fffff80003b6d690                0                0
fffff80003b6d780 stream      0      0 fffff80003d7eb10                0                0                0 /tmp/.X11-unix/X1
fffff80003b6db40 stream      0      0 fffff80003b27938                0                0                0 /var/run/devd.pipe
fffff80003b6d0f0 dgram       0      0                0 fffff80003b6d960                0                0
fffff80003b6d1e0 dgram       0      0                0 fffff80003b6d870                0 fffff80003b6d2d0
fffff80003b6d2d0 dgram       0      0                0 fffff80003b6d870                0                0
fffff80003b6d870 dgram       0      0 fffff80003d2eb10                0 fffff80003b6d1e0                0 /var/run/logpriv
fffff80003b6d960 dgram       0      0 fffff80003d2ece8                0 fffff80003b6d0f0                0 /var/run/log
fffff80003b6da50 seqpac      0      0 fffff80003b27760                0                0                0 /var/run/devd.seqpacket.pipe
charix@Poison:~ %
```

port 5801 跟 5901 看起來很可疑，參考

https://book.hacktricks.wiki/en/network-services-pentesting/pentesting-vnc.html

先 port forwarding 到 local 端

![image](https://hackmd.io/_uploads/HJvdbpg8Zx.png)

![image](https://hackmd.io/_uploads/SJhuWaeIbl.png)

然後根據他上面所說，前面那一陀奇怪的 secret 應該就是 password XD

![image](https://hackmd.io/_uploads/SJWqM6e8bg.png)


解出來長這樣

![image](https://hackmd.io/_uploads/BykiGpe8Zl.png)

依照連線方式連上去

![image](https://hackmd.io/_uploads/ByXJQpl8-e.png)


拿到 root terminal

![image](https://hackmd.io/_uploads/HywxQ6gLWl.png)


## Pwned!

![image](https://hackmd.io/_uploads/HkRlQTlIbl.png)
