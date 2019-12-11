# 初始化SDK

实例化MyKeySdk类，MYKEY轻量级认证方式调用，在主进程中进行SDK初始化，底层使用simplewallet协议逻辑，使用此初始化方法，dapp可以没有账户体系，不需要与MYKEY进行绑定操作。参数请详见类定义: [InitSimpleRequest](../../dive-into-mykey/classes-and-methods.md#lei-initsimplerequest)

```java
MYKEYSdk.getInstance().initSimple(new InitSimpleRequest().setDappName(Config.SAMPLE_DAPP_NAME)
        .setDappIcon(Config.SAMPLE_DAPP_ICON)
        .setCallback(Config.SAMPLE_DAPP_CALLBACK)
        .setDisableInstall(false)
        .setContractPromptFree(true)
        .setContext(this));
```

