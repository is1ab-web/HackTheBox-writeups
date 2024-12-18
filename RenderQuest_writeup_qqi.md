# RenderQuest

Solution
---

題目：https://app.hackthebox.com/challenges/537

![2](https://hackmd.io/_uploads/ByS3nHrO0.png)

先看看有什麼檔案，發現一個 txt 檔
![3](https://hackmd.io/_uploads/SJznaBH_C.png)

貼貼看這個 flag ，結果顯示 error
![4](https://hackmd.io/_uploads/SJRWRHru0.png)
 
看到 main.go 檔
![5](https://hackmd.io/_uploads/S1HuCBHOC.png)

基本上包含所有請求參數，當訪問給定的 ip 和 port 時，將能夠看到一個帶有接受輸入參數的網頁

所以我們可以嘗試在這裡使用 webhook ，因為它允許輸入外部的 link

```
func (p RequestData) FetchServerInfo(command string) string {
	out, err := exec.Command("sh", "-c", command).Output()
	if err != nil {
		return ""
	}
	return string(out)
}
```
*FetchServerInfo* 負責執行指定給他的 shell 命令並返回其輸出。

如果在命令執行過程中觸發錯誤，它將會返回一個空字串。



結論就是命令執行、錯誤檢查和結果回傳。

使用 [https://webhook.site](https://webhook.site) 工具新增一個客製化請求，讓我們可以呼叫 main.go 檔案中的 funciton ( FetchServerInfo )

![a](https://hackmd.io/_uploads/Hyt2L8rdC.png)

修改Content內容，試試 `"ls"` 列出檔案和目錄
![6](https://hackmd.io/_uploads/rkn3H8r_C.png)

複製生成的 url

![dashjsdakhj](https://hackmd.io/_uploads/r1gSW3oOA.png)

回到題目，並貼在欄位內

![bb](https://hackmd.io/_uploads/r1tzcLS_C.png)

有成功列出檔案和目錄

![7](https://hackmd.io/_uploads/BksKm8BO0.png)

看上一層目錄有甚麼什麼檔案
![8](https://hackmd.io/_uploads/H1jwSLSdC.png)
看到 flag57d0a278a2.txt 超可疑
![9](https://hackmd.io/_uploads/ry1EV8S_A.png)
用 `cat` 查看 flag57d0a278a2.txt 內容
![10](https://hackmd.io/_uploads/r1eWH8H_0.png =80%x)

得到 Flag 了
![flag](https://hackmd.io/_uploads/ryGq-bB_C.png)

![未命名.pngowned](https://hackmd.io/_uploads/ryLPwnsdR.png)

Vulnerability causes
---

```
func (p RequestData) FetchServerInfo(command string) string {
	out, err := exec.Command("sh", "-c", command).Output()
	if err != nil {
		return ""
	}
	return string(out)
}
```
> command 沒有經過驗證或參數化，所有指令會直接透過 shell 來執行
* 依照上面 writeup：
在 shell 中， payload 會被解讀成三個指令:
1. `cd ..`
2. `ls -la`
3. `cat flag210d2d1c90.txt`

	SSTI 
	---
	SSTI，全稱 Server Side Template Injection 伺服器模板注入，這是一種將惡意內容注入到 Web Server 執行命令的漏洞，可藉由 SSTI 導致 RCE 或讀取 Server 上的敏感資訊，因此這種漏洞通常很嚴重。
https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#ssti-in-go
	
	https://xz.aliyun.com/t/12642?time__1311=GqGxuDRiA%3DDse5YK0%3DYO2DAE4RhA%3DoD#toc-11

Patch
---

參數化後，使用正規表達式:

```
func (p RequestData) FetchServerInfo(command string) string {
	validCommand := egexp.MustCompile(`^[a-zA-Z0-9 .,?!-]+$`)
	if !validCommand.MatchString(command) {
		return "Invalid command"
	}
	out, err := exec.Command("sh", "-c", command).Output()
	if err != nil {
		return ""
	}
	return string(out)
}

```

* ^ [a-zA-Z0-9 .,?!-]+$：這是正則表達式，它定義了比對規則：
    * ^：符合字串的開頭
    * [a-zA-Z0-9 .,?!-]：表示允許的字元範圍，包括：
        * a-z：小寫字母
        * A-Z：大寫字母
        * 0-9：數字
        *  ：空格
        * . , ? ! -：基本標點符號
    * +：表示前面的字元可以重復一次或多次。
    * $：比對字串的結尾。



---
參考資料:
https://medium.com/@tanish.saxena26/hackthebox-renderquest-cf6c493d7b83
https://www.youtube.com/watch?v=Xhp34yb3Ybk
https://motasem-notes.net/domain-redirection-bypass-explained-hackthebox-renderquest-proxyasaservice/
https://ithelp.ithome.com.tw/m/articles/10272749
