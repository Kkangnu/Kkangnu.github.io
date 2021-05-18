---
layout: post
title: "ReactiveX&#95;Operators"
subtitle: "Operators"
date: 2021-03-28 21:16:13 0400
background: '/img/posts/06.jpg'
categories: [ReactiveX]
tag: [iOS]
---


# ReactiveX&#95;Operators

---

* # 연산자 소개

  ReactiveX를 지원하는 언어 별 구현체들은 다양한 연산자들을 제공하는데, 이 중에는 공통적으로 제공되는 연산자도 있지만 반대로 특정 구현체에서만 제공하는 연산자들도 존재한다. 또한, 언어별 구현체들은 이미 언어에서 제공하는 메서드의 이름과 유사한 형태로 연산자의 네이밍 컨벤션을 유지하고 있다.

  ## 연산자 체인

  거의 모든 연산자들은 Observable 상에서 동작하고 Observable을 리턴한다. 이 접근 방법은 연산자들을 연달아 호출 할 수 있는 연산자 체인을 제공한다. 연산자 체인에서 각각의 연산자는 이전 연산자가 리턴한 Observable을 기반으로 동작하며 동작에 따라 Observable을 변경한다.

  특정 클래스가 제공하는 여러 메서드들을 호출하여 클래스의 항목들을 변경하는 빌더 패턴과 같은 것도 존재한다. 빌더 패턴 역시 연산자 체인과 유사하게 메서드 체인을 제공한다. 하지만, 빌더 패턴의 메서드 체인과는 달리 Observable의 연산자 체인은 *호출 순서*에 따라 실행 결과가 달라지는 차이점이 있다.

  따라서, Observable의 연산자 체인은 원본 Observable과 독립적으로 실행될 수 없고 *순서대로* 실행되어야 한다. 왜냐하면, 이미 이해하고 있듯이 연산자 체인에서 먼저 실행된 연산자가 리턴한 Observable을 기반으로 다음 연산자가 동작하기 때문이다.

  ## ReactiveX의 연산자
  
  이 페이지에서는 먼저 ReactiveX의 “핵심” 연산자들을 나열하고, 각 연산자별로 제공된 링크를 통해 연산자들이 어떻게 동작하고 ReactiveX의 여러 언어별 구현체에서 연산자자가 어떻게 구현했는지 자세히 설명한다.
  
  그 후에는 “결정 트리”를 통해 여러분에게 필요한 적절한 연산자를 선택할 수 있는 유용한 가이드를 제공한다.
  
  마지막으로, ReactiveX의 다양한 언어 별 구현체에서 제공하는 모든 연산자들을 알파벳 순으로 소개한다. 뿐만 아니라, 각각의 연산자 역시 링크를 통해 언어 별 구현체가 제공하는 연산자와 가장 유사한 핵심 연산자에 관한 자세한 설명을 제공한다. (예를 들어, Rx.NET의 “SelectMany” 연산자가 제공하는 링크는 Rx.NET의 SelectMany 연산자가 구현하는 ReactiveX의 FlatMap 연산자에 대한 구체적으로 설명한다)
  
  만약 직접 연산자를 구현하고 싶다면 [연산자 구현하기](http://reactivex.io/documentation/ko/implement-operator.html)를 참고하기 바란다.
  
  #### 소개 순서
  
  1. [카테고리 별 연산자](http://reactivex.io/documentation/ko/operators.html#categorized)
  2. [Observable 연산자의 결정 트리](http://reactivex.io/documentation/ko/operators.html#tree)
  3. [Observable 연산자 리스트(알파벳순)](http://reactivex.io/documentation/ko/operators.html#alphabetical)
  
  # 카테고리 별 연산자
  
  ## Observable 생성
  
  새로운 Observable을 만드는 연산자들
  
  - [**`Create`**](http://reactivex.io/documentation/operators/create.html) — 직접적인 코드 구현을 통해 옵저버 메서드를 호출하여 Observable을 생성한다
  - [**`Defer`**](http://reactivex.io/documentation/operators/defer.html) — 옵저버가 구독하기 전까지는 Observable 생성을 지연하고 구독이 시작되면 옵저버 별로 새로운 Observable을 생성한다
  - [**`Empty`/`Never`/`Throw`**](http://reactivex.io/documentation/operators/empty-never-throw.html) — 아주 정확하고 제한된 행동을 하는 Observable을 생성한다
  - [**`From`**](http://reactivex.io/documentation/operators/from.html) — 다른 객체나 자료 구조를 Observable로 변환한다
  - [**`Interval`**](http://reactivex.io/documentation/operators/interval.html) — 특정 시간별로 연속된 정수형을 배출하는 Observable을 생성한다
  - [**`Just`**](http://reactivex.io/documentation/operators/just.html) — 객체 하나 또는 객채집합을 Observable로 변환한다. 변환된 Observable은 원본 객체들을 발행한다
  - [**`Range`**](http://reactivex.io/documentation/operators/range.html) — 연속된 범위(Range)의 정수를 발행하는 Observable을 생성한다
  - [**`Repeat`**](http://reactivex.io/documentation/operators/repeat.html) — 특정 항목이나 연속된 항목들을 반복적으로 배출하는 Observable을 생성한다
  - [**`Start`**](http://reactivex.io/documentation/operators/start.html) — 함수의 실행 결과를 배출하는 Observable을 생성한다
  - [**`Timer`**](http://reactivex.io/documentation/operators/timer.html) — 지정된 시간이 지나고 난 후 항목을 하나 배출하는 Observable을 생성한다
  
  ## Observable 변환
  
  Observable이 배출한 항목들을 변환하는 연산자들

  - [**`Buffer`**](http://reactivex.io/documentation/operators/buffer.html) — Observable로부터 정기적으로 항목들을 수집하고 묶음으로 만든 후에 묶음 안에 있는 항목들을 한번에 하나씩 배출하지 않고 수집된 묶음 단위로 배출한다
  - [**`FlatMap`**](http://reactivex.io/documentation/operators/flatmap.html) — 하나의 Observable이 발행하는 항목들을 여러개의 Observable로 변환하고, 항목들의 배출을 차례차례 줄 세워 하나의 Observable로 전달한다
  - [**`GroupBy`**](http://reactivex.io/documentation/operators/groupby.html) — 원본 Observable이 배출하는 항목들을 키(Key) 별로 묶은 후 Observable에 담는다. 이렇게 키 별로 만들어진 Observable들은 자기가 담고 있는 묶음의 항목들을 배출한다
  - [**`Map`**](http://reactivex.io/documentation/operators/map.html) — Observable이 배출한 항목에 함수를 적용한다
  - [**`Scan`**](http://reactivex.io/documentation/operators/scan.html) — Observable이 배출한 항목에 연속적으로 함수를 적용하고 실행한 후 성공적으로 실행된 함수의 리턴 값을 발행한다
  - [**`Window`**](http://reactivex.io/documentation/operators/window.html) — 정기적으로 Observable의 항목들을 더 작은 단위의 Observable 윈도우로 나눈 후에, 한번에 하나씩 항목들을 발행하는 대신 작게 나눠진 윈도우 단위로 항목들을 배출한다

  ## Observable 필터링

  소스 Observable에서 선택적으로 항목을 배출하는 연산자들
  
  - [**`Debounce`**](http://reactivex.io/documentation/operators/debounce.html) — Observable의 시간 흐름이 지속되는 상태에서 다른 항목들은 배출하지 않고 특정 시간 마다 그 시점에 존재하는 항목 하나를 Observable로부터 배출한다
  - [**`Distinct`**](http://reactivex.io/documentation/operators/distinct.html) — Observable이 배출하는 항목들 중 중복을 제거한 항목들을 배출한다
  - [**`ElementAt`**](http://reactivex.io/documentation/operators/elementat.html) — Obserable에서 *n*번째 항목만 배출한다
  - [**`Filter`**](http://reactivex.io/documentation/operators/filter.html) — 테스트 조건을 만족하는 항목들만 배출한다
  - [**`First`**](http://reactivex.io/documentation/operators/first.html) — 맨 첫 번째 항목 또는 조건을 만족하는 첫 번째 항목만 배출한다
  - [**`IgnoreElements`**](http://reactivex.io/documentation/operators/ignoreelements.html) — 항목들을 배출하지는 않고 종료 알림은 보낸다
  - [**`Last`**](http://reactivex.io/documentation/operators/last.html) — Observable의 마지막 항목만 배출한다
  - [**`Sample`**](http://reactivex.io/documentation/operators/sample.html) — 특정 시간 간격으로 최근에 Observable이 배출한 항목들을 배출한다
  - [**`Skip`**](http://reactivex.io/documentation/operators/skip.html) — Observable이 배출한 처음 *n*개의 항목들을 숨긴다
  - [**`SkipLast`**](http://reactivex.io/documentation/operators/skiplast.html) — Observable이 배출한 마지막 *n*개의 항목들을 숨긴다
  - [**`Take`**](http://reactivex.io/documentation/operators/take.html) — Observable이 배츨한 처음 *n*개의 항목들만 배출한다
  - [**`TakeLast`**](http://reactivex.io/documentation/operators/takelast.html) — Observable이 배출한 마지막 *n*개의 항목들만 배출한다

  ## Observables 결합

  여러 개의 소스 Observable들을 하나의 Observable로 만드는 연산자들

  - [**`And`/`Then`/`When`**](http://reactivex.io/documentation/operators/and-then-when.html) — 두 개 이상의 Observable들이 배출한 항목들을 'Pattern'과 'Plan' 중계자를 이용해서 결합한다
  - [**`CombineLatest`**](http://reactivex.io/documentation/operators/combinelatest.html) — 두 개의 Observable 중 하나가 항목을 배출할 때 배출된 마지막 항목과 다른 한 Observable이 배출한 항목을 결합한 후 함수를 적용하여 실행 후 실행된 결과를 배출한다
  - [**`Join`**](http://reactivex.io/documentation/operators/join.html) — A Observable과 B Observable이 배출한 항목들을 결합하는데, 이때 B Observable은 배출한 항목이 타임 윈도우를 가지고 있고 이 타임 윈도우가 열린 동안 A Observable은 항목의 배출을 계속한다. Join 연산자는 B Observable의 항목을 배출하고 배출된 항목은 타임 윈도우를 시작시킨다. 타임 윈도우가 열려 있는 동안 A Observable은 자신의 항목들을 계속 배출하여 이 두 항목들을 결합한다
  - [**`Merge`**](http://reactivex.io/documentation/operators/merge.html) — 복수 개의 Observable들이 배출하는 항목들을 머지시켜 하나의 Observable로 만든다
  - [**`StartWith`**](http://reactivex.io/documentation/operators/startwith.html) — 소스 Observable이 항목을 배출하기 전에 다른 항목들을 앞에 추가한 후 배출한다
  - [**`Switch`**](http://reactivex.io/documentation/operators/switch.html) — Observable들을 배출하는 Observable을 싱글 Observable로 변환하다. 변환된 싱글 Observable은 변환 전 소스 Observable들이 배출한 항목들을 배출한다
  - [**`Zip`**](http://reactivex.io/documentation/operators/zip.html) — 명시한 함수를 통해 여러 Observable들이 배출한 항목들을 결합하고 함수의 실행 결과를 배출한다

  ## 오류 처리 연산자

  Observable이 던진 오류를 복구할 수 있도록 도와주는 연산자들

  - [**`Catch`**](http://reactivex.io/documentation/operators/catch.html) — 오류를 무시하고 배출되는 항목들을 계속 진행시켜 'onError'로부터 전달된 오류를 복구한다
  - [**`Retry`**](http://reactivex.io/documentation/operators/retry.html) — 만약 소스 Observable이 'onError' 알림을 보낼 경우, 오류 없이 실행이 완료되기를 기대하며 재구독을 시도한다
  
  ## Observable 유틸리티 연산자
  
  Obserable과 함께 동작하는 유용한 도우미 연산자들
  
  - [**`Delay`**](http://reactivex.io/documentation/operators/delay.html) — Observable의 배출을 특정 시간동안 미룬다
  - [**`Do`**](http://reactivex.io/documentation/operators/do.html) — Observable의 생명주기 동안 발생하는 여러 이벤트에서 실행 될 액션을 등록한다
  - [**`Materialize`/`Dematerialize`**](http://reactivex.io/documentation/operators/materialize-dematerialize.html) — 배출된 항목이 어떤 알림을 통해 옵저버에게 전달 됐는지를 표현하며, 그 반대 과정을 수행할 수 있다
  - [**`ObserveOn`**](http://reactivex.io/documentation/operators/observeon.html) — 옵저버가 어느 스케줄러 상에서 Observable을 관찰할지 명시한다
  - [**`Serialize`**](http://reactivex.io/documentation/operators/serialize.html) — Observable이 직렬화된 호출을 생성하고 제대로 동작하도록 강제한다
  - [**`Subscribe`**](http://reactivex.io/documentation/operators/subscribe.html) — Observable이 배출하는 항목과 알림을 기반으로 동작한다
  - [**`SubscribeOn`**](http://reactivex.io/documentation/operators/subscribeon.html) — Observable을 구독할 때 사용할 스케줄러를 명시한다
  - [**`TimeInterval`**](http://reactivex.io/documentation/operators/timeinterval.html) — 항목들을 배출하는 Observable을, 항목을 배출하는데 걸린 시간이 얼마인지를 가리키는 Observable로 변환한다
  - [**`Timeout`**](http://reactivex.io/documentation/operators/timeout.html) — 소스 Obvservable을 그대로 전달하지만, 대신 특정 시간 동안 배출된 항목이 없으면 오류 알림을 보낸다
  - [**`Timestamp`**](http://reactivex.io/documentation/operators/timestamp.html) — Observable이 배출한 항목에 타임 스탬프를 추가한다
  - [**`Using`**](http://reactivex.io/documentation/operators/using.html) — 소스 Observable과 동일한 생명주기를 갖는 Observable을 생성하는데, 이 Observable은 생명주기가 완료되면 리소스를 종료하고 반환한다

  ## 조건과 불린 연산자(Boolean)

  하나 이상의 Observable 또는 Observable이 배출한 항목을 평가하는 연산자들

  - [**`All`**](http://reactivex.io/documentation/operators/all.html) — Observable이 배출한 전체 항목들이 어떤 조건을 만족시키는지 판단한다
  - [**`Amb`**](http://reactivex.io/documentation/operators/amb.html) — 두 개 이상의 소스 Observable이 주어 질때, 그 중 첫 번째로 항목을 배출한 Observable이 배출하는 항목들을 전달한다
  - [**`Contains`**](http://reactivex.io/documentation/operators/contains.html) — Observable이 특정 항목을 배출하는지 아닌지를 판단한다
  - [**`DefaultIfEmpty`**](http://reactivex.io/documentation/operators/defaultifempty.html) — 소스 Observable이 배출하는 항목을 전달한다. 만약 배출되는 항목이 없으면 기본 항목을 배출한다
  - [**`SequenceEqual`**](http://reactivex.io/documentation/operators/sequenceequal.html) — 두 개의 Observable이 항목을 같은 순서로 배출하는지 판단한다
  - [**`SkipUntil`**](http://reactivex.io/documentation/operators/skipuntil.html) — 두 번째 Observable이 항목을 배출하기 전까지 배출된 항목들을 버린다
  - [**`SkipWhile`**](http://reactivex.io/documentation/operators/skipwhile.html) — 특정 조건이 false를 리턴하기 전까지 Observable이 배출한 항목들을 버린다
  - [**`TakeUntil`**](http://reactivex.io/documentation/operators/takeuntil.html) — 두 번째 Observable이 항목을 발행하기 시작햤거나 두 번째 Observable이 종료되면 그 때부터 발행되는 항목들은 버린다
  - [**`TakeWhile`**](http://reactivex.io/documentation/operators/takewhile.html) — 특정 조건이 false를 리턴하기 시작하면 그 이후에 배출되는 항목들을 버린다

  ## 수학과 집계 연산자

  Observable이 배출하는 항목 전체를 대상으로 동작하는 연산자들

  - [**`Average`**](http://reactivex.io/documentation/operators/average.html) — Observable이 발행한 항목의 평균 값을 발행한다
  - [**`Concat`**](http://reactivex.io/documentation/operators/concat.html) — 두 개 이상의 Observable들이 항목을 발행할 때 Observable 순서대로 배출하는 항목들을 하나의 Observable 배출로 연이어 배출한다
  - [**`Count`**](http://reactivex.io/documentation/operators/count.html) — 소스 Observable이 발행한 항목의 개수를 배출한다
  - [**`Max`**](http://reactivex.io/documentation/operators/max.html) — Observable이 발행한 항목 중 값이 가장 큰 항목을 배출한다
  - [**`Min`**](http://reactivex.io/documentation/operators/min.html) — Observable이 발행한 항목 중 값이 가장 작은 항목을 배출한다
  - [**`Reduce`**](http://reactivex.io/documentation/operators/reduce.html) — Observable이 배출한 항목에 함수를 순서대로 적용하고 함수를 연산한 후 최종 결과를 발행한다
  - [**`Sum`**](http://reactivex.io/documentation/operators/sum.html) — Observable이 배출한 항목의 합계를 배출한다

  ## 역압(Backpressure) 연산자

  - [**backpressure operators**](http://reactivex.io/documentation/operators/backpressure.html) — 옵저버가 소비하는 것보다 더 빠르게 항목들을 생산하는 Observable을 복재하는 전략

  ## 연결 가능한 Observable 연산자

  좀 더 정확히 제어되는 구독 역학을 가진 전문 Observable들

  - [**`Connect`**](http://reactivex.io/documentation/operators/connect.html) — 구독자가 항목 배출을 시작할 수 있도록 연결 가능한 Observable에게 명령을 내린다
  - [**`Publish`**](http://reactivex.io/documentation/operators/publish.html) — 일반 Observable을 연결 가능한 Observable로 변환한다
  - [**`RefCount`**](http://reactivex.io/documentation/operators/refcount.html) — 일반 Observable처럼 동작하는 연결 가능한 Observable을 만든다
  - [**`Replay`**](http://reactivex.io/documentation/operators/replay.html) — 비록 옵저버가 Observable이 항목 배출을 시작한 후에 구독을 했다 하더라도 배출된 모든 항목들을 볼 수 있도록 한다
  
  ## Observable 변환 연산자
  
  - [**`To`**](http://reactivex.io/documentation/operators/to.html) — Observable을 다른 객체나 자료 구조로 변환한다
  
  # Observable 연산자 결정 트리
  
  이 트리는 여러분이 원하는 ReactiveX의 Observable 연산자를 찾는데 도움을 줄 것이다.
  
  - 나는 새로운 Observable을 생성하고 싶은데 그 Observable이
  
    특정 항목을 생성해야 한다면:[Just](http://reactivex.io/documentation/ko/operators/just.html)구독 시점에 호출된 함수를 통해 생성된 항목을 리턴해야 한다면:[Start](http://reactivex.io/documentation/ko/operators/start.html)구독 시점에 호출된 `Action`, `Callable`, `Runnable` 또는 그와 유사한 함수 등을 통해 생성된 항목을 리턴해야 한다면:[From](http://reactivex.io/documentation/ko/operators/from.html)지정된 시간 이후에 항목을 배출해야 한다면:[Timer](http://reactivex.io/documentation/ko/operators/timer.html)특정 `Array`, `Iterable` 또는 유사한 형태의 소스로부터 항목들을 배출해야 한다면:[From](http://reactivex.io/documentation/ko/operators/from.html)퓨처(Future)에서 항목을 조회해서 배출해야 한다면:[Start](http://reactivex.io/documentation/ko/operators/start.html)퓨처에서 연속된 항목을 가져와야 한다면:[From](http://reactivex.io/documentation/ko/operators/from.html)반복적으로 연속된 항목을 배출해야 한다면:[Repeat](http://reactivex.io/documentation/ko/operators/repeat.html)사용자가 정의한 로직을 통해 생성되어야 한다면:[Create](http://reactivex.io/documentation/ko/operators/create.html)각각의 옵저버가 Observable을 구독한 후에 생성되어야 한다면:[Defer](http://reactivex.io/documentation/ko/operators/defer.html)연속된 정수를 배출해야 한다면:[Range](http://reactivex.io/documentation/ko/operators/range.html)특정 시간 간격별로 항목을 배출해야 한다면:[Interval](http://reactivex.io/documentation/ko/operators/interval.html)특정 시간 후에 항목을 배출해야 한다면:[Timer](http://reactivex.io/documentation/ko/operators/timer.html)항목 배출 없이 실행을 완료해야 한다면:[Empty](http://reactivex.io/documentation/ko/operators/empty-never-throw.html)아무일도 하지 않아야 한다면:[Never](http://reactivex.io/documentation/ko/operators/empty-never-throw.html)
  
  - 다른 Observable을 결합시켜 새로운 Observable을 생성해야 한다
  
    그리고 순서와 상관없이 전달 된 모든 Observable이 가진 항목 전체를 배출해야 한다면:[Merge](http://reactivex.io/documentation/ko/operators/merge.html)그리고 전달 된 Observable의 순서대로 Observable이 가진 모든 항목들을 배출해야 한다면:[Concat](http://reactivex.io/documentation/ko/operators/concat.html)생성하고 싶은 Observable은, 두 개 이상의 Observable이 가진 항목들을 순서대로 결합시켜 새로운 항목을 배출해야 하는데:*각각*의 Observable이 항목을 배출 할 때마다 그 항목들을 결합시켜 배출해야 한다면:[Zip](http://reactivex.io/documentation/ko/operators/zip.html)Observable 중 *하나*라도 항목을 배출할 경우에 마지막으로 배출된 항목들을 결합시켜 배출해야 한다면:[CombineLatest](http://reactivex.io/documentation/ko/operators/combinelatest.html)하나의 Observable이 배출한 항목의 타임 윈도우가 열려있는 시간 동안 다른 Observable이 항목을 배출할 때마다 새로운 항목을 배출해야 한다면:[Join](http://reactivex.io/documentation/ko/operators/join.html)`Pattern`과 `Plan` 중계자를 이용해서 항목을 배출해야 한다면:[And/Then/When](http://reactivex.io/documentation/ko/operators/and-then-when.html)그리고 가장 최근에 항목을 배출한 Observable을 통해서만 항목을 배출해야 한다면:[Switch](http://reactivex.io/documentation/ko/operators/switch.html)
  
  - Observable이 배출한 항목들을 변환한 후에 다시 배출해야 하는데
  
    함수와 함께 항목을 한번에 하나씩 변환 후 배출해야 한다면:[Map](http://reactivex.io/documentation/ko/operators/map.html)해당 Observable이 배출한 모든 항목들을 하나의 Observable이 배출하는 형태로 배출해야 한다면:[FlatMap](http://reactivex.io/documentation/ko/operators/flatmap.html)순서대로 Observable이 배출한 항목들을 연결지어 배출해야 한다면:[ConcatMap](http://reactivex.io/documentation/ko/operators/flatmap.html)앞에서 실행 된 결과를 기반으로 항목을 변환한 후 배출해야 한다면:[Scan](http://reactivex.io/documentation/ko/operators/scan.html)타임 스탬프를 추가하여 변환한 후 배출해야 한다면:[Timestamp](http://reactivex.io/documentation/ko/operators/timestamp.html)항목 배출 전까지 경과한 시간을 가리키는 객체로 변환한 후에 배출해야 한다면:[TimeInterval](http://reactivex.io/documentation/ko/operators/timeinterval.html)
  
  - Observable이 항목을 배출하기 전에 항목의 배출 시간을 지연시켜야 한다면:
  
    [Delay](http://reactivex.io/documentation/ko/operators/delay.html)
  
  - Observable이 배출하는 항목들*과* 알림들을 다시 항목들로 변환 후 배출해야 하는데
  
    이때 배출하는 항목들을 `알림` 객체로 감싸서(wrapping) 배출해야 한다면:[Materialize](http://reactivex.io/documentation/ko/operators/materialize-dematerialize.html)이 알림 객체가 다시 풀릴 수(unwrapping) 있다면:[Dematerialize](http://reactivex.io/documentation/ko/operators/materialize-dematerialize.html)
  
  - Observable이 배출하는 모든 객체를 무시하고 completed/error 알림만 전달해야 한다면:
  
    [IgnoreElements](http://reactivex.io/documentation/ko/operators/ignoreelements.html)
  
  - Observable이 가진 항목을 그대로를 배출하지만 배출 전에 다른 항목들을 먼저 배출될 수 있도록 추가해야 한다면:
  
    [StartWith](http://reactivex.io/documentation/ko/operators/startwith.html)
  
    만약 소스 Observable이 비어있을 경우 기본 항목을 추가해야 한다면:[DefaultIfEmpty](http://reactivex.io/documentation/ko/operators/defaultifempty.html)
  
  - Observable이 배출하는 항목들을 모아둔 후 버퍼로 다시 배출해야 한다면:
  
    [Buffer](http://reactivex.io/documentation/ko/operators/buffer.html)
  
    그 중 마지막에 배출된 항목이 추가된 버퍼만 배출해야 한다면:[TakeLastBuffer](http://reactivex.io/documentation/ko/operators/takelast.html)
  
  - 하나의 Observable을 여러 Observable로 나눠야 한다:
  
    [Window](http://reactivex.io/documentation/ko/operators/window.html)
  
    그 중 유사한 항목들을 같은 Observable에 모아 두어야 한다면:[GroupBy](http://reactivex.io/documentation/ko/operators/groupby.html)
  
  - Observable이 배출한 특정 항목을 조회해야 하는데
  
    Observable이 완료되기 전에 마지막으로 배출한 항목을 조회해야 한다면:[Last](http://reactivex.io/documentation/ko/operators/last.html)배출된 항목이 단지 하나이고 이것을 조회해야 한다면:[Single](http://reactivex.io/documentation/ko/operators/first.html)배출한 첫 번째 항목을 조회해야 한다면:[First](http://reactivex.io/documentation/ko/operators/first.html)
  
  - Observable의 특정 항목만 재배출 해야 하는데
  
    어떤 조건을 만족시키지 않는 항목들을 필터링해서 재배출 해야 한다면:[Filter](http://reactivex.io/documentation/ko/operators/filter.html)첫 번째 항목만 재배출 해야 한다면:[First](http://reactivex.io/documentation/ko/operators/first.html)처음 몇 개의 항목*들*만 재배출 해야 한다면:[Take](http://reactivex.io/documentation/ko/operators/take.html)마지막 항목만 재배출 해야 한다면:[Last](http://reactivex.io/documentation/ko/operators/last.html)*몇 번째* 위치한 항목만 재배출 해야 한다면:[ElementAt](http://reactivex.io/documentation/ko/operators/elementat.html)재배출할 항목들이 처음 몇개 이후의 것들일 경우처음 *몇 개*의 항목들 이후의 것들 이라면:[Skip](http://reactivex.io/documentation/ko/operators/skip.html)특정 조건을 만족시킨 이후의 것들 이라면:[SkipWhile](http://reactivex.io/documentation/ko/operators/skipwhile.html)초기 특정 시간 이후에 배출된 항목들 이라면:[Skip](http://reactivex.io/documentation/ko/operators/skip.html)두 번째 Observable이 항목을 배출한 이후의 것들 이라면:[SkipUntil](http://reactivex.io/documentation/ko/operators/skipuntil.html)마지막 항목 몇개를 제외한 경우마지막 *몇 개* 항목을 제외한 것들 이라면:[SkipLast](http://reactivex.io/documentation/ko/operators/skiplast.html)특정 조건을 만족할때 까지의 것들 이라면:[TakeWhile](http://reactivex.io/documentation/ko/operators/takewhile.html)소스 Observable이 완료되기 이전 특정 시간 동안 배출한 것들을 제외한 것이라면:[SkipLast](http://reactivex.io/documentation/ko/operators/skiplast.html)두 번째 Observable이 항목을 배출한 이후에 배출된 것들을 제외한 것이라면:[TakeUntil](http://reactivex.io/documentation/ko/operators/takeuntil.html)주기적으로 Observable을 샘플링해서 재배출해야 한다면:[Sample](http://reactivex.io/documentation/ko/operators/sample.html)특정 시간이 지나고 나서 배출된 항목들만 재배출 해야 한다면:[Debounce](http://reactivex.io/documentation/ko/operators/debounce.html)이미 배출된 항목과 동일한 것들을 제외시켜 재배출 해야 한다면:[Distinct](http://reactivex.io/documentation/ko/operators/distinct.html)만약 중복된 항목이 바로 연이어 배출된다면:[DistinctUntilChanged](http://reactivex.io/documentation/ko/operators/distinct.html)항목 배출이 시작된 이후에 얼마 동안 구독을 지연시켜야 한다면:[DelaySubscription](http://reactivex.io/documentation/ko/operators/delay.html)
  
  - 항목들을 배출하는 Observable 컬랙션 중에 첫 번째로 항목을 배출하는 Observable의 항목만 배출해야 한다면:
  
    [Amb](http://reactivex.io/documentation/ko/operators/amb.html)
  
  - Observable이 배출한 연속된 항목 전체를 평가해야 한다
  
    그리고 항목 *전체*가 테스트를 통과했는지를 가리키는 boolean 타입 항목 하나를 배출해야 한다면:[All](http://reactivex.io/documentation/ko/operators/all.html)그리고 항목 전체 중 *하나라도* 테스트를 통과했는지를 가리키는 boolean 타입 항목 하나를 배출해야 한다면:[Contains](http://reactivex.io/documentation/ko/operators/contains.html)그리고 Observable이 항목을 배출하지 못했는지를 가리키는 boolean 타입 항목 하나를 배출해야 한다면:[IsEmpty](http://reactivex.io/documentation/ko/operators/contains.html)그리고 두 Observable이 같은 순서대로 항목들을 배출했는지를 가리키는 boolean 타입 하나를 배출해야 한다면:[SequenceEqual](http://reactivex.io/documentation/ko/operators/sequenceequal.html)그리고 전체 배출된 항목의 평균 값을 항목을 배출해야 한다면:[Average](http://reactivex.io/documentation/ko/operators/average.html)그리고 전체 배출된 항목의 합계를 배출해야 한다면:[Sum](http://reactivex.io/documentation/ko/operators/sum.html)그리고 얼마나 많은 항목들이 배출됐는지를 배출해야 한다면:[Count](http://reactivex.io/documentation/ko/operators/count.html)그리고 가장 큰 값을 가진 항목을 배출해야 한다면:[Max](http://reactivex.io/documentation/ko/operators/max.html)그리고 가장 작은 값을 가진 항목을 배출해야 한다면:[Min](http://reactivex.io/documentation/ko/operators/min.html)배출되는 항목 순서대로 각각에 집계 함수를 적용해서 결과를 배출해야 한다면:[Scan](http://reactivex.io/documentation/ko/operators/scan.html)
  
  - Observable이 배출한 전체 항목들을 특정 자료구조로 배출하고 싶다면
  
    [To](http://reactivex.io/documentation/ko/operators/to.html)
  
  - 연산자가 특정 [스케줄러](http://reactivex.io/documentation/scheduler.html) 상에서 동작해야 한다면:
  
    [SubscribeOn](http://reactivex.io/documentation/ko/operators/subscribeon.html)
  
    연산자가 옵저버한테 알림을 줄 때 동작할 스케줄러를 지정해야 한다면:[ObserveOn](http://reactivex.io/documentation/ko/operators/observeon.html)
  
  - 특정 이벤트가 발생 할 때 Observable 상에서 어떤 동작을 실행시켜야 한다면:
  
    [Do](http://reactivex.io/documentation/ko/operators/do.html)
  
  - 오류가 발생했을 때 Observable이 옵저버에게 오류를 알려야 하다면:
  
    [Throw](http://reactivex.io/documentation/ko/operators/empty-never-throw.html)
  
    만약 항목이 배출되지 않은 상태에서 특정 시간이 경과했다면[Timeout](http://reactivex.io/documentation/ko/operators/timeout.html)
  
  - 자연스럽게 Observable을 복구해야 하는데
  
    타임 아웃이 발생한 경우 백업 Observable로 전환시켜 복구해야 한다면:[Timeout](http://reactivex.io/documentation/ko/operators/timeout.html)앞에서 발생한 오류 알림으로부터 복구해야 한다면:[Catch](http://reactivex.io/documentation/ko/operators/catch.html)이전 Observable에 재구독을 시도해야 한다면:[Retry](http://reactivex.io/documentation/ko/operators/retry.html)
  
  - 동일한 생명주기를 가진 리소스를 Observable로 생성해야 한다면:
  
    [Using](http://reactivex.io/documentation/ko/operators/using.html)
  
  - Observable을 구독하고 Observable이 완료될 때까지 블로킹 상태에 있는 `퓨처(Future)`를 전달 받고 싶다면:
  
    [Start](http://reactivex.io/documentation/ko/operators/start.html)
  
  - 구독자의 요청 전까지 Observable이 항목을 구독자에게 배포하지 말아야 한다면:
  
    [Publish](http://reactivex.io/documentation/ko/operators/publish.html)
  
    그리고 맨 마지막 항목만을 배출해야 한다면:[PublishLast](http://reactivex.io/documentation/ko/operators/publish.html)그리고 배출 이후에 구독자가 구독을 시작했다 하더라고 동일하게 배출 순서를 전달해야 한다면:[Replay](http://reactivex.io/documentation/ko/operators/replay.html)하지만 모든 구독자가 한번에 구독을 해지할 수 있어야 한다면:[RefCount](http://reactivex.io/documentation/ko/operators/refcount.html)그리고 배출을 시작하도록 Observable에게 요청해야 한다면:[Connect](http://reactivex.io/documentation/ko/operators/connect.html)
  
  #### 참고
  
  - [Which Operator do I use?](http://xgrommx.github.io/rx-book/content/which_operator_do_i_use/index.html) by Dennis Stoyanov (a similar decision tree, specific to RxJS operators)
  
  # Observable 연산자 리스트(알파벳순)
  
  표준 연산자나 핵심 연산자의 이름은 **boldface**로 표시했다. 그 외 연산자들은 언어 별로 구현된 다양한 연산자들을 포함하거나 ReactiveX의 주요 연산자 이외의 특별한 연산자들을 포함한다.
  
  - [`Aggregate`](http://reactivex.io/documentation/operators/reduce.html)
  - [**`All`**](http://reactivex.io/documentation/operators/all.html)
  - [**`Amb`**](http://reactivex.io/documentation/operators/amb.html)
  - [`and_`](http://reactivex.io/documentation/operators/and-then-when.html)
  - [**`And`**](http://reactivex.io/documentation/operators/and-then-when.html)
  - [`Any`](http://reactivex.io/documentation/operators/contains.html)
  - [`apply`](http://reactivex.io/documentation/operators/create.html)
  - [`as_blocking`](http://reactivex.io/documentation/operators/to.html)
  - [`asObservable`](http://reactivex.io/documentation/operators/from.html)
  - [`AssertEqual`](http://reactivex.io/documentation/operators/sequenceequal.html)
  - [`asyncAction`](http://reactivex.io/documentation/operators/start.html)
  - [`asyncFunc`](http://reactivex.io/documentation/operators/start.html)
  - [**`Average`**](http://reactivex.io/documentation/operators/average.html)
  - [`averageDouble`](http://reactivex.io/documentation/operators/average.html)
  - [`averageFloat`](http://reactivex.io/documentation/operators/average.html)
  - [`averageInteger`](http://reactivex.io/documentation/operators/average.html)
  - [`averageLong`](http://reactivex.io/documentation/operators/average.html)
  - [`blocking`](http://reactivex.io/documentation/operators/to.html)
  - [**`Buffer`**](http://reactivex.io/documentation/operators/buffer.html)
  - [`bufferWithCount`](http://reactivex.io/documentation/operators/buffer.html)
  - [`bufferWithTime`](http://reactivex.io/documentation/operators/buffer.html)
  - [`bufferWithTimeOrCount`](http://reactivex.io/documentation/operators/buffer.html)
  - [`byLine`](http://reactivex.io/documentation/operators/map.html)
  - [`cache`](http://reactivex.io/documentation/operators/replay.html)
  - [`case`](http://reactivex.io/documentation/operators/defer.html)
  - [`Cast`](http://reactivex.io/documentation/operators/map.html)
  - [**`Catch`**](http://reactivex.io/documentation/operators/catch.html)
  - [`catchError`](http://reactivex.io/documentation/operators/catch.html)
  - [`catchException`](http://reactivex.io/documentation/operators/catch.html)
  - [`collect`](http://reactivex.io/documentation/operators/reduce.html)
  - [`collect`](http://reactivex.io/documentation/operators/filter.html) (RxScala version of **`Filter`**)
  - [**`CombineLatest`**](http://reactivex.io/documentation/operators/combinelatest.html)
  - [`combineLatestWith`](http://reactivex.io/documentation/operators/combinelatest.html)
  - [**`Concat`**](http://reactivex.io/documentation/operators/concat.html)
  - [`concat_all`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`concatMap`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`concatMapObserver`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`concatMapTo`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`concatAll`](http://reactivex.io/documentation/operators/concat.html)
  - [`concatWith`](http://reactivex.io/documentation/operators/concat.html)
  - [**`Connect`**](http://reactivex.io/documentation/operators/connect.html)
  - [`connect_forever`](http://reactivex.io/documentation/operators/connect.html)
  - [`cons`](http://reactivex.io/documentation/operators/startwith.html)
  - [**`Contains`**](http://reactivex.io/documentation/operators/contains.html)
  - [`controlled`](http://reactivex.io/documentation/operators/backpressure.html)
  - [**`Count`**](http://reactivex.io/documentation/operators/count.html)
  - [`countLong`](http://reactivex.io/documentation/operators/count.html)
  - [**`Create`**](http://reactivex.io/documentation/operators/create.html)
  - [`cycle`](http://reactivex.io/documentation/operators/repeat.html)
  - [**`Debounce`**](http://reactivex.io/documentation/operators/debounce.html)
  - [`decode`](http://reactivex.io/documentation/operators/from.html)
  - [**`DefaultIfEmpty`**](http://reactivex.io/documentation/operators/defaultifempty.html)
  - [**`Defer`**](http://reactivex.io/documentation/operators/defer.html)
  - [`deferFuture`](http://reactivex.io/documentation/operators/start.html)
  - [**`Delay`**](http://reactivex.io/documentation/operators/delay.html)
  - [`delaySubscription`](http://reactivex.io/documentation/operators/delay.html)
  - [`delayWithSelector`](http://reactivex.io/documentation/operators/delay.html)
  - [**`Dematerialize`**](http://reactivex.io/documentation/operators/materialize-dematerialize.html)
  - [**`Distinct`**](http://reactivex.io/documentation/operators/distinct.html)
  - [`distinctKey`](http://reactivex.io/documentation/operators/distinct.html)
  - [`distinctUntilChanged`](http://reactivex.io/documentation/operators/distinct.html)
  - [`distinctUntilKeyChanged`](http://reactivex.io/documentation/operators/distinct.html)
  - [**`Do`**](http://reactivex.io/documentation/operators/do.html)
  - [`doAction`](http://reactivex.io/documentation/operators/do.html)
  - [`doAfterTerminate`](http://reactivex.io/documentation/operators/do.html)
  - [`doOnCompleted`](http://reactivex.io/documentation/operators/do.html)
  - [`doOnEach`](http://reactivex.io/documentation/operators/do.html)
  - [`doOnError`](http://reactivex.io/documentation/operators/do.html)
  - [`doOnRequest`](http://reactivex.io/documentation/operators/do.html)
  - [`doOnSubscribe`](http://reactivex.io/documentation/operators/do.html)
  - [`doOnTerminate`](http://reactivex.io/documentation/operators/do.html)
  - [`doOnUnsubscribe`](http://reactivex.io/documentation/operators/do.html)
  - [`doseq`](http://reactivex.io/documentation/operators/subscribe.html)
  - [`doWhile`](http://reactivex.io/documentation/operators/repeat.html)
  - [`drop`](http://reactivex.io/documentation/operators/skip.html)
  - [`dropRight`](http://reactivex.io/documentation/operators/skiplast.html)
  - [`dropUntil`](http://reactivex.io/documentation/operators/skipuntil.html)
  - [`dropWhile`](http://reactivex.io/documentation/operators/skipwhile.html)
  - [**`ElementAt`**](http://reactivex.io/documentation/operators/elementat.html)
  - [`ElementAtOrDefault`](http://reactivex.io/documentation/operators/elementat.html)
  - [**`Empty`**](http://reactivex.io/documentation/operators/empty-never-throw.html)
  - [`emptyObservable`](http://reactivex.io/documentation/operators/empty-never-throw.html)
  - [`empty?`](http://reactivex.io/documentation/operators/contains.html)
  - [`encode`](http://reactivex.io/documentation/operators/map.html)
  - [`ensures`](http://reactivex.io/documentation/operators/do.html)
  - [`error`](http://reactivex.io/documentation/operators/empty-never-throw.html)
  - [`every`](http://reactivex.io/documentation/operators/all.html)
  - [`exclusive`](http://reactivex.io/documentation/operators/switch.html)
  - [`exists`](http://reactivex.io/documentation/operators/contains.html)
  - [`expand`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`failWith`](http://reactivex.io/documentation/operators/empty-never-throw.html)
  - [**`Filter`**](http://reactivex.io/documentation/operators/filter.html)
  - [`filterNot`](http://reactivex.io/documentation/operators/filter.html)
  - [`Finally`](http://reactivex.io/documentation/operators/do.html)
  - [`finallyAction`](http://reactivex.io/documentation/operators/do.html)
  - [`finallyDo`](http://reactivex.io/documentation/operators/do.html)
  - [`find`](http://reactivex.io/documentation/operators/contains.html)
  - [`findIndex`](http://reactivex.io/documentation/operators/contains.html)
  - [**`First`**](http://reactivex.io/documentation/operators/first.html)
  - [`FirstOrDefault`](http://reactivex.io/documentation/operators/first.html)
  - [`firstOrElse`](http://reactivex.io/documentation/operators/first.html)
  - [**`FlatMap`**](http://reactivex.io/documentation/operators/flatmap.html)
  - [`flatMapFirst`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`flatMapIterable`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`flatMapIterableWith`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`flatMapLatest`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`flatMapObserver`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`flatMapWith`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`flatMapWithMaxConcurrent`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`flat_map_with_index`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`flatten`](http://reactivex.io/documentation/operators/merge.html)
  - [`flattenDelayError`](http://reactivex.io/documentation/operators/merge.html)
  - [`foldl`](http://reactivex.io/documentation/operators/reduce.html)
  - [`foldLeft`](http://reactivex.io/documentation/operators/reduce.html)
  - [`for`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`forall`](http://reactivex.io/documentation/operators/all.html)
  - [`ForEach`](http://reactivex.io/documentation/operators/subscribe.html)
  - [`forEachFuture`](http://reactivex.io/documentation/operators/start.html)
  - [`forIn`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`forkJoin`](http://reactivex.io/documentation/operators/zip.html)
  - [**`From`**](http://reactivex.io/documentation/operators/from.html)
  - [`fromAction`](http://reactivex.io/documentation/operators/from.html)
  - [`fromArray`](http://reactivex.io/documentation/operators/from.html)
  - [`FromAsyncPattern`](http://reactivex.io/documentation/operators/from.html)
  - [`fromCallable`](http://reactivex.io/documentation/operators/from.html)
  - [`fromCallback`](http://reactivex.io/documentation/operators/from.html)
  - [`FromEvent`](http://reactivex.io/documentation/operators/from.html)
  - [`FromEventPattern`](http://reactivex.io/documentation/operators/from.html)
  - [`fromFunc0`](http://reactivex.io/documentation/operators/from.html)
  - [`from_future`](http://reactivex.io/documentation/operators/from.html)
  - [`from_iterable`](http://reactivex.io/documentation/operators/from.html)
  - [`fromIterator`](http://reactivex.io/documentation/operators/from.html)
  - [`from_list`](http://reactivex.io/documentation/operators/from.html)
  - [`fromNodeCallback`](http://reactivex.io/documentation/operators/from.html)
  - [`fromPromise`](http://reactivex.io/documentation/operators/from.html)
  - [`fromRunnable`](http://reactivex.io/documentation/operators/from.html)
  - [`Generate`](http://reactivex.io/documentation/operators/create.html)
  - [`generateWithAbsoluteTime`](http://reactivex.io/documentation/operators/create.html)
  - [`generateWithRelativeTime`](http://reactivex.io/documentation/operators/create.html)
  - [`generator`](http://reactivex.io/documentation/operators/create.html)
  - [`GetEnumerator`](http://reactivex.io/documentation/operators/to.html)
  - [`getIterator`](http://reactivex.io/documentation/operators/to.html)
  - [**`GroupBy`**](http://reactivex.io/documentation/operators/groupby.html)
  - [`GroupByUntil`](http://reactivex.io/documentation/operators/groupby.html)
  - [`GroupJoin`](http://reactivex.io/documentation/operators/join.html)
  - [`head`](http://reactivex.io/documentation/operators/first.html)
  - [`headOption`](http://reactivex.io/documentation/operators/first.html)
  - [`headOrElse`](http://reactivex.io/documentation/operators/first.html)
  - [`if`](http://reactivex.io/documentation/operators/defer.html)
  - [`ifThen`](http://reactivex.io/documentation/operators/defer.html)
  - [**`IgnoreElements`**](http://reactivex.io/documentation/operators/ignoreelements.html)
  - [`indexOf`](http://reactivex.io/documentation/operators/contains.html)
  - [`interleave`](http://reactivex.io/documentation/operators/merge.html)
  - [`interpose`](http://reactivex.io/documentation/operators/to.html)
  - [**`Interval`**](http://reactivex.io/documentation/operators/interval.html)
  - [`into`](http://reactivex.io/documentation/operators/reduce.html)
  - [`isEmpty`](http://reactivex.io/documentation/operators/contains.html)
  - [`items`](http://reactivex.io/documentation/operators/just.html)
  - [**`Join`**](http://reactivex.io/documentation/operators/join.html)
  - [`join`](http://reactivex.io/documentation/operators/sum.html) (string)
  - [`jortSort`](http://reactivex.io/documentation/operators/all.html)
  - [`jortSortUntil`](http://reactivex.io/documentation/operators/all.html)
  - [**`Just`**](http://reactivex.io/documentation/operators/just.html)
  - [`keep`](http://reactivex.io/documentation/operators/map.html)
  - [`keep-indexed`](http://reactivex.io/documentation/operators/map.html)
  - [**`Last`**](http://reactivex.io/documentation/operators/last.html)
  - [`lastOption`](http://reactivex.io/documentation/operators/last.html)
  - [`LastOrDefault`](http://reactivex.io/documentation/operators/last.html)
  - [`lastOrElse`](http://reactivex.io/documentation/operators/last.html)
  - [`Latest`](http://reactivex.io/documentation/operators/first.html)
  - [`latest`](http://reactivex.io/documentation/operators/switch.html) (Rx.rb version of **`Switch`**)
  - [`length`](http://reactivex.io/documentation/operators/count.html)
  - [`let`](http://reactivex.io/documentation/operators/publish.html)
  - [`letBind`](http://reactivex.io/documentation/operators/publish.html)
  - [`limit`](http://reactivex.io/documentation/operators/take.html)
  - [`LongCount`](http://reactivex.io/documentation/operators/count.html)
  - [`ManySelect`](http://reactivex.io/documentation/operators/flatmap.html)
  - [**`Map`**](http://reactivex.io/documentation/operators/map.html)
  - [`map`](http://reactivex.io/documentation/operators/zip.html) (RxClojure version of **`Zip`**)
  - [`MapCat`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`mapCat`](http://reactivex.io/documentation/operators/zip.html) (RxClojure version of **`Zip`**)
  - [`map-indexed`](http://reactivex.io/documentation/operators/map.html)
  - [`mapTo`](http://reactivex.io/documentation/operators/map.html)
  - [`mapWithIndex`](http://reactivex.io/documentation/operators/map.html)
  - [**`Materialize`**](http://reactivex.io/documentation/operators/materialize-dematerialize.html)
  - [**`Max`**](http://reactivex.io/documentation/operators/max.html)
  - [`MaxBy`](http://reactivex.io/documentation/operators/max.html)
  - [**`Merge`**](http://reactivex.io/documentation/operators/merge.html)
  - [`mergeAll`](http://reactivex.io/documentation/operators/merge.html)
  - [`merge_concurrent`](http://reactivex.io/documentation/operators/merge.html)
  - [`mergeDelayError`](http://reactivex.io/documentation/operators/merge.html)
  - [`mergeObservable`](http://reactivex.io/documentation/operators/merge.html)
  - [`mergeWith`](http://reactivex.io/documentation/operators/merge.html)
  - [**`Min`**](http://reactivex.io/documentation/operators/min.html)
  - [`MinBy`](http://reactivex.io/documentation/operators/min.html)
  - [`MostRecent`](http://reactivex.io/documentation/operators/first.html)
  - [`Multicast`](http://reactivex.io/documentation/operators/publish.html)
  - [`multicastWithSelector`](http://reactivex.io/documentation/operators/publish.html)
  - [`nest`](http://reactivex.io/documentation/operators/to.html)
  - [**`Never`**](http://reactivex.io/documentation/operators/empty-never-throw.html)
  - [`Next`](http://reactivex.io/documentation/operators/takelast.html)
  - [`Next`](http://reactivex.io/documentation/operators/first.html) (BlockingObservable version)
  - [`none`](http://reactivex.io/documentation/operators/contains.html)
  - [`nonEmpty`](http://reactivex.io/documentation/operators/contains.html)
  - [`nth`](http://reactivex.io/documentation/operators/elementat.html)
  - [**`ObserveOn`**](http://reactivex.io/documentation/operators/observeon.html)
  - [`ObserveOnDispatcher`](http://reactivex.io/documentation/operators/observeon.html)
  - [`observeSingleOn`](http://reactivex.io/documentation/operators/observeon.html)
  - [`of`](http://reactivex.io/documentation/operators/from.html)
  - [`of_array`](http://reactivex.io/documentation/operators/from.html)
  - [`ofArrayChanges`](http://reactivex.io/documentation/operators/from.html)
  - [`of_enumerable`](http://reactivex.io/documentation/operators/from.html)
  - [`of_enumerator`](http://reactivex.io/documentation/operators/from.html)
  - [`ofObjectChanges`](http://reactivex.io/documentation/operators/from.html)
  - [`OfType`](http://reactivex.io/documentation/operators/filter.html)
  - [`ofWithScheduler`](http://reactivex.io/documentation/operators/from.html)
  - [`onBackpressureBlock`](http://reactivex.io/documentation/operators/backpressure.html)
  - [`onBackpressureBuffer`](http://reactivex.io/documentation/operators/backpressure.html)
  - [`onBackpressureDrop`](http://reactivex.io/documentation/operators/backpressure.html)
  - [`OnErrorResumeNext`](http://reactivex.io/documentation/operators/catch.html)
  - [`onErrorReturn`](http://reactivex.io/documentation/operators/catch.html)
  - [`onExceptionResumeNext`](http://reactivex.io/documentation/operators/catch.html)
  - [`orElse`](http://reactivex.io/documentation/operators/defaultifempty.html)
  - [`pairs`](http://reactivex.io/documentation/operators/from.html)
  - [`pairwise`](http://reactivex.io/documentation/operators/buffer.html)
  - [`partition`](http://reactivex.io/documentation/operators/groupby.html)
  - [`partition-all`](http://reactivex.io/documentation/operators/window.html)
  - [`pausable`](http://reactivex.io/documentation/operators/backpressure.html)
  - [`pausableBuffered`](http://reactivex.io/documentation/operators/backpressure.html)
  - [`pluck`](http://reactivex.io/documentation/operators/map.html)
  - [`product`](http://reactivex.io/documentation/operators/sum.html)
  - [**`Publish`**](http://reactivex.io/documentation/operators/publish.html)
  - [`PublishLast`](http://reactivex.io/documentation/operators/publish.html)
  - [`publish_synchronized`](http://reactivex.io/documentation/operators/replay.html)
  - [`publishValue`](http://reactivex.io/documentation/operators/publish.html)
  - [`raise_error`](http://reactivex.io/documentation/operators/empty-never-throw.html)
  - [**`Range`**](http://reactivex.io/documentation/operators/range.html)
  - [**`Reduce`**](http://reactivex.io/documentation/operators/reduce.html)
  - [`reductions`](http://reactivex.io/documentation/operators/scan.html)
  - [**`RefCount`**](http://reactivex.io/documentation/operators/refcount.html)
  - [**`Repeat`**](http://reactivex.io/documentation/operators/repeat.html)
  - [`repeat_infinitely`](http://reactivex.io/documentation/operators/repeat.html)
  - [`repeatWhen`](http://reactivex.io/documentation/operators/repeat.html)
  - [**`Replay`**](http://reactivex.io/documentation/operators/replay.html)
  - [`rescue_error`](http://reactivex.io/documentation/operators/catch.html)
  - [`rest`](http://reactivex.io/documentation/operators/first.html)
  - [**`Retry`**](http://reactivex.io/documentation/operators/retry.html)
  - [`retry_infinitely`](http://reactivex.io/documentation/operators/retry.html)
  - [`retryWhen`](http://reactivex.io/documentation/operators/retry.html)
  - [`Return`](http://reactivex.io/documentation/operators/just.html)
  - [`returnElement`](http://reactivex.io/documentation/operators/just.html)
  - [`returnValue`](http://reactivex.io/documentation/operators/just.html)
  - [`runAsync`](http://reactivex.io/documentation/operators/from.html)
  - [**`Sample`**](http://reactivex.io/documentation/operators/sample.html)
  - [**`Scan`**](http://reactivex.io/documentation/operators/scan.html)
  - [`scope`](http://reactivex.io/documentation/operators/using.html)
  - [`Select`](http://reactivex.io/documentation/operators/map.html) (alternate name of **`Map`**)
  - [`select`](http://reactivex.io/documentation/operators/filter.html) (alternate name of **`Filter`**)
  - [`selectConcat`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`selectConcatObserver`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`SelectMany`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`selectManyObserver`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`select_switch`](http://reactivex.io/documentation/operators/switch.html)
  - [`selectSwitch`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`selectSwitchFirst`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`selectWithMaxConcurrent`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`select_with_index`](http://reactivex.io/documentation/operators/filter.html)
  - [`seq`](http://reactivex.io/documentation/operators/from.html)
  - [**`SequenceEqual`**](http://reactivex.io/documentation/operators/sequenceequal.html)
  - [`sequence_eql?`](http://reactivex.io/documentation/operators/sequenceequal.html)
  - [`SequenceEqualWith`](http://reactivex.io/documentation/operators/sequenceequal.html)
  - [**`Serialize`**](http://reactivex.io/documentation/operators/serialize.html)
  - [`share`](http://reactivex.io/documentation/operators/refcount.html)
  - [`shareReplay`](http://reactivex.io/documentation/operators/replay.html)
  - [`shareValue`](http://reactivex.io/documentation/operators/refcount.html)
  - [`Single`](http://reactivex.io/documentation/operators/first.html)
  - [`SingleOrDefault`](http://reactivex.io/documentation/operators/first.html)
  - [`singleOption`](http://reactivex.io/documentation/operators/first.html)
  - [`singleOrElse`](http://reactivex.io/documentation/operators/first.html)
  - [`size`](http://reactivex.io/documentation/operators/count.html)
  - [**`Skip`**](http://reactivex.io/documentation/operators/skip.html)
  - [**`SkipLast`**](http://reactivex.io/documentation/operators/skiplast.html)
  - [`skipLastWithTime`](http://reactivex.io/documentation/operators/skiplast.html)
  - [**`SkipUntil`**](http://reactivex.io/documentation/operators/skipuntil.html)
  - [`skipUntilWithTime`](http://reactivex.io/documentation/operators/skipuntil.html)
  - [**`SkipWhile`**](http://reactivex.io/documentation/operators/skipwhile.html)
  - [`skipWhileWithIndex`](http://reactivex.io/documentation/operators/skipwhile.html)
  - [`skip_with_time`](http://reactivex.io/documentation/operators/skip.html)
  - [`slice`](http://reactivex.io/documentation/operators/filter.html)
  - [`sliding`](http://reactivex.io/documentation/operators/window.html)
  - [`slidingBuffer`](http://reactivex.io/documentation/operators/buffer.html)
  - [`some`](http://reactivex.io/documentation/operators/contains.html)
  - [`sort`](http://reactivex.io/documentation/operators/to.html)
  - [`sort-by`](http://reactivex.io/documentation/operators/to.html)
  - [`sorted-list-by`](http://reactivex.io/documentation/operators/to.html)
  - [`split`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`split-with`](http://reactivex.io/documentation/operators/groupby.html)
  - [**`Start`**](http://reactivex.io/documentation/operators/start.html)
  - [`startAsync`](http://reactivex.io/documentation/operators/start.html)
  - [`startFuture`](http://reactivex.io/documentation/operators/start.html)
  - [**`StartWith`**](http://reactivex.io/documentation/operators/startwith.html)
  - [`startWithArray`](http://reactivex.io/documentation/operators/startwith.html)
  - [`stringConcat`](http://reactivex.io/documentation/operators/sum.html)
  - [`stopAndWait`](http://reactivex.io/documentation/operators/backpressure.html)
  - [`subscribe`](http://reactivex.io/documentation/operators/subscribe.html)
  - [**`SubscribeOn`**](http://reactivex.io/documentation/operators/subscribeon.html)
  - [`SubscribeOnDispatcher`](http://reactivex.io/documentation/operators/subscribeon.html)
  - [`subscribeOnCompleted`](http://reactivex.io/documentation/operators/subscribe.html)
  - [`subscribeOnError`](http://reactivex.io/documentation/operators/subscribe.html)
  - [`subscribeOnNext`](http://reactivex.io/documentation/operators/subscribe.html)
  - [**`Sum`**](http://reactivex.io/documentation/operators/sum.html)
  - [`sumDouble`](http://reactivex.io/documentation/operators/sum.html)
  - [`sumFloat`](http://reactivex.io/documentation/operators/sum.html)
  - [`sumInteger`](http://reactivex.io/documentation/operators/sum.html)
  - [`sumLong`](http://reactivex.io/documentation/operators/sum.html)
  - [**`Switch`**](http://reactivex.io/documentation/operators/switch.html)
  - [`switchCase`](http://reactivex.io/documentation/operators/defer.html)
  - [`switchIfEmpty`](http://reactivex.io/documentation/operators/defaultifempty.html)
  - [`switchLatest`](http://reactivex.io/documentation/operators/switch.html)
  - [`switchMap`](http://reactivex.io/documentation/operators/flatmap.html)
  - [`switchOnNext`](http://reactivex.io/documentation/operators/switch.html)
  - [`Synchronize`](http://reactivex.io/documentation/operators/serialize.html)
  - [**`Take`**](http://reactivex.io/documentation/operators/take.html)
  - [`take_with_time`](http://reactivex.io/documentation/operators/take.html)
  - [`takeFirst`](http://reactivex.io/documentation/operators/first.html)
  - [**`TakeLast`**](http://reactivex.io/documentation/operators/takelast.html)
  - [`takeLastBuffer`](http://reactivex.io/documentation/operators/takelast.html)
  - [`takeLastBufferWithTime`](http://reactivex.io/documentation/operators/takelast.html)
  - [`takeLastWithTime`](http://reactivex.io/documentation/operators/takelast.html)
  - [`takeRight`](http://reactivex.io/documentation/operators/last.html) (see also: [**`TakeLast`**](http://reactivex.io/documentation/operators/takelast.html))
  - [**`TakeUntil`**](http://reactivex.io/documentation/operators/takeuntil.html)
  - [`takeUntilWithTime`](http://reactivex.io/documentation/operators/takeuntil.html)
  - [**`TakeWhile`**](http://reactivex.io/documentation/operators/takewhile.html)
  - [`takeWhileWithIndex`](http://reactivex.io/documentation/operators/takewhile.html)
  - [`tail`](http://reactivex.io/documentation/operators/takelast.html)
  - [`tap`](http://reactivex.io/documentation/operators/do.html)
  - [`tapOnCompleted`](http://reactivex.io/documentation/operators/do.html)
  - [`tapOnError`](http://reactivex.io/documentation/operators/do.html)
  - [`tapOnNext`](http://reactivex.io/documentation/operators/do.html)
  - [**`Then`**](http://reactivex.io/documentation/operators/and-then-when.html)
  - [`thenDo`](http://reactivex.io/documentation/operators/and-then-when.html)
  - [`Throttle`](http://reactivex.io/documentation/operators/debounce.html)
  - [`throttleFirst`](http://reactivex.io/documentation/operators/sample.html)
  - [`throttleLast`](http://reactivex.io/documentation/operators/sample.html)
  - [`throttleWithSelector`](http://reactivex.io/documentation/operators/debounce.html)
  - [`throttleWithTimeout`](http://reactivex.io/documentation/operators/debounce.html)
  - [**`Throw`**](http://reactivex.io/documentation/operators/empty-never-throw.html)
  - [`throwError`](http://reactivex.io/documentation/operators/empty-never-throw.html)
  - [`throwException`](http://reactivex.io/documentation/operators/empty-never-throw.html)
  - [**`TimeInterval`**](http://reactivex.io/documentation/operators/timeinterval.html)
  - [**`Timeout`**](http://reactivex.io/documentation/operators/timeout.html)
  - [`timeoutWithSelector`](http://reactivex.io/documentation/operators/timeout.html)
  - [**`Timer`**](http://reactivex.io/documentation/operators/timer.html)
  - [**`Timestamp`**](http://reactivex.io/documentation/operators/timestamp.html)
  - [**`To`**](http://reactivex.io/documentation/operators/to.html)
  - [`to_a`](http://reactivex.io/documentation/operators/to.html)
  - [`ToArray`](http://reactivex.io/documentation/operators/to.html)
  - [`ToAsync`](http://reactivex.io/documentation/operators/start.html)
  - [`toBlocking`](http://reactivex.io/documentation/operators/to.html)
  - [`toBuffer`](http://reactivex.io/documentation/operators/to.html)
  - [`to_dict`](http://reactivex.io/documentation/operators/to.html)
  - [`ToDictionary`](http://reactivex.io/documentation/operators/to.html)
  - [`ToEnumerable`](http://reactivex.io/documentation/operators/to.html)
  - [`ToEvent`](http://reactivex.io/documentation/operators/to.html)
  - [`ToEventPattern`](http://reactivex.io/documentation/operators/to.html)
  - [`ToFuture`](http://reactivex.io/documentation/operators/to.html)
  - [`to_h`](http://reactivex.io/documentation/operators/to.html)
  - [`toIndexedSeq`](http://reactivex.io/documentation/operators/to.html)
  - [`toIterable`](http://reactivex.io/documentation/operators/to.html)
  - [`toIterator`](http://reactivex.io/documentation/operators/to.html)
  - [`ToList`](http://reactivex.io/documentation/operators/to.html)
  - [`ToLookup`](http://reactivex.io/documentation/operators/to.html)
  - [`toMap`](http://reactivex.io/documentation/operators/to.html)
  - [`toMultiMap`](http://reactivex.io/documentation/operators/to.html)
  - [`ToObservable`](http://reactivex.io/documentation/operators/from.html)
  - [`toSet`](http://reactivex.io/documentation/operators/to.html)
  - [`toSortedList`](http://reactivex.io/documentation/operators/to.html)
  - [`toStream`](http://reactivex.io/documentation/operators/to.html)
  - [`ToTask`](http://reactivex.io/documentation/operators/to.html)
  - [`toTraversable`](http://reactivex.io/documentation/operators/to.html)
  - [`toVector`](http://reactivex.io/documentation/operators/to.html)
  - [`tumbling`](http://reactivex.io/documentation/operators/window.html)
  - [`tumblingBuffer`](http://reactivex.io/documentation/operators/buffer.html)
  - [`unsubscribeOn`](http://reactivex.io/documentation/operators/subscribeon.html)
  - [**`Using`**](http://reactivex.io/documentation/operators/using.html)
  - [**`When`**](http://reactivex.io/documentation/operators/and-then-when.html)
  - [`Where`](http://reactivex.io/documentation/operators/filter.html)
  - [`while`](http://reactivex.io/documentation/operators/repeat.html)
  - [`whileDo`](http://reactivex.io/documentation/operators/repeat.html)
  - [**`Window`**](http://reactivex.io/documentation/operators/window.html)
  - [`windowWithCount`](http://reactivex.io/documentation/operators/window.html)
  - [`windowWithTime`](http://reactivex.io/documentation/operators/window.html)
  - [`windowWithTimeOrCount`](http://reactivex.io/documentation/operators/window.html)
  - [`windowed`](http://reactivex.io/documentation/operators/backpressure.html)
  - [`withFilter`](http://reactivex.io/documentation/operators/filter.html)
  - [`withLatestFrom`](http://reactivex.io/documentation/operators/combinelatest.html)
  - [**`Zip`**](http://reactivex.io/documentation/operators/zip.html)
  - [`zipArray`](http://reactivex.io/documentation/operators/zip.html)
  - [`zipWith`](http://reactivex.io/documentation/operators/zip.html)
  - [`zipWithIndex`](http://reactivex.io/documentation/operators/zip.html)
  - [`++`](http://reactivex.io/documentation/operators/concat.html)
  - [`+:`](http://reactivex.io/documentation/operators/startwith.html)
  - [`:+`](http://reactivex.io/documentation/operators/just.html)
