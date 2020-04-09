# KEYID ETH Contracts introduction

## Overview

MYKEY is a decentralized smart wallet, which means MYKEY account is a smart contract, rather than an EOA address. This highly improves security and usability.  
 MYKEY has 3 main characteristics:

* **Separated permissions**. Various action permissions\(like transferring asset, account management, etc.\) are owned by separated multiple keys.
* **Recoverable account**. All keys of MYKEY account are replaceable. In extreme cases when all keys are lost, one can still recover his/her account with the assist of emergency contacts.
* **Meta-transaction**. MYKEY users don't need to care about gas price or gas limit. All transactions are delivered by "postman" accounts with meta-transaction mechanism

![](../../.gitbook/assets/image%20%285%29.png)

MYKEY smart contracts can be divided by function into 4 main modules: **Account Module, Account Storage Module, Logic Management Module and Logic Module.**

| Contract | Mainnet Address |
| :--- | :--- |
| [Account](account-module.md#account-sol) | 0xEf004D954999EB9162aeB3989279eFf2161D5095 |
| [AccountCreator](account-module.md#accountcreator-sol) | 0x185479FB2cAEcbA11227db4186046496D6230243 |
| [AccountStorage](account-storage-module.md#accountstorage-sol) | 0xADc92d1fD878580579716d944eF3460E241604b7 |
| [LogicManager](logic-management-module.md#logicmanager-sol) | 0xDF8aC96BC9198c610285b3d1B29de09621B04528 |
| [AccountLogic](logic-module.md#accountlogic-sol) \(to be deprecated\) | 0x52dAb11c6029862eBF1E65A4d5c30641f5FbD957 |
| [new AccountLogic](logic-module.md#accountlogic-sol) | 0xe9737a94eABf50D4E252D7ab68E006639eA73E0D |
| [DualsigsLogic](logic-module.md#dualsigslogic-sol) | 0x039aA54fEbe98AaaDb91aE2b1Db7aA00a82F8571 |
| [TransferLogic](logic-module.md#transferlogic-sol) | 0x1C2349ACBb7f83d07577692c75B6D7654899BF10 |
| [DappLogic](logic-module.md#dapplogic-sol) | 0x847f5AbbA6A36c727eCfF76784eE3648BA868808 |



