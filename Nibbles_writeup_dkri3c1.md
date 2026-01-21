## Recon

```bash=
┌──(kali😺dkri3c1)-[~]
└─$ rustscan -a 10.129.2.170 -r 1-65535 --ulimit 5000 -- -sC -sV -Pn
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Breaking and entering... into the world of open ports.

[~] The config file is expected to be at "/home/kali/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.129.2.170:22
Open 10.129.2.170:80
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} -{{ipversion}} {{ip}} -sC -sV -Pn" on ip 10.129.2.170
Depending on the complexity of the script, results may take some time to appear.
[~] Starting Nmap 7.95 ( https://nmap.org ) at 2026-01-20 00:17 EST
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 00:17
Completed NSE at 00:17, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 00:17
Completed NSE at 00:17, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 00:17
Completed NSE at 00:17, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 00:17
Completed Parallel DNS resolution of 1 host. at 00:17, 0.02s elapsed
DNS resolution of 1 IPs took 0.02s. Mode: Async [#: 3, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 00:17
Scanning 10.129.2.170 [2 ports]
Discovered open port 22/tcp on 10.129.2.170
Discovered open port 80/tcp on 10.129.2.170
Completed SYN Stealth Scan at 00:17, 0.09s elapsed (2 total ports)
Initiating Service scan at 00:17
Scanning 2 services on 10.129.2.170
Completed Service scan at 00:17, 6.23s elapsed (2 services on 1 host)
NSE: Script scanning 10.129.2.170.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 00:17
Completed NSE at 00:17, 2.05s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 00:17
Completed NSE at 00:17, 0.27s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 00:17
Completed NSE at 00:17, 0.00s elapsed
Nmap scan report for 10.129.2.170
Host is up, received user-set (0.055s latency).
Scanned at 2026-01-20 00:17:18 EST for 9s

PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 c4:f8:ad:e8:f8:04:77:de:cf:15:0d:63:0a:18:7e:49 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD8ArTOHWzqhwcyAZWc2CmxfLmVVTwfLZf0zhCBREGCpS2WC3NhAKQ2zefCHCU8XTC8hY9ta5ocU+p7S52OGHlaG7HuA5Xlnihl1INNsMX7gpNcfQEYnyby+hjHWPLo4++fAyO/lB8NammyA13MzvJy8pxvB9gmCJhVPaFzG5yX6Ly8OIsvVDk+qVa5eLCIua1E7WGACUlmkEGljDvzOaBdogMQZ8TGBTqNZbShnFH1WsUxBtJNRtYfeeGjztKTQqqj4WD5atU8dqV/iwmTylpE7wdHZ+38ckuYL9dmUPLh4Li2ZgdY6XniVOBGthY5a2uJ2OFp2xe1WS9KvbYjJ/tH
|   256 22:8f:b1:97:bf:0f:17:08:fc:7e:2c:8f:e9:77:3a:48 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBPiFJd2F35NPKIQxKMHrgPzVzoNHOJtTtM+zlwVfxzvcXPFFuQrOL7X6Mi9YQF9QRVJpwtmV9KAtWltmk3qm4oc=
|   256 e6:ac:27:a3:b5:a9:f1:12:3c:34:a5:5d:5b:eb:3d:e9 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIC/RjKhT/2YPlCgFQLx+gOXhC6W3A3raTzjlXQMT8Msk
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 00:17
Completed NSE at 00:17, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 00:17
Completed NSE at 00:17, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 00:17
Completed NSE at 00:17, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.77 seconds
           Raw packets sent: 2 (88B) | Rcvd: 2 (88B)

```

## Exploit

連上網頁，只有一個 Hello World

