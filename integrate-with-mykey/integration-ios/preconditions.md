# Preconditions

For iOS users, please check the following steps:

### 1. Add MYKEYWalletLib-iOS.zip

Download MYKEYWalletLib-iOS.zip from following link, unzip and place it into your project

​[https://github.com/mykeylab/MYKEY-Client-SDK/tree/master/iOS](https://github.com/mykeylab/MYKEY-Client-SDK/tree/master/iOS)​

### 2. Add URL Scheme

Setup URL scheme in Xcode: Goto "Project-&gt;TARGETS-&gt;info-&gt;URL Types", click "add icon".

For example, use 'demoscheme' as follows 

![](../../.gitbook/assets/demoschema.png)

This configuration will generate a deeplink for MYKEY callback, which will be used in MYKEK SDK initialization. See [initWalletSimple](../../dive-into-mykey/classes-and-methods.md#initsimplerequest).

### 3. Add LSApplicationQueriesSchemes

Add one more option on "LSApplicationQueriesSchemes" in info.plist, the value is "mykey"

![](../../.gitbook/assets/info.list.png)

### 4. Set "Enable Bitcode"

Set "Enable Bitcode" to "false" in Build settings

![](../../.gitbook/assets/enable-bitcode.png)

### 5. Note

This library is using swift code, if your application is Objective-C project will need some special configuration on file Bridging-Header.h. If this file does not exist, please create an empty file Empty.swift, the project will generate Bridging-Header.h automatically.

![](../../.gitbook/assets/bridingheader.png)

