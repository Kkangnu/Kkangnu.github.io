---
layout: post
title: "Beginning Rxcocoa"
subtitle: "Beginning RxCocoa"
date: 2021-04-02 22:31:13 0400
background: '/img/posts/06.jpg'
categories: [ReactiveX]
tag: [iOS]
---

**[RxSwift] Beginning RxCocoa**

이 포스트는 [RxSwift - Reactive Programming with Swift](https://store.raywenderlich.com/products/rxswift) 책의 챕터 12에서 눈에 띄는 내용들만 요약해 둔 것입니다. 참고 바랍니다.

**1. RxCocoa**

·     UI Control과 다른 SDK 클래스를 wrapping한 커스텀 extension set

·     iOS, tvOS, macOS의 모든 플랫폼에서 동작

**2. ObserverType****과** **ObservableType**

·     ObserverType : 값을 주입(Inject)시킬 수 있는 타입

·     ObservableType : 값을 관찰할 수 있는 타입

**ControlProperty**

Subject 같이 프로퍼티에 새 값을 주입시킬 수 있고(ObserverType) 값의 변화도 관찰할 수 있는 타입(ObservableType).

·     예) UITextField+Rx.Swift의 text 프로퍼티는 ControlProperty

extension Reactive where Base: UITextField {

  /// Reactive wrapper for `text` property.

  public var text: ControlProperty<String?> {

​    return value

  }

  // 이후 내용 생략

}

·     ControlProperty는 ControlPropertyType를 따름

public struct ControlProperty<PropertyType> : ControlPropertyType {

// 이후 내용 생략

}

·     ControlPropertyType은 ObservableType과 ObserverType을 따름을 확인

public protocol ControlPropertyType : ObservableType, ObserverType {

  /// - returns: `ControlProperty` interface

  func asControlProperty() -> ControlProperty<Element>

}

**Binder**

·     ObserverType을 따름. 값을 주입시킬 수는 있지만, 값을 관찰할 수는 없음.

o  특징 : error를 값으로 받을 수 없음. error가 주입되면 Binder는 debug모드에서 fatalError() 발생시킴. production 모드에서는 런타임 에러로그를 출력.

·     예) UILabel+Rx.Swift에서 text 바인더 프로퍼티는 값을 주입만 시킬 수 있음.

extension Reactive where Base: UILabel {

  /// Bindable sink for `text` property.

  public var text: Binder<String?> {

​    return Binder(self.base) { label, text in

​      label.text = text

​    }

  }

·     Binder는 ObserverType를 따름을 확인

public struct Binder<Value>: ObserverType {

// 이후 내용 생략

}

**3. Observable****의** **Binding**

RxCocoa에서 binding은 단방향 binding                               

**Bind(to:)**

·     bind(to:)는 subscribe()의 별칭(alias or syntatic sugar)

·     bind(to: observer)를 호출하면 실제로는 subscribe(observer)가 실행됨

**4. Trait**

Trait이란? UI 작업시 코드를 쉽고 직관적으로 작성해 사용할 수 있도록 도와주는 특별한 Observable 클래스의 모음

**Trait****의** **규칙**

·     error를 방출하지 않음

·     메인스케쥴러에서 observe 됨

·     메인스케쥴러에서 subscribe됨

·     Signal을 제외한 나머지 Trait은 자원을 공유함(e.g share(replay:1))

**RxCocoa****의** **Traits**

·     ControlProperty : 컨트롤에 data를 binding 하기 위해 사용 (rx namespace 사용)

·     ControlEvent : 컨트롤의 event를 수신하기 위해 사용

·     Driver : error를 방출하지 않고 메인스레드에서 처리됨

·     Signal : Driver와 거의 동일. 한가지 다른 점은 자원을 공유하지 않음. (share(replay:1) 사용하지 않음). 즉, 새로운 subscriber에게 마지막 엘리먼트를 보내주지 않음

o  Signal은 event모델링에 유용하고, Driver는 state모델링에 더 적합

**Driver****와** **ControlProperty****의** **사용**

let search = searchCityName.rx.text.orEmpty

   .filter { !$0.isEmpty }

   .flatMapLatest { text in 

   return ApiController.shared.currentWeather(city: text)

   .catchErrorJustReturn(ApiController.Weather.empty)

   }

   .asDriver(onErrorJustReturn: ApiController.Weather.empty)

·     **.asDriver(onErrorJustReturn:)** : Observable에서 error가 방출됐을때 Driver에서 error 대신 지정한 기본 값을 리턴하도록 만들어 Driver에서 error가 방출되는 것을 막음

**bind(to:)** **코드** **대신** **drive()** **코드** **사용하기**

·     변경 전 : bind(to:) 사용

search.map { $0.icon }

  .bind(to: iconLabel.rx.text)

  .disposed(by: bag)

 

search.map { "\($0.humidity)%"}

  .bind(to: humidityLabel.rx.text)

  .disposed(by: bag)

 

search.map { $0.cityName }

  .bind(to: cityNameLabel.rx.text)

  .disposed(by: bag)

·     변경 후 : drive사용

search.map { $0.icon }

  .drive(iconLabel.rx.text)

  .disposed(by: bag)

 

search.map { "\($0.humidity)%"}

  .drive(humidityLabel.rx.text)

  .disposed(by: bag)

 

search.map { $0.cityName }

  .drive(cityNameLabel.rx.text)

  .disposed(by: bag)

**5. RxSwift****와** **RxCocoa****에서의** **Traits**

 ![image-20210526201210681](C:\Users\Rhee\AppData\Roaming\Typora\typora-user-images\image-20210526201210681.png)

 
