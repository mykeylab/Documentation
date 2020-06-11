# 深入MYKEY账户

MYKEY是一个基于KEYID协议的智能钱包，已部署在多个区块链上，为用户提供一站式的区块链服务，包括资产存储、理财、交易和DApp交互等。MYKEY的主要特征是具有分离权限，可恢复帐户和代付交易手续费等。MYKEY帐户通过一个或多个授权逻辑模块执行所有操作，这些逻辑模块通过签名验证用户身份。 此外，逻辑模块可以延迟升级。

## MYKEY账户结构 <a id="mykey&#x6709;&#x5565;&#x4E0D;&#x540C;&#xFF1F;"></a>

MYKEY帐户结构中有两种密钥：admin key（管理密钥）和operation key（操作密钥）。在KEYID智能合约中，“Admin Key”不能签署任何常规交易，只能签署带时间锁的交易来替换“Operation Key”。

![](../.gitbook/assets/image%20%2816%29.png)

Admin Key对应的私钥由移动设备生成，可以在账户注册期间通过恢复码或硬件钱包进行冷备份。Operation Key对应的私钥由移动设备生成并加密存储。

Admin Key和Operation Key的公钥用于注册帐户，存储在KEY ID智能合约中，以进行进一步的链上签名验证。

凭借这些创新的设计，MYKEY可以帮助DAPP项目方构建自己的原生DAPP，在DAPP和第三方钱包之间都不需要显示的进行签名。 此外，用户可以通过不同的Operation Key安全地管理资产、vault帐户、DAPP帐户、数据等，这些操作密钥在一个帐户中，我们称其为“自主身份”。 vault账户可以像普通账户一样接收资金，但也可以通过添加可选的安全步骤来防止立即撤回存储的资金。

## 链上密钥数据 <a id="keydata&#x8868;&#x4E2D;&#x7684;&#x5BC6;&#x94A5;"></a>

每个MYKEY帐户都有**一个**管理密钥和**一组**操作密钥。 管理员密钥（索引0）是管理所有密钥的最高权限。 操作键（索引大于0）管理特定功能。

| 序号 | 名称 | 功能 | 备注 |
| :--- | :--- | :--- | :--- |
| 0 | admin\(管理\) | 最高权限，管理所有密钥，但无具体操作权限 | 账户创建时默认生成 |
| 1 | asset\(资产\) | 用于资产转账 | 账户创建时默认生成 |
| 2 | adding\(添加\) | 用于添加其它操作密钥，添加或删除子账户，无需延时 | 账户创建时默认生成 |
| 3 | reserved\(保留\) | 其他所有未明确的权限 | 账户创建时默认生成 |
| 4 | assist\(辅助\) | 作为紧急联系人时，用于提出提案，同意提案，确认作为紧急联系人 | 后续可添加 |
| 5 | modify\(修改白名单\) | 用于修改子账户的密钥和白名单 | 后续可添加 |

{% tabs %}
{% tab title="ETH" %}
通过地址 [https://etherscan.io/address/0xadc92d1fd878580579716d944ef3460e241604b7\#readContract](https://etherscan.io/address/0xadc92d1fd878580579716d944ef3460e241604b7#readContract)

可以查询到MYKEY账号的密钥，比如账号0x67913A00a459fCd41CbF4124a887e8d8dE0742c0 索引为3的Reserved Key：

![](../.gitbook/assets/image%20%2817%29.png)
{% endtab %}

{% tab title="EOS" %}
MYKEY通过KEY ID智能合约重新设计了EOS帐户，该EOS主网上的合约包含两部分：

* [mykeymanager](https://bloks.io/account/mykeymanager)
* [mykeylogica1](https://bloks.io/account/mykeylogica1)

在EOS中，MYKEY支持诸如密钥恢复，层级权限，免费帐户，免费使用，身份认证，子帐户等功能。为便于理解，MYKEY尝试为最终用户隐藏上述技术概念。在MYKEY APP中，用户不必关心其EOS帐户的CPU / NET / RAM。

MYKEY帐户的每对密钥的公钥都将存储在合约[mykeymanager](https://bloks.io/account/mykeymanager)的表keydata中，我们可以使用ACCOUNT\_NAME作为范围来查询MYKEY帐户中的所有密钥。

例如，帐户mykeyhulu521: [https://bloks.io/account/mykeymanager?loadContract=true&tab=Tables&table=keydata&account=mykeymanager&scope=mykeyhulu521&limit=100](https://bloks.io/account/mykeymanager?loadContract=true&tab=Tables&table=keydata&account=mykeymanager&scope=mykeyhulu521&limit=100)

![](../.gitbook/assets/image%20%2818%29.png)
{% endtab %}
{% endtabs %}

## 账号冻结

在紧急情况下，用户可以立即冻结其帐户（通过冻结所有操作密钥），在这种情况下，所有操作键密钥（资产，添加，保留，辅助和修改）都将被禁用，直到用户取消冻结其帐户或更改所有操作权限。

## 紧急联系人

MYKEY帐户可以添加其他MYKEY帐户作为其紧急联系人，以保护帐户安全。 每个帐户最多可以有6个紧急联系人。 将某人添加为紧急联系人需要征得他的同意（紧急联系人的签名），并且将在21天后生效。

用户可以选择在创建帐户时添加初始紧急联系人，这样就可以立即生效。 在MYKEY APP中，最初的紧急联系人是MYKEY Lab。 

用户还可以删除紧急联系人，同样，这将在21天内生效。

## 账户管理提案

用户的紧急联系人，或者用户和紧急联系人共同提出多签提案，然后根据需要等待其他紧急联系人批准。

执行提案所需的签名数量为四舍五入的紧急联系人总数的60％。

达到多重签名阈值后，任何人都可以执行提案。

## 帐户恢复

MYKEY帐户中的所有密钥均可以替换。 因此，可以在私钥丢失或泄露等紧急情况下恢复用户的帐户。 不同的情形可以发起不同的提案，来实现账户的管理和恢复，具体的情形可以参考 [账户恢复机制](../key-id/account-recover.md#duo-qian-ti-an)。



