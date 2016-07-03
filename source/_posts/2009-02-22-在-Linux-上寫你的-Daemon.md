title: 在 Linux 上寫你的 Daemon
date: 2002-02-22 00:53:08
comments: true
categories: programming
tags: [deamon]
---

## Linux deamon

最近研究了一下如何寫`Linux`的`daemon`(相當於`Windows`的 service)。首先找到了[Linux Daemon Writing HOWTO](http://www.netzmafia.de/skripten/unix/linux-daemon-howto.html)。這篇文章寫的相當的清楚，基本上只要依照以下的原則就可以寫出基本的骨架。

- 呼叫 `Fork()` 並且讓父程序結束
- 改變`umask`為`0`
- 建立log
- 呼叫`setsid()`建立一個新的作業階段
- 改變目前的`工作路徑`到一個安全的地方，也就是說永遠都會存在的路徑。例如`根目錄`就是一個很好的選擇
- 關閉沒有必要的`檔案描述詞`
- 你這隻 daemon 所要做的事

另外要寫`daemon`有一點一定要注意，那就是盡可能使用防禦式的寫法來寫程式碼。因為`daemon`不像一般的程式，如果當掉了，是很難去追原因的。所以錯誤檢查要做好，並且一定要養成輸出`log`的習慣。

## 用 Python 來實作

如果你會寫`Python`，那麼也許你可以參考[A simple unix/linux daemon in Python](http://www.jejik.com/articles/2007/02/a_simple_unix_linux_daemon_in_python/)的作法。如果你仔細看一下他的程式碼，你會發現他的 _Daemon_ 這個 _class_ 也是依照上面的原則寫出來的。

有了一個`daemon`骨架之後，接下來就看有什麼需求。譬如，如果想監控某個目錄下，檔案的新增，刪減或修改，可以搭配 [inotify](http://www.ibm.com/developerworks/linux/library/l-ubuntu-inotify/index.html)。Python 也有相對應的模組 [pyinotify](http://pyinotify.sourceforge.net/)並將所監控的訊息紀錄在一個檔案。
