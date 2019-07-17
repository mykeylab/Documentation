# MYKEY Development Toolkit


## Introduction

MYKEY Development Toolkit, aka MDK, a toolkit for developers to develop applications based on the MYKEY account system. It contains MYKEY Client SDK, JSBridge Lib and SimpleWallet
protocol extension. This suite of development toolkit can help applications wake up MYKEY to login, transfer, contracts invoke, sign and other on-chain operations. This tooklit only support EOS chain by now, and will extend to other public chains with the roadmap of MYKEY for multi-chain.


## MYKEY Client SDK

The MYKEY Client SDK consists of the iOS SDK, Android SDK and MYKEY Core.

MYKEY Core is implemented by GO language, and encapsulates the interaction logic of MYKEY Client App to MYKEY server and blockchain. It is provided to Android in the form of binary package via JNI mode. iOS implemented via static library, and it wasn't open sourced by now.

The Android SDK and iOS SDK can help APP developers to simplify implement to use MYKEY accounts to log in, transfer, contract, and sign up.
The SDKs also include extra contract operations for the token issued by [eos-stake-token contract] (https://github.com/mykeylab/eos-stake-token), which also includes some sample code.


### Android SDK

[docuemnt](./MYKEY-ANDROID-SDK-EN.md)

### iOS SDK

[document](./MYKEY-iOS-SDK-EN.md)


## JSBridge Lib

JSBridge is injected javascript code in MYKEY dapp browser enviroment by default, it support Scatter protocol, developers can develop H5 dapp follow [Scatter Document](https://get-scatter.com/docs/api-reference), it will also support web3js protocol when MYKEY support ETH.

[JSBridge Documentation](./MYKEY-JSBridge-EN.md)


## SimpleWallet protocol

MYKEY follows the SimpleWallet protocol implementation. See the following document for details:

[SimpleWallet Documentation](./MYKEY-SimpleWallet-EN.md)


## DeepLink Protocol

### Open MYKEY Application

mykey://mykey.org/

### Open MYKEY and redirect to specific dapp

mykey://mykey.org/dapp?url=%encodeURIComponent(Dapp_URL)%


## Issue Report

Please submit issue in github

## FAQ
