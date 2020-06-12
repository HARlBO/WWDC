# What's New in Core Bluetooth

>  📅 2020.6.11 (THU)
>
> 🗂 WWDC2019 | Session : 901 | Category : System Frameworks
>  
> 🔗 [https://developer.apple.com/videos/play/wwdc2019/901/](https://developer.apple.com/videos/play/wwdc2019/901/)
>  <br />
>  <br />
> 🔖 Learn how to adopt privacy-enhancing changes in Core Bluetooth. Discover new possibilities with LE 2Mbps, advertising extensions, BR/EDR, and dual-mode devices. Understand how to debug your Core Bluetooth communication with the improvements to PacketLogger.
> <br />
<br />  

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled.png)

이번 해에 Core Bluetooth 가 BR/EDR 디바이스를 지원하도록 추가 되었다.

Core Bluetooth 가 어떤 transport 에서 작동 되는지에 상관 없이 모든 Bluetooth 디바이스에 도달하고 상호작용 할 수 있다는 것을 의미 한다.

##  Low Energy 2 Mbps 

### 🆕 LE 2 Mbps

- New feature in Bluetooth 5.0
- Physical layer rate increased from 1 Mbps to 2Mpbs between compatible devices
→  Core Bluetooth 가 같은 airtime 에 두배를 전송 할 수 있게 되었다.
- Faster and more power efficient connection
- Transparent to the application
- Accessories must support LE 2 Mbps
- Available starting with iPhone 8, Apple TV 4K, Apple Watch Series 4

### LE 2 Mbps Throughput

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%201.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%201.png)

##  Advertising Extensions 

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%202.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%202.png)

- Bluetooth 5.0 feature
- 작은 payload를 전송함으로써 3 개의 advertising channel 의 혼잡도를 줄였다.
- 더 큰 payload를 전송하기 위해 더 넓은 data channel로 보낸다.
- 31에서 255byte 로 상승했고, LE에서 전송 비율이 2 Mbps 이다.

### 🆕 Extended Scan

- Scans for extended advertisements
- Accessories must support extended advertisements with LE 2 Mbps
- Support extended advertisement payload up to 124 bytes
- 4 times that advertisement data that an accessory can send today
- Transparent to application
- New API to query for platform support

    ```swift
    class func supports(_ features: CGCentralManager.Feature) -> Bool 
    static var extendedScanAndConnect: CGCentralManager.Feature { get }
     
    ```

- Supports connections to connectable extended advertisements
- Improves existing connection exchange protocol

### 🆕 Legacy Connections

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%203.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%203.png)

1. Advertiser 는 연결 가능한 advertisement 를 통지
2. Scanner 가 연결을 원한다면 connection identification 전송
3. ACK 는 없음 
4. Scanner 는 connection indication 이 advertiser에게 도달했다고 가정
5. host processor 에게 새로운 connection이 있다고 알려줌

하지만 RF characteristic 이 서로 다를 수 있고 RF environment 가 매우 동적이기 때문에connection indication 이 advertiser 에게 도착하지 못할 수 도 있음  

→ 이 과정들이 베터리를 더 많이 소모하게 함

### 🆕 Extended Connections

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%204.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%204.png)

1. Advertiser 는 연결 가능한 extended advertisement 를 통지
2. Scanner 는 connection request 를 전송 가능
3. Advertiser 는 explicit connection response 를 전송
4. Scanner 가 connection response 를 받았을 때만 새로운 connection이 있다고 host processor 를 깨움
5. connection 이 link layer negotiation 들을 생략 하고  LE 2 Mbps 로 시작 할 수 있음

##  Core Bluetooth for BR/EDR 

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%205.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%205.png)

**GATT** 

- hierach-based 인 Bluetooth SIG 프로토콜
- service 와 characteristics 로 구성
- read, write, characteristics 의 변화에 대한 노티가 매우 쉬움

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%206.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%206.png)

System profile 이나 audio, A2DP, HFP remote-control profile 들을 시스템에서 작동 시킨다.  

반대 쪽에선 Low Energy의 경우 Core Bluetooth 를 GATT 위에서 구동하고, 그 프레임워크를 accessories 에서 interface로 사용 한다.

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%207.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%207.png)

두 layer 를 합쳤고, 이제 BR/EDR 과 Low Energy 모두 transparent access 가능하다.

API 의 많은 변화 없이 classic devices 와 low energy device 모두 사용 가능해진 것이다.

- Use Core Bluetooth with BR/EDR Bluetooth devices
- Bluetooth SIG GATT protocol running over BR/EDR
- Same CBPeripheral APIs
- New API in CBCentralManager
- Available today with latest iOS, watchOS, tvOS
- Add support to your accessory

### 🆕 Registering for Connection Events

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%208.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%208.png)

connection events 를 등록 하기 위한 API

### Connection Event

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%209.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%209.png)

### Incoming Connection

**Initialization**

```swift
private var central: CBCentralManager!
central = CBCentralManager(delegate: self, queue: nil)
```

**Registration**

```swift
let matchingOptions = [CGConnectionEventMatchingOption.serviceUUIDs : [myServiceUUID]]

central.registerForConnectionEvents(options: matchingOptions)
```

**Discover**

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2010.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2010.png)

**Connect / Pair**

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2011.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2011.png)

**Delegate Callback**

