---
title: 折腾记-0
tags:
  - Everything
  - Programming
date: 2018-01-30 14:13:57
---


为了把 RaidFinder 给实现出来，我决定直接自己手动实现一个 Filtered Stream API 的调用，来检查到底是需要什么 Track 关键词才能达到目的。

>  这里真的要吐槽一下新的 Twitter Developer 页面，以前大量的 API Reference 页面都已经404了，包括一些非常重要有用的 API  文档都能 404, 再看看那个“切换到夜间模式再切换回来之后主题色丢失”的前端 bug 修了几个月才修完，可以说 Twitter 真的药丸。

## 构造 OAuth 验证

查阅了文档得知，Stream API 需要进行在请求的 HTTP Header 中包含完整的用户验证，也就是走完整的 OAuth 验证流程，由于手上已经拥有了完整的用户和应用的 Key : Secret 对，所以直接进行构造。

一个完整的验证字段中必须包括如下内容：

1. Consumer Key

2. Nonce

3. Signature

4. Signature Method

   Twitter 只支持 HAMC-SHA1 的签名方法

5. Timestamp

6. Access Token

7. OAuth Version

而 Signature 字段又是根据其他6个字段以及 HTTP 请求的 URL 和参数（包括 POST 方法的 Request Body）来产生的，格式如下：

`请求方法&请求URL&其他6个字段&请求参数`

HMAC-SHA1 签名的产生还需要一个 Key, 这个 Key 是由用户的 Consumer Secret 和 Access Token Secret 来构建的。

## 抓包

