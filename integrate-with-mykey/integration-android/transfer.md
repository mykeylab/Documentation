# 转账

唤起MYKEY进行转账。参数请详见类定义:[TransferRequest](../../dive-into-mykey/classes-and-methods/#lei-transferrequest)和[MYKEYWalletCallback](../../dive-into-mykey/classes-and-methods/#lei-mykeywalletcallback)

```java
TransferRequest transferRequest =new TransferRequest()
        // chain的值可以为EOS（默认）, ETH，或者ANY。如果是“ANY”，MYKEY任选一条可用的链签名并修改chain为可用链的值(例如：ETH或EOS)，并返回给SDK接入方
        .setChain(ChainCons.EOS)
        .setFrom("bobbobbobbob")
        .setTo("alicealice11")
        .setAmount(0.0001)
        //ETH转账，memo必须为0x开头的16进制；ERC20转账，请删除该字段
        .setMemo("memo")   
        //ETH转账，合约名用"ETH"；ERC20转账，合约名用ERC20的合约名
        .setContractName("eosio.token")  
        .setSymbol("EOS")
        .setInfo("transfer to alicealice11")
        .setDecimal(4)
        // order ID which come from dapp server
        .setOrderId("BH1000001")
        // DApp CallbackUrl
        // param：{"protocol": "", "version": "", "tx_id": "", "ref": "", "account": "" }
        // return: same as SimpleWallet {"code": [0-2], "message": ""}
        .setCallBackUrl(Config.DAPP_CALLBACK_URL);

    MYKEYSdk.getInstance().transfer(transferRequest, new MYKEYWalletCallback() {
    @Override
    public void onSuccess(String dataJson) {
        // dataJson：{"txId":""}
        LogUtil.e(TAG, "onSuccess data: " + dataJson);
        Toast.makeText(activity, "success", Toast.LENGTH_LONG).show();
    }

    @Override
    public void onError(String payloadJson) {
        // payloadJson: {"errorCode":,"errorMsg":""}
        LogUtil.e(TAG, "onError payloadJson:" + payloadJson);
        Toast.makeText(activity, "error, payloadJson:" + payloadJson, Toast.LENGTH_LONG).show();
    }

    @Override
    public void onCancel() {
        LogUtil.e(TAG, "cancel");
        Toast.makeText(activity, "cancelled", Toast.LENGTH_LONG).show();
    }
});
```

