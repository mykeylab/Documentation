# Initiate SDK

To instantiate class MyKeySdk, this lightweight authorization will initiate SDK in major process and use the same logic as SimpleWallet protocol. By using such way, DAPPs don't need to have own accounts system. Binding with MYKEY is not needed either. Parameters are here:[InitSimpleRequest](../../dive-into-mykey/classes-and-methods.md#initsimplerequest)

```java
MYKEYSdk.getInstance().initSimple(new InitSimpleRequest().setDappName(Config.SAMPLE_DAPP_NAME)
        .setDappIcon(Config.SAMPLE_DAPP_ICON)
        .setCallback(Config.SAMPLE_DAPP_CALLBACK)
        .setDisableInstall(false)
        .setContractPromptFree(true)
        .setContext(this));
```

