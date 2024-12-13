##### tags: `is1ab-note`
# is1ab Hackthebox筆記 

## <span class="red">**baby nginxatsu (Easy)**</span>

* <span class="blue">**Nginx（發音同「engine X」）**</span>

* <span class="blue">**本題出自HACK THE BOX，難易度屬於EASY這個類別**</span>

### <span class="red">**解題過程**</span>

1. **按照題目給的靶機的URL，將其複製後於分頁開啟，會發現一個Login介面(如下圖)。首先考慮看看能不能用一些簡單且常見的<span class="light_purple">++SQL Injection++</span>成功登入(<span class="light_purple">像是：' OR 1=1 - -</span>)**

* ![image](https://hackmd.io/_uploads/SymHWjBXke.png)
* ![image](https://hackmd.io/_uploads/rk0TMsSXyg.png)
* <span class="blue">**上圖的密碼隨便輸入**</span>

&emsp;

2. **然後就會發現網站跳出「Email位址必須是合法(合理的)」，出題者有可能<span class="light_purple">對User Input進行了過濾</span>。我不再嘗試SQL Injection，因為我發現了「Create a new account」按鈕，於是點按鈕進入Sign Up頁面**

* ![image](https://hackmd.io/_uploads/ByE3Qjr7yl.png)

&emsp;

3. **進入Register後我隨便建立了一個簡單的帳號(點擊Register)，並使用該帳號進行登錄**

* ![image](https://hackmd.io/_uploads/SkZ7UjSXJe.png)
* ![image](https://hackmd.io/_uploads/Bkyzvsrmyl.png)

&emsp;

4. **成功登入後會發現，新的頁面讓你可以自訂自己的config並且建立。在這裡我的處理方式是先按照網頁上有的功能先進行探索，於是我按照他的預設值直接建立了一個新的config**

* ![image](https://hackmd.io/_uploads/H1OhOirQke.png)

&emsp;

5. **點擊按鈕後會產生一個config，並且這個config是可以點擊的，於是我就點擊看看。到新的頁面後首先發現的是「Look at your own nginx config file」這句話，於是我開始觀察自己的config有沒有線索**

* ![image](https://hackmd.io/_uploads/H19FYjBXJe.png)
* ![image](https://hackmd.io/_uploads/ryvd5jBQ1x.png)

&emsp;

6. **然後他就在定義server這裡給出提示訊息，註解的部分基本上表明了重要資訊可能會藏在/storage底下，於是我直接以URL搜尋的方式進入/storage底下進行查看**

* ![image](https://hackmd.io/_uploads/BJQG3srmyx.png)

* **後來我查過<span class="purple">「autoindex=on」</span>的作用，打開的話會使Server端在使用者輸入一個目錄的網址並訪問後，<span class="blue">++直接將網址底下所有檔案和子目錄的名稱列出於網頁上++**</span>

&emsp;

7. **根據剛剛獲得的資訊，階段的首要目標是進入<span class="light_purple">/storage</span>，於是我修改了一下URL，將原本的<span class="light_purple">/config/51</span>更改成<span class="light_purple">/storage</span>。儘管我已經在輸入完URL確認我有輸入port，但仍然無法連上這個網站，於是我不再把注意力放在URL的部分**

* ![image](https://hackmd.io/_uploads/HJm3G2BXkx.png)
* ![image](https://hackmd.io/_uploads/Syz8Q2r7ke.png)
* ![image](https://hackmd.io/_uploads/SyfBXnH71x.png)

&emsp;

8. **這時候我嘗試了很多方法，像是：**
    (1) **使用<span class="light_purple">DevTools</span>查看<span class="light_purple">Cookie</span>的訊息**

* ![image](https://hackmd.io/_uploads/H18USnBQkg.png)
* ![image](https://hackmd.io/_uploads/rkKs5nrQke.png)
* **這個方法沒辦法找到能夠利用的資訊，<span class="blue">將這個value在線上用<span class="light_purple">++base64解碼++</span>後會發現在沒有金鑰的情況下是無法將解碼後的值**</span>

    (2) **使用<span class="light_purple">Burp Suite</span>攔截<span class="light_purple">封包</span>進行進一步的觀察**

* ![image](https://hackmd.io/_uploads/rk0YrRB7yg.png)

&emsp;

9. **經過很久，當我再次確認URL時才發現，port會在第一次訪問時被吃掉(這部分會放在問題與討論)，當我再次輸入port並按下Enter後，就能夠發現Index of /storage的頁面。仔細查看該頁面會發現一個名稱是「db備份」的gz檔，於是就點擊把gz檔下載下來**

* ![image](https://hackmd.io/_uploads/BkNhI0HQkl.png)
* ![image](https://hackmd.io/_uploads/SkWjJkI71g.png)
* ![image](https://hackmd.io/_uploads/S1upDCBm1x.png)
* ![image](https://hackmd.io/_uploads/HJ5AvRrmkl.png)

&emsp;

10. **下載下來後將檔案解壓縮會發現一個.sqlite的檔案，這時找一個線上能夠跑.sqlite的DB環境跑跑看(e.g. https://sqliteonline.com/)，將剛剛下載的檔案打開，看到User的Table後點開，發現有password，這時就可以用SQL語法去查看User這個Table裡面有沒有資料**

* ![image](https://hackmd.io/_uploads/rJluY0SQyg.png)

&emsp;

11. **如果有資料，馬上從第一個開始檢查User，看看admin會不會把資訊存在DB裡，結果第一個就很像admin了(從adm懷疑)，這時馬上回到原本的網頁試試看能不能成功登入**

* ![image](https://hackmd.io/_uploads/SkPz90SXJl.png)
* ![image](https://hackmd.io/_uploads/rk7U9Crmyl.png)

&emsp;

12. **然後你會發現，失敗了。但是在經過一段時間思考後，想起了<span class="red">++密碼通常不會以明文的方式存放在DB之中++</span>，這時候就可以使用一些線上解密軟體，解密完發現密碼看起來是有意義的，於是再去登錄試試看**

* ![image](https://hackmd.io/_uploads/Byr_j0rX1l.png)
* ![image](https://hackmd.io/_uploads/rJQYsCrQJx.png)
* ![image](https://hackmd.io/_uploads/rk4zT0BX1e.png)

&emsp;

13. **然後就大功告成!!!**

* ![image](https://hackmd.io/_uploads/SyC26ASXJg.png)

&emsp;

### <span class="red">**過程中一些觀察心得**</span>

1. **path會變 => 不要在會變動的path name後面進行path traversal**

2. **有cookie考慮用BurpSuite去攔截，查看資訊**

3. **題目的~~密碼加密~~可以先考慮使用MD5(以這題來說)的~~演算法加密~~**
    
    - <span class="red">**更新2</span>：<span class="red">並非加密</span>，密碼系統至少要<span class="light_purple">明文</span>、<span class="light_purple">密文</span>、<span class="light_purple">金鑰</span><span class="red">任兩種</span>才能夠破解**
    
    - **<span class="red">更新2</span>：<span class="light_purple">MD5</span> 是「<span class="blue">雜湊(hash)</span>」，雜湊可能會是 <span class="light_purple">SHA</span> 系列或是 <span class="light_purple">MD5</span> 系列，而題目在想讓我們發現明文的情境下，很大機率使用的是 <span class="light_purple">MD5</span> (因為已經被破解，認證為不安全)**
    
    - **<span class="red">更新2</span>：尤其這題的字串是32長度(<span class="blue">32位元</span>)，那這種長度可能會是**
        * **key 的長度**
        * **hash 函數**
    
    - **<span class="red">更新2</span>：雜湊是丟任何長度的東西，都會產生<span class="red">一樣長度</span>的 value ，就像「很混亂的大雜燴火鍋」。同時，<span class="red">雜湊是單向的，正常無法破解(不可逆)</span>，而 <span class="light_purple">MD5</span> 因為時代的演進已經不安全了， <span class="light_purple">SHA128</span> 同樣也是，好的 hash 想要破解是不容易的(像是 <span class="light_purple">SHA512</span> )**
    

&emsp;

### <span class="red">**結論和問題與討論**</span>

* **如果可以登入網站就先登入，不要先以攻擊為方向。先登入一般使用者或許也有機會能夠從內部漏洞直接提權**

* **目前不知道<span class="red">為何第一次以URL的方式搜尋<span class="light_purple">/storage</span>會被擋下來?</span>**
    - <span class="red">**更新1</span>：目前可能的原因是當我第一次在做URL的訪問時，他會<span class="blue">進行redirect</span>把我導向port80(或是任一個非原本port的網站)，作為一種簡單的防禦機制**

* **Index of讓我想到<span class="purple">Google Hacking Database</span>，在<span class="purple">Google Hacking Database</span>上有提供很多Index of有關的Exploit(像是直接在Google下 <span class="light_purple">Index of /etc/passwd</span> 就會有網站的一些跟密碼有關資訊被顯示出來)。現在看來其中一項因素應該跟autoindex設定成on有關**
