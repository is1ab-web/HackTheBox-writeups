# Neonify

Solution
---

題目：https://app.hackthebox.com/challenges/303

一進去就會看到以下畫面
![1](https://hackmd.io/_uploads/SJnEEt_oC.png)

嘗試輸入一些東西
![2](https://hackmd.io/_uploads/ry07IYdsC.png)
直接 print 在下面

嘗試輸入特殊符號
![3](https://hackmd.io/_uploads/H107LtujR.png)
> 顯示 Malicious Input Detected

看一下他給的 code
![4](https://hackmd.io/_uploads/SkSQU5OsA.png)

先看一下 neon.rb
```ruby=
class NeonControllers < Sinatra::Base

  configure do
    set :views, "app/views"
    set :public_dir, "public"
  end

  get '/' do
    @neon = "Glow With The Flow"
    erb :'index'
  end

  post '/' do
    if params[:neon] =~ /^[0-9a-z ]+$/i
      @neon = ERB.new(params[:neon]).result(binding)
    else
      @neon = "Malicious Input Detected"
    end
    erb :'index'
  end

end
```

* `if params[:neon] =~ /^[0-9a-z ]+$/i` 這邊檢查使用者輸入他是否只包含數字、字母（不區分大小寫）和空格。
![5](https://hackmd.io/_uploads/SywJXs_sA.png)https://stackoverflow.com/questions/577653/difference-between-a-z-and-in-ruby-regular-expressions
**!! 發現這裡有可以換行繞過的問題 !!**

* `erb :'index'` 找 views 目錄下的 index.erb 文件，使用 ERB 模板處理。![6未命名](https://hackmd.io/_uploads/B1DPS3di0.png =50%x)


* `@neon = ERB.new(params[:neon]).result(binding)` 這部分建立一個新的 ERB 模板，其中 `params[:neon]` 是用戶輸入的內容，並把他當作ERB 模板來處理。 `.result(binding)` 是渲染這個模板的一個方法，`@neon`的值會插入到 HTML 中，生成最終的頁面內容。


以下是 index.erb
```html=
<!DOCTYPE html>
<html>
<head>
    <title>Neonify</title>
    <link rel="stylesheet" href="stylesheets/style.css">
    <link rel="icon" type="image/gif" href="/images/gem.gif">
</head>
<body>
    <div class="wrapper">
        <h1 class="title">Amazing Neonify Generator</h1>
        <form action="/" method="post">
            <p>Enter Text to Neonify</p><br>
            <input type="text" name="neon" value="">
            <input type="submit" value="Submit">
        </form>
        <h1 class="glow"><%= @neon %></h1>
    </div>
</body>
</html>

```

**!! 可執行 ruby SSTI 模版語法 !!**

查 Ruby SSTI Payload

https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#erb-ruby
![7](https://hackmd.io/_uploads/rJEfkTOsA.png)

使用 Burp Suite - Decoder 功能
![8](https://hackmd.io/_uploads/Hkquxpdi0.png)

並沒有顯示出flag
![10](https://hackmd.io/_uploads/Hymi-Tdo0.png)
因為這樣無法通過正規表示式的條件。（只包含數字、字母（不區分大小寫）和空格）

在前面加上隨機字母
![9](https://hackmd.io/_uploads/BJ3LW6_i0.png)
得到 flag 了 !

Vulnerability causes
---
1. ruby 過濾字串保護機制寫法可以被換行方法繞過
2. 可執行 ruby SSTI 模版語法

    SSTI 
    ---
    SSTI，全稱 Server Side Template Injection 伺服器模板注入，這是一種將惡意內容注入到 Web Server 執行命令的漏洞，可藉由 SSTI 導致 RCE 或讀取 Server 上的敏感資訊，因此這種漏洞通常很嚴重。
    
Patch
---
1. 檢測網站的分隔符號與換行能否繞過字串過濾檢查
2. 檢測所有網站模版是否能執行外部參數

簡介 ERB 語法
---
只執行 Ruby code 但不輸出結果時使用：
```
<% Ruby code... %>
```

執行 Ruby code 且在 HTML 中插入輸出結果時使用 `=` 符號：
```
<%= Ruby code... %>
```

要將 Ruby code 轉為註解時可以使用 `#` 符號：
```
<%# Comment... %>
```
