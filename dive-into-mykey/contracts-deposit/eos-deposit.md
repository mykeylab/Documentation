# EOS deposit

**For EOS**, the deposit actions are inline actions, which is pushed by Postman with users' signature on MYKEY's backend. Those inline actions can be displayed in the transaction details on the blockchain browser.

Below are the introduction to how to identify the MYKEY actions for exchanges and dapps. 

### Exchange identify MYKEY transaction

The official tool 'history-tool' is recommended, which store all action traces into database. It doesn't distinguish between ordinary transactions and contract transactions.

Comparing to mongo plugin, history-tool has a better efficiency. It support PostgreSQL/RocksDB.

Here is the compiling and installation steps\(fill-pg as an example\):

1.code repository

[https://github.com/EOSIO/history-tools](https://github.com/EOSIO/history-tools)

Caution: The current version\(0.2.0\) has two tables\(action\_trace\_authorization和action\_trace\_auth\_sequence\) with massive data which we won't use. If you comment the source code before compiling, you could reduce the amount of data by more than half.

{% embed url="https://github.com/EOSIO/history-tools/blob/4859e331588676b5c45ef0b393ce9cdf72375b13/src/fill\_pg\_plugin.cpp\#L777-L782" %}

2. compile:

refer the link: [https://eosio.github.io/history-tools/build-ubuntu-1804.html](https://eosio.github.io/history-tools/build-ubuntu-1804.html).  The compiling process is slow.

3. lack of .so file

After compiling, add **fill-pg** to your PATH.  If it lacks of .so, you can run **lddfill-pg** to identify which file is missing.

4. start

Please add parameter '**--fpg-create**' for the first start.

5. pg maintenance

Table action\_trace will be huge. Assume the aps is 500, then more than 40 millions rows will be added per day. It is highly recommended to partition the table by **block\_num**.  Oldest partitions could be dropped when history data is cleared. 

In the action\_trace table, actions are processed in block\_num order. You can filter the targeted token from act\_account, and identify the actions with exchanges address as "to".

Due to the existence of micro fork, the transaction can be determined to be successful when the block\_num is lower than the irreversible block. \(pg will repair micro-forked transactions\)



### DAPP identify MYKEY transaction

Deploying and maintaining history-tool plugin is too heavy for most dapps. It's recommended to use the graphql API to query/listen actions from dfuse. [https://docs.dfuse.io/reference/eosio/graphql/](https://docs.dfuse.io/reference/eosio/graphql/)

If the transaction id is already known, you could use /v0/search/transactions to get all actions and other details for this transaction.

### MYKEY transaction sample

For example, account mykeyhulu511 transfer  0.2826 EOS to account nowwearegoin, the transaction id is: [https://eosq.app/tx/ad0e029edf6c8e3ae84b586f36737170fc27c5d995f1564422112050464b48cc](https://eosq.app/tx/ad0e029edf6c8e3ae84b586f36737170fc27c5d995f1564422112050464b48cc)

Identify the transaction action and extract the data:

![](../../.gitbook/assets/image%20%288%29.png)

