# Logic Management Module

Logic contracts authorized by Logic Management Module can be added or removed with delay, and the pending time can also be altered with delay.  
 Any update of logic contracts should follow [strict procedures](https://docs.mykey.org/v/English/key-id/keyid-contract-upgrade-process).  
 Below is a diagram showing the procedure of logic update:

[![MYKEY logic update diagram](https://github.com/mykeylab/keyid-eth-contracts/raw/master/images/MYKEY%20logic%20update%20diagram.png)](https://github.com/mykeylab/keyid-eth-contracts/blob/master/images/MYKEY%20logic%20update%20diagram.png)

### LogicManager.sol

Description: management of all logic contracts  
 `function submitUpdatePendingTime()`: alter pending time \(with delay\)  
 `function triggerUpdatePendingTime()`: trigger updating pending time  
 `function submitUpdate()`: update logic contract \(with delay\)  
 `function cancelUpdate()`: cancel an update of logic contract  
 `function triggerUpdate()`: trigger updating logic contract  
 `function isAuthorized()`: check if a contract is an authorized logic contract  
 `function getAuthorizedLogics()`: get all authorized logic contracts

