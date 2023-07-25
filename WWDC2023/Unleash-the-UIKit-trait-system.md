# Unleash the UIKit trait system

> 🗂WWDC23  | Category : UI Frameworks
>
> 🔗 [https://developer.apple.com/videos/play/wwdc2023/10057/](https://developer.apple.com/videos/play/wwdc2023/10057/)
>  <br />
>  <br />
> 🔖  Discover powerful enhancements to the trait system in UIKit. Learn how you can define custom traits to add your own data to UITraitCollection, modify the data propagated to view controllers and views with trait override APIs, and adopt APIs to improve flexibility and performance. We'll also show you how to bridge UIKit traits with SwiftUI environment keys to seamlessly access data from both UIKit and SwiftUI components in your app.
> <br />
<br />   

## Understanding traits

### Traits

Traits are independent pieces of data that the system automatically propagates to every view controller and view in your app

UIKit provides many built-in system traits,

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled.png)

iOS 17 부터 custom traits 정의 가능
<br /> 

### Trait collections

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%201.png)

trait collections 은 traits 와 관련 변수들을 포함

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%202.png)

iOS 17 부터 trait collections 에 new API 추가

1. 클로저를 취하는 새로운 initializer 
    - 클로저 내부에서 `mutableTraits` 를 받는다
    - mutable container 는 새로운 프로토콜 `UIMutableTraits` 를 conform
    - initializer 가 immutable `UITraitCollection` 인스턴스를 리턴

1. 기존 trait collection 의 값들을 수정하여 새로운 인스턴스를 생성해주는 `modifyingTraits` 메소드

직접 train collections 를 이렇게 생성해서 사용 할 수도 있지만 대부분 우리는 trait environment 에서 trait collections 을 얻어서 사용한다.
<br /> 

### Trait environments

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%203.png)

trait environments 에는 UIWindowScnene, UIWindow, UIPresentationController, UIViewController, UIView 가 있고 각각 고유의 trait collection 을 갖는다.
<br /> 

### Trait hierarchy

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%204.png)

- trait environment 은 앱을 통해 flow 타는 trait hierarchy 로 연결되어 있다
- 각각의 trait environment 는 parent environment 의 trait 값들을 상속 받는다
<br /> 

### View controller and view trait hierarchy

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%205.png)

iOS 17 이전

- ViewController 는 Parent View Controller 의 trait 을 직접 상속
- View Controller 에 속한 View 들은 View Controller 로 부터 trait 상속
- View Controller 에 속하지 않은 View 들은 superView 로 부터 trait 상속

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%206.png)

→ View hierarchy 에 있는 trait 의 플로우는  View Controller 에 속한 View 에서 끊긴다

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%207.png)

iOS17 에서는 trait hierarchy 를 통합

- View Controller 는 View 의 superview 에서 trait 상속

→ View Controller 가 이제 view hierarchy 에서 traits 를 상속 받기 때문에, ViewController 가 업데이트된 trait 을 받기 위해서 ViewController 의 View 는 반드시 hierarchy 에 있어야 함

→ View 가 hierarchy 에 추가 되기 전에 ViewController 의 trait collection 에 접근하면, ViewController 는 trait 의 최신값을 가져올 수 없다 
<br /> 

### View controller trait updates

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%208.png)

 `viewWillAppear` 는 View 가 hierarchy 에 추가 되기 전에 항상 호출됨

`viewIsAppearing` 

- `viewIsAppearing`  is called after viewWillAppear
- View 가 hierarchy 에 추가되면, View Controller, View 모두 최신 triat collection 을 갖게됨
- `viewIsAppearing`  is a drop-in replacement for nearly all cases where you’re using viewWillAppear
- this new method back-deploys all the way to iOS 13.
<br /> 

### View trait updates

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%209.png)

- iOS 17 에서는 view trait 업데이트의 일관성과 성능 향상
- View 는 hierarchy 안에 있을 때만 trait collection 을 없데이트함
- 일단 hierarchy 안에 있으면, 각각의 view 는 layout 이 수행되기 전에 즉시 자신의 trait collection 만 업데이트
- 가장 best practice 는 layout 동안 trait 을 사 용
    - View 는 `layoutSubViews()` 메소드 안에서 traitCollection 을 사용
    - `layoutSubViews()` 는 View 에서 `setNeedsLayout()` 이 호출 될때마다 동작 하기 때문에, 여러번 호출 할 때 반복된 작업을 피해야함
<br />
 
## Defining custom traits
<br /> 

### Uses for custom triats

When to define a new custom trait

- 여러  children 에게 데이터를 전달 해야 할때
    - ParentViewController → Muliple ChildViewControllers
    - SuperView → all of its subviews
- 다른 컴포넌트에 데이터를 전달 할때
    - 많은 레이어들이 nested 된 경우, direct connection 은 없을때
- 포함하고 있는 뷰컨 같은 environment 정보들을 넘겨줄때

trait system 이 강력하긴 하지만, 데이터 전달로서 사용하는건 좋지 않음

성능을 위해서는 직접 데이터를 전달하는것이 아닌, 값을 추가할때 사용해야 한다

**Custom trait**

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2010.png)

trait 을 정의 하면, 바로 `UITraitCollection`, `UIMutableTraits` 의 새로운 API 사용 가능

struct 자체를 키값처럼 사용

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2011.png)

Custom trait 정의 할때 extension 도 써주어야 함

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2012.png)

색상에 영향을 주는 trait 은 비용이 비싸기 때문에, 드물게 변화하는 trait 에만 사용

name → 디버거에 표시되는 이름

identifier 

- 인코딩 같은 추가 기능에 적합한 trait 으로 만들어줌
- reverse-DNS 포멧 사용
- 앱내에서 유니크 하도록

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2013.png)

