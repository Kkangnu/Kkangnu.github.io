---
layout: post
title: "SwiftUI) Annotation"
subtitle: "스유 @State, @ObservedObject, @EnvironmentObject"
date: 2021-05-20 21:36:13 0400
background: '/img/posts/06.jpg'
categories: [iOS]
tag: [iOS]

---



# SwiftUI) Annotion들



## @State

[[SwiftUI - @State](https://developer.apple.com/documentation/swiftui/state)]

- SwiftUI는 state로 선언한 모든 프로퍼티의 스토리지를 관리

- state 값이 변경되면 View가 appearance를 invalidates하고 body를 다시 계산

- 주어진 View에서 State를 Single source of truth로 사용할 것

- State인스턴스는 Value자체가 아님. 값을 읽고 변경하는 수단.

- State의 기본값에 접근하면 value프로퍼티를 사용

- View의 body에서만 state프로퍼티에 접근할 것 

  (따라서, View의 클라이언트에서 State에 접근하지 못하도록 State프로퍼티를 private로 선언

  [state는 특정 View에 속하고, View 외부에서 "절대" 사용되지 않는 간단한 프로퍼티 적합

  -> 해당 상태가 절대로 escape되지 않도록 설계되었다는 것을 강조하기 위해 private선언!!])

- @State를 앞에 추가하면 SwiftUI가 자동으로 변경사항을 observe하고 해당 state를 사용하는 view부분을 업데이트



```swift
struct SegmentedControlView: View {
	var body: some View {
    Picker("", selection: .constant(0)) { 
      Text("A").tag(0) 
      Text("B").tag(1) 
      Text("C").tag(2) 
    }.pickerStyle(SegmentedPickerStyle())
  }
}
```

> selection 파라메터가 binding됨
>
> constant(0)이 입력되었고, selection이 0에서 변경안됨
>
> 현재 selection된 "상태"를 변경하고 해당 state가 저 segmentedControl을 다시 그려야하는데.
>
> 이럴때 @State로 프로퍼티 선언하면 됨



```swift
	struct SegmentedControlView: View {
    @State private var selectedIndex = 0 
    var body: some View { 
      Picker("", selection: $selectedIndex) {
        Text("A").tag(0)
        Text("B").tag(1)
        Text("C").tag(2)
      }.pickerStyle(SegmentedPickerStyle())
    }
  }
```



> selection 파라메터가 binding타입이 들어가고,
>
> 특정 state를 selection에 binding 할수 있다.
>
> $연산자를 이용해서 바인딩





------

## @ObservedObject

[[SwiftUI - ObservedObject](https://developer.apple.com/documentation/swiftui/observedobject)]

- @State - 특정 view에서 사용하는 프로퍼티
- @ObservedObject - 여러 프로퍼티나 메서드, 여러 View에서 공유할 수 있는 커스텀타입
- 외부 참조 타입을 사용한다는점 제외하면 @State와 매우 유사
- @ObservedObject와 함께 사용할 타입은 ObservableObject 프로토콜을 따라야함
- observed objecct가 데이터가 변경되었음을 View에 알리는 방법은 여러가지가 있음
- 제일 쉬운 방법은 @Published프로퍼티 래퍼 사용 = SwiftUI에 view reload를 트리거함



[[@ObservedObject 간단 예제](https://www.hackingwithswift.com/quick-start/swiftui/how-to-use-observedobject-to-manage-state-from-external-objects)]

- UserSettings의 score앞에 @Published 사용
- @Pulished사용으로 score가 변경되면 view를 reload함
- UserSetting이 class인 이유는 ObservableObject가 class-bound프로토콜





------

## @EnvironmentObject

[[SwiftUI - EnvironmentObject](https://developer.apple.com/documentation/swiftui/environmentobject)]

- 바인딩 가능한 객체가 변경될 때 마다 현재 View를  invalidate하기 위해 상위 view에서 제공한 binding가능한 객체를 사용하는 dynamic view property
- 반드시 environmentObject(_:)메서드를 호출하여 상위 뷰에서 모델 객체를 설정해야함
- 모든 view가 읽을 수 있는 shared data

```swift
	class UserSettings: ObservableObject { 
    @Published var score = 0 
  }
```

> UserSettings에 environmentObject(_:)메서드를 호출하여 상위 뷰에서 모델 객체를 설정



상위 객체 SceneDelegate

```swift
let settings = UserSettings() 
let contentViews = ContentViews().environmentObject(settings)

if let windowScene = scene as? UIWindowScene { 
  let window = UIWindow(windowScene: windowScene) 
  window.rootViewController = UIHostingController(rootView: contentViews) 
  self.window = window window.makeKeyAndVisible()                                                                                                                                       }
```

> 상위 뷰에서 UserSettings를 environmentObject 호출하여 모델 객체로 설정
>
> UserSetting을 쓰는 쪽에서 @EnvironmentObject로 설정



```swift
struct ContentViews: View { 
	@EnvironmentObject var settings: UserSettings 
}
```

> @EnvironmentObject 선언



```swift
struct SubView: View {
	@EnvironmentObject var settings: UserSettings
	
	var body: some View {
		Text("\(settings.score)")
	}
}
```

> @ObservedObject로 동일 기능 구현 가능함
>
> @EnvironmentObject로 선언하면 앱 전체 공유 가능
>
> EnvironmentObject가 바인딩된 곳 View는 자동 업데이트



내용 : [[Zedd0202](https://zeddios.tistory.com/964)]

출처 : [[Zedd0202](https://zeddios.tistory.com/964)]
