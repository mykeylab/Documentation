# Call contracts

Pull up MYKEY for contract calls, support multiple action combination calls, support ContractAction and TransferAction two types of action types. Please refer to the class definition for the parameters [ContractRequest](../../dive-into-mykey/classes-and-methods.md#class-contractrequest) and [MYKEYWalletCallback](../../dive-into-mykey/classes-and-methods.md#class-mykeywalletcallback)

```java
ContractRequest contractRequest = new ContractRequest()
        // EOS use ChainCons.EOS，ETH use ChainCons.ETH
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

TransferAction transferActionRequest = new TransferAction();
transferActionRequest.setAccount("eosio.token")
        .setName("transfer")
        .setInfo("transfer to alicealice11")
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

