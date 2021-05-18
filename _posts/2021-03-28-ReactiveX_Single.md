---
layout: post
title: "ReactiveX&#95;Single"
subtitle: "Single"
date: 2021-03-28 21:26:13 0400
background: '/img/posts/06.jpg'
categories: [ReactiveX]
tag: [iOS]
---


# ReactiveX&#95;Single

---

* # Single

  RxJava(그리고 RxGroovy나 RxScala 같은 reactivex의 구현체들)는 [Observable](http://reactivex.io/documentation/ko/observable.html)과 유사한 “Single”을 제공한다.

  Single은 Obvservable의 한 형태이지만, Observable처럼 존재하지 않는 곳에서부터 무한대까지의 임이의 연속된 값들을 배출하는 것과는 달리, 항상 한 가지 값 또는 오류 알림 둘 중 하나만 배출한다.

  이런 이유 때문에, Single을 구독할 때는 Observable을 구독할 때 사용하는 세 개의 메서드(`onNext`, `onError`, 그리고 `onCompleted`) 대신 다음의 두 메서드만 사용할 수 있다:

  - onSuccess

    Single은 자신이 배출하는 하나의 값을 이 메서드를 통해 전달한다

  - onError
  
    Single은 항목을 배출할 수 없을 때 이 메서드를 통해 Throwable 객체를 전달한다
  
  Single은 이 메서드 중 하나만 그리고, 한 번만 호출한다. 메서드가 호출되면 Single의 생명주기는 끝나고 구독도 종료된다.
  
  # Single 연산자를 통한 구성
  
  Observable과 마찬가지로, Single도 다양한 연산자들을 제공한다. 이 중 어떤 연산자들은 Observable과 Single을 섞어서 사용할 수 있도록 Observable의 영역과 Single의 영역을 연결하는 인터페이스 역할을 수행한다:
  
  | 연산자                       | 리턴 타입    | 설명                                                         |
  | :--------------------------- | :----------- | :----------------------------------------------------------- |
  | `compose`                    | `Single`     | 이 연산자를 통해 사용자 정의 연산자를 만들 수 있다           |
  | `concat` 그리고 `concatWith` | `Observable` | 여러 개의 Single이 배출한 항목들을 Observable이 배출하는 형태로 변환한다 |
  | `create`                     | `Single`     | 명시적으로 subscriber 메서드 호출을 통해 Single을 생성한다   |
  | `delay`                      | `Single`     | Single의 항목 배출을 명시된 시간 만큼 미룬다                 |
  | `doOnError`                  | `Single`     | onError 메서드가 호출될 때 전달한 메서드를 실행하는 Single을 리턴한다 |
  | `doOnSuccess`                | `Single`     | onSuccess 메서드가 호출될 때 전달한 메서드를 실행하는 Single을 리턴한다 |
  | `error`                      | `Single`     | 오류를 구독하는 구독자에게 즉시 오류가 발생했음을 알리는 Single을 리턴한다 |
  | `flatMap`                    | `Single`     | Single이 배출하는 항목에 적용된 함수의 결과를 담은 Single을 리턴한다 |
  | `flatMapObservable`          | `Observable` | Single이 배출한 항목에 적용된 함수의 결과를 담은 Observable을 리턴한다 |
  | `from`                       | `Single`     | 퓨처를 Single로 변환한다                                     |
  | `just`                       | `Single`     | 명시한 항목을 배출하는 Single을 리턴한다                     |
  | `map`                        | `Single`     | 소스 Single이 배출한 항목에 적용된 함수의 결과를 배출하는 Single을 리턴한다 |
  | `merge`                      | `Single`     | 두 번째 Single을 배출하는 Single을 전달 받은 후, 두 번째 Single이 배출한 항목을 배출하는 Single을 리턴한다 |
  | `merge` 그리고 `mergeWith`   | `Observable` | 여러 Single이 배출한 항목들을 머지 후 Observable을 리턴하며 리턴된 Observable은 머지된 Single들의 항목들을 배출한다 |
  | `observeOn`                  | `Single`     | 특정 [스케줄러](http://reactivex.io/documentation/ko/scheduler.html) 상에서 구독자 메서드를 호출하는 Single을 리턴한다 |
  | `onErrorReturn`              | `Single`     | 명시된 항목을 배출하는 Single을 오류 알림을 보내는 Single로 변환한다 |
  | `subscribeOn`                | `Single`     | 특정 [스케줄러](http://reactivex.io/documentation/ko/scheduler.html) 상에서 동작하는 Single을 리턴한다 |
  | `timeout`                    | `Single`     | 특정 시간 동안 소스 Single이 항목을 배출하지 못하면 오류 알림을 보내는 Single을 리턴한다 |
  | `toSingle`                   | `Single`     | 항목 하나를 배출하는 Observable을 Single로 변환한다          |
  | `toObservable`               | `Observable` | Single을 Observable로 변환하며 변환된 Observable은 Single이 배출 할 항목을 배출한 후 종료된다 |
  | `zip` 그리고 `zipWith`       | `Single`     | 두 개 이상의 Single들이 배출한 항목에 적용된 함수의 결과를 담은 항목을 하나 배출하는 Single을 리턴한다 |
  
  지금부터는 마블 다이어그램을 통해 이 연산자들에 대해 설명한다. 이 다이어그램은 마블 다이어그램을 통해 어떻게 Single이 표현되는지 보여준다:
  
  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.legend.png)
  
  ## compose
  
  ## concat 그리고 concatWith
  
  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.concat.png)

  이 연산자의 인스턴스 버전도 존재한다:

  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.concatWith.png)
  
  ## create
  
  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.create.png)
  
  ## delay
  
  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.delay.png)
  
  다른 버전의 연산자로 특정 [스케줄러](http://reactivex.io/documentation/ko/scheduler.html) 상에서 Single의 배출을 지연시킨다:
  
  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.delay.s.png)
  
  ## doOnError
  
  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.doOnError.png)
  
  ## doOnSuccess
  
  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.doOnSuccess.png)
  
  ## error
  
  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.error.png)
  
  ## flatMap
  
  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.flatMap.png)
  
  ## flatMapObservable
  
  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.flatMapObservable.png)
  
  ## from
  
  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.from.Future.png)
  
  [스케줄러](http://reactivex.io/documentation/ko/scheduler.html)를 파라미터로 받는 다른 버전 역시 존재한다: ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.from.Future.s.png)

  ## just

  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.just.png)

  ## map

  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.map.png)

  ## merge 그리고 mergeWith

  한 버전은 Single을 파라미터로 전달 받는데, 이때 전달된 Single은 두 번째 Single을 배출한다. 그 후 이 연산자는 두 번째 Single이 배출한 항목을 배출하는 또 다른 Single을 리턴한다:

  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.merge.oo.png)

  또 다른 버전은 두 개 이상의 Single을 전달 받고 머지한 후에 Observable을 리턴하는데 이때 리턴되는 Observable은 (임의의 순서에 따라)소스 Single이 배출한 항목들을 배출한다:

  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.merge.png)

  ## observeOn

  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.observeOn.png)

  ## onErrorReturn

  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.onErrorReturn.png)

  ## subscribeOn

  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.subscribeOn.png)

  ## timeout

  timeout 연산자는 Single이 구독된 이후 특정 시간 동안 항목이 배출되지 않으면 Single이 오류 알림을 보낸다. 특정 시간을 명시해 타임 아웃을 설정할 수 있다:

  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.timeout.1.png)

  연산자에 특정 [스케줄러스](http://reactivex.io/documentation/ko/scheduler.html)를 명시할 수도 있다:

  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.timeout.1s.png)

  timeout 연산자 중 하나는 타임 아웃이 발생하면 오류 알림을 보내는 대신 백업 싱글로 대체시키는 기능을 제공한다:

  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.timeout.2.png)

  또한, 이 연산자는 [스케줄러](http://reactivex.io/documentation/ko/scheduler.html) 명시적인 버전도 제공한다:

  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.timeout.2s.png)

  ## toObservable

  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.toObservable.png)

  ## zip zipWith

  ![img](https://raw.github.com/wiki/ReactiveX/RxJava/images/rx-operators/Single.zip.png)
