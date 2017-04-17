CocoaMQTT
=========
![PodVersion](https://img.shields.io/cocoapods/v/CocoaMQTT.svg)
![Platforms](https://img.shields.io/cocoapods/p/CocoaMQTT.svg)
![License](https://img.shields.io/cocoapods/l/BadgeSwift.svg?style=flat)
![Swift version](https://img.shields.io/badge/swift-3.1-orange.svg)

MQTT v3.1.1 client library for iOS and OS X written with Swift 3


Build
=====

Build with Xcode 8.0 / Swift 3


Installation
=====
###CocoaPods
Install using [CocoaPods](http://cocoapods.org) by adding this line to your Podfile:

````ruby
use_frameworks! # Add this if you are targeting iOS 8+ or using Swift
pod 'CocoaMQTT'  
````
Then, run the following command:

```bash
$ pod install
```

###Carthage
Install using [Carthage](https://github.com/Carthage/Carthage) by adding the following lines to your Cartfile:

````
github "robbiehanson/CocoaAsyncSocket" "master"
github "CrazyWisdom/MSWeakTimer" "master"
github "emqtt/CocoaMQTT" "master"
````
Then, run the following command:

```bash
$ carthage update --platform iOS
```
Last if you're building for OS X:

- On your application targets “General” settings tab, in the “Embedded Binaries” section, drag and drop CocoaMQTT.framework from the Carthage/Build/Mac folder on disk.

If you're building for iOS, tvOS:

- On your application targets “General” settings tab, in the “Linked Frameworks and Libraries” section, drag and drop each framework you want to use from the Carthage/Build folder on disk.

- On your application targets “Build Phases” settings tab, click the “+” icon and choose “New Run Script Phase”. Create a Run Script with the following contents: 
`/usr/local/bin/carthage copy-frameworks`

- and add the paths to the frameworks you want to use under “Input Files”, e.g.:
```
$(SRCROOT)/Carthage/Build/iOS/CocoaMQTT.framework
```


Usage
=====

Example in Example project:

```swift
let clientID = "CocoaMQTT-" + String(NSProcessInfo().processIdentifier)
let mqtt = CocoaMQTT(clientID: clientID, host: "localhost", port: 1883)
mqtt.username = "test"
mqtt.password = "public"
mqtt.willMessage = CocoaMQTTWill(topic: "/will", message: "dieout")
mqtt.keepAlive = 60
mqtt.delegate = self
mqtt.connect()

```


CocoaMQTT
==========

```swift
/**
 * Blueprint of the MQTT client
 */
protocol CocoaMQTTClient {
    var host: String { get set }
    var port: UInt16 { get set }
    var clientID: String { get }
    var username: String? {get set}
    var password: String? {get set}
    var cleanSession: Bool {get set}
    var keepAlive: UInt16 {get set}
    var willMessage: CocoaMQTTWill? {get set}

    func connect() -> Bool
    func publish(_ topic: String, withString string: String, qos: CocoaMQTTQOS, retained: Bool, dup: Bool) -> UInt16
    func publish(_ message: CocoaMQTTMessage) -> UInt16
    func subscribe(_ topic: String, qos: CocoaMQTTQOS) -> UInt16
    func unsubscribe(_ topic: String) -> UInt16
    func ping()
    func disconnect()
}
```


CocoaMQTTDelegate
=================

```swift
/**
 * MQTT Delegate
 */
@objc public protocol CocoaMQTTDelegate {
    /// MQTT connected with server
    func mqtt(_ mqtt: CocoaMQTT, didConnect host: String, port: Int)
    func mqtt(_ mqtt: CocoaMQTT, didConnectAck ack: CocoaMQTTConnAck)
    func mqtt(_ mqtt: CocoaMQTT, didPublishMessage message: CocoaMQTTMessage, id: UInt16)
    func mqtt(_ mqtt: CocoaMQTT, didPublishAck id: UInt16)
    func mqtt(_ mqtt: CocoaMQTT, didReceiveMessage message: CocoaMQTTMessage, id: UInt16 )
    func mqtt(_ mqtt: CocoaMQTT, didSubscribeTopic topic: String)
    func mqtt(_ mqtt: CocoaMQTT, didUnsubscribeTopic topic: String)
    func mqttDidPing(_ mqtt: CocoaMQTT)
    func mqttDidReceivePong(_ mqtt: CocoaMQTT)
    func mqttDidDisconnect(_ mqtt: CocoaMQTT, withError err: Error?)
    @objc optional func mqtt(_ mqtt: CocoaMQTT, didReceive trust: SecTrust, completionHandler: @escaping (Bool) -> Void)
}
```


AsyncSocket and Timer
=====================

These third-party functions are used:

* [GCDAsyncSocket.h](https://github.com/robbiehanson/CocoaAsyncSocket)
* [MSWeakTimer.h](https://github.com/mindsnacks/MSWeakTimer)


LICENSE
=======

MIT License (see `LICENSE`)

## Contributors

* [@andypiper](https://github.com/andypiper)
* [@turtleDeng](https://github.com/turtleDeng)


Author
======

Feng Lee <feng@emqtt.io>


Twitter
======

https://twitter.com/emqtt


