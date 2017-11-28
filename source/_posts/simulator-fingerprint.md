---
title: 在模擬器中使用指紋辨識
date: 2017-11-28 16:16:06
categories: Dev
tags: [Android, Simulator, AVD, adb, fingerprint]
---
簡單紀錄一下要如何使用指紋辨識
<!-- more -->
### Step 0
確認有安裝 `Android SDK Tools Revistion 24.3` 以上的板本

### Step 1
啟用 Screen Lock

{% img /simulator-fingerprint/Screenshot_1511857225.png 288 162  %}

### Step 2
進入新增 Fingerprint 的畫面

{% img /simulator-fingerprint/Screenshot_1511857449.png 288 162 %}

### Step 3
輸入指令：
``` cmd
adb -e emu finger touch <自取ID>
```

如此就設定完一個指紋

------

之後再需要指紋的地方，輸入上述一樣的指令通過辨識囉~

{% img /simulator-fingerprint/Screenshot_1511858373.png 288 162 %}
<br/>
