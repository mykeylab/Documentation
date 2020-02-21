# EOS充值

MYKEY用户的交易，通过MYKEY服务端的Postman账户（携带用户签名）上链， 用户的充值是内部交易，在区块链浏览器上会显示在交易详情中。

下面会介绍如何从程序端识别合约充值：

### 交易所识别

推荐使用官方的工具history-tool,  将交易中所有的action trace存储到数据库中。从而不需要区分普通交易和合约交易，统一处理。

对比mongo插件，history-tool效率更高，支持PostgreSQL/RocksDB。

编译安装步骤\(fill-pg为例）

1. 代码: [https://github.com/EOSIO/history-tools](https://github.com/EOSIO/history-tools) 注意: 当前版本\(0.2.0\)会写一些数据量很大但是我们又不需要的表\(action\_trace\_authorization和action\_trace\_auth\_sequence\),由于没有配置,所以只能在编译前修改源码,注释掉往这2个表的写入\(可以减少一半以上数据量\) [https://github.com/EOSIO/history-tools/blob/4859e331588676b5c45ef0b393ce9cdf72375b13/src/fill\_pg\_plugin.cpp\#L777-L782](https://github.com/EOSIO/history-tools/blob/4859e331588676b5c45ef0b393ce9cdf72375b13/src/fill_pg_plugin.cpp#L777-L782)
2. 编译: 参考[https://eosio.github.io/history-tools/build-ubuntu-1804.html](https://eosio.github.io/history-tools/build-ubuntu-1804.html). 注意编译比较慢, 尽量8核16G内存
3. 运行环境缺少.so 编译完成后可以把fill-pg复制到需要运行的环境,如果启动报缺少.so, 运行lddfill−pglddfill-pg看少什么, 从编译环境复制到相应目录就可以了
4. 启动: 第一次启动需要参数--fpg-create 创建数据库和表结构
5. pg维护\(数据清理\) action\_trace表的数据量会很大, 按500aps算, 一天4000多W数据. 需要根据block\_num对action\_trace表做partition, 清理历史数据的时候drop掉最老的partition. 

**在action\_trace表中，按照block\_num顺序处理，act\_account中过滤出关注的token，识别出目标地址是交易所充值地址的action。**

由于存在微分叉，当block\_num低于不可逆块时，才可以判定交易成功。\(pg会修复微分叉的交易）

### DAPP识别

history-tool部署维护较重，DAPP方建议使用dfuse的graphql接口来查询/监听交易action。[https://docs.dfuse.io/reference/eosio/graphql/](https://docs.dfuse.io/reference/eosio/graphql/)

如果已有交易id，可以使用/v0/search/transactions来获取交易中所有的action和其他交易详情。

### MYKEY转账举例

比如账号 mykeyhulu511 转 nowwearegoin  0.2826 EOS，交易ID为：[https://eosq.app/tx/ad0e029edf6c8e3ae84b586f36737170fc27c5d995f1564422112050464b48cc](https://eosq.app/tx/ad0e029edf6c8e3ae84b586f36737170fc27c5d995f1564422112050464b48cc)

识别出转账交易，从中提取出data就行：

![](../../.gitbook/assets/image%20%287%29.png)



