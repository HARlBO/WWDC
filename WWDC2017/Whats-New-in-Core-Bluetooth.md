# What's New in Core Bluetooth

>  📅 2020.6.9 (TUE)
>
> 🗂 WWDC2017 | Session : 712 | Category : System Frameworks
>
> 🔗 [https://developer.apple.com/videos/play/wwdc2017/712/](https://developer.apple.com/videos/play/wwdc2017/712/)
>  <br />
>  <br />
>  🔖 Discover how watchOS 4 makes it possible for a watchOS app to communicate with Bluetooth Low Energy accessories. Learn about changes to Core Bluetooth that improve reliability and enable high performance streaming connections with Bluetooth Low Energy Accessories. Understand the best practices in Bluetooth Low Energy accessory design.
> <br />
<br />  

### Centrals and Peripherals

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled.png)

Bluetooth Low Energy has two roles

- Central : look for devices that are around your environment
- Peripherals : beacon to the world, send out data or just let their presence be known

Bluetooth Low Energy 의 두가지 역할

- Central :주변에 있는 디바이스 탐색
- Peripherals : 신호 보내기, 데이터 전송, 존재 알리기

### GATT Database

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%201.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%201.png)

After you find this device that's around you, you can connect to your device and then you have bidirectional communication through what's called the GATT Protocol(takes all of your data and expose them in a hierarchy called services and characteristics).

CGService : contain characteristics inside of it

주변에 있는 디바이스를 찾으면, 너의 기기에 연결 할 수 있고, GATT 프로토콜을 통해 양방향 소통이 가능하다. GATT 프로토콜은 너의 모든 데이터를 가지고 있고 services and characteristics 라고 불리는 계층에 노출한다.

CBSerivce 는 CBCharacteristic 을 포함하고 있다.

대부분의 경우, 너의 애플 디바이스는 연결의 중간 역할을 할 것이고 peripheral 에 연결 할 것이다. 반대의 경우도 될 수 있다.

### Reading Characteristics as a Central

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%202.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%202.png)

`retrieveConnectedPeripherals`  :  연결 되어있는 peripheral 들을 검색 할 수 있다.

`retrievePeripherals(withIdentifiers :)` :  호출 할 수 있는 ㅊㅊ의 identifier을 가지고 있으면 peripheral 을 가져 올 수 있다.

## Enhanced Reliability

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%203.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%203.png)

디바이스를 하루 종일 연결시켜 놓고 싶어한다. 차고 있는 동안이나 자는 동안에도 데이터에 접근하고 싶어하며 액세서리와의 연결이 신뢰되어야 한다.

### Backrounded Apps

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%204.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%204.png)

### CGCentralManager restoration

- **Central operation s can continue when your app is not running**
    - Scan for new devices with services
    - Connect to an already known device

Central은 새로운 기기를 스캔 할 수 있고, 너의 앱이 메모리 이슈 등이 있을 때 Core Blutooth가 계속 널 위해 디바이스를 찾아 줄 것이다.

```swift
public let CBCentralManagerOptionRestoreIdentifierKey: String

optional public func centralManager(_ central: CBCentralManager, 
																		willRestoreState dict: [String: Any])
```

CGCentralManager를 초기화 할 때 restoreIdentifier 를 전달한다. 액세서리에 연결하는 등의 액션이 끝나면, 앱이 실행 되고 오래 지났더라도, 우리는 앱을 재실행 하고 콜백을 보내 준다. 

CentralManager가 state를 복원 할 것이다.

- Peripheral operations can continue when your app is not running
    - Publish local services
    - Advertise service UUID

Peripheral은 앱이 더이상 작동하고 있지 않더라도 무언가를 계속 할 수 있고 앱이 필요 할 때 재실행 할 것이다.

```swift
public let CBPeripheralManagerOptionRestoreIdentifierKey: String

optional public func peripheralManager(_ peripheral: CGPeripheralManager,
																			 willRestoreState dict: [String: Any])
```

여기서도 시스템의 유니크한 restoreIdentifier가 있다. 그리고 돌아왔을 때 현재 state를 `CBPeripheralManagerRestoredServiceKey:`를 통해 알려주고,  어떤 서비스가 여전히 publish하는지 알려준다.

### 🆕 State Preservation and Restoration

**Works across device reboot or Bluetooth system events**

- Try to ask for as few system resources as possible
- Background activities will be stopped if
    - User force quits the app
    - User disables Bluetooth

디바이스가 재시동 되어도 연결을 유지 시켜 줄 것이다!  scan 할 수 있는 서비스의 개수를  제한 했다.

### 🆕 Write Without Response

- Write Without Response would be dropped due to memory pressure
- New property will tell your app if more data can be sent

```swift
open class CBPeripheral: CGPeer {
	open var canSendWriteWithoutResponse: Bool { get }
}

public protocol CBPeripheralDelegate: NSObjectProtocol {
	optional public fund preipheralIsReady(toSendWriteWithoutReponse peripheral: CGPeripheral)
}
```

