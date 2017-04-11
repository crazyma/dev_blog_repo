---
title: How to remove toolbar shadow / 去除 Toolbar 的陰影
date: 2017-04-11 12:44:17
categories: AndroidDev
tags: [Android, Toolbar]
---

設計 Android 介面時，經常會使用到 `CoordinatorLayout`, `ToolBar` & `Toolbar` 來建立頁面，但有時候你會不小心弄出這樣子的 ToolBar (上下都會有陰影)
</br>
{% img /shadow/device-2017-04-11-133533.png 538 136 XDDDd %}
</br>
這裡提出幾個解決方法:
</br>
**[方法1]  將 AppBarLayout 的 elevation設為 0**

此作法會除掉上下陰影

``` xml
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    ...
    >

    <android.support.design.widget.AppBarLayout
        ...
        app:elevation="0dp"
        ...
        >

        <android.support.v7.widget.Toolbar
            ...
        />

    </android.support.design.widget.AppBarLayout>

    ...

</android.support.design.widget.CoordinatorLayout>
```q
</br>
{% img /shadow/device-2017-04-11-134158.png 539 125 %}
</br>
**[方法2]  remove `android:windowDrawsSystemBarBackgrounds`**
此參數就是通知系統：由我們自己決定 status bar 的顏色。
通常是為了給有 `DrawerLayout` 的**OverLay Status Bar** 之效果使用。如果你沒有要使用，則可以直接移除

``` xml
<style name="AppTheme.NoActionBar">
    <item name="windowActionBar">false</item>
    <item name="windowNoTitle">true</item>
    <!-- remove these 2 lines below    -->
    <item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>
</style>
```
</br>
{% img /shadow/device-2017-04-11-135523.png 540 125 %}
</br>

如果你也沒有其他東西會 Overlay 到 status bar，你也可以把 `fitsSystemWindows` 刪掉
``` xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    ...
    android:fitsSystemWindows="true" <!-- remove this line -->
    ...
    >

</android.support.design.widget.CoordinatorLayout>

```

**[方法3]  change  `android:statusBarColor`**
``` xml
<style name="AppTheme.NoActionBar">
    <item name="windowActionBar">false</item>
    <item name="windowNoTitle">true</item>
    <item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@color/colorPrimaryDark</item>
    <!-- <item name="android:statusBarColor">@android:color/transparent</item> -->
</style>
```


### Reference
- [How to remove top border shadow from ActionBar](http://stackoverflow.com/questions/34210184/how-to-remove-top-border-shadow-from-actionbar)
- [How to remove the shadow above the app bar?](http://stackoverflow.com/questions/35702896/how-to-remove-the-shadow-above-the-app-bar)
- [Android appbarlayout elevation appears in status bar](http://stackoverflow.com/questions/33726318/android-appbarlayout-elevation-appears-in-status-bar)
- [如何将DrawerLayout显示在ActionBar/Toolbar和status bar之间](http://solo.farbox.com/blog/how-do-i-use-drawerlayout-to-display-over-the-actionbar-or-toolbar-and-under-the-status-bar)
- [Why would I want to fitsSystemWindows?](https://medium.com/google-developers/why-would-i-want-to-fitssystemwindows-4e26d9ce1eec)