![image](https://hackmd.io/_uploads/SyA5l9nSbe.png)

view-source 找到路徑 `/nibbleblog/`

![image](https://hackmd.io/_uploads/SyM2g5nSZx.png)

連進去之後長這樣

![image](https://hackmd.io/_uploads/Bk4Cgc2Hbx.png)

feroxbuster 炸網頁

```bash=
┌──(kali😺dkri3c1)-[~]
└─$ feroxbuster -u "http://10.129.2.170/nibbleblog/" -x .php

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.13.1
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.129.2.170/nibbleblog
 🚩  In-Scope Url          │ 10.129.2.170
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.13.1
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 💲  Extensions            │ [php]
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
403      GET       11l       32w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
404      GET        9l       32w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
301      GET        9l       28w      317c http://10.129.2.170/nibbleblog => http://10.129.2.170/nibbleblog/
301      GET        9l       28w      323c http://10.129.2.170/nibbleblog/admin => http://10.129.2.170/nibbleblog/admin/
200      GET        9l       16w      270c http://10.129.2.170/nibbleblog/plugins/latest_posts/languages/ru_RU.bit
200      GET        9l       12w      129c http://10.129.2.170/nibbleblog/plugins/my_image/languages/es_ES.bit
200      GET        9l       11w      172c http://10.129.2.170/nibbleblog/plugins/my_image/languages/ru_RU.bit
200      GET        9l       17w      173c http://10.129.2.170/nibbleblog/plugins/latest_posts/languages/fr_FR.bit
200      GET        9l       19w      183c http://10.129.2.170/nibbleblog/plugins/latest_posts/languages/es_ES.bit
200      GET        9l       16w      159c http://10.129.2.170/nibbleblog/plugins/latest_posts/languages/en_US.bit
301      GET        9l       28w      325c http://10.129.2.170/nibbleblog/content => http://10.129.2.170/nibbleblog/content/
200      GET        2l       50w     1936c http://10.129.2.170/nibbleblog/content/private/config.xml
200      GET        2l       45w     1142c http://10.129.2.170/nibbleblog/content/private/notifications.xml
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/content/private/keys.php
200      GET       46l      203w    21457c http://10.129.2.170/nibbleblog/content/public/upload/nibbles_0_nbmedia.jpg
200      GET      107l      603w    72467c http://10.129.2.170/nibbleblog/content/public/upload/nibbles_0_thumb.jpg
200      GET      303l      967w   114530c http://10.129.2.170/nibbleblog/content/public/upload/nibbles_0_o.jpg
301      GET        9l       28w      327c http://10.129.2.170/nibbleblog/languages => http://10.129.2.170/nibbleblog/languages/
200      GET      326l     1740w    17135c http://10.129.2.170/nibbleblog/languages/en_US.bit
200      GET      288l      921w    16627c http://10.129.2.170/nibbleblog/languages/zh_TW.bit
200      GET      288l      905w    16495c http://10.129.2.170/nibbleblog/languages/zh_CN.bit
200      GET      287l     1754w    17569c http://10.129.2.170/nibbleblog/languages/nl_NL.bit
200      GET      288l     1797w    18351c http://10.129.2.170/nibbleblog/languages/it_IT.bit
200      GET      288l     1942w    19170c http://10.129.2.170/nibbleblog/languages/fr_FR.bit
200      GET      288l     1575w    17763c http://10.129.2.170/nibbleblog/languages/de_DE.bit
200      GET      288l     1645w    18190c http://10.129.2.170/nibbleblog/languages/pl_PL.bit
200      GET      288l     2061w    18787c http://10.129.2.170/nibbleblog/languages/vi_VI.bit
200      GET      288l     1810w    18341c http://10.129.2.170/nibbleblog/languages/es_ES.bit
200      GET       11l       13w      402c http://10.129.2.170/nibbleblog/sitemap.php
200      GET      305l     1646w    25081c http://10.129.2.170/nibbleblog/languages/ru_RU.bit
200      GET      288l     1748w    17998c http://10.129.2.170/nibbleblog/languages/pt_PT.bit
301      GET        9l       28w      324c http://10.129.2.170/nibbleblog/themes => http://10.129.2.170/nibbleblog/themes/
200      GET        8l       15w      302c http://10.129.2.170/nibbleblog/feed.php
200      GET       16l       51w      555c http://10.129.2.170/nibbleblog/themes/note-2/config.bit
200      GET       30l       99w     1073c http://10.129.2.170/nibbleblog/themes/techie/config.bit
200      GET       16l       33w      463c http://10.129.2.170/nibbleblog/themes/simpler/config.bit
200      GET       15l       45w      469c http://10.129.2.170/nibbleblog/themes/echo/config.bit
200      GET       43l      197w    16352c http://10.129.2.170/nibbleblog/themes/techie/screenshot.jpg
200      GET       22l       70w      688c http://10.129.2.170/nibbleblog/themes/medium/config.bit
200      GET       21l       59w      559c http://10.129.2.170/nibbleblog/themes/medium/init.bit
200      GET       71l      358w    27337c http://10.129.2.170/nibbleblog/themes/note-2/screenshot.jpg
200      GET       96l      507w    38409c http://10.129.2.170/nibbleblog/themes/echo/screenshot.jpg
200      GET       55l      348w    27055c http://10.129.2.170/nibbleblog/themes/medium/screenshot.jpg
200      GET       99l      597w    48368c http://10.129.2.170/nibbleblog/themes/simpler/screenshot.jpg
200      GET       67l      147w     1530c http://10.129.2.170/nibbleblog/themes/medium/templates/default.bit
301      GET        9l       28w      325c http://10.129.2.170/nibbleblog/plugins => http://10.129.2.170/nibbleblog/plugins/
200      GET       27l       96w     1401c http://10.129.2.170/nibbleblog/admin.php
200      GET       60l      158w     2197c http://10.129.2.170/nibbleblog/plugins/about/plugin.bit
200      GET       19l       29w      255c http://10.129.2.170/nibbleblog/plugins/hello/plugin.bit
200      GET       36l       61w      805c http://10.129.2.170/nibbleblog/plugins/categories/plugin.bit
200      GET       42l       69w      934c http://10.129.2.170/nibbleblog/plugins/sponsors/plugin.bit
200      GET       86l      217w     2788c http://10.129.2.170/nibbleblog/plugins/open_graph/plugin.bit
200      GET       36l       70w     1331c http://10.129.2.170/nibbleblog/plugins/quick_links/plugin.bit
200      GET       42l       73w     1016c http://10.129.2.170/nibbleblog/plugins/maintenance_mode/plugin.bit
200      GET       99l      231w     2987c http://10.129.2.170/nibbleblog/plugins/twitter_cards/plugin.bit
200      GET       68l      134w     1688c http://10.129.2.170/nibbleblog/plugins/tag_cloud/plugin.bit
200      GET       42l       70w      951c http://10.129.2.170/nibbleblog/plugins/html_code/plugin.bit
200      GET       39l       65w      898c http://10.129.2.170/nibbleblog/plugins/pages/plugin.bit
200      GET       25l       41w      580c http://10.129.2.170/nibbleblog/plugins/slogan/plugin.bit
200      GET       56l      112w     1439c http://10.129.2.170/nibbleblog/plugins/analytics/plugin.bit
200      GET        9l       10w      248c http://10.129.2.170/nibbleblog/admin/ajax/security.bit
200      GET       12l       32w      303c http://10.129.2.170/nibbleblog/plugins/about/languages/en_US.bit
200      GET       12l       37w      347c http://10.129.2.170/nibbleblog/plugins/about/languages/fr_FR.bit
200      GET       12l       39w      340c http://10.129.2.170/nibbleblog/plugins/about/languages/es_ES.bit
200      GET       13l       15w      430c http://10.129.2.170/nibbleblog/admin/boot/ajax.bit
200      GET       12l       33w      455c http://10.129.2.170/nibbleblog/plugins/about/languages/ru_RU.bit
200      GET       25l       24w      767c http://10.129.2.170/nibbleblog/admin/boot/blog.bit
200      GET       21l       20w      618c http://10.129.2.170/nibbleblog/admin/boot/admin.bit
200      GET       15l       16w      468c http://10.129.2.170/nibbleblog/admin/boot/feed.bit
200      GET       51l       99w      902c http://10.129.2.170/nibbleblog/admin/js/functions.js
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/plugin.class.php
200      GET       71l      116w     1240c http://10.129.2.170/nibbleblog/admin/js/ajax_form.bit
200      GET        1l       21w      277c http://10.129.2.170/nibbleblog/admin/js/system.php
200      GET        1l        3w       86c http://10.129.2.170/nibbleblog/admin/ajax/posts.php
200      GET        1l        3w       86c http://10.129.2.170/nibbleblog/admin/ajax/settings.php
200      GET        1l        3w       86c http://10.129.2.170/nibbleblog/admin/ajax/categories.php
200      GET        8l       20w      268c http://10.129.2.170/nibbleblog/plugins/categories/languages/ru_RU.bit
200      GET        1l        3w       86c http://10.129.2.170/nibbleblog/admin/ajax/uploader.php
200      GET        8l       22w      187c http://10.129.2.170/nibbleblog/plugins/categories/languages/es_ES.bit
200      GET        1l        3w       86c http://10.129.2.170/nibbleblog/admin/ajax/pages.php
200      GET        1l        3w       86c http://10.129.2.170/nibbleblog/admin/ajax/uploader%20(copy).php
200      GET        9l       23w      205c http://10.129.2.170/nibbleblog/plugins/sponsors/languages/en_US.bit
200      GET        9l       25w      234c http://10.129.2.170/nibbleblog/plugins/sponsors/languages/es_ES.bit
200      GET        9l       24w      307c http://10.129.2.170/nibbleblog/plugins/sponsors/languages/ru_RU.bit
200      GET        1l        3w       86c http://10.129.2.170/nibbleblog/admin/ajax/posts_get_video_info.php
200      GET       10l       34w      481c http://10.129.2.170/nibbleblog/plugins/tag_cloud/languages/ru_RU.bit
200      GET        8l       26w      194c http://10.129.2.170/nibbleblog/plugins/twitter_cards/languages/en_US.bit
200      GET        8l       17w      134c http://10.129.2.170/nibbleblog/plugins/twitter_cards/languages/es_ES.bit
200      GET        9l       15w      155c http://10.129.2.170/nibbleblog/plugins/maintenance_mode/languages/fr_FR.bit
200      GET        8l       30w      212c http://10.129.2.170/nibbleblog/plugins/slogan/languages/es_ES.bit
200      GET        1l        2w       13c http://10.129.2.170/nibbleblog/admin/ajax/mobile.php
200      GET        8l       31w      210c http://10.129.2.170/nibbleblog/plugins/slogan/languages/en_US.bit
200      GET        8l       12w      116c http://10.129.2.170/nibbleblog/plugins/open_graph/languages/ru_RU.bit
200      GET        8l       12w      109c http://10.129.2.170/nibbleblog/plugins/open_graph/languages/fr_FR.bit
200      GET        1l        3w       86c http://10.129.2.170/nibbleblog/admin/ajax/comments.php
200      GET        8l       34w      249c http://10.129.2.170/nibbleblog/plugins/slogan/languages/fr_FR_bit
200      GET        8l       12w      109c http://10.129.2.170/nibbleblog/plugins/open_graph/languages/en_US.bit
200      GET       10l       46w      354c http://10.129.2.170/nibbleblog/plugins/tag_cloud/languages/es_ES.bit
200      GET        8l       11w      139c http://10.129.2.170/nibbleblog/plugins/hello/languages/ru_RU.bit
200      GET        8l       11w      109c http://10.129.2.170/nibbleblog/plugins/hello/languages/fr_FR.bit
200      GET       30l       46w      740c http://10.129.2.170/nibbleblog/admin/controllers/settings/regional.bit
200      GET       11l       14w      352c http://10.129.2.170/nibbleblog/admin/controllers/settings/seo.bit
200      GET       11l       14w      352c http://10.129.2.170/nibbleblog/admin/controllers/settings/notifications.bit
200      GET       13l       34w      412c http://10.129.2.170/nibbleblog/admin/controllers/settings/general.bit
200      GET      105l      209w     2647c http://10.129.2.170/nibbleblog/admin/boot/rules/98-plugins.bit
200      GET       96l      325w     3796c http://10.129.2.170/nibbleblog/admin/boot/rules/2-objects.bit
200      GET       61l      153w     1869c http://10.129.2.170/nibbleblog/admin/boot/rules/5-url.bit
200      GET       17l       24w      531c http://10.129.2.170/nibbleblog/admin/controllers/dashboard/view.bit
200      GET       15l       19w      112c http://10.129.2.170/nibbleblog/admin/boot/rules/99-misc.bit
200      GET       27l       39w      470c http://10.129.2.170/nibbleblog/admin/boot/rules/5-regional.bit
200      GET       89l      195w     3102c http://10.129.2.170/nibbleblog/admin/boot/rules/1-fs_php.bit
200      GET       91l      333w     7130c http://10.129.2.170/nibbleblog/admin/boot/rules/11-admin.bit
200      GET       75l      176w     2090c http://10.129.2.170/nibbleblog/admin/boot/rules/10-pager.bit
200      GET       38l       83w     1068c http://10.129.2.170/nibbleblog/admin/templates/login/index.bit
200      GET       78l      194w     2720c http://10.129.2.170/nibbleblog/admin/templates/easy4/index.bit
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/db/nbxml.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/db/db_tags.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/db/db_categories.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/db/db_settings.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/db/db_notifications.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/db/db_posts.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/db/db_pages.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/db/db_users.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/db/db_comments.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/number.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/crypt.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/language.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/html.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/image.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/pager.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/redirect.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/post.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/resize.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/session.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/validation.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/url.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/category.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/video.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/social.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/date.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/text.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/blog.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/email.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/page.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/filesystem.class.php
200      GET       25l       34w      385c http://10.129.2.170/nibbleblog/admin/views/dashboard/view.bit
200      GET       21l       66w     1265c http://10.129.2.170/nibbleblog/admin/views/dashboard/quick_start.bit
200      GET       27l       52w      794c http://10.129.2.170/nibbleblog/admin/views/dashboard/last_comments.bit
200      GET      160l      375w     4744c http://10.129.2.170/nibbleblog/admin/js/reveal/jquery.reveal.js
200      GET       11l       22w      305c http://10.129.2.170/nibbleblog/admin/views/user/send_forgot.bit
200      GET       25l       74w     1127c http://10.129.2.170/nibbleblog/admin/views/user/login.bit
200      GET       19l       53w      763c http://10.129.2.170/nibbleblog/admin/views/user/forgot.bit
200      GET       64l      130w     1845c http://10.129.2.170/nibbleblog/admin/views/dashboard/notifications.bit
200      GET        1l       47w     3532c http://10.129.2.170/nibbleblog/admin/js/tinymce/jquery.tinymce.min.js
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/defensio/Defensio.php
200      GET       31l       38w      593c http://10.129.2.170/nibbleblog/admin/views/post/new_quote.bit
200      GET       33l       41w      620c http://10.129.2.170/nibbleblog/admin/views/post/new_simple.bit
200      GET       21l       39w      638c http://10.129.2.170/nibbleblog/admin/views/dashboard/drafts.bit
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/net.class.php
200      GET       36l       45w      698c http://10.129.2.170/nibbleblog/admin/views/post/edit.bit
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/cookie.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/helpers/plugin.class.php
200      GET       93l      172w     2327c http://10.129.2.170/nibbleblog/admin/views/post/new_video.bit
200      GET       95l      156w     2287c http://10.129.2.170/nibbleblog/admin/views/post/list.bit
200      GET        4l     1304w    83615c http://10.129.2.170/nibbleblog/admin/js/jquery/jquery.js
200      GET       59l       99w     1283c http://10.129.2.170/nibbleblog/plugins/latest_posts/plugin.bit
200      GET       88l      174w     1622c http://10.129.2.170/nibbleblog/update.php
200      GET        1l       11w       78c http://10.129.2.170/nibbleblog/install.php
200      GET        2l        6w       93c http://10.129.2.170/nibbleblog/content/private/posts.xml
200      GET       51l      110w     1404c http://10.129.2.170/nibbleblog/plugins/my_image/plugin.bit
200      GET        9l       12w      129c http://10.129.2.170/nibbleblog/plugins/my_image/languages/fr_FR.bit
200      GET        9l       12w      123c http://10.129.2.170/nibbleblog/plugins/my_image/languages/en_US.bit
200      GET        2l        6w       95c http://10.129.2.170/nibbleblog/content/private/pages.xml
200      GET        2l       21w      325c http://10.129.2.170/nibbleblog/content/private/categories.xml
200      GET        2l       17w      503c http://10.129.2.170/nibbleblog/content/private/users.xml
200      GET        2l        6w       97c http://10.129.2.170/nibbleblog/content/private/tags.xml
200      GET        2l       14w      431c http://10.129.2.170/nibbleblog/content/private/comments.xml
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/content/private/shadow.php
200      GET       85l      155w     1672c http://10.129.2.170/nibbleblog/themes/simpler/css/page.css
200      GET       77l      120w     1340c http://10.129.2.170/nibbleblog/themes/simpler/css/plugins.css
200      GET      171l      270w     3027c http://10.129.2.170/nibbleblog/themes/simpler/css/main.css
200      GET      262l      483w     4839c http://10.129.2.170/nibbleblog/themes/simpler/css/post.css
200      GET       88l      191w     1473c http://10.129.2.170/nibbleblog/themes/simpler/css/rainbow.css
200      GET       35l       57w     1729c http://10.129.2.170/nibbleblog/themes/simpler/css/normalize.css
200      GET       50l      155w    28396c http://10.129.2.170/nibbleblog/themes/note-2/js/scripts.js
200      GET       10l     3958w   260266c http://10.129.2.170/nibbleblog/admin/js/tinymce/tinymce.min.js
200      GET      146l     1032w    82541c http://10.129.2.170/nibbleblog/admin/templates/easy4/css/img/grey.png
200      GET       71l      165w     1955c http://10.129.2.170/nibbleblog/themes/note-2/templates/default.bit
200      GET       83l      177w     1852c http://10.129.2.170/nibbleblog/themes/simpler/templates/default.bit
200      GET       77l      120w     1340c http://10.129.2.170/nibbleblog/themes/medium/css/plugins.css
200      GET       88l      191w     1473c http://10.129.2.170/nibbleblog/themes/medium/css/rainbow.css
200      GET      378l      554w    13600c http://10.129.2.170/nibbleblog/themes/note-2/css/styles.css
200      GET       43l      143w    28199c http://10.129.2.170/nibbleblog/themes/simpler/js/rainbow-custom.min.js
200      GET       61l      168w     2987c http://10.129.2.170/nibbleblog/index.php
200      GET       68l      141w     1634c http://10.129.2.170/nibbleblog/themes/echo/templates/default.bit
200      GET        8l       24w      302c http://10.129.2.170/nibbleblog/plugins/slogan/languages/ru_RU.bit
200      GET        8l       24w      201c http://10.129.2.170/nibbleblog/plugins/categories/languages/fr_FR.bit
200      GET        9l       35w      418c http://10.129.2.170/nibbleblog/plugins/analytics/languages/ru_RU.bit
200      GET        8l       22w      173c http://10.129.2.170/nibbleblog/plugins/categories/languages/en_US.bit
200      GET        9l       37w      312c http://10.129.2.170/nibbleblog/plugins/analytics/languages/fr_FR.bit
200      GET        9l       15w      153c http://10.129.2.170/nibbleblog/plugins/maintenance_mode/languages/en_US.bit
200      GET        9l       36w      280c http://10.129.2.170/nibbleblog/plugins/analytics/languages/en_US.bit
200      GET        9l       39w      309c http://10.129.2.170/nibbleblog/plugins/analytics/languages/es_ES.bit
200      GET        9l       21w      178c http://10.129.2.170/nibbleblog/plugins/html_code/languages/fr_FR.bit
200      GET        9l       15w      219c http://10.129.2.170/nibbleblog/plugins/maintenance_mode/languages/ru_RU.bit
200      GET        8l       23w      274c http://10.129.2.170/nibbleblog/plugins/twitter_cards/languages/ru_RU.bit
200      GET        8l       12w      109c http://10.129.2.170/nibbleblog/plugins/open_graph/languages/es_ES.bit
200      GET        9l       17w      161c http://10.129.2.170/nibbleblog/plugins/maintenance_mode/languages/es_ES.bit
200      GET        8l       24w      195c http://10.129.2.170/nibbleblog/plugins/twitter_cards/languages/fr_FR.bit
200      GET        9l       30w      249c http://10.129.2.170/nibbleblog/plugins/sponsors/languages/fr_FR.bit
200      GET       33l       66w     1014c http://10.129.2.170/nibbleblog/admin/controllers/categories/edit.bit
200      GET       96l      152w     2439c http://10.129.2.170/nibbleblog/admin/views/page/list.bit
200      GET       71l      127w     1745c http://10.129.2.170/nibbleblog/admin/controllers/page/edit.bit
200      GET       30l       37w      563c http://10.129.2.170/nibbleblog/admin/views/page/new.bit
200      GET       14l       23w      530c http://10.129.2.170/nibbleblog/admin/controllers/comments/list.bit
200      GET       65l      111w     1556c http://10.129.2.170/nibbleblog/admin/controllers/page/new.bit
200      GET       14l       22w      526c http://10.129.2.170/nibbleblog/admin/controllers/comments/settings.bit
200      GET       14l       14w      356c http://10.129.2.170/nibbleblog/admin/controllers/plugins/list.bit
200      GET       16l       19w      355c http://10.129.2.170/nibbleblog/admin/controllers/plugins/uninstall.bit
200      GET       19l       34w      549c http://10.129.2.170/nibbleblog/admin/controllers/plugins/install.bit
200      GET       59l      118w     1969c http://10.129.2.170/nibbleblog/admin/controllers/plugins/config.bit
200      GET      109l      229w     3125c http://10.129.2.170/nibbleblog/admin/controllers/post/edit.bit
200      GET       14l       25w      524c http://10.129.2.170/nibbleblog/admin/controllers/post/list.bit
200      GET       25l       40w      747c http://10.129.2.170/nibbleblog/admin/controllers/page/list.bit
200      GET       19l       45w      728c http://10.129.2.170/nibbleblog/admin/views/plugins/config.bit
200      GET       27l       51w      754c http://10.129.2.170/nibbleblog/admin/controllers/categories/list.bit
200      GET        8l       10w      138c http://10.129.2.170/nibbleblog/plugins/pages/languages/ru_RU.bit
200      GET       89l      156w     2079c http://10.129.2.170/nibbleblog/admin/controllers/post/new.bit
200      GET       13l       22w      395c http://10.129.2.170/nibbleblog/plugins/quick_links/languages/ru_RU.bit
200      GET       26l       69w     1278c http://10.129.2.170/nibbleblog/admin/views/plugins/list.bit
200      GET       81l      265w     4729c http://10.129.2.170/nibbleblog/admin/views/comments/settings.bit
200      GET       13l       25w      287c http://10.129.2.170/nibbleblog/plugins/quick_links/languages/en_US.bit
200      GET        8l       11w      109c http://10.129.2.170/nibbleblog/plugins/pages/languages/fr_FR.bit
200      GET       13l       30w      332c http://10.129.2.170/nibbleblog/plugins/quick_links/languages/es_ES.bit
200      GET        8l       10w      101c http://10.129.2.170/nibbleblog/plugins/pages/languages/en_US.bit
200      GET       13l       26w      326c http://10.129.2.170/nibbleblog/plugins/quick_links/languages/fr_FR.bit
200      GET        8l       16w      130c http://10.129.2.170/nibbleblog/plugins/pages/languages/es_ES.bit
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/api/login.class.php
200      GET        0l        0w        0c http://10.129.2.170/nibbleblog/admin/kernel/api/comment.class.php
200      GET       67l      162w     2713c http://10.129.2.170/nibbleblog/admin/views/settings/regional.bit
200      GET       51l      158w     2365c http://10.129.2.170/nibbleblog/admin/views/settings/username.bit
200      GET       59l      179w     3220c http://10.129.2.170/nibbleblog/admin/views/settings/notifications.bit
200      GET       64l      208w     3738c http://10.129.2.170/nibbleblog/admin/views/settings/image.bit
200      GET       28l       48w      932c http://10.129.2.170/nibbleblog/admin/views/settings/themes.bit
200      GET       97l      309w     4774c http://10.129.2.170/nibbleblog/admin/views/settings/general.bit
200      GET       90l      172w     2458c http://10.129.2.170/nibbleblog/admin/views/categories/list.bit
200      GET      125l      346w     5337c http://10.129.2.170/nibbleblog/admin/views/settings/seo.bit
200      GET       33l      107w     1360c http://10.129.2.170/nibbleblog/admin/views/categories/edit.bit
200      GET        8l       11w      106c http://10.129.2.170/nibbleblog/plugins/hello/languages/en_US.bit
200      GET       41l       80w     1502c http://10.129.2.170/nibbleblog/admin/controllers/user/login.bit
200      GET       10l       42w      306c http://10.129.2.170/nibbleblog/plugins/tag_cloud/languages/en_US.bit
200      GET       51l      100w     1484c http://10.129.2.170/nibbleblog/admin/controllers/user/forgot.bit
200      GET        7l        4w       75c http://10.129.2.170/nibbleblog/admin/controllers/user/logout.bit
200      GET       10l       46w      355c http://10.129.2.170/nibbleblog/plugins/tag_cloud/languages/fr_FR.bit
200      GET       44l       59w     1164c http://10.129.2.170/nibbleblog/admin/controllers/user/send_forgot.bit
200      GET        9l       21w      169c http://10.129.2.170/nibbleblog/plugins/html_code/languages/en_US.bit
200      GET      112l      227w     2925c http://10.129.2.170/nibbleblog/admin/boot/rules/98-blog.bit
200      GET        9l       19w      168c http://10.129.2.170/nibbleblog/plugins/html_code/languages/es_ES.bit
200      GET        9l       19w      207c http://10.129.2.170/nibbleblog/plugins/html_code/languages/ru_RU.bit
200      GET       54l       95w     1250c http://10.129.2.170/nibbleblog/admin/boot/rules/98-comments.bit
200      GET       37l       53w      824c http://10.129.2.170/nibbleblog/admin/boot/rules/98-constants.bit
200      GET       31l       50w      711c http://10.129.2.170/nibbleblog/admin/boot/rules/10-seo.bit
200      GET       63l      643w     4628c http://10.129.2.170/nibbleblog/README
200      GET       21l       56w      693c http://10.129.2.170/nibbleblog/themes/techie/js/sidebar.js
200      GET       10l       27w     1728c http://10.129.2.170/nibbleblog/themes/echo/js/script.js
200      GET      426l      614w    10275c http://10.129.2.170/nibbleblog/themes/echo/css/style.css
200      GET       21l       53w      615c http://10.129.2.170/nibbleblog/themes/techie/js/taglist.js
200      GET       10l       24w      323c http://10.129.2.170/nibbleblog/themes/techie/js/browser-update.js
200      GET       43l      143w    28395c http://10.129.2.170/nibbleblog/themes/techie/js/rainbow-custom.min.js
404      GET        9l       33w      299c http://10.129.2.170/nibbleblog/Reports%20List
404      GET        9l       33w      303c http://10.129.2.170/nibbleblog/Reports%20List.php
200      GET       89l      186w     2347c http://10.129.2.170/nibbleblog/themes/techie/templates/default.bit
404      GET        9l       33w      301c http://10.129.2.170/nibbleblog/external%20files
200      GET      130l      224w     3049c http://10.129.2.170/nibbleblog/admin/views/comments/list.bit
404      GET        9l       33w      300c http://10.129.2.170/nibbleblog/Style%20Library
404      GET        9l       33w      304c http://10.129.2.170/nibbleblog/Style%20Library.php
404      GET        9l       33w      297c http://10.129.2.170/nibbleblog/modern%20mom
404      GET        9l       34w      302c http://10.129.2.170/nibbleblog/neuf%20giga%20photo
404      GET        9l       33w      301c http://10.129.2.170/nibbleblog/Web%20References
404      GET        9l       33w      305c http://10.129.2.170/nibbleblog/Web%20References.php
404      GET        9l       33w      297c http://10.129.2.170/nibbleblog/My%20Project
404      GET        9l       33w      297c http://10.129.2.170/nibbleblog/Contact%20Us
404      GET        9l       33w      301c http://10.129.2.170/nibbleblog/Contact%20Us.php
404      GET        9l       33w      298c http://10.129.2.170/nibbleblog/Donate%20Cash
404      GET        9l       33w      302c http://10.129.2.170/nibbleblog/Donate%20Cash.php
404      GET        9l       33w      296c http://10.129.2.170/nibbleblog/Home%20Page
404      GET        9l       33w      300c http://10.129.2.170/nibbleblog/Home%20Page.php
404      GET        9l       33w      301c http://10.129.2.170/nibbleblog/Privacy%20Policy
404      GET        9l       33w      301c http://10.129.2.170/nibbleblog/Press%20Releases
404      GET        9l       33w      305c http://10.129.2.170/nibbleblog/Press%20Releases.php
404      GET        9l       33w      305c http://10.129.2.170/nibbleblog/Privacy%20Policy.php
404      GET        9l       33w      295c http://10.129.2.170/nibbleblog/Site%20Map
404      GET        9l       33w      299c http://10.129.2.170/nibbleblog/Site%20Map.php
404      GET        9l       33w      295c http://10.129.2.170/nibbleblog/About%20Us
404      GET        9l       33w      299c http://10.129.2.170/nibbleblog/Bequest%20Gift
404      GET        9l       33w      303c http://10.129.2.170/nibbleblog/Bequest%20Gift.php
404      GET        9l       33w      300c http://10.129.2.170/nibbleblog/Gift%20Form.php
404      GET        9l       34w      303c http://10.129.2.170/nibbleblog/Life%20Income%20Gift
404      GET        9l       34w      307c http://10.129.2.170/nibbleblog/Life%20Income%20Gift.php
404      GET        9l       33w      297c http://10.129.2.170/nibbleblog/New%20Folder
404      GET        9l       33w      301c http://10.129.2.170/nibbleblog/New%20Folder.php
404      GET        9l       33w      298c http://10.129.2.170/nibbleblog/Site%20Assets
404      GET        9l       33w      302c http://10.129.2.170/nibbleblog/Site%20Assets.php
404      GET        9l       34w      298c http://10.129.2.170/nibbleblog/What%20is%20New
[####################] - 2m     61470/61470   0s      found:302     errors:50
[####################] - 2m     60000/60000   595/s   http://10.129.2.170/nibbleblog/
[####################] - 6s     60000/60000   10106/s http://10.129.2.170/nibbleblog/admin/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 6s     60000/60000   10281/s http://10.129.2.170/nibbleblog/plugins/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 6s     60000/60000   10379/s http://10.129.2.170/nibbleblog/plugins/about/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 7s     60000/60000   8861/s  http://10.129.2.170/nibbleblog/plugins/my_image/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 7s     60000/60000   8872/s  http://10.129.2.170/nibbleblog/plugins/latest_posts/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 7s     60000/60000   9072/s  http://10.129.2.170/nibbleblog/plugins/my_image/languages/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 7s     60000/60000   9074/s  http://10.129.2.170/nibbleblog/plugins/latest_posts/languages/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   372671/s http://10.129.2.170/nibbleblog/content/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   422535/s http://10.129.2.170/nibbleblog/content/tmp/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 5s     60000/60000   11669/s http://10.129.2.170/nibbleblog/content/private/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   384615/s http://10.129.2.170/nibbleblog/content/public/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   410959/s http://10.129.2.170/nibbleblog/content/public/pages/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   350877/s http://10.129.2.170/nibbleblog/content/public/posts/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   361446/s http://10.129.2.170/nibbleblog/content/public/comments/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 1s     60000/60000   102215/s http://10.129.2.170/nibbleblog/content/public/upload/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 1s     60000/60000   89286/s http://10.129.2.170/nibbleblog/languages/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   372671/s http://10.129.2.170/nibbleblog/themes/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   208333/s http://10.129.2.170/nibbleblog/themes/note-2/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   165289/s http://10.129.2.170/nibbleblog/themes/simpler/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 2s     60000/60000   30380/s http://10.129.2.170/nibbleblog/themes/techie/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   196721/s http://10.129.2.170/nibbleblog/themes/echo/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   176471/s http://10.129.2.170/nibbleblog/themes/medium/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 3s     60000/60000   21067/s http://10.129.2.170/nibbleblog/themes/note-2/templates/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 2s     60000/60000   29211/s http://10.129.2.170/nibbleblog/themes/note-2/js/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 10s    60000/60000   6068/s  http://10.129.2.170/nibbleblog/themes/note-2/css/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 2s     60000/60000   31813/s http://10.129.2.170/nibbleblog/themes/note-2/views/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 8s     60000/60000   7600/s  http://10.129.2.170/nibbleblog/themes/echo/views/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 8s     60000/60000   7610/s  http://10.129.2.170/nibbleblog/themes/simpler/templates/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 2s     60000/60000   32189/s http://10.129.2.170/nibbleblog/themes/simpler/css/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 3s     60000/60000   20007/s http://10.129.2.170/nibbleblog/themes/simpler/js/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 3s     60000/60000   21254/s http://10.129.2.170/nibbleblog/themes/simpler/views/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 10s    60000/60000   6097/s  http://10.129.2.170/nibbleblog/themes/techie/views/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 8s     60000/60000   7536/s  http://10.129.2.170/nibbleblog/themes/echo/css/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 4s     60000/60000   15536/s http://10.129.2.170/nibbleblog/themes/echo/templates/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 8s     60000/60000   7534/s  http://10.129.2.170/nibbleblog/themes/echo/js/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 10s    60000/60000   6119/s  http://10.129.2.170/nibbleblog/themes/techie/css/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 10s    60000/60000   6229/s  http://10.129.2.170/nibbleblog/themes/techie/templates/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 8s     60000/60000   7431/s  http://10.129.2.170/nibbleblog/themes/techie/js/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 10s    60000/60000   6138/s  http://10.129.2.170/nibbleblog/themes/medium/js/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 10s    60000/60000   6130/s  http://10.129.2.170/nibbleblog/themes/medium/views/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 10s    60000/60000   6134/s  http://10.129.2.170/nibbleblog/themes/medium/css/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   472441/s http://10.129.2.170/nibbleblog/themes/medium/templates/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 6s     60000/60000   9838/s  http://10.129.2.170/nibbleblog/themes/medium/controllers/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   389610/s http://10.129.2.170/nibbleblog/plugins/categories/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   317460/s http://10.129.2.170/nibbleblog/plugins/hello/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   322581/s http://10.129.2.170/nibbleblog/plugins/slogan/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   338983/s http://10.129.2.170/nibbleblog/plugins/html_code/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 0s     60000/60000   331492/s http://10.129.2.170/nibbleblog/plugins/tag_cloud/ => Directory listing (add --scan-dir-listings to scan)
[####################] - 5s     60000/60000   10913/s http://10.129.2.170/nibbleblog/plugins/quick_links/ => Directory listing (add --scan-dir-listings to scan)   
```

在 `/update.php` 找到 NibbleBlog Version

![image](https://hackmd.io/_uploads/S1cdfchBbx.png)

在 https://github.com/hadrian3689/nibbleblog_4.0.3 找到 POC，但是需要先登入才能做到 RCE

在 `/private/users.xml` 找到 username = admin

![image](https://hackmd.io/_uploads/HJ9kEcnSZg.png)

猜測用 hydra 去爆破，而登入路徑是 `admin.php`

![image](https://hackmd.io/_uploads/SyW6Yc2SWg.png)

炸一炸發現 hydra 會爛掉，於是開始通靈，猜機器名稱: `nibbles`

用 `admin/nibbles` 成功登入 admin panel

![image](https://hackmd.io/_uploads/SyAgc93Bbg.png)

進入 panel 之後用剛剛找到 exploit 去打，會發現成功拿到 shell，但是這個 shell 不太好用，於是找看看有沒有其他語言可以利用，發現有 php，於是用它來彈 rev shell

![image](https://hackmd.io/_uploads/rJuLh9nBbg.png)

在使用者目錄找到 user flag

![image](https://hackmd.io/_uploads/ByMK3c3rZe.png)

用 `sudo -l` 發現可以執行 `/home/nibbler/personal/stuff/monitor.sh`，於是把 `monitor.sh` 改成 `/bin/bash`，而成功提權

![image](https://hackmd.io/_uploads/BkWC393HWe.png)

成功拿到 root flag

![image](https://hackmd.io/_uploads/By8Jp9nrbe.png)



## Pwned!


![image](https://hackmd.io/_uploads/H1vla9nr-g.png)
