# ETH

## Compatible with web3 protocol

MYKEY is compatible with web3 protocol. You can develop dapp with web3 protocol, and access it with MYKEY's default browser. 

You can also refer below link to get more details about eth web3 protocol:

[https://learnblockchain.cn/docs/web3js-0.2x/web3.html](https://learnblockchain.cn/docs/web3js-0.2x/web3.html)

## Verify signing with MYKEY

Due to the unique design of MYKEY, the client will use **Reserved Key** to sign messages, and the server backend should use **Reserved Key** to verify signatures. For example, we use web3 protocol to sign and verify a message:

```javascript
let sigUtil = require('eth-sig-util') 
let Web3 = require('web3'); 
let web3 = new Web3(new Web3.providers.HttpProvider("https://mainnet.infura.io/v3/56444e75b6a24070a374f791bd25f811")); 
let json = require('./AccountStorage.abi.json'); 
let AccountStorageABI = json.abi 
let AccountStorageAddr = '0xADc92d1fD878580579716d944eF3460E241604b7' 
let AccountStorageIns = new web3.eth.Contract(AccountStorageABI, AccountStorageAddr); 

// 1. get mykey account Reserved key 
// https://docs.mykey.org/v/English/dive-into-mykey/mykey-on-eos#mykey-account-structure
let account = '0x67913A00a459fCd41CbF4124a887e8d8dE0742c0' 

// account proxy 
let reservedKeyAddr = await AccountStorageIns.methods.getKeyData(account, 3).call(); 
console.log(account, "reserved key:", reservedKeyAddr) // should be 0xd2F9b4652D80FA870207C2b421B8437d7D54a484
// 2. sign message, 
web3.personal.sign("hello", web3.eth.coinbase, console.log); 
let message = 'hello' 
let privKeyHex = '78e2219400da88378b746499ec8ff0d6aa97f806950276f28c65b9d569f32f84' // prvkey of '0xd2F9b4652D80FA870207C2b421B8437d7D54a484' 
let privKey = Buffer.from(privKeyHex, 'hex') 
let msgParams = { data: message }
let signed = sigUtil.personalSign(privKey, msgParams) 
console.log("signature:", signed)

// 3. recover 
msgParams.sig = signed 
let recovered = sigUtil.recoverPersonalSignature(msgParams) 
console.log("recovered:", recovered) 
console.log(reservedKeyAddr.toLowerCase() === recovered.toLowerCase())
```

