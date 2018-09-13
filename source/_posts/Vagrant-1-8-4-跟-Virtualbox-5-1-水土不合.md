---
title: Vagrant 1.8.4 跟 Virtualbox 5.1 水土不合
comments: true
tags:
  - vagrant
  - virtualbox
date: 2016-07-14 23:20:06
categories:
---

跟上頭反應了好久，老闆終於丟了一台空機器給我當測試機。馬上先安裝 Ubuntu trusty LTS 作業系統，並且裝了最新版的 `Virtualbox 5.1` 和 `vagrant 1.8.4`。

結果執行 `vagrant up` 時，噴出一堆錯誤：
> If you believe you already have a provider available, make sure it
> is properly installed and configured. You can see more details about
> why a particular provider isn't working by forcing usage with
> `vagrant up --provider=PROVIDER`, which should give you a more specific
> error message for that particular provider.

真是看到鬼了，照著錯誤訊息加了 `--provider=virtualbox` 還是看到一樣的錯誤訊息。

不得已只好打開 `debug` 的 log，看看有沒有有用的資訊

```shell
 VAGRANT_LOG=debug vagrant up --provider=virtualbox
```

在眾多的訊息中，終於發現關鍵的資訊，原來`1.8.4`只支援到`5.0`
> Vagrant has detected that you have a version of VirtualBox installed
> that is not supported by this version of Vagrant. Please install one of
> the supported versions listed below to use Vagrant:
>
> 4.0, 4.1, 4.2, 4.3, 5.0

後來去爬了`vagrant`的 [issues](https://github.com/mitchellh/vagrant/issues)，此問題 [#7411](https://github.com/mitchellh/vagrant/issues/7411) 已經有不少討論了。
也剛好有人[PR](https://github.com/mitchellh/vagrant/pull/7574/files)了解法。這個問題預計會修在`1.8.5`，不過也不知道什麼時候才會出。我又不想裝回舊的`virtualbox`。索性照著此[PR](https://github.com/mitchellh/vagrant/pull/7574/files)的改法，修改`vagrant`資料夾下的檔案。

問題就順利解決了。