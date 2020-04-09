# Account Module

Account Module is composed with account template contract\(Account.sol\) and account proxy contract\(AccountProxy.sol\). Account template contract is the specific implementation of MYKEY account, while account proxy contract delegates all invocations to account template contract, executing specific operations. This proxy mechanism can save gas costs of creating a huge amount of accounts.

### Account.sol

Description: account template contract with specific implementation of MYKEY account  
 `function init()`: initialization after account creation, initializing account in Account Storage Module and Logic Module  
 `function invoke()`: invoke arbitrary external contracts  
 `function enableStaticCall()`: register methods defined in Logic Module  
 `function changeManager()`: change account's Logic Management Module  
 `function ()`: fallback function, delegating invocation to Logic Module

### AccountProxy.sol

Description: account proxy contract  
 `function ()`: fallback function, delegating invocation to account template contract

### AccountCreator.sol

Description: account creation contract  
 `function createAccount()`: create accounts\(proxy contract\) and initialize accounts  
 `function setAddresses()`: set addresses for account creation