테마에 따른 커스텀 다이나믹 컬러 정의

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2014.png)
<br /> 

### **Custom trait best practices**

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2015.png)

- 간단한 struct 나 enum 같은 value types 사용 
Class 기반의 trait 는 피하기
- 가장 효율적인 데이터 타입은 Bool, Int, Double, enum with Int raw value
`enum` with Int raw value 이 가장 유용한 데이터 타입
- 커스터머 타입은 `Equatable` 프로토콜을 구현해야함
<br /> 

### **Custom triats in Swift and Objective-C**

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2016.png)

Obj-C 도 사용 가능!

but… 옵씨, Swift 에 각각 cutom trait 정의해주어햐 한다
<br /> 

## Applying overrides
<br /> 

### Trait overrides

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2017.png)

Trait overrdies 는 trait hierachy 내부의 데이터를 수정하기 위한 매커니즘이다

iOS 17 부터는 trait overrides 를 더 적용하기 쉬워짐

각각의 trait envirionment classesdp `traitOverrides` 프로퍼티가 생김

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2018.png)

trait overrides 는 트리 구조 안에서 어떤 위치에서든 trait 값을 바꾼다

이 중 한 환경에서 적용되면 모든 하위 계층에 다 적용

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2019.png)

parent 에서 적용된 traitCollection 은 상속받고 있는 child 에도 적용

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2020.png)

아마 trait override 가 즉시 반영 되지 않을 수 있다

- View 는 layout 직전에 trait collection 을 업데이트 하기 때문에 
view 의 trait override 수정은 `layoutSubviews` 전까지는 바로 반영되지 않는다
- `traitOverrides` 프로퍼티는  override 가 적용되었는지, 제거되었는지도 확인해줌

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2021.png)

- contains 메소드로 이미 있는 거라면 제거, 없으면 적용
- `traitOverrides` 는 값을 세팅하기 위한 input mechanism 이다
    
    → Trait value 를 read 하기 위해서는 `traitCollection` 프로퍼티를 사용해야 함
    
- override 가 세팅된것이 없을 때, `traitOverrides` 에서 읽으면 exception 발생
<br /> 

### Maximize performance

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2022.png)

- 각각의 `traitOverride` 는 비용이 들기 때문에 꼭 필요한 경우에만 사용
- 값을 수정하는 경우, 시스템은 하위 trait collection 을 모두 업데이트 해야하기 때문에, `traitOverride` 를 수정하는 빈도수를 최소화 해야함
- 필요한 계층에 가장 가까운 곳에 수정하자
<br /> 

## Handling changes

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2023.png)

✔ `**traitCollectionDidChange(_:)` is deprecated in iOS17**

- 어떤 traitCollection 을 care 하는지 시스템은 알지 못하기 때문에 모든 traitCollection 이 변할때마다 계속 메소드를 콜해야함
- 각각의 trait 에 대한 change 를 등록하는 new API
    - target action 방식 또는 clouse
    - subclass 에서 메소드를 override 할필요 없기 때문에 어디서든 changes 를 옵저빙 가능

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2024.png)

iOS 17 이전 버전을 대응해서 `traitCollectionDidChange` 를 계속 사용해야 한다면,

어떤 특정 trait 에 대한 change 를 바라보고 있는지에 대한 구현을 추가해야함

⬇️ iOS 17 ⬇️

**Closure**

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2025.png)

- `UITraitHorizontalSizeClass` 같은 모든 시스템 trait 에 대한 심볼이 새로 생김
- 모든 trait 에 대한 변화에 대해 호출되지 않기 때문에 더이상 old, new trait 값들을 비교할 필요가 없음
- trait 에 변화가 생긴 객체가 첫번째 파라미터로 전달되기 때문에, weak 레퍼런스 캡쳐 할 필요 없이 바로 사용 할 수 있음 (`self: Self` 로 사용해야함)

- 다른 trait environment 에 대한 trait changes 도 옵저브 가능
- 다른 뷰에서  시스템 trait, 커스텀 trait 모두 변하면 클로저 실행
- register 한 뷰의 타입을 첫번째 파라미터로 적어줌

**target-action**

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2026.png)

- target 파라미터 생략 가능 (생략하면 registerForTraitChange 호출되는 같은 객체)
- view, previousTraitCollection 파라미터
<br /> 
### Semantic system trait sets

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2027.png)

- 시스템 다이나믹 컬러에 영향을 주는 system trait 리턴
- UIImage(named:) 사용해서 이미지 로드시 system traits 의 subset 리턴
<br /> 

### Unresgistration

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2028.png)
<br /> 

### Maximize performance

- 실제로 의존하는 trait 만 register 등록
- 변화에 대해 즉시 반응하지 않도록 해라
layoutSubviews 메소드 내부에서 trait 을 사용한다면, `setNeedsLayout` 을 호출해서 trait change 를 invalidate 해라 ? → View 가 layoutSubviews 를 받고, 즉시 업데이트를 하지 않도록
<br /> 

## SwiftUI bridging

UIKit → SwiftUI 로 데이터를 전달하는 끊김 없는 새로운 방법이 생기게 됨!

### Intergrating UIKit and SwiftUI

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2029.png)

UIKit 의 Custom trait 은 SwiftUI 의 environment keys 와 매우 유사

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2030.png)

둘을 브릿지 하기 위해서는 `UITraitBridgedEnvironmentKey` 프로토콜만 채택해주면 됨

UIKit → SwiftUI

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2031.png)

SwiftUI → UIKit

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2032.png)
<br /> 
### Next steps

![Untitled](/WWDC2023/images/Unleash-the-UIKit-trait-system/Untitled%202.png/Untitled%2033.png)
