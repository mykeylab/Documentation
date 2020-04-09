# Account Storage Module

Account Storage Module stores data of every MYKEY account, including a set of public keys, emergency contacts, delayed actions and multi-sig proposals. Only invocations sent from Logic Module are allowed to call get/set functions in Account Storage Module.

### AccountStorage.sol

`function initAccount()`: initialization of account storage  
 `function getKeyData()`: get key data  
 `function setKeyData()`: set key data  
 `function getBackupAddress()`: get address of emergency contact  
 `function getBackupEffectiveDate()`: get effective time of emergency contact  
 `function getBackupExpiryDate()`: get expiry time of emergency contact  
 `function setBackup()`: set data of emergency contact  
 `function clearBackupData()`: remove an emergency contact  
 `function getDelayDataHash()`: get hash of delayed item  
 `function getDelayDueTime()`: get due time of delayed item  
 `function setDelayData()`: set delayed item  
 `function clearDelayData()`: remove a delayed item  
 `function getProposalDataHash()`: get hash of proposal data  
 `function getProposalDataApproval()`: get approvals of proposal  
 `function setProposalData()`: set a proposal  
 `function clearProposalData()`: remove a proposal

