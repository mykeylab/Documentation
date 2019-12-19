# 初始化SDK

## 1. 添加文件 "MYKEYWalletLib.h" 到  "AppDelegate.m"

Swift

```swift
import MYKEYWalletLib
```

Object-C

```objectivec
#import <MYKEYWalletLib/MYKEYWalletLib-Swift.h>
```

实例化MYKEYWallet类，在主进程中进行SDK初始化，底层使用simplewallet协议逻辑，使用此初始化方法，dapp可以没有账户体系，不需要与MYKEY进行绑定操作。参数请详见类定义:[InitSimpleRequest](https://github.com/mykeylab/Documentation/blob/master/Chinese/MYKEY_iOS_SDK.md#class-initsimplerequest)

```swift
let initSimpleData = InitSimpleRequest()
initSimpleData.dappName = "DappNameA"
initSimpleData.dappIcon = "https:.../xx.png"
initSimpleData.scheme = "demoscheme"
initSimpleData.disableInstall = true
MYKEYWallet.shared.initWalletSimple(initSimpleData: initSimpleData)
```

Scheme参数，[参考1.2 scheme](preconditions.md#2-tian-jia-url-scheme)

## 2. 在"application:openURL:"调用handlerUrl

```swift
MYKEYWallet.shared.handleUrl(url: url)
```
