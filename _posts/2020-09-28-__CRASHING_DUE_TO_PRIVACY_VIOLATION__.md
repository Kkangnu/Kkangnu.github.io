---
layout: post
title: "&#95;&#95;CRASHING_DUE_TO_PRIVACY_VIOLATION&#95;&#95;"
subtitle: "CRASHING_DUE_TO_PRIVACY_VIOLATION 해결"
date: 2020-09-28 21:06:13 0400
background: '/img/posts/06.jpg'
categories: [iOS]
tag: [iOS]
---


# &#95;&#95;CRASHING_DUE_TO_PRIVACY_VIOLATION&#95;&#95; 문제 수정

---

* 문제

> 앱이 가끔 &#95;&#95;CRASHING_DUE_TO_PRIVACY_VIOLATION&#95;&#95; 문제로 죽음

* 원인

> 충돌 앱의 .plist에 **필수** 권한이 없는 경우에 발생합니다. 권한 수준은 Apple 새로운 iOS 버전에서 변경 될 수 있습니다. 따라서 작동하지 코드는 .plists의 새로운 요구 사항이 iOS 업데이트에서 작동하지 않습니다.



* **NSFaceIdUsageDescription** 관련 권한 내용

IPhone X에서 Touch ID / Face ID 를 사용하는 경우 info.plist에

**NSFaceIdUsageDescription** 키가 없는 것이 원인 일 수 있다.

키는 iOS 11에서 추가 되었으며 iOS 11.3이 출시 된 이후에는 필수가  iOS 11.3 릴리스 이후에 iPhone X에서 충돌이 급증한 것을 감안할 때, iOS 11.3 후에 필수가 된 것입니다. Apple [here](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html):

```swift
NSFaceIDUsageDescription (String -iOS)
이 키는 앱이 Face ID를 사용하는 이유를 설명 할 수 있습니다.

중요 : 사용자의 개인 정보를 보호하기 위해 iOS 11 이후에 연결하여 하드웨어가 지원하는 경우 Face ID에 액세스하는 iOS 앱은 그렇게 할 의도를 정적으로 선언해야합니다. NSFaceIDUsageDescription 키 앱의 Info.plist 파일에 포함이 주요 목표 문자열을 제공합니다. 앱이 해당 목적 문자열없이 Face ID에 액세스하려고하면 응용 프로그램이 종료 될 수 있습니다.

이 키는 iOS 11 이상에서 지원됩니다.
```



* **NSPhotoLibraryUsageDescription** 관련 권한 내용

**NSPhotoL LibraryUsageDescription** 의 경우 **iOS 6** 및 **iOS 11.3 이하** 에서이 키를 사용 하는 경우 문제가 없지만 **iOS 11.3 이상** 이후 에는 추가해야합니다.



* 이외 **블루투스 권한, 카메라 권한** 등등이 있을 수 있지만 나의 경우 위에 두개의 문제였다.
