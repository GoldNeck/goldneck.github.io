---
layout: post
title:  "flutter Android toolchain 문제 해결방법. (cmdline-tools component is missing, Android license status unknown.)"
summary: ""
author: goldtree
date: '2024-10-05 21:28:12 +0900'
category: 'flutter'
tags: DB
#thumbnail: /assets/img/posts/index_3.jpg
isShowThumbnail: false
keywords: Flutter, Android SDK, cmdline-tools, Flutter Doctor, Android 라이선스, cmdline-tools component is missing, Android license status unknown.
usemathjax: false
permalink: /blog/flutter_provider_1/
---

```bash
Android toolchain - develop for Android devices (Android SDK version 34.0.0)
    ✗ cmdline-tools component is missing
      Run `path/to/sdkmanager --install "cmdline-tools;latest"`
      See https://developer.android.com/studio/command-line for more details.
    ✗ Android license status unknown.
      Run `flutter doctor --android-licenses` to accept the SDK licenses.
      See https://flutter.dev/to/windows-android-setup for more details.
```
위에 오류가 나온 이유
1. cmdline-tools 컴포넌트가 누락됨  
2. Android 라이선스 상태가 알 수 없음  


##### ✗ cmdline-tools component is missing  해결  
AndroidStudio 실행  
Setting -> Languages & Frameworks -> Android SDK -> SDK Tools 탭 클릭 -> Android SDK Command-line Tools  
체크 후 Apply  
![android setting](/assets/img/posts/flutter/1.png)


##### ✗ Android license status unknown. 해결  
```bash
flutter doctor --android-licenses
```

터미널 실행 후 아래 명령어 실행하고 전부 y 해주면 된다.  