```swift
// Connection Event
func centralManager(_ central: CBCentralManager,
										connectionEventDidOccur event: CGConnectionEvent,
										for peripheral: CBPeripheral_ {
	
	// Handle connection event

}
```

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2012.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2012.png)

### Outgoing Connection

**Connection Out**

```swift
private var central: CBCentralManager?
central = CBCentralManager(delegate: self, queue: nil)

central?.connect(myPeripheral, option: nil)
```

**BR/EDR Page**

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2013.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2013.png)

**Connected**

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2014.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2014.png)

##  Core Bluetooth for dual-mode 

### Improving Dual-Mode Pairing

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2015.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2015.png)

### 🆕 Cross Transport Key Derivation

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2016.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2016.png)

### Instead of Inquiry

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2017.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2017.png)

유저가 블루투스 발견과 페어링 전체를 제어를 원할 때,  너의 앱 경험을 방해하는 블루투스 설정에 가서 inquiry scan 을 하는 대신에,  액세서리로 부터 low energy scan 을 찾을 수 있다.

### Low Energy Scan

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2018.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2018.png)

### CTKD - Pairing

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2019.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2019.png)

만약 디바이스를 찾았다면, LE 를 통해 연결 하고 protective characteristic 에 접근 할 수 있다.

### Key Derivation

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2020.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2020.png)

일단 페어링이 되면, CTKD 로 인해 LE key 를 갖게 되고, BR/EDR key 또한 가져올 수 있다.

### CTKD - BR/EDR Connected

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2021.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2021.png)

이것은 이제 너가 유저를 혼란스럽게 하는 페어링을 트리거 할 필요 없이,  앱에서 전체 경험을 할 수 있도록 머물면서  BR/EDR 연결을 할 수 있다는 것을 의미한다.

### Improving Dual-Mode Connections

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2022.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2022.png)

홈 오디오 디바이스를 새로 출시 한다고 생각해보자. 그리고 디바이스는 뮤직 이나 팟캐스트 같은 미디어를 앱을 통해 연결 한다면 좋을 것이다.

iOS 는 BR/EDR 이라는 채널을 만들어 줄 것이다.

### 🆕 Bridging

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2023.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2023.png)

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2024.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2024.png)

BR/EDR Connection을 전달 하면, low energy 를 통해 디바이스와 연결하고, 만약 발견하면 즉시 BR/EDR 을 통해 가능한 많은 profile 과 연결 할 것이다.

유저는 끊김 없이 모든 멀티미디어 profile 을 사용 할 수 있게 된다.

##  Privacy Update 

### Enhancements

- User authorization
- Accessory notifications

### User Authorization

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2025.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2025.png)

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2026.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2026.png)

### Adoption

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2027.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2027.png)

1. 유저에게 너의 앱이 왜 블루투스에 접근이 필요한지 전달해야 한다.
2. 그것은 의무적인 String 이고 적용하지 않는다면 앱이 실행 할 때 크래시가 난다.
3. 앱 리뷰 프로세스에서 모든 description 이 비어있지 않고 의미 있는지 확인 한다.

**New property**

```swift
var authorization: CBManagerAuthorization { get }

enum CBManagerAuthorization: Int {
	init?(rawValue: Int)
	var rawValue: Int { get }
	case notDetermined
	case restricted
	case denied
	case allowedAlways
}
```

**Flow**

```swift
func centralManagerDidUpdateState(_ central: CBCentralManager)
func peripheralManagerDidUpdateState(_ peripheral: CBPeripheralManager)

open var state: CBManagerState { get }

// if ( state == CBManagerState.unauthoized)
open var authorization: CBManagerAuthorization { get }
```

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2028.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2028.png)

### Accessory Notifications

- Apple Notification Center Service (ANCS)
- GATT server servcie
- Allows accessories to receive notifications from iOS

### ANCS Privacy Update

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2029.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2029.png)

### New ANCS Privacy API

```swift
public let CBConnectPeripheralOptionRequiresANCS: String

optional func centralManager(_central: CGCentralManager, ddidUpdateANCSAuthorizationFor
														peripheral: CBPeripheral)

open var ancsAuthorized: Bool { get }

```

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2030.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2030.png)

##  Core Bluetooth PacketLogger 

### OverView

- Bluetooth packet analysis application
- Visualizer for packet logs inside sysdiagnose
- Decode all protocols defined by Bluetooth SIG and Apple
- Rich filtering options
- Search by text or regex
- Comment and flag packets
- Export raw data for analysis

### Top Level View

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2031.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2031.png)

ACI, ATT 프로토콜만 필터링 할 수 도 있고, 누르면 각각의 패킷에 대해 전체 프로토콜 계층을 볼 수도 있다.

### Hierarchical View

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2032.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2032.png)

raw bytes 까지 각각의 프로토콜을 볼 수 있다 

### 🆕 Live Capture

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2033.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2033.png)

맥에 연결 된 iOS 디바이스에 login profile 을 설치하고, PacketLogger 을 실행 하면, iOS 디바이스에서 액세서리로  live Bluetooth traffic 을 capture 할 수 있다. 또한 여러개의 iOS 디바이스를 연결 할 수도 있다.

![/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2034.png](/WWDC2019/images/Whats-New-in-Core-Bluetooth/Untitled%2034.png)