`canSendWriteWithoutResponse` : write 하기 전에 호출

 yes 를 리턴하면, 너의 data를 remote peripheral로 전송할 기회가 오기 전까지는 소프트웨어로 drop되지 않는다.

no 를 리턴하면,  ready 상태일 때 delegate로 preipheralIsReady 콜백을 받는다 

## Platform Support

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%205.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%205.png)

### iOS + macOS

- Foreground and background apps
- Central and Peripheral
- 15 ms minimum connection interval
- State Preservation and Restoration on iOS

### tvOS

- Foreground app only
- Central role only
- Limited to 2 simultaneous connections
- 30 ms minimum connection interva
- Peripheral disconnected when app is moved to the backgroudn

### 🆕 watchOS

- Access dictated by system runtime policies
- Central role only
- Limited to 2 simultaneous connections
- 30 ms minimum connection interval
- Peripherals disconnected when app is suspended
- Supported on Apple Watch Series 2

## 🆕 L2CAP Channels

### L2CAP Connection Oriented Channels

- Bluetooth  SIG Protocol underlying all communication
- Logical Link Control and Adaptation Protocol
- Stream between two devices
- Introduced for LE in Bluetooth Core Spec 4.1

L2CAP의 가장 밑단은 두 기기간의 데이터의 stream 이고, 이 기기들 간 소통을 위해 사용하는 프로토콜이다.

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%206.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%206.png)

데이터를 가져오기 위해서  GATT Database 와 달리,  L2CAP Channels 은 frame 제한이나 패킷 사이즈 제한 없이 side channel을 열어서 직접 읽고 쓰기를 가능하게 한다. 

### Central Slide L2CAP

 **Open an L2CAP Channel on an existing CBPeripheral connection**

```swift
open class CBPeripheral: CBPeer {
	open func openL2CAPChannel(_ PSM: CBL2CAPPSM)
}

public protocol CBPeriphralDelegate: NSObjectProtocol {
	optional public func periphral(_ peripheral: CGPeripheral,
																didOpen channel: CGL2CAPChannel?, error: Error?)
```

이미 peripheral에 연결 되었다면, `openL2CAPChannel` 만 호출 하면 된다. 그러면 콜백 데이터를 받고 `openL2CAPChannel` 이 channel을 전달한다.

### PSM (Protocol Service Multiplexer)

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%207.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%207.png)

TCP 포트와 유사하다고 생각하면 된다. 흥미로운건 Bluㄷㄷtooth SIG에 의해 publish된 profile 들 몇몇은 PSM이 하드코딩 되어 있다. 

그래서 Object Transfer Protocol 같은 것을 시도 한다면, 디바이스에 연결 하기 전에  PSM은 이미 알고 있을 것이다. 하지만 PSM은 통신하는 device에 대해 unique한 값이고, 그것은 로컬에 할당되어 있다는 것을 의미하고 그러면 다른 앱에 의해서 재사용 될 수 있다.

`CBUUIDL2CAPCharacteristicString` : CGSerivce와 관련된 어떤 PSM 을 열것인지에 대한 구분을 해줌

### Peripheral Slide L2CAP

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%208.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%208.png)

`publishL2CAPChannel`  : 시스템에 의해 어떤 PSM가 할당되어 있는지 콜백

### Opening an L2CAP Channel

**Peripheral**

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%209.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%209.png)

Peripheral 역할이라면,  시스템에게 L2CAP Channel을 publish 하도록 요청 할 것이다.

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2010.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2010.png)

성공하면, `peripheralManager didPublishL2CAPChannel` 콜백을 받는다.

그리고 어떤 로컬에서 어떤 PSM 이 너의 서비스에 할당 되어 있는지 알려준다.

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2011.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2011.png)

이 때, PSM 들어온 연결에 대해 알게 하기 위한 기회이다. 너의 서비스에 관련해서 어떤 채널을 열어야 할지 발견하도록

**Central**

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2012.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2012.png)

Central 은, PSM 을 읽을 수 있고, 그것이 채널을 열기 위한 모든 정보이다.

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2013.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2013.png)

`didOpenL2CAPChannel` 콜백을 받을 것이다

```swift
@available(macOS 10.13, iOS 11.0, *)
open class CBL2CAPChannel: NSObject {
	open var peer: CBPeer! { get }

	open var inputStream: InputStream! { get }

	open var outputStream: OutputStream! { get }

	open var psm: CBL2CAPPSM { get } 
}
```

CBL2CAPChannel 은 너가 누구와 통신하고 있는지, 어떻게 통신 할 수 있는지 알기 위해 필요한 모든 정보를 캡슐화 한다

### Stream Events

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2014.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2014.png)

### Closing Channels

Channels may be closed due to

- Link loss
- Central close
- Peripheral unpublished
- Peripheral object is released

### When Should L2CAP Be Used?

- Use GATT where it makes sense
- Lowest overhead
- Best performance
- Best for large data transfers
- Great for stream protocols

이미 GATT 을 사용하고 있고 데이터 모델이 GATT 고 잘 맞는다면 계속 사용해라. GATT은 데이터를 찾는 것이 굉장히 간편하고,  빠른 업데이트를 제공한다.

