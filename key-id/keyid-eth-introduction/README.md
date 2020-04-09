# KEYID ETH合约介绍

**概述：**

* 每个MYKEY账户对应一个AccountProxy合约，用户的账户地址是合约地址，而不是普通的ETH地址。
* 所有MYKEY账户数据存储在AccountStorage合约中，包括每个账户的密钥，紧急联系人等。
* MYKEY账户通过各个Logic合约来发起交易或操作，包括收发资产，管理账户（如修改密钥/紧急联系人）等。
* MYKEY账户密钥的签名是发起交易或操作的唯一凭证。有了签名，任何普通ETH账户（postman）都可以为MYKEY账户代发交易上链。

![](../../.gitbook/assets/image%20%289%29.png)



