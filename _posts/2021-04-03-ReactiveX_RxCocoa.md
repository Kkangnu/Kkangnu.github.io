---
layout: post
title: "Rxcocoa"
subtitle: "RxCocoa 맛보기"
date: 2021-04-03 21:30:13 0400
background: '/img/posts/06.jpg'
categories: [ReactiveX]
tag: [iOS]
---

# **RxCocoa 맛보기**

 

RxCocoa는 애플 환경의 애플리케이션을 제작하기 위한 도구들을 모아놓은 Cocoa Framework를 Rx와 합친 기능을 제공하는 라이브러리입니다.

 

RxCocoa는 RxSwift를 기반으로 만들어진 라이브러리이므로 사용하기 위해서는 CocoaPod과 같은 의존성 관리 도구에 별도로 추가해주어야 합니다.

 

**RxCocoa**



- UI Control과 다른 SDK 클래스를 wrapping 한 커스텀 extension set
- iOS, tvOS, macOS 등 다양한 플랫폼에서 동작

 

**ObserverType** **과 ObservableType**



- ObserverType: 값을 주입시킬 수 있는 타입
- ObservableType: 값을 관찰할 수 있는 타입

 

**ControlProperty**



Subject 와 같이 프로퍼티에 새 값을 주입시킬 수 있고 값의 변화도 관찰할 수 있는 타입으로 ControlPropertyType을 준수함.

 

이 ControlPropertyType 은 observableType 과 observerType 을 준수함.

 

**Binder**



- ObserverType을 준수함, 따라서 값을 생성해내고 주입할 수는 있으나 값을 관찰할 수는 없음.
- error 이벤트를 방출할 수 없음.
- RxCocoa 에서 binding은     Publisher 에서 Subscriber 로 향하는 단방향 binding임.
- bind(to:) 메소드는 subscribe() 의 별칭
- bind(to: observer) 를 호출하게 되면 subscribe(observer) 가 실행됨.

 

**Traits**



Traits는 UI 처리에 특화된 Observable 입니다.

RxCocoa가 제공하는 Traits는 아래와 같습니다.

 

- ControlProperty: 컨트롤에 data를 binding 하기 위해 사용.
- ControlEvent: 컨트롤의 event를 수신하기 위해 사용.
- Driver: error를 방출하지 않고 메인스레드에서 처리됨.
- Signal: Driver 와 거의 동일하나 자원을 공유하지 않음.

RxCocoa에서 Traits 는 아래와 같은 특징들을 갖습니다.

- error 를 방출하지 않음.
- 메인 스케줄러에서 observe 됨.
- 메인 스케줄러에서 subscribe 됨.
- Signal 을 제외한 나머지     Traits 들은 자원을 공유함.

bind(to:) 메소드는 메인스레드 실행을 보장함.

 

textField.rx.text

  .orEmpty

  .subscribe(onNext: { [**weak** **self**] str **in**

​    DispatchQueue.main.async {

​      **self**?.valueLabel.text = str

​    }

  })

  .disposed(by: disposedBag)

 

textField.rx.text

  .orEmpty

  .observeOn(MainScheduler.instance)

  .subscribe(onNext: { [**weak** **self**] str **in**

​    **self**?.valueLabel.text = str

  })

  .disposed(by: disposeBag)

위와 같이 main Thread로 전환하고 subscribe 할 필요 없이 bind를 이용하면 두 가지 모두 한번에 해결된다.

**textField**.rx.text

  .orEmpty

  .bind(**to**: **valueLabel**.rx.text)

  .disposed(**by**: **disposeBag**)

즉 Binder를 이용하면 binding 작업을 언제나 Main Thread에서 실행해주기 때문에 쓰레드에 대한 관리를 해 줄 필요가 없다는 장점이 있음.

 

**Beginning RxCocoa**



(아래 내용은 Raywenderlich의 RxSwift - Ch12.Beginning RxCocoa 내용을 기반으로 작성하였습니다.)

 

