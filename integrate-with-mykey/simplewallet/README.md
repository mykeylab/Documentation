# SimpleWallet Protocol Compatible

### Sample code for redirect to MYKEY

**Reminder:** In Android client, for avoid conflict with other wallets, developer can use following code to rediect to MYKEY precisely through set the MYKEY packge name: com.mykye.id

```java
try {
    Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse(url));
    intent.setPackage("com.mykey.id");
    intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
    context.startActivity(intent);
} catch (Exception e) {
    e.printStackTrace();
}
```

### Login and Transfer

MYKEY follows the SimpleWallet protocol implementation. See the following document for details:

[https://github.com/southex/SimpleWallet/blob/master/README\_en.md](https://github.com/southex/SimpleWallet/blob/master/README_en.md)

Beside support **login** and **transfer** of SimpleWallet specification，MYKEY also additionally supports **contract** and **signature** method.

**Special Notice:** [MYKEY account structure](../../dive-into-mykey/mykey-on-eos.md#mykey-account-structure) is different with other EOS account, if dapp verify signature in their server side, should use the public key of Reserved, more details see this [Document](../../dive-into-mykey/mykey-on-eos.md#2-for-dapps-compatible-with-scatter)

### Call Contract

#### Sequence diagram for Mobile applcaiton wake up

![](../../.gitbook/assets/image%20%285%29.png)

Please pass the data to MYKEY as follows, the data format is json:

```java
// Contract call data format

{
    protocol    string   // protocol name，use "SimpleWallet" by default
    version     string   // protocol version, e.g. "1.0"
    action      string   // action type, use "transaction"
    dappName    string   // DApp name
    dappIcon    string   // DApp icon url
    desc        string   // Semantic description of MYKEY display to the user contract call
    callback    string   // Deeplink MYKEY callback to DApp, e.g. custom://custom.com/contract
    notifyUrl   string   // The callback URL endpoint of DAppServer for receive success notification from MYKEY
    ContractRequest [  // Arrary of contact actions, include transfer and non-transfer actions
      { // non-transfer
          account   string // contract code name
          name      string // contract action name
          info      string // Semantic description of MYKEY display to the user about this action
          data      object // The parameter object passed according to the contract abi definition e.g. {key1: value1, key2: value2 }
      },
      { // transfer
          account   string //  contract code name
          name      string // contract action name
          info      string // Semantic description of MYKEY display to the user about this action
          TransferDataRequest {
            from     string // From
            to       string // To
            quantity string // Amount and Symbol，e.g. "1.0000 EOS"
            memo     string // Memo
          }
      }
    ]
    expired        number   // Only in web QR code mode, expire date,unix timestamp
}
```

### Sign

#### Sequence diagram for Mobile applcaiton wake up

![](../../.gitbook/assets/image%20%287%29.png)

Please pass the data to MYKEY as follows, the data format is json:

```java
// Sign call data format
{
    protocol    string   // protocol name，use "SimpleWallet" by default
    version     string   // protocol version, e.g. "1.0"
    action      string   // action type, use "sign"
    dappName    string   // DApp name
    dappIcon    string   // DApp icon url
    desc        string   // Semantic description of MYKEY display to the user contract call
    message     string   // Unsigned messages
    callback    string   // Deeplink MYKEY callback to DApp, e.g. custom://custom.com/contract
    notifyUrl   string   // The callback URL endpoint of DAppServer for receive success notification from MYKEY
    expired     number   // Only in web QR code mode, expire date,unix timestamp
}
```



