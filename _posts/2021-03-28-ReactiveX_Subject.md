---
layout: post
title: "ReactiveX&#95;Subject"
subtitle: "Subject"
date: 2021-03-28 21:36:13 0400
background: '/img/posts/06.jpg'
categories: [ReactiveX]
tag: [iOS]
---


# ReactiveX&#95;Subject

---

* # 주제

  주제(subject)는 옵저버나 Observable처럼 행동하는 ReactiveX의 일부 구현체에서 사용 가능한 일종의 교각 혹은 프록시라고 볼 수 있는데, 그 이유는 주제는 옵저버이기 때문에 하나 이상의 Observable을 구독할 수 있으며 동시에 Observable이기도 하기 때문에 항목들을 하나 하나 거치면서 재배출하고 관찰하며 새로운 항목들을 배출할 수도 있다.

  하나의 주제는 하나의 Observable을 구독하면서, (Observable이 "차가운" Observable인 경우 즉, 옵저버가 구독을 시작하기 전까지 항목들의 배출을 지연시키는 Observable일 경우)Observable이 항목들을 배출시키도록 동작시킨다. 그 결과로 인해 원래는 "차가운" Observable이었던 주제를 "뜨거운" Observable로 만들기도 한다.

  #### 참고

  - [To Use or Not to Use Subject](http://davesexton.com/blog/post/To-Use-Subject-Or-Not-To-Use-Subject.aspx) from Dave Sexton’s blog
- [Introduction to Rx: Subject](http://www.introtorx.com/Content/v1.0.10621.0/02_KeyTypes.html#Subject)
  - [101 Rx Samples: ISubject and ISubject](http://rxwiki.wikidot.com/101samples#toc44)
- [Advanced RxJava: Subject](http://akarnokd.blogspot.hu/2015/06/subjects-part-1.html) by Dávid Karnok
  - [Using Subjects](http://xgrommx.github.io/rx-book/content/getting_started_with_rxjs/subjects.html) by Dennis Stoyanov
  
  ## 주제의 종류
  
  모두 4 종류의 `Subject`가 존재하며, 각각의 Subject는 특정 상황에 맞도록 설계되었다. 그렇기 때문에 모든 상황에서 아무 주제(Subject)를 임의대로 사용할 수 없으며 일부 구현체는 주제를 다른 이름으로 부르기도 한다(예를 들어, RxScala는 "주제(Subject)"를 "발행주제(PublishSubject)"로 부른다).
  
  ### AysncSubject
  
![img](http://reactivex.io/documentation/operators/images/S.AsyncSubject.png)
  
`AsyncSubject`는 소스 Observable로부터 배출된 마지막 값(만)을 배출하고 소스 Observalbe의 동작이 완료된 후에야 동작한다. (만약, 소스 Observable이 아무 값도 배출하지 않으면 `AsyncSubject` 역시 아무 값도 배출하지 않는다.)
  
![img](http://reactivex.io/documentation/operators/images/S.AsyncSubject.e.png)
  
또한 `AsyncSubject`는 맨 마지막 값을 뒤 이어 오는 옵저버에 전달하는데, 만약 소스 Observable이 오류로 인해 종료될 경우 `AsyncSubject`는 아무 항목도 배출하지 않고 발생한 오류를 그대로 전달한다.
  
#### 참고
  
- [Introduction to Rx: AsyncSubject](http://www.introtorx.com/Content/v1.0.10621.0/02_KeyTypes.html#AsyncSubject)
  
### BehaviorSubject
  
![img](http://reactivex.io/documentation/operators/images/S.BehaviorSubject.png)
  
옵저버가 `BehaviorSubject`를 구독하기 시작하면, 옵저버는 소스 Observable이 가장 최근에 발행한 항목(또는 아직 아무 값도 발행되지 않았다면 맨 처음 값이나 기본 값)의 발행을 시작하며 그 이후 소스 Observable(들)에 의해 발행된 항목들을 계속 발행한다.
  
  ![img](http://reactivex.io/documentation/operators/images/S.BehaviorSubject.e.png)
  
만약, 소스 Observable이 오류 때문에 종료되면 `BehaviorSubject`는 아무런 항목도 배출하지 않고 소스 Observable에서 발생한 오류를 그대로 전달한다.
  
#### 참고
  
  - [Introduction to Rx: BehaviorSubject](http://www.introtorx.com/Content/v1.0.10621.0/02_KeyTypes.html#BehaviorSubject)
  
  ### PublishSubject
  
![img](http://reactivex.io/documentation/operators/images/S.PublishSubject.png)
  
`PublishSubject`는 구독 이후에 소스 Observable(들)이 배출한 항목들만 옵저버에게 배출한다.
  
  주의할 점은, `PublishSubject`는 (이를 막지 않는 이상) 생성 시점에서 즉시 항목들을 배출하기 시작할 것이고 이런 특성 때문에 주제가 생성되는 시점과 옵저버가 이 주제를 구독하기 시작하는 그 사이에 배출되는 항목들을 잃어 버릴 수 있다는 단점이 있다. 따라서, 소스 Observable이 배출하는 모든 항목들의 배출을 보장해야 한다면 [`Create`](http://reactivex.io/documentation/ko/operators/create.html)을 사용해서 명시적으로 "차가운" Observable(항목들을 배출하기 전에 모든 옵저버가 구독을 시작했는지 체크한다)을 생성하거나, PublishSubject 대신 `ReplaySubject`를 사용해야 한다.
  
  ![img](http://reactivex.io/documentation/operators/images/S.PublishSubject.e.png)

  만약, 소스 Observable이 오류 때문에 종료되면 `BehaviorSubject`는 아무런 항목도 배출하지 않고 소스 Observable에서 발생한 오류를 그대로 전달한다.

  ### ReplaySubject
  
  ![img](http://reactivex.io/documentation/operators/images/S.ReplaySubject.png)
  
  `ReplaySubject`는 옵저버가 구독을 시작한 시점과 관계 없이 소스 Observable(들)이 배출한 모든 항목들을 모든 옵저버에게 배출한다.
  
  `ReplaySubject`는 몇 개의 생성자 오버로드를 제공하는데 이를 통해, 재생 버퍼의 크기가 특정 이상으로 증가할 경우, 또는 처음 배출 이후 지정한 시간이 경과할 경우 오래된 항목들을 제거한다.
  
  만약, `ReplaySubject`을 옵저버로 사용할 경우, 멀티 스레드 환경에서는 [Observable 계약](http://reactivex.io/documentation/contract.html) 위반과 주제에서 어느 항목 또는 알림을 먼저 재생해야 하는지 알 수 없는 모호함이 동시에 발생할 수 있기 때문에 (비순차적) 호출을 유발시키는 `onNext`(또는 그 외 `on`) 메서드를 사용하지 않도록 주의해야 한다.
  
#### 참고
  
- [Introduction to Rx: ReplaySubject](http://www.introtorx.com/Content/v1.0.10621.0/02_KeyTypes.html#ReplaySubject)
