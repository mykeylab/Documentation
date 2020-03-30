# Call contracts

Pull up MYKEY for contract calls, support multiple action combination calls, support ContractAction and TransferAction two types of action types. Please refer to the class definition for the parameters [ContractRequest](../../dive-into-mykey/classes-and-methods/#class-contractrequest) and [MYKEYWalletCallback](../../dive-into-mykey/classes-and-methods/#class-mykeywalletcallback)

```java
ContractRequest contractRequest = new ContractRequest()
        // chain could be ANY, EOS, and ETH. EOS is the default value. If it's ANY, MYKEY will try EOS first, then ETH to return an account
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
For the call contracts of ETH, please pay attention to below items:
a. Before sending data to MYKEY, determine if it is a contract action, the chain is ETH, and the binary is empty, then call the Go method ethJsonToBinary to generate a binary
b. Before sending data to MYKEY, make sure that all actions have the chain field set (the original chain field should be on the outermost Contract object)
c. ETH SDK does not support multi-action
*/
TransferAction transferActionRequest = new TransferAction();
transferActionRequest.setAccount("eosio.token")
        .setName("transfer")
        .setInfo("transfer to alicealice11")
        .setAbi("xxxxx")    // only use for ETH
        .setBinary("xxxxx") // only use for ETH
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

