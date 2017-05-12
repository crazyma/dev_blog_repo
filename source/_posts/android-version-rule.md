---
title: The Rules of Android VersionName & VersionCode
date: 2017-05-12 17:38:02
categories: AndroidDev
tags: [Android, Semantic Versioning]
---
這篇來記錄我現在採用的 VersionName VersionCode 的規則
<!-- more -->
## Version Name
Version Name 的形式通常會有三組數字組成，例如: `2.4.31`
第一個數字稱為 `Major`: 通常是用在 Project 有大幅度的改版，或是會有無法上下相容的狀況
第二個數字稱為 `Minor`: 表示有新的功能上線，且是上下相容的
第三個數字稱為 `Patch`: 表示有一些錯誤的修正，或是 Config Info 的調整...等等

另外，因應開發期 & 測試期，會在後面再加上 `suffix`，例如
`2.4.31-SNAPSHOT`: 表示是開發中的版本，這個版本不會對外 Release
`2.4.31-RC2`: RC 是 `Release Candidates`的意思，表示是給別人測試使用的(例如 Google Play 的 Beta 測試版)，後面的數字表示第 n 版的測試

## Version Code
Version Code 是從 Version Name轉換過來，以下是範例：
- `2.4.31-SNAPSHOT` -> **2043100**
- `1.2.3-SNAPSHOT` -> **1020300**
- `3.1.85-RC1` -> **3018501**
- `2.11.1-RC2` -> **2110102**
- `2.4.31` -> **2043199**
- `1.11.1` -> **1110199**

要注意的是**最後兩個數字**
- `SHANSHOT`: `00`結尾
- `RCx`: `x`結尾，x = 1~98
- `Release`: `99`結尾

## 後話
這個規則其實就是參考別人的開發經驗，以及自己的喜好決定的，並沒有硬性規定要怎麼做
只要訂定適合自己開發上的規則就可以囉

## Reference
- [Automatic versionCode generation in Android Gradle](https://blog.jayway.com/2015/03/11/automatic-versioncode-generation-in-android-gradle/)
- [語意化版本 2.0.0](http://semver.org/lang/zh-TW/)
