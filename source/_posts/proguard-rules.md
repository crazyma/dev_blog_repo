---
title: My ProGuard Rules
date: 2017-04-15 15:19:13
categories: AndroidDev
tags: [Android, ProGuard]
---
通常在 release apk 時，大家都會使用 `ProGuard` 做基本的混淆
有鑒於最近發生混淆之後，app 出現超級難找的 bug
於是這篇來記錄一下我自己的 **ProGuard Rules**
<!-- more -->

基本上因為做了混淆產生 bug，大部分都是因為 Code 涉及到 [Reflection 機制](https://github.com/JustinSDK/JavaSE6Tutorial/blob/master/docs/CH16.md)
因為混淆之後，就沒辦法透過 Reflection 動態找到你原本的目標
所以你需要設定 proguard rules，讓這些 code 不會被混淆

我自己開發時，大多是因為使用到**外部 Library** 才導致 code 混淆後出現問題
我比較常用的 Library 有： `Joda`, `Okhttp/Okio/Picasso`, `EventBus` & `Gson`
以下就是目前我針對這些 Library 統整後的 `proguard-rules`

``` pro
# Joda Time 2.3
-dontwarn org.joda.convert.**
-dontwarn org.joda.time.**
-keep class org.joda.time.** { *; }
-keep interface org.joda.time.** { *; }

# Okhttp, Okio & picasso
-dontwarn com.squareup.okhttp.**
-dontwarn okio.**

#EventBus3
-keepattributes *Annotation*
-keepclassmembers class ** {
    @org.greenrobot.eventbus.Subscribe <methods>;
}
-keep enum org.greenrobot.eventbus.ThreadMode { *; }

#Gson
-keep class com.crazyma.data.** { *; }
```

**小提醒**
1. 新版 `Okhttp3` 作者其實有說不用設定 protection ([reference](https://github.com/square/okhttp/issues/2230#issuecomment-212926355)) ，但是因為 Picasso 目前預設還是會使用到 `舊版 okhttp` 內容，所以還是需要寫進來
2. `Gson part` 要保護的不是 gson library，而是 **'你要用 Gson 進行轉換的那些 data class'** (如果你稍微了解的話，就會知道 Gson 裡面就是一堆 Reflection)
3. 另外我也有愛使用 `RxJava2`，舊版的 RxJava 也需要寫 ProGuard Rules，但是 `RxJava2` 目前看來是不太需要([reference](https://github.com/artem-zinnatullin/RxJavaProGuardRules#note-about-rxjava-2x))
