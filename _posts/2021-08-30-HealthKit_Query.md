---
layout: post
title: "HealthKit Query 종류"
subtitle: "HKQuery를 알아보자"
date: 2021-08-30  09:58:28 -0400
background: '/img/posts/06.jpg'
categories: [iOS]
tag: [iOS]


---

# ***HealthKit Query 종류***

[TOC]

# 1. [HKQuery](https://developer.apple.com/documentation/healthkit/hkquery)

> HealthKit의 모든 쿼리 클래스에 대한 추상 클래스입니다.

+ 선언

```swift
class HKQuery : NSObject
```

* 개요

>  이 `HKQuery`클래스는 HealthKit 저장소에서 데이터를 검색하는 모든 쿼리 개체의 기초입니다. `HKQuery`클래스는 추상 클래스입니다. 직접 인스턴스화하면 안 됩니다. 대신 항상 구체적인 하위 클래스 중 하나를 사용하여 작업합니다.

---

# 2. [HKSampleQuery](https://developer.apple.com/documentation/healthkit/hksamplequery)

> 현재 HealthKit 저장소에 저장된 모든 일치하는 샘플의 스냅샷을 반환하는 일반 쿼리입니다.

* 선언

```swift
class HKSampleQuery : HKQuery
```

* 개요

> 당신은 어떤 구체적인 하위 클래스를 검색하는 샘플 쿼리를 사용할 수 있습니다 [`HKSample`](https://developer.apple.com/documentation/healthkit/hksample)포함한 클래스, , , , 및 객체.[`HKCategorySample`](https://developer.apple.com/documentation/healthkit/hkcategorysample)[`HKCorrelation`](https://developer.apple.com/documentation/healthkit/hkcorrelation)[`HKQuantitySample`](https://developer.apple.com/documentation/healthkit/hkquantitysample)[`HKWorkout`](https://developer.apple.com/documentation/healthkit/hkworkout)샘플 쿼리는 제공된 유형 및 조건자와 일치하는 샘플 개체를 반환합니다. 반환된 샘플에 대한 정렬 순서를 제공하거나 반환된 샘플 수를 제한할 수 있습니다. 다른 쿼리 클래스를 사용하여 보다 전문화된 검색 및 계산을 수행할 수 있습니다. 자세한 내용은 을(를) 참고하십시오 [`HKQuery`](https://developer.apple.com/documentation/healthkit/hkquery). 
>
> 샘플 쿼리는 변경할 수 없습니다. 쿼리의 속성은 쿼리가 처음 생성될 때 설정되며 변경할 수 없습니다.

* 메모

> 많은 HealthKit 클래스와 마찬가지로 클래스는 하위 클래스로 분류되어서는 안 됩니다.`HKSampleQuery`

---

# 3.[`HKQuantitySeriesSampleQuery`](https://developer.apple.com/documentation/healthkit/hkquantityseriessamplequery)

> 수량 샘플과 연결된 시리즈 데이터에 액세스하는 쿼리입니다.

* 선언

```swift
class HKQuantitySeriesSampleQuery : HKQuery
```

* 개요

> [`HKQuantitySeriesSampleBuilder`](https://developer.apple.com/documentation/healthkit/hkquantityseriessamplebuilder) 를 사용하여 [`HKQuantity`](https://developer.apple.com/documentation/healthkit/hkquantity)샘플에 추가된 개별 개체 에 액세스하려면 시리즈 쿼리를 사용합니다 .

* 중요한

> 많은 일반적인 계산의 경우 대신 통계 쿼리를 사용하는 것이 좋습니다. 
>
> 통계 쿼리는 샘플이 단일 수량을 나타내든 시리즈를 나타내든 상관없이 수량 데이터를 올바르게 처리합니다.

---

# 4. [HKAnchoredObjectQuery](https://developer.apple.com/documentation/healthkit/hkanchoredobjectquery)

>  새로운 변경 사항의 스냅샷과 장기 실행 쿼리로서의 지속적인 모니터링을 포함하여 HealthKit 저장소에 변경 사항을 반환하는 쿼리입니다.

* 선언

```swift
class HKAnchoredObjectQuery : HKQuery
```

* 개요

> 고정된 개체 쿼리는 HealthKit 저장소에서 새 데이터를 검색하는 쉬운 방법을 제공합니다. [`HKAnchoredObjectQuery`](https://developer.apple.com/documentation/healthkit/hkanchoredobjectquery) 은 해당 쿼리에서 수신한 마지막 샘플 또는 삭제된 개체에 해당하는 앵커 값을 반환합니다. 후속 쿼리는 이 앵커를 사용하여 결과를 최근에 저장되거나 삭제된 개체로만 제한할 수 있습니다.
>
> 고정된 개체 쿼리는 대부분 변경할 수 없습니다. 개체를 인스턴스화한 후 쿼리의 [`updateHandler`](https://developer.apple.com/documentation/healthkit/hkanchoredobjectquery/1615691-updatehandler)속성을 할당할 수 있지만 개체를 인스턴스화할 때 다른 모든 속성을 설정해야 합니다. 변경할 수 없습니다.

---

# 5. [HKObserverQuery](https://developer.apple.com/documentation/healthkit/hkobserverquery)

> HealthKit 스토어가 일치하는 샘플을 저장하거나 삭제할 때 HealthKit 스토어를 모니터링하고 앱을 업데이트하는 장기 실행 쿼리입니다.

* 선언

```swift
class HKObserverQuery : HKQuery
```

* 개요

> 관찰자 쿼리는 백그라운드 대기열에서 장기 실행 작업을 설정합니다. 이 작업은 HealthKit 저장소를 감시하고 저장소가 일치하는 데이터를 저장하거나 제거할 때 경고합니다. 앱은 관찰자 쿼리를 사용하여 다른 앱 및 장치의 변경 사항에 응답합니다.
>
> 관찰자 쿼리는 변경할 수 없습니다. 처음 만들 때 속성을 설정하고 변경할 수 없습니다.

---



# 6.[`HKCorrelationQuery`](https://developer.apple.com/documentation/healthkit/hkcorrelationquery)

> 상관 관계의 내용을 기반으로 복잡한 검색을 수행하고 일치하는 모든 샘플의 스냅샷을 반환하는 쿼리입니다.

* 선언

```swift
class HKCorrelationQuery : HKQuery
```

* 개요

> 상관 샘플은 여러 수량 또는 범주 샘플을 그룹화하는 컨테이너 역할을 합니다. [`HKSample`](https://developer.apple.com/documentation/healthkit/hksample)개체를 사용 하여 상관 관계를 검색 할 수 있지만 상관 관계 쿼리를 사용하면 포함된 샘플을 기반으로 더 복잡한 필터링이 가능합니다. 특히 상관 쿼리를 사용하면 상관에 저장된 각 샘플 유형에 대해 별도의 술어를 제공할 수 있습니다. 상관 관계의 술어와 모든 샘플 술어가 일치하는 경우에만 상관 관계가 리턴됩니다.

---

# 7.[`HKDocumentQuery`](https://developer.apple.com/documentation/healthkit/hkdocumentquery)

> 현재 HealthKit 저장소에 저장된 일치하는 모든 문서의 스냅샷을 반환하는 쿼리입니다.

* 선언

```swift
class HKDocumentQuery : HKQuery
```

* 개요

> `HKDocumentQuery` 개체를 사용 하여 HealthKit 저장소에서 문서를 검색합니다. 검색 결과를 필터링하기 위한 조건자, 반환된 샘플의 정렬 순서 또는 반환된 샘플 수에 대한 제한을 제공할 수 있습니다.
>
> 문서 쿼리는 변경할 수 없습니다. 쿼리의 속성은 쿼리가 처음 생성될 때 설정됩니다. 그들은 변경할 수 없습니다.

---

# 8.[`HKVerifiableClinicalRecordQuery`](https://developer.apple.com/documentation/healthkit/hkverifiableclinicalrecordquery)

> SMART 건강 카드에 대한 일회성 액세스에 대한 쿼리입니다.

* 선언

```swift
class HKVerifiableClinicalRecordQuery : HKQuery
```

* 개요

> [`HKVerifiableClinicalRecordQuery`](https://developer.apple.com/documentation/healthkit/hkverifiableclinicalrecordquery)개체를 사용하여 SMART 건강 카드에 대한 일회성 액세스를 요청합니다. 예를 들어 다음 코드는 지난 6개월 이내에 예방 접종을 나타내는 카드를 요청합니다.

---

# 9.[`HKHeartbeatSeriesQuery`](https://developer.apple.com/documentation/healthkit/hkheartbeatseriesquery)

> 하트비트 시리즈 샘플에 포함된 하트비트 데이터를 반환하는 쿼리입니다.

* 선언

```swift
class HKHeartbeatSeriesQuery : HKQuery
```



---

# 10.[`HKElectrocardiogramQuery`](https://developer.apple.com/documentation/healthkit/hkelectrocardiogramquery)

> 심전도 샘플에 대한 기본 전압 측정값을 반환하는 쿼리입니다.

* 선언

```swift
class HKElectrocardiogramQuery : HKQuery
```

* 개요

> `HKElectrocardiogramQuery`쿼리를 사용하여 [`HKElectrocardiogram`](https://developer.apple.com/documentation/healthkit/hkelectrocardiogram)샘플과 관련된 개별 전압 측정에 액세스합니다 .

---

# 11.[`HKQueryDescriptor`](https://developer.apple.com/documentation/healthkit/hkquerydescriptor)

> 데이터 유형 및 술어를 기반으로 샘플 세트를 지정하는 설명자.

* 선언

```swift
class HKQueryDescriptor : NSObject
```

* 개요

> 설명자를 사용하여 여러 데이터 유형을 반환하는 쿼리를 만듭니다. [`HKSampleQuery`](https://developer.apple.com/documentation/healthkit/hksamplequery)[`HKAnchoredObjectQuery`](https://developer.apple.com/documentation/healthkit/hkanchoredobjectquery)또는 [`HKObserverQuery`](https://developer.apple.com/documentation/healthkit/hkobserverquery)인스턴스 만들 때 설명을 사용할 수 있습니다.

---

# 12.[`HKSourceQuery`](https://developer.apple.com/documentation/healthkit/hksourcequery)

> HealthKit 스토어에 일치하는 쿼리를 저장한 앱 및 장치와 같은 소스 목록을 반환하는 쿼리입니다.

* 선언

```swift
class HKSourceQuery : HKQuery
```

* 개요

> 소스 쿼리는 지정된 샘플 유형과 일치하는 샘플을 저장한 소스 목록을 반환합니다. 소스는 앱 또는 기기(예: Apple Watch 또는 Bluetooth 심박수 모니터)일 수 있습니다.
>
> 소스 쿼리는 변경할 수 없습니다. 해당 속성은 처음 생성될 때 설정되며 변경할 수 없습니다.

---

# 13.[`HKStatisticsQuery`](https://developer.apple.com/documentation/healthkit/hkstatisticsquery)

> 일치하는 수량 샘플 집합에 대해 통계 계산을 수행하고 결과를 반환하는 쿼리입니다.

* 선언

```swift
class HKStatisticsQuery : HKQuery
```

* 개요

> 통계 쿼리는 일치하는 샘플 집합에 대한 공통 통계를 계산합니다. 통계 쿼리를 사용하여 불연속 수량 집합의 최소값, 최대값 또는 평균값을 계산하거나 누적 수량의 합계를 계산하는 데 사용할 수 있습니다. 가능한 계산의 전체 목록은 [`HKStatisticsOptions`](https://developer.apple.com/documentation/healthkit/hkstatisticsoptions)을 참조하십시오 . 사용 가능한 수량 유형에 대한 자세한 내용과 이 유형이 불연속 값인지 누적 값인지 알아보려면 [데이터 유형을](https://developer.apple.com/documentation/healthkit/data_types) 참조하십시오 .
>
> 수량 샘플에만 통계 쿼리를 사용할 수 있습니다. 운동 또는 상관 샘플에 대한 통계를 계산하려면 적절한 쿼리를 수행하고 데이터를 직접 처리해야 합니다.
>
> 통계 쿼리는 변경할 수 없습니다. 속성은 처음 생성될 때 설정되며 변경할 수 없습니다.

---

# 14.[`HKStatisticsCollectionQuery`](https://developer.apple.com/documentation/healthkit/hkstatisticscollectionquery)

> 일련의 고정 길이 시간 간격에 대해 여러 통계 쿼리를 수행하는 쿼리입니다.

* 선언

```swift
class HKStatisticsCollectionQuery : HKQuery
```

* 개요

> 통계 수집 쿼리는 종종 그래프 및 차트에 대한 데이터를 생성하는 데 사용됩니다. 예를 들어, 하루의 총 걸음 수 또는 각 시간의 평균 심박수를 계산하는 통계 수집 쿼리를 만들 수 있습니다. 관찰자 쿼리와 마찬가지로 컬렉션 쿼리는 장기 실행 쿼리로 작동하여 HealthKit 저장소의 콘텐츠가 변경될 때 업데이트를 수신할 수도 있습니다.
>
> * 중요한
>
>   > 수량 샘플이 있는 통계 수집 쿼리만 사용할 수 있습니다. 운동 또는 상관 샘플에 대한 통계를 계산하려면 적절한 쿼리를 수행하고 데이터를 직접 처리해야 합니다.
>
> 통계 수집 쿼리는 대부분 변경할 수 없습니다. 개체를 인스턴스화한 후 쿼리 [`initialResultsHandler`](https://developer.apple.com/documentation/healthkit/hkstatisticscollectionquery/1615755-initialresultshandler)및 [`statisticsUpdateHandler`](https://developer.apple.com/documentation/healthkit/hkstatisticscollectionquery/1615723-statisticsupdatehandler) 속성을 할당할 수 있습니다 . 개체를 인스턴스화할 때 다른 모든 속성을 설정해야 하며 변경할 수 없습니다.
>
> 통계 조회에 대한 자세한 내용은 [`HKStatisticsQuery`](https://developer.apple.com/documentation/healthkit/hkstatisticsquery)을 참조하십시오 .

