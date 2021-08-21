---
title: 实用100条命令行
tags:
---





 记录一下各种常用的命令行以拯救一下自己日渐退化的记忆力。

## 编码器相关

`ffmpeg -i abc.mp4 -c:v copy -c:a copy output.mp4 `

`mp4box -add 123.mp4#trackid=01 -add 123.m4a -new 1234.mp4`

`x264 --demuxer y4m --preset slower --tune film --crf 17.0 --deblock -1:-1 --keyint 600 --min-keyint 1 --open-gop --b-adapt 2 --bframes 8 --ref 13 --qpmax 36 --qcomp 0.75 --rc-lookahead 75 --aq-strength 1.2 --merange 24 --me tesa --subme 10 --no-fast-pskip --colormatrix bt709 --input-depth 16 --output "" -`

## 证书相关

制作pkcs12格式的证书文件：

`.\openssl.exe pkcs12 -export -out cert.p12 -inkey cert.pem -in cert.pem -passout pass:xxxxxxxxxxxx`