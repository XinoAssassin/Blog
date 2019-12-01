---
title: 用youtube-dl在直播进行中同时下载
tags:
  - Tutorial
date: 2019-08-02 00:21:00
---


基于今天花谱Live的下载失败，一怒之下又翻了翻youtube-dl的文档，发现可以在直播的同时直接截流保存成文件，用这种方法可以大大加快海盗效率。

## 准备工作

- 良好的网络环境
- 空闲的CPU资源
- [youtube-dl](https://ytdl-org.github.io/youtube-dl/index.html)
- [FFmpeg](https://ffmpeg.org/)
- 一点点命令行基础

这里建议将youtube-dl和FFmpeg所在的目录加入环境变量中，用户或者系统的均可。另外你还要搞清楚自己的本地代理端口号，一般为1080。

## 配置文件与命令

youtube-dl支持配置文件，可以免去每次手动输一长串命令的麻烦，在Windows下其默认读取的配置文件位于用户目录下的`youtube-dl.conf`，即`%userprofile%\youtube-dl.conf`。

这边准备好了两份配置文件，一份是直播同时下载，另一份是平常下载视频。两份配置文件都需要手动更改里面的代理端口号，和自己环境所匹配，其他不需要进行更改。

普通下载：

```powershell
--proxy socks5://127.0.0.1:1081
-f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best'
-o '%(uploader)s/%(title)s.%(ext)s'
--add-metadata
--write-thumbnail
--ignore-errors
--extract-audio
--audio-format best
--audio-quality 0
--keep-video
--embed-thumbnail
```
直播的同时进行下载：

```powershell
--proxy http://127.0.0.1:1081
-f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best'
```

分别将这两块命令行保存成两个文件，修改Proxy行的端口为自己使用的，建议一个命名为`youtube-dl.conf`放置在`%userprofile%`目录下，另一个换个名字也存`%userprofile%`目录下方便调用。

以上工作完成之后打开命令提示符，注意是cmd不是PowerShell，因为后者在管道操作上面有一些不同，这边是用最简单的cmd来完成。命令：

`youtube-dl -o - U2B-LINK|ffmpeg -i - -vcodec copy -acodec copy "OUTPUT.mp4"`

如果你默认的配置文件不是用于直播时同时下载的，那么请指定配置文件：

`youtube-dl -o - --config-location PATH\TO\CONFIG U2B-LINK|ffmpeg -i - -vcodec copy -acodec copy "OUTPUT.mp4"`

注意，`-o`之后的`-`和`-i`之后的`-`均不能遗漏，这是管道操作最重要的两点之一，还有一点是管道操作符`|`，跟反斜杠`\`同一个键。命令中的`U2B-LINK`就是油管链接、`PATH\TO\CONFIG`就是youtube-dl的配置文件具体位置，`OUTPUT.mp4`是输出的文件名。

`>>endl;`