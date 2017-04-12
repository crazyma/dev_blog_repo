---
title: Upload Library to Bintray(JCenter) | 上傳自己的 Library 至 Bintray(JCenter)
date: 2017-04-12 23:26:42
categories: AndroidDev
tags: [Android, Bintray, JCenter, library]
---
**JCenter** 已經是目前 Android Studio 預設的 pubic repositories
許多 Library 也都放在 JCenter
這篇文章以我自己做的 JsonCake Library
簡單示範如何上傳自己的 Library

## Step 1. 註冊帳號
首先至 [Bintray](https://bintray.com/) 上註冊帳號

順利註冊之後，點擊maven圖示，並且創建一個new project：
{% img /upload-library-to-bintray/0-aZduq4thGxjcNQdR.png %}

填寫相關資訊：
{% img /upload-library-to-bintray/0-aCg_NhLMnbRX9qzV.png %}
{% img /upload-library-to-bintray/0-YvLr_9WjLQ4OrV5y.png %}

**一些小提醒**
1. 習慣上，project name都是小寫
2. Version Control可以直接填上github的網址

如此一來，網站上的設定完成了。

## Step 2. 設定Android Studio Project

一般來講，你會將你的 Library 寫成另一個 module(Android Library)
以我的專案 `JsonCake` 為例，有兩個module：
1. **app**：專案本身，不會上傳至 JCenter
2. **jsoncake**：Library 部分，也是我們要上傳的部分

{% img /upload-library-to-bintray/0-yi8KiidzalP_jfiS.png %}

### 第一步
在 Project 的 `build.gradle` 新增以下兩行

``` gradle
classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'
classpath 'com.github.dcendents:android-maven-gradle-plugin:1.3'
```

所以整個build.gradle如下：
``` gradle
// Top-level build file where you can add configuration options common to all sub-projects/modules.
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:1.2.3'
        classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.2'
        classpath 'com.github.dcendents:android-maven-gradle-plugin:1.3'
        // NOTE: 上述Library請確定為1.3以上版本，部分網路上教學使用1.2以下的版本，會造成錯誤
    }
}
allprojects {
    repositories {
        jcenter()
    }
}
```

### 第二步
在 `local.properties` 檔案中加入以下兩行
``` gradle
bintray.apikey=<YOUR_API_KEY>
bintray.user=<YOU_USERNAME>
```
- **user name**：是你 bintray 的註冊 username
- **Api Key**：可以至 Bintray > Edit Profile > API Key 查詢。如下圖：

{% img /upload-library-to-bintray/0-CBPD6aYZu1Cb9uDu.png %}

### 第三步
編輯 Library Module 的 `build.gradle`
在 `apply plugin: 'com.android.library'` 下方新增

``` gradle
ext {
    bintrayRepo = 'maven'
    bintrayName = 'jsoncake'
    publishedGroupId = 'com.crazyma.jsoncakelib'
    libraryName = 'JsonCake'
    artifact = 'jsoncake'
    libraryDescription = 'A wrapper Library using okhttp and gson to make doing http operation much easier.'
    siteUrl = 'https://github.com/crazyma/JsonCake'
    gitUrl = 'https://github.com/crazyma/JsonCake.git'
    libraryVersion = '1.0.0'
    developerId = 'crazyma'
    developerName = 'David Ma'
    developerEmail = 'david77115@gmail.com'
    licenseName = 'The Apache Software License, Version 2.0'
    licenseUrl = 'http://www.apache.org/licenses/LICENSE-2.0.txt'
    allLicenses = ["Apache-2.0"]
}
#請自行修改相關內容
```

**小提醒**
到時候設定 dependencies 就是依照上述的設定，輸入以下script code：
`compile ‘(publishedGroupId):(artifact):(libraryVersion)’`
上述三項最好與 module 本身的 `pachage name : module name : versionName` 相同
我第一次建立的時候就是artifact與module name不同，導致一直無法找到正確的檔案

並在該 build.gradle 的最後面新增以下兩行
``` gradle
apply from: 'https://raw.githubusercontent.com/attwellBrian/JCenter/master/installv1.gradle'
apply from: 'https://raw.githubusercontent.com/attwellBrian/JCenter/master/bintrayv1.gradle'
```

完整的 build.gradle 如下
``` gradle
apply plugin: 'com.android.library'
ext {
    bintrayRepo = 'maven'
    bintrayName = 'jsoncake'
    publishedGroupId = 'com.crazyma.jsoncakelib'
    libraryName = 'JsonCake'
    artifact = 'jsoncake'
    libraryDescription = 'A wrapper Library using okhttp and gson to make doing http operation much easier.'
    siteUrl = 'https://github.com/crazyma/JsonCake'
    gitUrl = 'https://github.com/crazyma/JsonCake.git'
    libraryVersion = '1.0.0'
    developerId = 'crazyma'
    developerName = 'David Ma'
    developerEmail = 'david77115@gmail.com'
    licenseName = 'The Apache Software License, Version 2.0'
    licenseUrl = 'http://www.apache.org/licenses/LICENSE-2.0.txt'
    allLicenses = ["Apache-2.0"]
}
android {
    compileSdkVersion 22
    buildToolsVersion "22.0.1"
    defaultConfig {
        minSdkVersion 14
        targetSdkVersion 22
        versionCode 1
        versionName "1.0.0"     }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}
dependencies {
    //  your dependencies
}
apply from: 'https://raw.githubusercontent.com/attwellBrian/JCenter/master/installv1.gradle'
apply from: 'https://raw.githubusercontent.com/attwellBrian/JCenter/master/bintrayv1.gradle'
```

## Step 3. Run Gradle
如果你是第一次使用，請先在 Android Studio 的 Terminal 輸入：
``` shell
$ ./gradlew install
```

順利的話，會出現 `BUILD SUCCESSFUL` 字樣

接著可以開始上傳。輸入：
``` shell
$ ./gradlew bintrayUpload
```
一樣出現 `BUILD SUCCESSFUL` 字樣，就表示上傳成功

{% img /upload-library-to-bintray/0-ckNxLY_AzPg5Eikl.png %}


此時就已經可以使用這個 Library 囉
如果想要直接使用，需要在 app 的 `build.gradle` 中設定
``` gradle
repositories {
    maven {
        url 'https://dl.bintray.com/<your_bintray_username>/maven'
    }
}
...
dependencies {
    compile 'com.crazyma.jsoncakelib:jsoncake:1.0.0'
}
```

上述的 url `https://dl.bintray.com/<your_bintray_username>/maven`
可以在 Bintray 專案頁面的右上方找到
{% img /upload-library-to-bintray/0-ql0olv2dnEQofGsv.png %}

## Step 4. 同步到 JCenter

請點擊專案頁面右下角的Add to JCenter
{% img /upload-library-to-bintray/0-Pr-zX2V3vnRu__6l.png %}

依據指示送出同步要求
等待數小時，會有認證信寄至你的信箱
同步就完成囉
(該認證只需要做一次)
之後就可以直接在專案中使用囉 (記得確保專案有設定jcenter為default repository喔)
``` gradle
  compile 'com.crazyma.jsoncakelib:jsoncake:1.0.0'
```
如果想要確認Project有沒有上傳成功，可以至 http://jcenter.bintray.com/ 查詢
以後就可以跟全世界分享你的Library囉 (L)

## 後記
如果你希望你的 Project 也同步上傳到 **Maven Central**，也可以先依照本文的方式上傳至 Bintray，再同步至 **Maven Central**。
詳情請看下面提供的參考教學網站。
但為了簡單化，這裏就不提及了。可以參考其他的教學喔。

[1] [How to distribute your own Android library through jCenter and Maven Central from Android Studio](https://inthecheesefactory.com/blog/how-to-upload-library-to-jcenter-maven-central-as-dependency/en)
(該介紹使用 com.github.dcendents:android-maven-gradle-plugin:1.2，記得改成1.3。)
