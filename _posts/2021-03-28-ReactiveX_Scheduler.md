---
layout: post
title: "ReactiveX&#95;Scheduler"
subtitle: "Scheduler"
date: 2021-03-28 21:46:13 0400
background: '/img/posts/06.jpg'
categories: [ReactiveX]
tag: [iOS]
---


# ReactiveX&#95;Scheduler

---

* # 스케줄러

  Observable 연산자 체인에 멀티스레딩을 적용하고 싶다면, 특정 스케줄러를 사용해서 연산자(또는 특정 Observable)를 실행하면 된다.

  ReactiveX의 일부 Observable 연산자는 사용할 스케줄러를 파라미터로 전달 받기도 하는데, 이 연산자들은 자신이 처리할 연산의 일부 또는 전체를 전달된 스케줄러 내에서 실행한다.

  기본적으로, Observable과 연산자 체인은 이처럼 스케줄러를 통해 동작하고 `Subscribe` 메서드가 호출되는 스레드를 사용해서 옵저버에게 알림을 보낸다. SubscribeOn 연산자는 다른 스케줄러를 지정해서 Observable이 처리해야 할 연산자들을 실행 시킨다. 그리고, ObserveOn 연산자는 Observable이 옵저버에게 알림을 보낼때 사용 할 스케줄러를 명시한다.

  아래 그림이 보여주듯, SubscribeOn 연산자는 Observable이 연산을 위해 사용할 스레드를 지정하며, 연산자 체인 중 아무 곳에서 호출해도 문제되지 않는다. 하지만, ObserveOn 연산자는 연산자 체인 중 Observable이 사용할 스레드가 호출 체인 중 어느 시점에서 할당되는지에 따라 *그 후*에 호출되는 연산자는 영향을 받는다. 그렇기 때문에, 어쩌면 여러분은 특정 연산자를 별도의 스레드에서 실행 시키기 위해 연산자 체인 중 한 군데 이상에서ObserveOn을 호출하게 될 것이다.

  ![ObserveOn and SubscribeOn](http://reactivex.io/documentation/operators/images/schedulers.png)

  #### 참고
  
  - [Introduction to Rx: Scheduling and Threading](http://www.introtorx.com/Content/v1.0.10621.0/15_SchedulingAndThreading.html)
  - [Rx Workshop: Schedulers](http://channel9.msdn.com/Series/Rx-Workshop/Rx-Workshop-Schedulers)
  - [Using Schedulers](http://xgrommx.github.io/rx-book/content/getting_started_with_rxjs/scheduling_and_concurrency.html) by Dennis Stoyanov