写了半个下午的 OAuth, 写完运行之后还是返回一个 401 给我，仔细检查代码也没有发现哪里出错了。没有办法之下，我想到了抓原版的请求包来看它到底用了什么关键词。于是立即安装 [sbt](http://www.scala-sbt.org/index.html) 和 JDK. 其实 Windows 上的 sbt 也就是提供一个 jar 包和 sbt.bat 这个运行脚本来设定 sbt 所需要的运行环境而已。

安装完了，`sbt run` 先跑一记看看，然后让我没想到的是，在性能比 VPS 强了几倍的情况下，这玩意儿还是跑的稀慢，也可能是 Windows 的问题，总之，构建初始运行环境就花了大概一个半小时，不容易等到开始编译 scala 资源了，开始疯狂报错了。

首先是一个 Python 脚本报错导致后续脚本无法继续，查询项目文档后得知 Windows 上需要 Python 2.X, 而我安装的是 3.6, 没想到在这里会第一次碰到 Python 的版本问题，没办法，下 2.7 改 Path, 好通过了，然后继续报错。

之后报错都是因为 Java 运行环境的限制，主要问题出在内存的设定上，Google 之，然后在 Java 控制面板中添加全局参数，`sbt run` 之后开任务管理器看到参数起效了，遂继续运行下去，然而还是会报内存不够的错误，不解，好在每次都能按部就班的往下跑一点，几次重启之后也搞定了。把 twitter4j.properties 放到根目录下，`sbt run` 之后发现貌似请求没走代理，Twitter 请求撞墙超时。这时想到用 Proxifier 将 java.exe 的请求重定向到本地代理去，重定向之后就跑起来了，程序启动的一瞬间有两个 Search API 的请求和一个 Stream API 的请求，然后全是访问 pic.twitter.com 的请求，下图片的。

既然 Proxifier 能够重定向流量，那么我把 java.exe 的请求重定向到 Fiddler 的端口去不就能看到包内容了。但是切过去却发现，所有请求都不通了。期初以为是 Fiddler 没走代理，根据别人的经验添加了转发规则之后仍然出错，百思不得其解中，突然看到程序的出错信息不再是访问 Twitter 超时了，而是 SSL 证书出错。

怎么会呢？我明明把 Fiddler 的证书添加到系统中去了。Google 之，发现 Windows 上的 JRE 自己实现了一套证书环境，我去你大爷的垃圾 Oracle, 放着系统自己的轮子不用非要用自己那套垃圾轮子给人麻烦，前面的网络请求也是自己造的轮子直接跳过系统代理。

没办法，寄人篱下。于是 Google 到添加 SSL 证书的办法，添加完之后发现还需要自定义的命令行让 JRE 读证书，同样使用 Java 控制面板添加参数，然而这次发现参数并没有起效。

> 切记 keytool 这玩意儿要用管理员身份来跑，添加完之后 keytool 会生成一个 keystore 文件，建议将它移动在自己 Home 文件夹中并重命名为 .keystore

既然 \$JAVA_OPTS 这个环境变量看上去对 sbt 没用，那我改 $SBT_OPTS, 利用 Everything 这玩意儿强大的搜索功能找到了存在于 `sbt 安装目录\config` 下面的 sbtopts 文件，好就是你了，添加自定义命令行，`sbt run`, 然并卵，一点作用都没起。一头雾水中我打开 sbt.bat 这个 Windows 版 sbt 的实际起效文件，发现了这行：

```shell
set FN=%SBT_HOME%\..\conf\sbtconfig.txt
```

<del>我去你妈的，也是个不喜欢用一套轮子的。</del>

把原本写进 sbtopts 文件的自定义命令行改写到 sbtconfig.txt 中，Java 命令行的前缀 `-J` 并不用添加，最后如下：

``` batch
-DproxySet=true 
-DproxyHost=127.0.0.1
-DproxyPort=8888   'Fiddler 的代理端口
-Djavax.net.ssl.trustStore=<path\to\FiddlerKeystore> '刚刚生成的 keystore 文件的位置，如果移动到了 $Home 下面并重命名为 .keystore 的话这行就不用加
-Djavax.net.ssl.trustStorePassword=<Keystore Password> '刚刚 keytool 让你敲的密码
```

`sbt run`

终于，我在 Fiddler 中看到了 java.exe 的请求，并且正确地解密了出来。我花了半天时间探寻的内容终于呈现在我眼前：

`count=0&track=%E5%8F%82%E5%8A%A0%E8%80%85%E5%8B%9F%E9%9B%86%EF%BC%81%2C%3A%E5%8F%82%E6%88%A6ID%2CI%20need%20backup%21%2C%3ABattle%20ID&stall_warnings=true`

URL Decode 之后就是：

`count=0&track=参加者募集！,:参戦ID,I need backup!,:Battle ID&stall_warnings=true`

我看到这个之后差点吐血，原来他妈就是简单的用`,` 这玩意儿来分隔开 Track 关键词就能实现日语推的实时 filter???

还要继续实验。

## 外网访问

前文有讲，家里网络“重做”之后我就把主路由的 DMZ 主机挂在二级路由上，二级路由的 DMZ 挂在自己机器上，这样外网的请求就直接能到自己机器上。

然而从外部使用 DDNS 域名+端口号的方式不能直接访问到 RaidFinder. 哦好像是要配上一个 HTTP Server, 那简单，跑一个 http.server 80 不就行了。

是行了，能访问到所有的静态资源，但是看着 Console 中不断提示的 `ws/raids?keepAlive=true` 404, 我突然想起来前不久看到的 WebSockets 简介中提到的，要建立一个 WebSockets 连接是需要从 HTTP 请求“转发升级”过去的。于是想到用 NGINX, 下载，这玩意儿才 1MB+ 的大小，真他妈绿色环保。

把自己服务器上的 nginx.conf 拖出来抄配置，终于理解了之前搜到的配置的用处：

```ini
proxy_pass http://127.0.0.1:$port;
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
```

配置中最后两句就是为 WebSockets 而服务的，是一个 HTTP 请求转为 WebSockets 连接的关键点。

那么一切设定好，开始跑，问题又来了：外网无法访问，内网 80 转发正常。

## >> endl;

最后这篇折腾记的终点就在这里了，不知道为何，访问就是无限转圈，但是起初寻找关键词的目的还是完成了。