# 账号恢复机制

### MYKEY账号的密钥序号和功能定义

| 序号（链上数据表中） | 名称 | 功能 | 备注 |
| :--- | :--- | :--- | :--- |
| 0 | admin\(管理\) | 最高权限，管理所有密钥，但无具体操作权限 | 账户创建时即生成 |
| 1 | asset\(资产\) | 用于资产转账 |  |
| 2 | adding\(添加\) | 用于添加其他operation key，添加或删除子账户，无需延时 |  |
| 3 | reserved\(保留\) | 其他所有未明确的权限 |  |
| 4 | assist\(紧急联系人\) | 作为紧急联系人时，用于提出提案，同意提案，确认作为紧急联系人 | 后续添加 |
| 5 | modify\(修改白名单\) | 用于修改子账户的密钥和白名单 |  |

### **延时交易**

修改admin key，operation key，移除backup，unfreeze等操作都需要发送一个deferred tx。先将deferred id，到期时间，tx数据等存入数据表，然后发送延时交易，到期时会自动执行。如果到期时没有被执行，则可以调用kickdeferred方法来手动触发。

### **多签提案**

多签提案分两种：一，client\(当事人\)想要在backup的帮助下加速某一延时操作\(如修改admin key/operation key等\)，此种提案需要由client和某一backup共同签名发起，再由其他backup响应，且此种提案不可取消（但可覆盖），对应下表**③-快速提案**；二，client在丢失recovery code的情况下请求backup帮助重置admin key，此种提案由某一backup发起，再由其他backup响应，此种提案可以被client的admin key取消\(防止backup联合作恶的情况\)，对应下表**②-紧急提案**。

| 修改管理密钥 | 方式 | 签名密钥 | 提案期\(proposal period\) | 等待期\(deferred tx period\) |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| ①-普通**\(非提案\)** | client独自发起 | client's Admin Key | 无提案期 | 21天 | 可取消 | 不可覆盖 |  |  |
| ②-紧急提案 | backup独自发起 | backup's Assist Key | 提案人是backup | 可取消 | 可覆盖 | 30天 | 可取消 | 不可覆盖 |
| ③-快速提案 | client与backup共同发起 | client's Admin Key + backup's Assist Key | 提案人是client | 不可取消 | 可覆盖 | 立即，无等待期 |  |  |

| 修改或解冻操作密钥 | 方式 | 签名密钥 | 提案期\(proposal period\) | 等待期\(deferred tx period\) |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| ①-普通**\(非提案\)** | client独自发起 | client's Admin Key | 无提案期 | 7天 | 可取消 | 不可覆盖 |
| ③-快速提案 | client与backup共同发起 | client's Admin Key + backup's Assist Key | 提案人是client | 不可取消 | 可覆盖 | 立即，无等待期 |

