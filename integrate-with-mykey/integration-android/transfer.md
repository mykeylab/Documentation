# Transfer

Pull up MYKEY to transfer. See the class definition for the parameters:[TransferRequest](../../dive-into-mykey/classes-and-methods/#class-transferrequest) and [MYKEYWalletCallback](../../dive-into-mykey/classes-and-methods/#class-mykeywalletcallback)

dapp server or client should query the user's ReserveKey from MYKEY SmartContract data to verify the signature, see detail in [KEYS in MYKEY](../../dive-into-mykey/mykey-on-eos.md#mykey-account-structure) and [MYKEY Verify Sign](../../dive-into-mykey/mykey-on-eos.md#integrate-eos-dapps-with-mykey)

```java
TransferRequest transferRequest =new TransferRequest()
        // chain could be EOS, and ETH. EOS is the default value.
        .setChain(ChainCons.EOS)
        .setFrom("bobbobbobbob")
        .setTo("alicealice11")
        .setAmount(0.0001)
        //For ETH transfer, memo should be a hex start with 0x; For ERC20 transfer, this field must be deleted.
        .setMemo("memo")   
        //For ETH transfer, please use "ETH" as contract name; For ERC20 transfer, please use the contract of ERC20 token
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

