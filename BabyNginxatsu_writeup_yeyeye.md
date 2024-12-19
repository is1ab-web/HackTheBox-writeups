# baby nginxatsu
## ![94.237.51.182_41871_auth_login](https://hackmd.io/_uploads/ByRMAI441l.png)

### 第1次觀察:
- 是一個登陸頁面
- 可以註冊帳號
![94.237.51.182_41871_auth_login](https://hackmd.io/_uploads/HJSGC8EN1e.png)

### 第1次嘗試
- 用sql injection打打看
![94.237.51.182_41871_auth_login (1)](https://hackmd.io/_uploads/S1JYCUE4kl.png)
失敗了
![94.237.51.182_41871_auth_login (2)](https://hackmd.io/_uploads/S1eDnCI4Nyg.png)
### 第2次觀察:
- 註冊登陸後有一個自訂nginx設定的設定檔
![94.237.51.182_41871_](https://hackmd.io/_uploads/HJV_kv4EJx.png)
- 點了generate config，並進入config檢視畫面看了一段有關/storage的註解
![image](https://hackmd.io/_uploads/H12BeDE4Je.png)
![image](https://hackmd.io/_uploads/S1Uhew4NJg.png)

### 第2次嘗試:
- 嘗試直接進入 /storage的路徑下
![image](https://hackmd.io/_uploads/HyDm2y5Xyl.png)
進去了，還找到了db的backup

- 解壓縮後看裡面有一個sqlite的檔案
![image](https://hackmd.io/_uploads/rJ-aJlcXJg.png)
- 打開後看到裡面有這些table，只有users的table有值
![image](https://hackmd.io/_uploads/rywkGgqmyg.png)
- 看起來在users存了帳號跟加密的密碼
![image](https://hackmd.io/_uploads/BJsoflq7kx.png)
- 嘗試用常見的密碼加密方式解碼，最後找到用MD5解碼
![image](https://hackmd.io/_uploads/Hkii5g97Jx.png)
成功
![image](https://hackmd.io/_uploads/Hyu3GwNEye.png)



### 總結:
- 把私密資訊或註解顯示在前端
	:arrow_right_hook: **information leak**
- 透過更改url來存取檔案儲存地址
    :arrow_right_hook: **path traversal**
