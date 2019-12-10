# 签名

唤起MYKEY进行Sign签名操作。参数请详见类定义:[SignRequest](../../shen-ru-mykey/mykey_android_sdk.md#lei-signrequest)和[MYKEYWalletCallback](../../shen-ru-mykey/mykey_android_sdk.md#lei-mykeywalletcallback)

dapp服务端或者客户端需要从合约数据中获取该用户在MYKEY的ReserveKey进行验签，获取方式参考[KEYS in MYKEY](../../shen-ru-mykey/eos-shang-de-mykey.md#keydata表中的密钥) 和 [MYKEY Verify Sign](../../shen-ru-mykey/eos-shang-de-mykey.md#如何检查dapps是否在mykey-webview中运行)

```java
SignRequest signRequest = new SignRequest()
    // EOS用 ChainCons.EOS，ETH用 ChainCons.ETH
    .setChain(ChainCons.EOS)
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

