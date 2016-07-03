layout: post
title: "解決 Plex Media Server 讀不到 Aperture 的問題"
date: 2014-08-03 23:02:25 +0800
comments: true
categories: 雜七雜八
tags:
- plex
- aperture
---

{% asset_img title.png [Plex Channels] %}

## 前言

使用 [Plex Media Server](https://plex.tv) 一陣子了，主要是拿來當家裡的影音中心。
因為我使用 **Aperture** 來管理所有的照片，因此 [Plex Media Server](https://plex.tv) 透過 **Aperture** 來分享照片。

但是常常會發生 [Plex Media Server](https://plex.tv) 讀不到 **Aperture** 的情形。
使用上就非常的不方便，常常要重新開啟 [Plex Media Server](https://plex.tv) 讓一切正常。
後來發現只要我在 **Aperture** 裡面做一些操作之後，[Plex Media Server](https://plex.tv) 就讀不到 **Aperture** 了。

原來 [Plex Media Server](https://plex.tv) 是透過 Aperture Library XML (ApertureData.xml) 來讀取 **Aperture** 的資料。
只要這個檔案有改變，[Plex Media Server](https://plex.tv) 就讀不到了。

爬了一下網路的文章，滿多人都有遇到這個問題，解決方法不外乎就是重開 [Plex Media Server](https://plex.tv) XD
既然沒有好的解決方法，只好就此症狀對症下藥了。

想法非常的簡單，就是自動偵測 ApertureData.xml 有沒有更新，如果有就重新開啟 [Plex Media Server](https://plex.tv)。

在 Mac OS 下，其實可以透過內建的 **launchd** 做到這些事。

## 進入正題

這邊我分成兩個部分：

* 如果 ApertureData.xml 有更新則砍掉 [Plex Media Server](https://plex.tv) 這隻程式
* [Plex Media Server](https://plex.tv) 要一直保持啟動。


`lauchd` 會載入存放在 `~/Library/LaunchAgents/` 的 `plist`

接下來詳細的說明各個 `plist` 的內容：

### 監控 `ApertureData.xml`
> 如果 ApertureData.xml 有更新，就執行會砍掉 Plex Media Server 這隻程式的 script

新增 `com.plex.killme.plist` 內容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC -//Apple Computer//DTD PLIST 1.0//EN
http://www.apple.com/DTDs/PropertyList-1.0.dtd>
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.plex.killme</string>
     <key>ProgramArguments</key>
         <array>
             <string>/Users/userA/script/kill-plex</string>
             <string>restart</string>
         </array>
    <key>WatchPaths</key>
    <array>
        <string>/Users/userA/Pictures/Aperture Library.aplibrary/ApertureData.xml</string>
    </array>
</dict>
</plist>
```

新增 kill-plex.sh 內容如下：

```
#!/bin/bash
sleep 60
killall "Plex Media Server”
```

當所監控的檔案有更新的時候，`launchd` 就會去執行 `kill-plex.sh`。
`kill-plex.sh` 一開始會先等待60秒之後再砍掉 [Plex Media Server](https://plex.tv)。
為什麼要先等待60秒？其實這是實驗結果導向。
因為 `launchd` 會一直保持 [Plex Media Server](https://plex.tv) 在執行的狀態，
只要一發現 [Plex Media Server](https://plex.tv) 沒有在執行，就會馬上重新啟動它。
如果在 ApertureData.xml 正在更新的當下，馬上砍掉 [Plex Media Server](https://plex.tv) 再重新啟動，
常常也會發生看不到 **Aperture** 的情況。
因此就先等60秒，看起來一切就正常了。

### Keep alive
> [Plex Media Server](https://plex.tv) 要一直保持啟動

新增 `com.plex.keepalive.plist` 內容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
     <key>Label</key>
     <string>com.plex.keepalive</string>
     <key>Program</key>
     <string>/Applications/Plex Media Server.app/Contents/MacOS/Plex Media Server</string>
     <key>KeepAlive</key>
     <true/>
</dict>
</plist>
```

### 準備就緒

當新增這兩個 `plist` 到 `~/Library/LaunchAgents/` 裡，如何馬上啟用？
可以執行以下的指令：

```
launchctl load ~/Library/LaunchAgents/com.plex.killme.plist
launchctl load ~/Library/LaunchAgents/com.plex.keepalive.plist
```

以後不管 **Aperture** 做了什麼操作，我不用再手動重新啟動 [Plex Media Server](https://plex.tv) 了。
