# Account recovery mechanism

### Public keys and their function of MYKEY account

| Index\(data on chain\) | name | function | remark |
| :--- | :--- | :--- | :--- |
| 0 | admin | The highest authority, manage all keys, but no specific operation authority | created by default |
| 1 | asset | Userd to transfer assets | created by default |
| 2 | adding | Used to add other operation keys, add or delete sub-accounts without delay  | created by default |
| 3 | reserved | Used by other undefined actions | created by default |
| 4 | assist | As an emergency contact, it is used to make a proposal, agree to the proposal, and confirm as an emergency contact | added when it's needed |
| 5 | modify | Used to modify the public keys of sub accounts and whitelist |  |

### Defer transaction

To change admin key, operation key, and remove backup, unfreeze etc. actions needs to send a deferred tx. First store the deferred id, expiration time, tx data, etc. into the data table, then send the deferred transaction, which will be automatically executed when it expires. If it is not executed when it expires, you can call the kickdeferred method to trigger it manually.

### **Multisig proposals**

There are two types of multisig proposals: First, the client\(account owner\), with the help of a backup, wants to accelerate a certain delay operation \(such as modifying the admin key / operation key, etc.\), then responded by other backup, and this proposal can not be cancelled \(but can be overwritten\), corresponding to the following table **③-quick proposal**; Second, the client requests backup to help reset the admin key in case of missing recovery code. Once a backup is initiated and then responded by other backups, this kind of proposal can be cancelled by the client's admin key \(to prevent the situation of backup joint cooperation\), corresponding to the following table **②-urgent proposal**.

In the scenarios below, 'client' is the user, and 'backup' is one of the user's emergency contacts.

| Change Admin Key | Initiated by | Signing Key | Proposal Period | Deferred TX Period |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Change Admin Key | Initiated by | Signing Key | Proposal Period | Deferred TX Period |  |  |  |  |
| ①-Normal**\(not proposal\)** | only client | client's Admin Key | N/A | 21 days | cancellable | not replaceable |  |  |
| ②-Urgent Proposal | only backup | backup's Assist Key | backup is the proposer | cancellable | replaceable | 30 days | cancellable | not replaceable |
| ③-Quick Proposal | both client and backup | client's Admin Key + backup's Assist Key | client is the proposer | not cancellable | replaceable | immediate |  |  |

| Change or Unfreeze Operation Keys | Initiated by | Signing Key | Proposal Period | Deferred TX Period |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| ①-Normal**\(not proposal\)** | only client | client's Admin Key | N/A | 7 days | cancellable | not replaceable |
| ③-Quick Proposal | both client and backup | client's Admin Key + backup's Assist Key | client is the proposer | not cancellable | replaceable | immediate |

