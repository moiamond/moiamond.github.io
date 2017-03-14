title: 使用 Docker 快速架設 RTMP 串流平台
comments: true
tags:
  - docker
  - streaming
date: 2016-07-04 15:04:12
categories:
---


為了在開發機器上測試**RTMP**串流，研究了一下目前可行的方案，最後選定用Nginx+RTMP module+Docker來方便的達到此需求。

過程大概就是下面四個步驟:

1. 安裝所有能編譯**Nginx**的相關套件
1. 到**Nginx**的官網抓取最新的原始碼
1. Git clone 最新的 [nginx-rtmp-module](https://github.com/arut/nginx-rtmp-module.git) 
1. 開始編譯**nginx**原始碼 (要打開打開**nginx-rtmp-module**編譯選項)

為了之後可以很方便的無腦啟動，因此整理成了`Dockerfile`，詳細的步驟可以[在此](https://github.com/moiamond/dockerfiles/tree/master/nginx-rtmp)找到

只要執行
```
./build.sh
```
就會啟動一個名為`streamserver`﹑支援RTMP協定的伺服器

查看RTMP的統計資料
```
http://docker:8800/stat
```

串流位址
```
rtmp://docker:1935/live/mystream
```