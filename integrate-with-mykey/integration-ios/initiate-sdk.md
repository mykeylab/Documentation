# Initiate SDK

### 1. Add "MYKEYWalletLib.h" file in "AppDelegate.m"

Swift

```swift
import MYKEYWalletLib
```

Object-C

```objectivec
#import <MYKEYWalletLib/MYKEYWalletLib-Swift.h>
```

### 2. Init

To instantiate class MyKeySdk, this lightweight authorization will initiate SDK in major process and use the same logic as SimpleWallet protocol. By using such way, DAPPs don't need to have own accounts system. Binding with MYKEY is not needed either. Parameters are here:[InitSimpleRequest](../../dive-into-mykey/classes-and-methods/#initsimplerequest)

```swift
let initSimpleData = InitSimpleRequest()
initSimpleData.dappName = "DappNameA"
initSimpleData.dappIcon = "https:.../xx.png"
initSimpleData.scheme = "demoscheme"
initSimpleData.disableInstall = true
MYKEYWallet.shared.initWalletSimple(initSimpleData: initSimpleData)
```

For the param scheme, [refer scheme configuration in 1.2](https://github.com/mykeylab/Documentation/blob/master/Chinese/MYKEY_iOS_SDK.md#12-%E5%9C%A8xcode%E8%AE%BE%E7%BD%AEurl-scheme-project-targets-info-url-types-%E6%B7%BB%E5%8A%A0-url-scheme)

### 3. Invoke handlerUrl in "application:openURL:"

```swift
MYKEYWallet.shared.handleUrl(url: url)
```

