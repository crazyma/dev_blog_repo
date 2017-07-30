---
title: 在 Gitlab 上建立 Android CI - (1) 設定 Runner
date: 2017-06-26 11:10:30
categories: AndroidDev
tags: [Android, CI, Gitlab]
---
> 這個系列是來紀錄如何在 Gitlab 上建立 CI 流程(**Continuous Integration**)
> 這一篇是介紹如何在 Gitlab 建立基本的 CI

<!-- more -->
# 前情提要
1. 首先你要先對 Gitlab CI 有基本的認識，建議可以先看看[這篇文章](https://medium.com/@ome450901/gitlab-ci-for-android-%E7%AD%86%E8%A8%98-ae8115781454)
2. 我是使用另一台閒置的電腦作為 Runner，也就是 **Specific Runners**
3. 確認作為 Runner 的電腦有以下的設定，例如:
  a. Java SDK & Android SDK
  b. 將 adb 加入環境變數
  c. 有 Android Device Connection(模擬器或實機都可)

---

# 設定 Runner

## 1. 安裝 Runner

> 首先你要先[確認你的 gitlab 的版本](https://stackoverflow.com/questions/21068773/checking-of-gitlab-version/22138457)，然後從 [gitlab-ci-multi-runner](https://gitlab.com/gitlab-org/gitlab-ci-multi-runner) 下載安裝適合的 Runner

``` cmd
# install runner whose version is 1.11.2
$ apt-get install gitlab-ci-multi-runner=1.11.2

# or
$ sudo curl --output /usr/local/bin/gitlab-runner https://gitlab-ci-multi-runner-downloads.s3.amazonaws.com/v1.11.2/binaries/gitlab-ci-multi-runner-darwin-amd64

# if you want to install the latest version
$ sudo curl --output /usr/local/bin/gitlab-runner https://gitlab-ci-multi-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-ci-multi-runner-darwin-amd64
```

## 2. 註冊 Runner

### step 1
在 Gitlab repo 的 Runner 頁面，可以看到這個下面的畫面
請記住紅字的 **URL** & **registration token**

![](https://i.imgur.com/XfSYenY.png)

### step 2
在安裝 gitlab-runner 的電腦上執行 `$ gitlab-runner register`

``` cmd
$ gitlab-runner register
WARNING: Running in user-mode.                     
WARNING: Use sudo for system-mode:                 
WARNING: $ sudo gitlab-runner...                   

Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
<your_setup_url_in_gitlab_repo>

Please enter the gitlab-ci token for this runner:
<your_token_in_gitlab_repo>

Please enter the gitlab-ci description for this runner:
<your_description>

Please enter the gitlab-ci tags for this runner (comma separated):
<your_runner_tag>

Whether to run untagged builds [true/false]:
[false]: false

Registering runner... succeeded

Please enter the executor: shell, ssh, virtualbox, docker+machine, docker-ssh+machine, kubernetes, docker, docker-ssh, parallels:
shell
```
> 因為是 Runner 裝在自己的電腦，所以 executor 直接使用 **shell**

## 3. 啟動 gitlab
``` cmd
$ gitlab-runner install
$ gitlab-runner start
```

---

這樣 Runner 就設定完成


## Reference
- [Runner](http://docs.gitlab.com/ce/ci/runners/README.html#shared-vs-specific-runners)
- [gitlab-ci-multi-runner](https://gitlab.com/gitlab-org/gitlab-ci-multi-runner)
- [Install GitLab Runner](https://docs.gitlab.com/runner/install/)
