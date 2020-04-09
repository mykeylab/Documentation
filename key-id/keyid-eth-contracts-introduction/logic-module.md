# Logic Module

Logic Module contains logic contracts, which implements specific operations like replacing keys, freezing account, transferring asset and calling external contract etc. In every logic contract, there is an entry method\(`function enter()`\) which validates signature. So the entry method must be called first before executing any operation.  
 Currently, there are 4 logic contracts in Logic Module: _**AccountLogic, DualsigsLogic, TransferLogic and DappLogic.**_

### AccountLogic.sol

Description: implement logic of account management  
 `function changeAdminKey()`: change admin key \(with delay\)  
 `function triggerChangeAdminKey()`: trigger changing admin key  
 `function changeAdminKeyByBackup()`: change admin key \(proposed by emergency contact, with delay\)  
 `function triggerChangeAdminkeyByBackup()`: trigger changing admin key proposed by emergency contact  
 `function addOperationKey()`: add an operation key  
 `function changeAllOperationKeys()`: change all operation keys \(with delay\)  
 `function triggerChangeAllOperationKeys()`: trigger changing all operation keys  
 `function freeze()`: freeze account  
 `function unfreeze()`: unfreeze account \(with delay\)  
 `function triggerUnfreeze()`: trigger unfreezing account  
 `function removeBackup()`: remove an emergency contact \(with delay\)  
 `function cancelDelay()`: cancel delayed operation  
 `function cancelAddBackup()`: cancel adding emergency contact  
 `function cancelRemoveBackup()`: cancel removing emergency contact  
 `function proposeAsBackup()`: propose a proposal as an emergency contact  
 `function approveProposal()`: approve a proposal  
 `function executeProposal()`: execute a proposal

### DualsigsLogic.sol

Description: implement logic of dual signatures  
 `function changeAdminKeyWithoutDelay()`: change admin key immediately  
 `function changeAllOperationKeysWithoutDelay()`: change all operation keys immediately  
 `function unfreezeWithoutDelay()`: unfreeze account immediately  
 `function addBackup()`: add an emergency contact  
 `function proposeByBoth()`: user proposes a proposal together with an emergency contact  
 `function executeProposal()`: execute a proposal

### TransferLogic.sol

Description: implement logic of transferring asset  
 `function transferEth()`: transfer ETH  
 `function transferErc20()`: transfer ERC20 token  
 `function transferApprovedErc20()`: transfer approved ERC20 token  
 `function transferNft()`: transfer Non-fungible token  
 `function transferApprovedNft()`: transfer approved Non-fungible token

### DappLogic.sol

Description: implement logic of interacting with external contracts  
 `function callContract()`: call external contract  
 `function callMultiContract()`: call multiple external contracts atomically

