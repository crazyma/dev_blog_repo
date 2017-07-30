---
title: 在 Gitlab 上建立 Android CI - (4) 在 CI 產生 Signed APK
date: 2017-07-07 23:17:49
categories: AndroidDev
tags: [Android, CI, Gitlab]
---
> 介紹如何直接在 CI 上 產生 signed apk

<!-- more -->

# 說明
簡述一下我自己的做法
1. 將 `keystore.jks` 放在 Runner 的電腦
2. 將打包要用的資訊寫道 repo 的 `Variables` 裡，例如 password, alias...
3. 在 build.gradle 使用這些變數

> 會採取這個做法，是希望 keystore 的密碼就不會被 git 記錄到

### 1. 設置 keytore
以我的例子，是放在 `~/Workspace/Android/keystore/my_keystore.jks`

### 2. 設定 Variable
**2-1 進入設定 > Variable**
![](https://i.imgur.com/TeY9OAA.png)
<br/>
**2-2 新增 Variable**
![](https://i.imgur.com/yiDIJ1d.png)

新增的變數有四個
    - KEYSTORE_FILE: the path of keysotre file
    - KEYSTORE_PASSWORD
    - KEYSTORE_ALIAS
    - KEYSTORE_ALIAS_PASSWORD

 ![](https://i.imgur.com/RP70DAa.png)
 ![](https://i.imgur.com/uDsDKW3.png)

### 3. 修改 app build.gradle

首先可以先參考這個文章:[善用 GRADLE 建立安全的 KEYSTORE 設定](https://blog.mosil.biz/2015/08/android-keystore-info-extra-from-build-gradle/)
延續這個文章，因為 Runner 無法抓到 local 端的 `local.properties`，所以加了一些設定，從 Gitlab CI 的 **Variable** 獲得參數來打包：

``` gradle    
android {

    # NEW
    signingConfigs {
        sign
    }

    ...

    buildTypes {
        release {
            signingConfig signingConfigs.sign # NEW
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

...

# NEW
def ksFile = rootProject.file('local.properties')
def props = new Properties();

if (ksFile.canRead()) {
    # using local.properties
    props.load(new FileInputStream(ksFile))

    if (props != null) {
        android.signingConfigs.sign.storeFile file(props['KEYSTORE_FILE'])
        android.signingConfigs.sign.storePassword props['KEYSTORE_PASSWORD']
        android.signingConfigs.sign.keyAlias props['KEYSTORE_ALIAS']
        android.signingConfigs.sign.keyPassword props['KEYSTORE_ALIAS_PASSWORD']
    } else {
        println 'some entries in \'local.properties\' not found!'
    }
} else {
    # using Gitlab CI Variable
    println 'Use Gitlab CI Variables'
    android.signingConfigs.sign.storeFile file(System.getenv('KEYSTORE_FILE'))
    android.signingConfigs.sign.storePassword System.getenv('KEYSTORE_PASSWORD')
    android.signingConfigs.sign.keyAlias System.getenv('KEYSTORE_ALIAS')
    android.signingConfigs.sign.keyPassword System.getenv('KEYSTORE_ALIAS_PASSWORD')
}

```

### 4. 新增 job:
``` yml
# .gitlab-ci.yml

buildSignedApk:
  tags:
    - android_ci_sample
  stage: buildApk
  script:
    - ./gradlew assembleRelease
  artifacts:
    paths:
    - app/build/outputs/
```

如此就可以在 CI 中打包 signed apk
![](https://i.imgur.com/36PKe1f.png)
