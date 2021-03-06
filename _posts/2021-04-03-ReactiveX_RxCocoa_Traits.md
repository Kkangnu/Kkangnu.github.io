---
layout: post
title: "Rxcocoa Traits"
subtitle: "RxCocoa 특성"
date: 2021-04-03 21:31:13 0400
background: '/img/posts/06.jpg'
categories: [ReactiveX]
tag: [iOS]
---

# RxSwift. Traits 정리하기

## Triat, Single, Completable, Maybe, Driver, ControlProperty, ControlEvent

## 목차

- [General](https://devmjun.github.io/archive/Traits#general)

- - [Why](https://devmjun.github.io/archive/Traits#why)
  - [How       they work](https://devmjun.github.io/archive/Traits#how-they-work)

- [RxSwift      traits](https://devmjun.github.io/archive/Traits#rxswift-traits)

- - [Single](https://devmjun.github.io/archive/Traits#single)

  - - [Creating        a Single](https://devmjun.github.io/archive/Traits#creating-a-single)

  - [Completable](https://devmjun.github.io/archive/Traits#completable)

  - - [Creating        a Completable](https://devmjun.github.io/archive/Traits#creating-a-completable)

  - [Maybe](https://devmjun.github.io/archive/Traits#maybe)

  - - [Creating        a Maybe](https://devmjun.github.io/archive/Traits#creating-a-maybe)

- [RxCocoa      traits](https://devmjun.github.io/archive/Traits#rxcocoa-traits)

- - [Driver](https://devmjun.github.io/archive/Traits#driver)

  - - [Why        is it named Driver](https://devmjun.github.io/archive/Traits#why-is-it-named-driver)
    - [Practical        usage example](https://devmjun.github.io/archive/Traits#practical-usage-example)

  - [ControlProperty       / ControlEvent](https://devmjun.github.io/archive/Traits#controlproperty--controlevent)



## General

### Why

Swift는 애플리케이션의 정확성과 안정성 향상시키고 Rx를 보다 직관적이고 직접적인 경험으로 사용하는데 사용할수 있는 강력한 유형 시스템을 갖추고 있습니다. Traits는 모든 경계에서 사용할수 있는 원시 Observable과 비교할때 인터페이스 경계에서 observable 프로퍼티를 전달하고 보장하며, 문법적으로도 더 쉽고 구체적인 사용 사례를 타켓팅하는데 도움이됩니다…

*Note: Trait* *의* *일부는* *documnets(* *예를들어서* `*Driver*`*)* *에* *설명되어* *있습니다* *.* [*https://github.com/ReactiveX/RxSwift/tree/master/RxCocoa*](https://github.com/ReactiveX/RxSwift/tree/master/RxCocoa)

### How they work

Traits은 간단하게 struct로 읽기전용 Observable sequence property와 함께 렙핑 되어있습니다.

```
struct Single<Element> {
    let source: Observable<Element>
}
 
struct Driver<Element> {
    let source: Observable<Element>
}
...
```

observable sequence에 대한 빌더 패턴 구현의 일종이라고 생각할수 있습니다. Trait이 만들어지면, `asObservable()`을 호출하면 그것을 흔한 observable seqeunce로 다시 변환합니다.



## RxSwift traits

### Single

`Single`은 Observable의 변형으로 일련의 요소를 방출하는 대신 `항상`` ``단일`` ``요소` 또는 `오류를`` ``방출`하도록 보장합니다.

- 정확히 하나의 요소 또는     error를 방출합니다.
- 부수작용을 공유하지 않습니다.

Single을 사용하는 일반적인 예는 응답, 오류만 반환할수 있는 HTTP 요청을 수행하는데 사용되지만 단일요소를 사용하여 무한 스트림 요소가 아닌 단일 요소만 관리하는 경우를 모델하는데 사용할수 있습니다.

#### Creating a Single

Single을 만드는것은 Observable을 만드는것과 비슷합니다. 아래의 간단한 예제를 살펴 봅니다.

```swift
func getRepo(_ repo: String) -> Single<[String: Any]> {
    return Single<[String: Any]>.create { single in
        let task = URLSession.shared.dataTask(with: URL(string: "https://api.github.com/repos/\(repo)")!) { data, _, error in
            if let error = error {
                single(.error(error))
                return
            }
 
            guard let data = data,
                  let json = try? JSONSerialization.jsonObject(with: data, options: .mutableLeaves),
                  let result = json as? [String: Any] else {
                single(.error(DataError.cantParseJSON))
                return
            }
 
            single(.success(result))
        }
 
        task.resume()
 
        return Disposables.create { task.cancel() }
    }
}
```



위의 함수를 만들고 아래처럼 사용할수 있습니다.

```swift
getRepo("ReactiveX/RxSwift")
    .subscribe { event in
        switch event {
            case .success(let json):
                print("JSON: ", json)
            case .error(let error):
                print("Error: ", error)
        }
    }
    .disposed(by: disposeBag)
```

또는 `subscribe(onSuccess:onError:)` 을 아래 처럼 사용할수 있습니다.

```swift
getRepo("ReactiveX/RxSwift")
    .subscribe(onSuccess: { json in
                   print("JSON: ", json)
               },
               onError: { error in
                   print("Error: ", error)
               })
    .disposed(by: disposeBag)
```

subscription은 `SingleEvent` 타입의 요소를 포함하는 `.success`, `.error`일 수 있는 `SingleEvent`열거를 제공합니다. 첫번째 이벤트 이후에는 더이상 이벤트가 발생하지 않습니다.

그것은 또한 원본 Observable sequence에asSingle()를 사용하여 `Single`로 변경하여 사용이 가능합니다.



### Completable

`Completable`은 변화무쌍한 `Observable` 입니다. `complete` 하거나, `error` 를 방출하고, 아무 요소도 방출하지 않는것을 보장합니다.

- 제로 요소 방출
- 완료 이벤트 또는 에러 방출
- 부수작용을 공유하지 않습니다.

`Completable`은 완료에 따른 요소에 신경쓰지 않은 경우 사용하면 유용합니다. 요소를 내보낼수 없는 경우 Observable를 사용하여 비교할수 있습니다.

#### Creating a Completable

Completable을 생성하는것은 Observable과 비슷합니다. 아래의 간단한 예제를 봐주세요.

```swift
func cacheLocally() -> Completable {
    return Completable.create { completable in
       // Store some data locally
       ...
       ...
 
       guard success else {
           completable(.error(CacheError.failedCaching))
           return Disposables.create {}
       }
 
       completable(.completed)
       return Disposables.create {}
    }
}
```

위처럼 만들고 아래 처럼 사용할수 있습니다.

```swift
cacheLocally()
    .subscribe { completable in
        switch completable {
            case .completed:
                print("Completed with no error")
            case .error(let error):
                print("Completed with an error: \(error.localizedDescription)")
        }
    }
    .disposed(by: disposeBag)
```

`subscribe(onCompleted:onError:)` 를 사용하여 아래처럼 사용합니다.

```swift
cacheLocally()
    .subscribe(onCompleted: {
                   print("Completed with no error")
               },
               onError: { error in
                   print("Completed with an error: \(error.localizedDescription)")
               })
    .disposed(by: disposeBag)
```

구독은 `CompletableEvent`의 열거형을 제공합니다. 열거형은 `.completed` -> 오류없이 완료된 작업 또는 `.error`중 하나 일 수 있습니다. 첫번째 이벤트를 넘어서 더이상의 이벤트가 발생하지 않습니다.



### Maybe

어쩌면 `Single`과 `Completable`사이에 있는 Observable의 변형입니다. 단일 요소를 방출하거나 요소를 방출하지 않고 완료하거나 오류를 낼수 있습니다. ( Single, Completable 두가지 모두 가지고 있음)

*Note:* *이* *세가지* *이벤트중* *하나라고* *방출되면* `*Maybe*`*는* *종료됩니다**.* *즉**,* *완료된* *요소도* *요소를* *방출할수* *없고**,* *요소를* *내* *보낸* *요소도* *완료* *이벤트를* *내보낼수* *없습니다**.(????* *뭔역활이지**..)*

- 완료된 이벤트, 싱글이벤트 또는 오류를 방출합니다.
- 부수작용을 공유하지 않습니다.

Maybe를 요소를 방출하는 연사자 모델을 위해 사용할수 있습니다. 그러나 요소들을 방출하는것이 꼭 필요해야하는것은 아닙니다.



#### Creating a Maybe

Maybe는 Observable만드는것과 비슷합니다. 아래의 예제를 확인합니다.

```swift
func generateString() -> Maybe<String> {
    return Maybe<String>.create { maybe in
        maybe(.success("RxSwift"))
 
        // OR
 
        maybe(.completed)
 
        // OR
 
        maybe(.error(error))
 
        return Disposables.create {}
    }
}
```

아래 처럼 사용할수 있습니다.

```swift
generateString()
    .subscribe { maybe in
        switch maybe {
            case .success(let element):
                print("Completed with element \(element)")
            case .completed:
                print("Completed with no element")
            case .error(let error):
                print("Completed with an error \(error.localizedDescription)")
        }
    }
    .disposed(by: disposeBag)
```

`subscribe(onSuccess:onError:onCompleted:)`를 사용합니다.

```swift
generateString()
    .subscribe(onSuccess: { element in
                   print("Completed with element \(element)")
               },
               onError: { error in
                   print("Completed with an error \(error.localizedDescription)")
               },
               onCompleted: {
                   print("Completed with no element")
               })
    .disposed(by: disposeBag)
```

Observable sequence를 `.asMaybe()`를 사용하여 변환가능합니다.



## RxCocoa traits

### Driver

Driver은 아마 가장 정요한 특성입니다. UI레이어에 reactive 코드를 작성하는 직관적인 방법을 제공하거나 애플리케이션에서 데이터 스트림을 모델링하는 모든 경우를 위한것입니다.

- 오류가 없습니다.(오류를 방출하지 않는다는 의미)
- observe는     Main scheulder에서 발생합니다.
- 부수작용을 공유합니다(`shareReplayLatestWhileConnected`)

#### Why is it named Driver

의도된 사용사례는 애플리케이션을 구동(drive)하는 시퀀스를 모델링하는 사례였습니다.

E.g.

- Drive UI from     CoreData model
- Drive UI     using values from other UI elements (bindings) …

정상적인 OS drvicer와 마찬가지로 시퀀스 오류가 발생하면 애플리케이션이 사용자 입력에 응답하지 않습니다.

UI요소와 애플리케이션 논리가 일반적으로 스레드로부터 안전하지 않기 때문에 이러한 요소가 mainThread에서 관찰되는것은 매우 중요합니다.

또한 `Drvier`은 부수작용을 공유하는 observable sequence를 만듭니다.

E.g.

#### Practical usage example

아래의 코드는 전형적인 예시입니다.

```swift
let results = query.rx.text
    .throttle(0.3, scheduler: MainScheduler.instance)
    .flatMapLatest { query in
        fetchAutoCompleteItems(query)
    }
 
results
    .map { "\($0.count)" }
    .bind(to: resultCount.rx.text)
    .disposed(by: disposeBag)
 
results
    .bind(to: resultsTableView.rx.items(cellIdentifier: "Cell")) { (_, result, cell) in
        cell.textLabel?.text = "\(result)"
    }
    .disposed(by: disposeBag)
```

이 코드의 의도된 동작은 다음과 같습니다.

- 사용자 입력 조절
- 서버에 적속하여 사용자 결과목록 가져오기(검색어당 한번)
- 결과를 두개의     UI요소(result     tableview 및     label)에 바인딩합니다.

그렇다면 이 코드의 문제점은 무엇입니까?

- `fetchAutoCompleteItems` observable     sequence errors 방출( 연결 실패, 파싱에러 등     ), 오류로 인해 모든 항목의 바인딩이 해제되고     UI가 더이상 새로운 쿼리에 응답하지 않습니다.
- `fetchAutoCompleteItems`가 일부     background thread에서 결과를 반환하면 Result가 비 결정적 크래시를 유발할 수 있는 백그라운드 스레드의     UI요소에 바인딩됩니다.
- Result는 두개의     UI요소에 바인딩됩니다. 즉 각 사용자 쿼리에 대해 두개의     HTTP 요청이 만들어지고, 각     UI요소에 하나씩 의도된 동작이 아닙니다.

코드의 더 적절한 부분은 아래와 같습니다.

```swift
let results = query.rx.text
    .throttle(0.3, scheduler: MainScheduler.instance)
    .flatMapLatest { query in
        fetchAutoCompleteItems(query)
            .observeOn(MainScheduler.instance)  // results are returned on MainScheduler
            .catchErrorJustReturn([])           // in the worst case, errors are handled
    }
    .shareReplay(1)                             // HTTP requests are shared and results replayed
                                                // to all UI elements
 
results
    .map { "\($0.count)" }
    .bind(to: resultCount.rx.text)
    .disposed(by: disposeBag)
 
results
    .bind(to: resultsTableView.rx.items(cellIdentifier: "Cell")) { (_, result, cell) in
        cell.textLabel?.text = "\(result)"
    }
    .disposed(by: disposeBag)
```

이러한 모든 요구사항이 대형 시스템에서 제대로 처리되는지 확인하는 것은 어려울 수 있지만 컴파일러와 traits을 사용하여 이러한 요구 사항이 충족되었음을 입증하는 간단한 방법이 있습니다.

아래의 코드는 거의 같습니다.

```swift
let results = query.rx.text.asDriver()        // This converts a normal sequence into a `Driver` sequence.
    .throttle(0.3, scheduler: MainScheduler.instance)
    .flatMapLatest { query in
        fetchAutoCompleteItems(query)
            .asDriver(onErrorJustReturn: [])  // Builder just needs info about what to return in case of error.
    }
 
results
    .map { "\($0.count)" }
    .drive(resultCount.rx.text)               // If there is a `drive` method available instead of `bindTo`,
    .disposed(by: disposeBag)              // that means that the compiler has proven that all properties
                                              // are satisfied.
results
    .drive(resultsTableView.rx.items(cellIdentifier: "Cell")) { (_, result, cell) in
        cell.textLabel?.text = "\(result)"
    }
    .disposed(by: disposeBag)
```

그래서 여기서 무슨일이 일어나는걸까?

첫번째로 `asDriver`매소드는 `ControlProperty` trait을 `Driver` trait으로

```swift
query.rx.text.asDriver()
```

해야할 특별한 일이 없습니다. `Driver`에는 `ControlProperty`의 모든 속성과, 추가로 더 많은 trait이 있습니다. 기본적으로 observable sequence는 Driver Traits으로 감싸인데 그게 전부입니다.

두번째로 변경되는것은

```swift
.asDriver(onErrorJustReturn: [])
```

모든 observable sequence는 3개의 속성을 만족하는 `Driver` trait으로 변경할수 있습니다.

- 오류를 방출하지 않습니다.
- main     scheduler 에서     observe합니다.
- 부수작용을 공유합니다.(`shareReplayLatestWhileConnected`)

그렇다면 이러한 속성이 어떻게 충족되는지 확인하려면 어떻게 해야하나요? 그냥 정상적인 Rx연산자를 사용하고, asDriver(onErrorJustReturn: [])은 다음 코드와 동일합니다.

```swift
let safeSequence = xs
  .observeOn(MainScheduler.instance)       // observe events on main scheduler
  .catchErrorJustReturn(onErrorJustReturn) // can't error out
  .shareReplayLatestWhileConnected()       // side effects sharing
return Driver(raw: safeSequence)           // wrap it up
```

마지막 부분은 bindTo를 사용하는 대신 drive를 사용하고 있습니다.

`drive`는 `Driver trait`에만 정의됩니다. 즉, 코드의 어딘가에서 드라이브를 보게되면 observable sequence가 오류없이 절대로 UI 요소에 바인딩하기에 안전한 main Thread에서 observe합니다.

하지만 누군가 drive method를 `ObservableType` 또는 다른 인터페이스 에서 작동하도록 정의할수 있으므로 결과를 `let results: Driver<[Results]>`으로 임시 정의를 만드는것이 안전합니다.

그러나 UI에 바인딩하기전에 완전한 입증 요소가 필요할것입니다. 그러나 이것이 현실적인 시나리오인지 여부를결정하기 위해 독자에게 맡길것입니다..(걍 예제좀 짜주지..)

## ControlProperty / ControlEvent

### ControlProperty

UI요소의 속성을 나타내기위한 `Observable`/`ObservableType`을 위한 Trait

값의 순서는 초기 제어값과 사용자 시작 값 변경을 나타냅니다. 프로그램상의 값 변화는 리포트되지 않았습니다.

그것의 속성은 아래와 같습니다.

- 결코 실패하지 않습니다(     it never fails )

- `shareReplay(1)` 행동

- - 구독할때 [stateful](http://egloos.zum.com/outspace/v/3124886)상태를 유지합니다(구독자 호출하고 있음)      마지막 요소가 생성되면 즉시 재생(replayed)      됩니다.

- 할당해제되는     contol의 완성하는     sequence(?)

- 절대 오류가 발생하지 않습니다.

- MainScheduler.instance에서 이벤트를 전달합니다.

#### Practical usage example

`UISearchBar + Rx` 와 `UISegmentedControl + Rx`에서 매우 훌륭한 실제 예제를 찾을 수 있습니다.

```swift
extension Reactive where Base: UISearchBar {
    /// Reactive wrapper for `text` property.
    public var value: ControlProperty<String?> {
        let source: Observable<String?> = Observable.deferred { [weak searchBar = self.base as UISearchBar] () -> Observable<String?> in
            let text = searchBar?.text
            
            return (searchBar?.rx.delegate.methodInvoked(#selector(UISearchBarDelegate.searchBar(_:textDidChange:))) ?? Observable.empty())
                    .map { a in
                        return a[1] as? String
                    }
                    .startWith(text)
        }
 
        let bindingObserver = UIBindingObserver(UIElement: self.base) { (searchBar, text: String?) in
            searchBar.text = text
        }
        
        return ControlProperty(values: source, valueSink: bindingObserver)
    }
}
extension Reactive where Base: UISegmentedControl {
    /// Reactive wrapper for `selectedSegmentIndex` property.
    public var selectedSegmentIndex: ControlProperty<Int> {
        return value
    }
    
    /// Reactive wrapper for `selectedSegmentIndex` property.
    public var value: ControlProperty<Int> {
        return UIControl.rx.value(
            self.base,
            getter: { segmentedControl in
                segmentedControl.selectedSegmentIndex
            }, setter: { segmentedControl, value in
                segmentedControl.selectedSegmentIndex = value
            }
        )
    }
}
```

### ControlEvent

UI요소의 이벤트를 나타내는 `Observable`/`ObservableType`을 위한 Traits

속성은 다음과 같습니다.

- 결코 실패하지 않습니다.
- 구독자에게 초기값을 전송하지 않습니다.
- 할당해제되는     Control의 완성하는 시퀀스
- 절대 오류가 발생하지 않습니다.
- MainScheduler.instance     에서 이벤트를 전달합니다.

`ControlEvent`의 구현은 (`subscribeOn(ConcurrentMainScheduler.instance)`)에서 이벤트 시퀀스가 구독되도록 합니다.

#### Practical usage example

아래는 전형적인 예제입니다.

```swift
public extension Reactive where Base: UIViewController {
    
    /// Reactive wrapper for `viewDidLoad` message `UIViewController:viewDidLoad:`.
    public var viewDidLoad: ControlEvent<Void> {
        let source = self.methodInvoked(#selector(Base.viewDidLoad)).map { _ in }
        return ControlEvent(events: source)
    }
}
```

UICollectionView + Rx 에서 아래와 같은 방법을 찾았다…

```swift
extension Reactive where Base: UICollectionView {
    
    /// Reactive wrapper for `delegate` message `collectionView:didSelectItemAtIndexPath:`.
    public var itemSelected: ControlEvent<IndexPath> {
        let source = delegate.methodInvoked(#selector(UICollectionViewDelegate.collectionView(_:didSelectItemAt:)))
            .map { a in
                return a[1] as! IndexPath
            }
        
        return ControlEvent(events: source)
    }
}
```

 

내용: [[Mjun' s Blog](https://devmjun.github.io/)]

출처: [[Mjun' s Blog](https://devmjun.github.io/)]
