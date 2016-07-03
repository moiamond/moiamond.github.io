title: Docker架設Trac Wiki(題目暫定)
comments: true
date: 2015-09-07 22:27:20
categories:
tags:
- Docker
- trac

---

## 前言

使用 Docker 也一段時間了，balah balah ...



## Trac

### Install Trac on Ubuntu trusty

```shell
$ apt-get install trac
$ easy_install --upgrade Trac==1.0.8
```

### 寫成`Dockerfile`
```
FROM ubuntu:trusty

RUN apt-get update && apt-get install -y \
    trac \
    && rm -rf /var/lib/apt/list/*

RUN easy_install --upgrade Trac==1.0.8
```

## Trac 基本操作

### 開新專案
```
$ mkdir repo
$ trac-admin ./repo initenv
Creating a new Trac environment at /root/repo

Trac will first ask a few questions about your environment
in order to initialize and prepare the project database.

 Please enter the name of your project.
 This name will be used in page titles and descriptions.

Project Name [My Project]>

 Please specify the connection string for the database to use.
 By default, a local SQLite database is created in the environment
 directory. It is also possible to use an already existing
 PostgreSQL database (check the Trac documentation for the exact
 connection string syntax).

Database connection string [sqlite:db/trac.db]>
```

### 使用 tracd 測試剛剛建立的專案
```
$ tracd -p 8080 ./repo
```