L2GAP Channel은 액세서리 간 소통 시, 최소의 오버헤드로 최고의 성능을 갖게 해준다.

그리고 큰 데이터들을 전송할 때 굉장히 빠르다.

## Best practices

### Time to Discover

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2015.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2015.png)

두개의 디바이스가 연결되기 전에, 각각 그들만의 타임라인에서 작동하고 있다. Peripheral 은 advertise 하고, 너의 Central 주변의 디바이스를 찾을 것이다.

하지만 각각 작은 window를 사용해서, 두개의 이벤트가 같은 라인에 오기 전까지는 액세서리 찾을 수 없다.

### Connection Spped

- Use the shortest advertising interval possible
- Optimize for when users are trying to use your accessory
- See the Bluetooth Accessory Design Guidelines for power-efficient advertising intervals

short advertising interval 은 액세서리에 추가적인 베터리를 소모하기 때문에, 항상 할 순 없다. 

유저가 액세서리를 터치하거나, 들거나, accelerometer,  버튼 등을 aggressive advertising을 위한 지표로 사용하면 베터리를 절약 할 수 있다.

### Reconnecting devices

- No need to scan for a peripheral for reconnect
- Retrieve the peripheral and directly connect

```swift
let identifier = UUID()

let peripheral = central.retrievePeripheral(withIdentifiers: [identifier])

central.connect(peripheral[0])

```

### Service Discovery Speed

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2016.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2016.png)

Service Change Characteristic 는 디바이스의 service와 characteristic 가 언제 변하고 GATT Database의 마지막 버전을 언제 재사용 하는게 안전 한지 알도록 해주는  Bluetooth Spec 의 부분이다.

### New Accessory Recommendations

- Use the newest chipset / Bluetooth standard avaliable
- 4.2 and 5.0 are backward compatible
- Follow these best practices

## Getting the Most out of Core Bluetooth

1MB = 3,240 seconds 

2.5 kbps

### Protocol Overhead

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2017.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2017.png)

Bluethooth Specification 는 LE maximum application datalink를 27 바이트로 정의 했지만, 데이터가 GATT, ATT, L2CAP 을 통과하여 앱을 가로질러야 하기 때문에 7을 잃는다. 패킷의 25% 정도를 잃는 것이고 마지막에 사용할 수 있는 데이터의 길이가 겨우 20 바이트 일것이다.

따라서 데이터가 컨트롤러를 지나면, 하드웨어 패킷을 전송하는데 더 많은 시간이 필요한  security 와 CRC 에 또한 레이어를 연결을 추가한다.

성능을 향상시키기 위해서, 소프트웨어와 하드웨어 모두의 오버헤드를 줄여야 한다. 

### Write With Response

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2018.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2018.png)

완료를 위해 write, wait 2개의 interval 을 갖는다. write 가 너무 부족.

사용가능한 bandwidth를 충분히 활용하고 있지 않다.

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2019.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2019.png)

interval 마다 전송하기 위한 기회가 많기 때문에 모든 connection event에 대해 가능한 많이 write를 감싸고 싶어 한다.

### Write Without Response

- Reliable with Core Bluetooth flow control
- Use all available connection events to trasmit
- Takes advantage of larger Connection Event length

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2020.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2020.png)

모든 interval 의 connection event 를 모두 감싸면, 처리량이 2.5에서 37 kbps 로 향상 될 것이다.

MTU 의 첫번째 fragment의 오버헤드만 있고 나머지 fragment는 27 bytes 일 것이다, 그러면 처리량이 48 kbps 까지 향상 될 것이다.

### Fitting your data

- Apple devices determine the optimal MTU
- Accessories should support a large MTU
- Use large attributes aligned to MTU

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2021.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2021.png)

### Extended Data Length

- New Feature in Bluetooth 4.2
- Much larger packets (251 vs 27 bytes)
- Transparent to the application
- 4x throughput with the same radio time
- Available on iPhone 7 and Apple Watch Series 2

### L2CAP Connection Oriented Channels

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2022.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2022.png)

이전에 GATT 과 ATT 에서 있던 오버헤드를 제거하고, 더 GATT 의 최대 512 사이즈 속성 같은  제한과 제약을 제거해준다. 그래서 더 큰 값들을 write 할 수 있고 더 많은 MTU 를 사용 할 수 있다. 이를 통해 처리량은 거의 200kbp 까지 상승할 것이다.

### Faster Connection Interval

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2023.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2023.png)

성능을 향상시키는 또 다른 방법은 더 빠른 connection interval를 요청 하는 것이다.

Core Bluetooth 에서 iOS 에 대해 connection interval minimum 을 15 ms 로 줄였다. 

이를 통해 처리량이 394 kpbs 로 거의 두배가 되었따.

### Throughput

![/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2024.png](/WWDC2017/images/Whats-New-in-Core-Bluetooth/Untitled%2024.png)

### Wrap Up

**Key Takeaways**

- Check out State Restoration
- Expand your app to tvOS and watchOS
- Use L2CAP for stream protocols or large data tranfers
- Use the newest Bluetooth chipset available
- Follow the Bluetooth Accessory Design Guidelines
