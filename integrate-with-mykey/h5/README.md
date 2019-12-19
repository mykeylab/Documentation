# Mobile通过H5页面接入

JSBridge 为MYKEY应用中心内嵌的浏览器环境中默认支持的JS注入库，支持web3协议；也支持Scatter协议，开发者可以使用[Scatter文档](https://get-scatter.com/developers/settingupforwebapps)进行H5 DApp的开发。

**特别注意:**[MYKEY的账号体系](../../dive-into-mykey/mykey-on-eos.md#mykey帐户结构)与其他的EOS账号有所差异，需要在服务端验签时使用**Reserved公钥**进行验签，详细请查阅[文档](../../dive-into-mykey/mykey-on-eos.md#2-dui-yu-yu-scatter-jian-rong-de-dapp)。

## web3协议验签

因为mykey账号的独特设计，客户端会使用**Reserved公钥**签名，服务端需要使用**Reserved公钥**验证签名。比如我们使用web3协议进行签名和验签：

```javascript
let sigUtil = require('eth-sig-util') 
let Web3 = require('web3'); 
let web3 = new Web3(new Web3.providers.HttpProvider("
https://mainnet.infura.io/v3/56444e75b6a24070a374f791bd25f811
")); 
let json = require('./AccountStorage.abi.json'); 
let AccountStorageABI = json.abi 
let AccountStorageAddr = '0xADc92d1fD878580579716d944eF3460E241604b7' 
let AccountStorageIns = new web3.eth.Contract(AccountStorageABI, AccountStorageAddr); 

// 1. get mykey account Reserved key 
// https://github.com/mykeylab/Documentation/blob/master/English/MYKEY on EOSIO.md
let account = '0x67913A00a459fCd41CbF4124a887e8d8dE0742c0' 

// account proxy let reservedKeyAddr = await AccountStorageIns.methods.getKeyData(account, 3).call(); console.log(account, "reserved key:", reservedKeyAddr) 
// should be 0xd2F9b4652D80FA870207C2b421B8437d7D54a484
// 2. sign message, web3.personal.sign("hello", web3.eth.coinbase, console.log); let message = 'hello' 
let privKeyHex = '78e2219400da88378b746499ec8ff0d6aa97f806950276f28c65b9d569f32f84' // prvkey of '0xd2F9b4652D80FA870207C2b421B8437d7D54a484' let privKey = Buffer.from(privKeyHex, 'hex') 
let msgParams = { data: message }
let signed = sigUtil.personalSign(privKey, msgParams) console.log("signature:", signed)

// 3. recover msgParams.sig = signed let recovered = sigUtil.recoverPersonalSignature(msgParams) 
console.log("recovered:", recovered) 
console.log(reservedKeyAddr.toLowerCase() === recovered.toLowerCase())
```

