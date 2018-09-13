---
title: Doxygen 初體驗
date: 2008-04-26 21:38:52
comments: true
categories: programming
tags: [doxygen, python]
---

[Doxygen](http://www.stack.nl/~dimitri/doxygen/index.html)是產生程式碼說明文件的工具，支援的語言也非常多。使用的方式也很簡單，在 Windows 系統下首先安裝它的[工具程式](http://ftp.stack.nl/pub/users/dimitri/doxygen-1.5.5-setup.exe)。
接下來最重要的部份則是，你要如何告訴 doxygen 一些規則，好讓它能夠自動幫你產生文件。

以下是 [doxygen 說明文件的一個範例](http://www.stack.nl/~dimitri/doxygen/docblocks.html#pythonblocks):

```python
## @package pyexample
#  Documentation for this module.
#
#  More details.

## Documentation for a function.
#
#  More details.
def func():
    pass

## Documentation for a class.
#
#  More details.
class PyClass:

    ## The constructor.
    def __init__(self):
        self._memVar = 0;

    ## Documentation for a method.
    #  @param self The object pointer.
    def PyMethod(self):
        pass

    ## A class variable.
    classVar = 0;

    ## @var _memVar
    #  a member variable
```

最後再用[Doxywizard](http://www.stack.nl/~dimitri/doxygen/doxywizard_usage.html)就可以很簡單的產生說明文件了。
