---
title: 使用 Data Binding 遇到 Lint 報錯
date: 2017-06-01 17:55:06
categories: AndroidDev
tags: [Android, Android Studio, Lint, Data Binding]
---
當使用 Data Binding 的 ``@BindingAdapter` 的時候
lint 會回報一些 warning
<!-- more -->
## Issue 1
### Application namespace for attribute app:leftMessage will be ignored.

這個問題是因為使用 `prefix`, 移掉就可以

### 錯誤寫法
``` java
@BindingAdapter("bind:marginTop")
public static void setMarginTop(View view, float padding) {
  //  ...
}
```
### 正確寫法
``` java
@BindingAdapter("marginTop")
public static void setMarginTop(View view, float padding) {
  //  ...
}
```
<br/>

---

## Issue 2
### Method references using '.' is deprecated

常發生在呼叫 listener 的時候，請改用 `::`

### 錯誤寫法
``` xml
  android:onClick="@{click.onActionButtonClick}"
```

### 正確寫法
``` xml
  android:onClick="@{click::onActionButtonClick}"
```