이번 챕터에서는 RxCocoa에 대한 빠른 이해를 위해 [OpenWeathermap API](http://openweathermap.org/) 를 이용한 날씨 앱을 만들어볼 것입니다.

 

예제 프로젝트 Wundercast에 적용된 라이브러리는 RxSwift, RxCocoa, SwiftyJSON 입니다.

 

위 서비스에서 제공하는 API 사용을 위해 API Key를 생성하여 프로젝트 내에서 사용합니다.

 

**RxCocoa****를 사용하여 데이터 표시하기**



 

ApiController.swift에서 SON 데이터를 매핑할 데이터 모델로 쓰일 Weather struct 를 확인할 수 있습니다.

 

실질적으로 Weather 구조체 타입의 Observable을 반환하는 함수를 살펴보겠습니다.

**func** **currentWeather**(city: String) -> **Observable**<**Weather**> {

​    **return** **Observable**.just(**Weather**(cityName: city,

​      temperature: 20,

​      humidity: 90,

​      icon: iconNameToChar(icon: "01d")))

  }

날씨를 조회할 도시 이름을 인자로 전달받아 Weahter 타입의 Observable 구조체를 반환하는 함수입니다.

 

이 함수는 실존하지 않는 가짜 도시명과 더미 데이터를 리턴하여 실제 통신을 통하여 서버로부터 데이터를 받아오기 전까지 표시됩니다.

 

위 함수를 ViewController.swift 파일의 viewDidLoad() 메서드 내에서 호출해보도록 하겠습니다.

 

**ApiController**.shared.currentWeather(city: "RxSwift")

​    .observeOn(**MainScheduler**.instance)

​    .subscribe(onNext: { [**weak** **self**] data **in**

​      **guard** **let** **self** = **self** **else** { **return** }

​      **self**.tempLabel.text = "\(data.temperature)"

​      **self**.iconLabel.text = data.icon

​      **self**.humidityLabel.text = "\(data.humidity)"

​      **self**.cityNameLabel.text = data.cityName

​    })

 

가져온 Weather 타입 데이터를 기반으로 UI에 뿌려주는 작업이므로 Main Thread 에서 실행해주었습니다.

 

![image](file:///C:/Users/Rhee/AppData/Local/Temp/msohtmlclip1/01/clip_image001.png)

 

위 코드는 정상적으로 실행은 되나 두 가지 문제가 발생합니다.

- 컴파일러 경고 발생 (Result of call to     'subscribe(onNext:onError:onCompleted:onDisposed:) is unused')
- 텍스트필드 입력부분에 대한 구현 부재

첫 번째 문제가 발생하는 이유는 위에서 사용한 subscribe(onNext:onError:onCompleted:onDisposed) 메소드가 Disposable을 반환하지만 이에 대한 처리를 진행하지 않아 발생하는 문제입니다.

 

따라서 disposeBag 객체를 생성하여 dispose를 아래와 같이 진행해줍니다.

**private** **let** disposeBag = **DisposeBag**()

 

**ApiController**.shared.currentWeather(city: "RxSwift")

​      .observeOn(**MainScheduler**.instance)

​      .subscribe(onNext: { [**weak** **self**] data **in**

​        **guard** **let** **self** = **self** **else** { **return** }

​        **self**.tempLabel.text = "\(data.temperature)"

​        **self**.iconLabel.text = data.icon

​        **self**.humidityLabel.text = "\(data.humidity)"

​        **self**.cityNameLabel.text = data.cityName

​      })

​      .disposed(by: disposeBag)

 

이는 ViewController 의 릴리즈 여부에 따라 구독 취소/dispose를 하게 됩니다.

 

이를 통해 리소스 낭비를 막을수 있습니다.

 

두 번째 문제는 RxCocoa 프레임워크가 제공하는 rx 기능을 통해 textFiled 입력부분을 제어해보도록 합니다.

searchCityName.rx.text

​      .filter { ($0 ?? "").count > 0 }

​      .flatMapLatest { text **in**

​        **return** **ApiController**.shared.currentWeather(city: text ?? "Error")

​          .catchErrorJustReturn(**ApiController**.**Weather**.empty)

​      }

​      .observeOn(**MainScheduler**.instance)

​      .subscribe(onNext: { [**weak** **self**] data **in**

​        **guard** **let** **self** = **self** **else** { **return** }

​        **self**.tempLabel.text = "\(data.temperature)"

​        **self**.iconLabel.text = data.icon

​        **self**.humidityLabel.text = "\(data.humidity)"

​        **self**.cityNameLabel.text = data.cityName

​      })

​      .disposed(by: disposeBag)

textField의 rx 익스텐션 기능을 이용하여 입력받는 text 값을 제어합니다.

 

filter 를 통해 입력값의 유효성을 간단히 검증하고 flatMapLatest를 통하여 마지막으로 입력된 시퀀스 이벤트만 구독하도록 합니다.

 

이를 currentWeather 메소드의 인자로 전달하고 반환되는 Observable<Weather> 를 기존대로 스레드 전환을 통해 처리합니다.

 

![image](file:///C:/Users/Rhee/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

작업 흐름은 위와 같습니다.

 

TextFiled 입력 -> Filter -> flatMap -> subscribe -> 작업 처리

 

**OpenWeather API****를 통해 데이터 가져오기**



 

API 통신을 통하여 JSON 형태의 데이터를 받아올 것이며 response는 아래 형태와 같습니다.

 {

   "weather": [

​     {

​       "id": 741,

​       "main": "Fog",

​       "description": "fog",

​       "icon": "50d"

​     }

   ],

 }

main 은 아래와 같이 온도와 습도를 표시하는데 이용할 데이터들을 묶어놓았습니다.

 "main": {

​      "temp": 271.55,

​      "pressure": 1043,

​      "humidity": 96,

​      "temp_min": 268.15,

​      "temp_max": 273.15

​    }

  }

통신을 통해 데이터를 가져오는데 이용할 함수는 아래와 같습니다.

**private** **func** **buildRequest**(method: String = "GET", pathComponent: String, params: [(String, String)]) -> **Observable**<**JSON**> {

 

​    **let** url = baseURL.appendingPathComponent(pathComponent)

​    **var** request = **URLRequest**(url: url)

​    **let** keyQueryItem = **URLQueryItem**(name: "appid", value: apiKey)

​    **let** unitsQueryItem = **URLQueryItem**(name: "units", value: "metric")

​    **let** urlComponents = **NSURLComponents**(url: url, resolvingAgainstBaseURL: **true**)!

 

​    **if** method == "GET" {

​      **var** queryItems = params.map { **URLQueryItem**(name: $0.0, value: $0.1) }

​      queryItems.append(keyQueryItem)

​      queryItems.append(unitsQueryItem)

​      urlComponents.queryItems = queryItems

​    } **else** {

​      urlComponents.queryItems = [keyQueryItem, unitsQueryItem]

 

​      **let** jsonData = **try**! **JSONSerialization**.data(withJSONObject: params, options: .prettyPrinted)

​      request.httpBody = jsonData

​    }

 

​    request.url = urlComponents.url!

​    request.httpMethod = method

 

​    request.setValue("application/json", forHTTPHeaderField: "Content-Type")

 

​    **let** session = **URLSession**.shared

 

​    **return** session.rx.data(request: request).map { **try** **JSON**(data: $0) }

  }

RxCocoa의 URLSession wrapper를 이용하여 네트워크 요청을 보내며 동작은 아래와 같습니다.

- baseURL에 subPath를     추가하여 GET / POST 방식의 요청을 작성합니다.
- application/json 에 요청 콘텐츠 타입으 설정합니다.
- JSON 객체로 데이터를 매핑하여 반환합니다.

 

**return** session.rx.**data**(request: request).map { **try** JSON(**data**: $0)}

해당 부분이 request를 통해 받은 Data타입의 response를 JSON 타입의 Observable로 매핑하여 반환하는 부분입니다.

 

![image](file:///C:/Users/Rhee/AppData/Local/Temp/msohtmlclip1/01/clip_image003.png)

앞서 더미 데이터를 가져오는데 이용했던 함수 currentWeather(city:) 를 아래와 같이 변경합니다.

**func** **currentWeather**(city: String) -> **Observable**<**Weather**> {

​    **return** buildRequest(pathComponent: "weather", params: [("q", city)])

​      .**map** { json in

​        **return** Weather(cityName: json["name"].**string** ?? "Unknown",

​          temperature: json["main"]["temp"].**int** ?? -1000,

​          humidity: json["main"]["humidity"].**int** ?? 0,

​          icon: iconNameToChar(icon: json["weather"][0]["icon"].**string** ?? "e"))

​    }

  }

JSON 타입 Observable 을 반환하는 buildRequest(pathComponent:params:) 메소드를 반환합니다.

 

내부에서 JSON 데이터를 SwiftyJSON을 이용하여 필요 데이터를 추출하여 Weather타입으로 가공하여 반환하게되어 결국 Weather 타입의 Observable이 반환됩니다.

 

위와 같이 코드를 작성하고 실행해보겠습니다.

 

![image](file:///C:/Users/Rhee/AppData/Local/Temp/msohtmlclip1/01/clip_image004.png)

실시간으로 API를 통해 받아온 데이터를 기반으로 UI에 정상적으로 뿌려지는 것을 확인할 수 있습니다.

 

**Observable** **바인딩**



 

RxCocoa의 바인딩은 단방향 데이터 스트림입니다.

1. Observable 바인딩이란?

두 개의 연결된 속성에 대한 관계를 생각해보면 이해가 한결 쉽습니다.

![image](file:///C:/Users/Rhee/AppData/Local/Temp/msohtmlclip1/01/clip_image005.png)

 

Producer & Receiver 혹은 Publisher &Subscriber 모델이라고 부르는 형태입니다.

 

Producer가 값을 생성하고 이 값은 Receiver에게 바인드 되어집니다.

 

Receiver가 값을 수신하면 이를 처리합니다.

 

앞서 말씀드렸듯 RxSwift에서의 바인딩은 단방향 스트림이므로 역방향 바인딩은 성립하지 않습니다.

 

**bind(to:)**

 

바인딩 로직을 위해 제공되는 기본 함수는 bind(to:) 입니다.

 

관찰가능한 Observable 타입을 다른 속성에 바인딩하기 위해서는 Receiver가 당연히 ObservableType 이어야 합니다.

 

이제 바인딩이 무엇인지 알았으니 직접 앱에 적용해보겠습니다.

 

첫 번째로 할 일은 UILabel 과 subscribe(onNext:) 에 적절한 데이터를 할당하기 위하여 observable을 리팩토링 하는 것 입니다.

 

viewDidLoad() 를 아래와 같이 변경합니다.

 

**override** **func** **viewDidLoad**() {

​    **super**.viewDidLoad()

​    // Do any additional setup after loading the view, typically from a nib.

 

​    style()

 

​    // 1

​    **let** search = searchCityName.rx.text

​      .filter { ($0 ?? "").count > 0 }

​      .flatMapLatest { text **in**

​        **return** **ApiController**.shared.currentWeather(city: text ?? "Error")

​      }

​      .share(replay: 1)

​      .observeOn(**MainScheduler**.instance)

 

 

​    // 2

​    search.map { "\($0.temperature)℃"}

​      .bind(to: tempLabel.rx.text)

​      .disposed(by: disposeBag)

 

​    // 3

​    search.map { "\($0.humidity)%"}

​      .bind(to: humidityLabel.rx.text)

​      .disposed(by: disposeBag)

 

​    search.map { "\($0.cityName)"}

​      .bind(to: cityNameLabel.rx.text)

​      .disposed(by: disposeBag)

 

​    search.map { "\($0.icon)"}

​      .bind(to: iconLabel.rx.text)

​      .disposed(by: disposeBag)

 

​      }

주석을 따라서 살펴보도록 하겠습니다.

- 사용자가 textField에 입력되는 값을 flatMapLatest     연산자를 통해 최신값을 기준으로 currentWeather 함수의 인자로 전달하며     해당 결과 값의 작업을 메인 스레드로 전환합니다.

- 결과값 Weather 타입의 Observable을     기반으로 bind 로직을 구현합니다.

- - Weather.temperature 값을 tempLabel.rx.text 에 바인드하는 것 처럼 모든 label에 적절한 값을 바인딩합니다.

![image](file:///C:/Users/Rhee/AppData/Local/Temp/msohtmlclip1/01/clip_image006.png)

이와 같이 Weather 타입 모델링을 통해 가독성 높은 재사용 가능 코드로 전환이 가능합니다.

 

**Traits****를 이용한 코드 개선**



RxCocoa는 bind(to:) 외에도 Cocoa 프레임워크와 UIKit에 대한 진보된 기능들을 제공합니다.

 

해당 기능들을 담당하는 것을 Trait이라고 칭하며 이는 UI 처리에 특화된 Observable 항목의 특수 구현을 제공합니다.

 

Trait 는 UI 작업에 대하여 직관적이고 작성하기 쉬운 코드를 작성하는데 도움이 되는 Observable 특수 클래스 입니다.

 

**ControlProperty** **와 Driver**

 

**Trait**의 특장점은 앞서 살펴봤으나 다시 한번 정리하겠습니다.

- Trait은 에러 방출이 불가합니다.
- Trait은 메인 스케줄러에서 관찰 / 구독 합니다.
- Trait은 부수작용들을 공유합니다.

RxCocoa에서 제공하는 Trait은 아래와 같습니다.

- ControlProperty: 데이터를 UI에 바인딩할 떄 rx 익스텐션을 통해 사용합니다.
- ControlEvent: UI 컴포넌트에서의 이벤트 리스너 역할을 합니다. (ex: 텍스트필드에서의     리턴 버튼 터치)
- Driver: 에러를 방출하지 않는 특별한 Observable 입니다. 모든 과정은 메인 스레드에서 이루어집니다.

Trait을 이용하면 앞서 UI 작업을 위해 호출했던 .observeOn(MainScheduler.instance) 와 같은 메소드는 잊어버려도 좋습니다.

 

기존의 코드를 ControlProperty 와 Driver를 이용하여 리팩토링 해보겠습니다.

 

**let** search = searchCityName.rx.text

​      .filter { ($0 ?? "").count > 0 }

​      .flatMapLatest { text **in**

​        **return** **ApiController**.shared.currentWeather(city: text ?? "Error")

​      }

​      .asDriver(onErrorJustReturn: **ApiController**.**Weather**.empty)  // error 핸들링

위 코드에서 중요한 부분은 제일 마지막 줄 입니다.

 

.asDriver 메소드를 통해 Observable<Weather> 를 Driver<Weather> 타입으로 변환하며 방출된 에러에 대한 핸들링을 진행합니다.

 

이후, Observable 에서는 bind(to:) 메소드를 통해 바인딩 했으나 Driver는 drive() 메소드를 이용합니다.

 

따라서 바인딩 로직 코드를 아래와 같이 고쳐줍니다.

search.**map** { "\($0.temperature)℃"}

​      .drive(tempLabel.rx.text)

​      .disposed(by: disposeBag)

 

​    search.**map** { "\($0.humidity)%" }

​      .drive(humidityLabel.rx.text)

​      .disposed(by: disposeBag)

 

​    search.**map** { $0.cityName }

​      .drive(cityNameLabel.rx.text)

​      .disposed(by: disposeBag)

 

​    search.**map** { $0.icon }

​      .drive(iconLabel.rx.text)

​      .disposed(by: disposeBag)

 
