# MYKEY Development Toolkit


## 简介

MYKEY Development Toolkit(MDK)是方便开发者基于MYKEY账户体系开发应用的工具包。其包含MYKEY Client SDK，JSBridge Lib，Deeplink以及SimpleWallet协议扩展。该套开发者工具包可以帮助应用唤起MYKEY进行登录、转账、合约调用、签名等多种链上操作。该工具包暂只支持EOS链，并会随着MYKEY对多链的支持进行拓展。


## MYKEY Client SDK

MYKEY Client SDK 由 iOS SDK, Android SDK和MYKEY Core组成。

MYKEY Core通过GO语言实现，封装了MYKEY与MYKEY服务端和区块链交互的逻辑，目前以二进制包形式提供给Android以JNI方式，iOS以静态库方式调用，暂不对外开源。

Android SDK和iOS SDK可以帮助简化APP开发者使用MYKEY账号进行登录、转账、合约、签名操作。同时也封装了使用[eos-stake-token合约](https://github.com/mykeylab/eos-stake-token)发行的通证的额外合约操作，其中也包含了一些示例代码实现。

### Android SDK

[文档](./MYKEY_ANDROID_SDK.md)

### iOS SDK

[文档](./MYKEY_iOS_SDK.md)

## JSBridge Lib

JSBridge 为MYKEY应用中心内嵌的浏览器环境中默认支持的JS注入库，其支持Scatter协议，开发者可以使用[Scatter文档](https://get-scatter.com/docs/api-reference)进行H5 DApp的开发。在支持以太坊后也会支持web3协议。

[JSBridge Documentation](./MYKEY_JSBridge.md)


## SimpleWallet协议

MYKEY遵循SimpleWallet协议实现，详细请见以下文档:

[SimpleWallet Documentation](./MYKEY_SimpleWallet.md)


## DeepLink协议

### 打开MYKEY

mykey://mykey.org/

### dapp跳转MYKEY并直接打开

mykey://mykey.org/dapp?url=%encodeURIComponent(Dapp_URL)%

## ISSUE报告

请在github中的issue板块提交ticket

## FAQ
