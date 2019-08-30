---
title: \[Cordova\] iOS 錯誤訊息 tool \'xcodebuild\' requires Xcode
date: "2019-08-19 11:07:46"
tags:
  - Cordova
  - iOS
  - Xcode
---

An error when set-up iOS app with Cordova.

<!-- more -->

## 起因

使用 MAC 進行開發的工程師們，有許多工具都需要依附在 Xcode 上，通常我們只要從 App Store 將 Xcode 下載下來，並且執行一次，就可以將需要的元件都安裝完畢。

但最近公司內有個燙手山芋是用 Cordova + Vue 來建立雙平台的 APP，架起專案的過程中，卻遇到一段沒麽輕易處理的錯誤：

{% asset_img error.png %}

```
xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory '/Library/Developer/CommandLineTools' is a command line tools instance
```

經過一番 Google 後才知道...`xcode-select`的開發目錄是指向：

**/Library/Developer/CommandLineTools**

因此一旦下列兩個狀況都有發生，就會遇見這個問題:

1. 專案需要完整的 Xcode.
2. Command Line Tools 安裝順序早於完整 Xcode.

---

## 解法

1. 確定安裝完整的 Xcode (直接從 App store 安裝)
2. 啟動一次 Xcode 使其自動安裝各類元件
3. 執行下方指令，以切換路徑就大功告成啦！

```shell
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```

---

## 詳細討論

- [Github issues - node-gyp](https://github.com/nodejs/node-gyp/issues/569)
- [xcode-select active developer directory error](https://stackoverflow.com/questions/17980759/xcode-select-active-developer-directory-error/17980786#17980786)
