# 创建账户模块

MYKEY调用AccountCreator合约方法为每个用户创建账户，即AccountProxy合约，只存储一部分数据，所有逻辑都delegate call到Account合约。目的是为了减少账户合约部署的成本。

创建账户时，传入参数包括一组公钥，一个初始紧急联系人\(MYKEY Lab\)和一组初始的Logic模块\(AccountLogic, DualsigsLogic, TransferLogic, DappLogic\)。账户地址，公钥和紧急联系人数据写入AccountStorage合约。

