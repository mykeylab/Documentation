# Mobile Dapp with H5 pages

JSBridge is injected javascript code in MYKEY dapp browser enviroment by default, which support Scatter protocol, and web3js protocol.

Here we will show how to authorize with MYKEY.  For more methods of JSBridge, please check the [JS Extensions](js-extensions.md).

You can directly develop H5 pages compatible with the web3 protocol and Scatter protocol,  then access them through the built-in browser of MYKEY.

For more information about the eth web3 protocol, please refer to: [https://web3js.readthedocs.io/en/v1.2.0/web3-eth.html](https://web3js.readthedocs.io/en/v1.2.0/web3-eth.html)

For more information on the Scatter protocol, please refer to: [https://get-scatter.com/developers/settingupforwebapps](https://get-scatter.com/developers/settingupforwebapps)

To verify the MYKEY signature, please refer to [verify signature at backend server.](../../sign-in-with-mykey/verify-signature-on-server-backend.md) JSBridge support more functions, please refer to [JSBridge extensions](js-extensions.md).

## How to identify the dapps that MYKEY opens

There are two methods to check if dapps are running in MYKEY client.

1. Use navigator.userAgent to check. MYKEY webview will append MYKEY/\[version\] after the original userAgent.

```javascript
function isMYKEY(){
    return navigator.userAgent.indexOf("MYKEY") > -1;
}
```

2. For EOS, use scatter.getIdentity to check if the response data contain type:'MYKEY', the return data snippet

```javascript
{
    accounts: [{authority: 'active', blockchain: 'eos', name: [ACCOUNT_NAME], type: 'MYKEY'}],
    publicKey: [OperationKey 3/Reserved key]
};
```





\*\*\*\*



## 

