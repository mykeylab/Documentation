# 合约调用

唤起MYKEY进行合约调用, 支持多Action组合调用, 支持ContractAction和TransferAction两种形式的action类型。 参数请详见类定义:[ContractRequest](../../dive-into-mykey/classes-and-methods/#lei-contractrequest) 和 [MYKEYWalletCallback](../../dive-into-mykey/classes-and-methods/#lei-mykeywalletcallback)

```java
ContractRequest contractRequest = new ContractRequest()
        // chain的值可以为EOS（默认）, ETH，或者ANY。如果是“ANY”，MYKEY任选一条可用的链签名并修改chain为可用链的值(例如：ETH或EOS)，并返回给SDK接入方
        .setChain(ChainCons.EOS)
        .setInfo("Perform the mortgage REX operation")
        // order ID which come from dapp server
        .setOrderId("BH1000001")
        // DApp CallbackUrl
        // param：{"protocol": "", "version": "", "tx_id": "", "ref": "", "account": ""}
        // return: same as SimpleWallet {"code": [0-2], "message": ""}
        .setCallBackUrl(Config.DAPP_CALLBACK_URL);
ContractAction contractActionRequest = new ContractAction();
contractActionRequest.setAccount("eosio")
        .setName("buyram")
        .setInfo("buy ram")
        // JSON string support, eg:setData("{\"player\":\"bobbobbobbob\",\"receiver\":\"alicealice11\",\"quant\":\"1.0000 EOS\"}")
        .setData(new BuyRamDataEntity().setPayer("bobbobbobbob").setReceiver("alicealice11").setQuant("1.0000 EOS"));
contractRequest.addAction(contractActionRequest);


/**
对于ETH的合约调用，请注意:
a、发送数据到MYKEY之前，判断如果是合约action，且链为ETH，且binary为空，则调用Go方法ethJsonToBinary生成binary
b、发送数据到MYKEY之前，确保所有action都设置了chain字段（原来的chain字段应该在最外层Contract对象上）
c、ETH SDK暂时不支持多action
*/
TransferAction transferActionRequest = new TransferAction();
transferActionRequest.setAccount("eosio.token")
        .setName("transfer")
        .setInfo("transfer to alicealice11")
        .setAbi("xxxxx")    // ETH转账用
        .setBinary("xxxxx") // ETH转账用
        .setData(new TransferData().setFrom("bobbobbobbob").setTo("alicealice11").setQuantity("0.0001 EOS").setMemo("memo"));
contractRequest.addAction(transferActionRequest);

MYKEYSdk.getInstance().contract(contractRequest, new MYKEYWalletCallback() {
    @Override
    public void onSuccess (String dataJson) {
        // dataJson：{"txId":""}
        LogUtil.e(TAG, "onSuccess");
        Toast.makeText(activity, "success", Toast.LENGTH_LONG).show();
    }

    @Override
    public void onError (String payloadJson) {
        // payloadJson: {"errorCode":,"errorMsg":""}
        LogUtil.e(TAG, "onError payloadJson:" + payloadJson);
        Toast.makeText(activity, "error, payloadJson: " + payloadJson, Toast.LENGTH_LONG).show();
    }

    @Override
    public void onCancel() {
        LogUtil.e(TAG, "cancel");
        Toast.makeText(activity, "cancelled", Toast.LENGTH_LONG).show();
    }
});
```

