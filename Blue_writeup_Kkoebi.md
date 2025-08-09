HackTheBox: Blue
===

## Topic

### Lab
#### HackTheBox: 
[https://app.hackthebox.com/machines/Blue](https://app.hackthebox.com/machines/Blue)

 <img width="367" height="140" alt="image" src="https://github.com/user-attachments/assets/12eb69b5-e087-428f-a63a-3bec71b754ed" />

1.	開啟VPN連線到HTB，並確認VPN連線的IP

 <img width="676" height="191" alt="image" src="https://github.com/user-attachments/assets/f43a62ef-2bca-4db5-a7d2-92993d79a003" />

2.	執行下列指令掃描靶機開啟的Port，及顯示可能漏洞，由掃描結果可以得知，靶機有MS17-010漏洞，該漏洞可能會導致RCE攻擊。
nmap -A -p- --min-rate=1000 -T4 --script vuln 10.10.10.40
 
<img width="619" height="392" alt="image" src="https://github.com/user-attachments/assets/f46dd2de-6401-4e49-b922-73994f4214ab" />

3.	透過下列指令檢查靶機開放的SMB分享，可以使用nmap或smbclient確認。
 1.	nmap --script smb-enum-shares.nse -p445 10.10.10.40

<img width="625" height="489" alt="image" src="https://github.com/user-attachments/assets/ae2dd635-3ea2-43ce-a4ff-ce53d9bbc56d" />
<img width="529" height="222" alt="image" src="https://github.com/user-attachments/assets/6974444d-0bf4-49f4-b5a5-4f1efd013f43" />
  
 2.	smbclient -L 10.10.10.40

 <img width="659" height="221" alt="image" src="https://github.com/user-attachments/assets/61526d26-7fd5-4192-bed9-4fcbb1464162" />

4.	開始利用漏洞入侵，先介紹最簡單的一種利用metasploit來入侵。先執行msfconsole。

 <img width="595" height="391" alt="image" src="https://github.com/user-attachments/assets/a1947b21-7820-4b6e-8e45-7e95e47f2bae" />

5.	先利用 auxiliary/scanner/smb/smb_version模組，來確認靶機的相關資訊。

 <img width="616" height="268" alt="image" src="https://github.com/user-attachments/assets/812ccf89-5e9f-4266-be37-13810b3c7721" />

6.	搜尋有關ms-17-010相關模組資訊
search ms17-010

 <img width="654" height="188" alt="image" src="https://github.com/user-attachments/assets/f3324b38-bceb-4927-ab50-62e5a4e8cc7f" />

7.	利用exploit/windows/smb/ms17_010_eternalblue模組來入侵，先設定使用參數，然後檢查靶機是否符合入侵條件。
 
<img width="663" height="170" alt="image" src="https://github.com/user-attachments/assets/6d671ddb-4710-4d8c-9843-fcba863c884e" />

8.	執行模組，出現WIN代表順利入侵成功。
 
 <img width="673" height="309" alt="image" src="https://github.com/user-attachments/assets/8d078c94-5ceb-49cc-bfee-5f0425e15837" />
<img width="673" height="71" alt="image" src="https://github.com/user-attachments/assets/12404d6e-102d-41e5-abde-57ed5c563cb3" />

9.	輸入 shell進入連線畫面。
 
<img width="610" height="90" alt="image" src="https://github.com/user-attachments/assets/ec2f7052-abf9-4bd8-ac6e-0e9df9a7f78e" />

10.	確認目前帳號的權限，可以確定是最高管理者權限。
    
 <img width="235" height="62" alt="image" src="https://github.com/user-attachments/assets/a3741a4b-42a0-41cd-b8fc-3566882e273d" />

11.	進到C槽根目錄
    
 <img width="493" height="350" alt="image" src="https://github.com/user-attachments/assets/07fa91a1-da71-4b0b-b634-34a8062c8bbb" />

12.	搜尋並印出第1個Flag 【user.txt】 的 內容。

<img width="449" height="376" alt="image" src="https://github.com/user-attachments/assets/192a4ca0-543b-4b74-9e08-decec83daeeb" />

13.	搜尋並印出第2個Flag 【root.txt】 的 內容。
 
<img width="398" height="261" alt="image" src="https://github.com/user-attachments/assets/a3277d58-f328-4fb6-b751-745b73b3786e" />
