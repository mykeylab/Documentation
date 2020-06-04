# 更多MYKEY登录的技术原理和流程

‌MYKEY登录允许使用MYKEY帐号登录访问第三方应用，发送区块链交易，具有如下特点：‌

* 受保障 的非托管区块链账户
* 免费的 区块链使用
* 多链支持及多语言服务

## MYKEY登录技术原理 <a id="mykey-deng-lu-ji-shu-yuan-li"></a>

MYKEY登录依赖ECDSA-secp256k1的签名和验签方法。由第三方应用发起登录请求，用户通过MYKEY提供指定的签名，第三方应用验证签名的合法性，完成认证。‌

## MYKEY登录认证流程 <a id="mykey-deng-lu-ren-zheng-liu-cheng"></a>

![](https://gblobscdn.gitbook.com/assets%2F-L_kSF8M0oUJ6majSGmG%2F-M0YSRVyQSjS4VvlSrQG%2F-M0Yb9LW01qV75YcPtBj%2Fimage.png?alt=media&token=6b63a9fe-ba0e-4c4c-90ac-7558972729df)‌

### 1. 发起认证请求 <a id="1-fa-qi-ren-zheng-qing-qiu"></a>

第三方应用发起认证请求，MYKEY会根据渠道采取不同的处理策略。‌

（1）H5页面‌

MYKEY兼容scatter协议和web3协议，使用scatter和web3 JS库的方法获取到账户信息即可完成登录。‌

（2）PC WEB‌

第三方应用按照SimpleWallet或者WalletConnect协议将请求参数编码成QR Code并展示，使用MYKEY扫码并授权，通过后MYKEY回调第三方应用以完成登录。‌

（3）APP‌

接入SDK，SDK初始化后，可直接调用authority方法唤醒MYKEY。‌

### 2. 签名/返回回执 <a id="2-qian-ming-fan-hui-hui-zhi"></a>

MYKEY接收并解析参数，并根据传递的参数，返回用户的账号信息。‌

（1）chain参数‌

chain参数的值，可为ANY, EOS, 或者ETH。‌

如果chain参数为ANY，或者未传递chain参数，MYKEY按照默认顺序选择验签的链（先EOS后ETH）。‌

如果指定了chain参数，MYKEY会查询指定链的账号，并返回账号信息。‌

如果用户未在指定的链上无账号，MYKEY返回账号为空，第三方应用可提示用户开通该链。‌

### 3. 解析回执，请求公钥‌ <a id="3-jie-xi-hui-zhi-qing-qiu-gong-yao"></a>

第三方应用解析MYKEY的返回回执，获得用户的mykeyUID和用户地址公钥。‌

（1）检查该mykeyUID是否注册过。‌

如果该mykeyUID未注册过，则在本地数据库登记mykeyUID和对应链的用户地址，并选择一条链作为查询地址。‌

如果已注册过，则直接选择原mykeyUID所关联的用户地址作为查询地址。‌

（2）查询公钥地址及状态‌

第三方应用可以选择直接通过区块链服务，如infura查询用户的ETH公钥，通过dfuse查询用户的EOS公钥。查询方法可参考 [验签代码示例](https://app.gitbook.com/@mykey/s/mykey-docs/~/drafts/-M4xqUNAZZV5VvERvh0L/third-party-login/verify-example) 中的 获取ReservedKey。‌

对安全性要求不是很高的第三方应用，也可以直接信任上一步MYKEY返回的公钥。‌

### 4. 返回公钥及其状态 <a id="4-fan-hui-gong-yao-ji-qi-zhuang-tai"></a>

区块链服务商或MYKEY返回对应地址的公钥信息。‌

### 5. 验证签名 <a id="5-yan-zheng-qian-ming"></a>

如果公钥状态不是可用状态（status=1），则直接返回失败。‌

如果公钥是可用状态，检查（1）签名与用户公钥是否对应；（2）签名内容是否与请求内容相同。如验证通过，则返回成功，否则返回验签失败。‌

第三方应用服务端验证签名的示例代码，可参考 [验签代码示例](https://app.gitbook.com/@mykey/s/mykey-docs/~/drafts/-M4xqUNAZZV5VvERvh0L/third-party-login/verify-example)。‌



| ​ |
| :--- |




