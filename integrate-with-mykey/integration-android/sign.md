# Sign

Pull up MYKEY for Signature operation. See the class definition for the parameters: [SignRequest](../../dive-into-mykey/classes-and-methods/#class-signrequest) and [MYKEYWalletCallback](../../dive-into-mykey/classes-and-methods/#class-mykeywalletcallback)

dapp server or client should query the user's ReserveKey from MYKEY SmartContract data to verify the signature, see detail in [KEYS in MYKEY](../../dive-into-mykey/mykey-on-eos.md#mykey-account-structure) and [MYKEY Verify Sign](../../dive-into-mykey/mykey-on-eos.md#integrate-eos-dapps-with-mykey)

```java
SignRequest signRequest = new SignRequest()
    // chain could be ANY, EOS, and ETH. EOS is the default value. If it's ANY, MYKEY will try EOS first, then ETH to return an account
    .setChain(ChainCons.EOS)
    // message should be a hex string start with 0x
    .setMessage("Messages that need to be signed, [it could be random which come from dapp server]")
    // DApp CallbackUrl
    // param：{"protocol": "", "version": "", "message": "", "sign": "", "ref": "", "account": ""}
    // return: same as SimpleWallet {"code": [0-2], "message": ""}
    .setCallBackUrl(Config.DAPP_CALLBACK_URL);


MYKEYSdk.getInstance().sign(signRequest, new MYKEYWalletCallback() {
    @Override
    public void onSuccess(String dataJson) {
        // dataJson：{"sign":""}
        LogUtil.e(TAG, "onSuccess data：" + dataJson);
        Toast.makeText(activity, "success", Toast.LENGTH_LONG).show();
    }

    @Override
    public void onError(String payloadJson) {
        // payloadJson: {"errorCode":,"errorMsg":""}
        LogUtil.e(TAG, "onError payloadJson: " + payloadJson);
        Toast.makeText(activity, "error, payloadJson: " + payloadJson, Toast.LENGTH_LONG).show();
    }

    @Override
    public void onCancel() {
        LogUtil.e(TAG, "cancel");
        Toast.makeText(activity, "cancelled", Toast.LENGTH_LONG).show();
    }
});
```

