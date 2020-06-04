# More detailed tech and process of  MYKEY login

MYKEY login allows the use of MYKEY account login to access third-party applications and send blockchain transactions, with the following characteristics:‌

* Protected unmanaged blockchain account
* Free to use blockchain
* Multi-chain support and multi-language services

## technology principle of login with MYKEY  <a id="technology-principle-of-login-with-mykey"></a>

MYKEY login relies on ECDSA-secp256k1's signature and verification methods. A third-party application initiates a login request. The user provides a specified signature through MYKEY. The third-party application verifies the validity of the signature and completes the authentication.‌

## authentication process of login with MYKEY <a id="authentication-process-of-login-with-mykey"></a>

![](https://gblobscdn.gitbook.com/assets%2F-L_kSF8M0oUJ6majSGmG%2F-M0_QwRCxAZE-nnOj6gi%2F-M0_VbaiWbiCGLwV0tR0%2Fimage.png?alt=media&token=1c2e5b52-1062-4984-8bf5-b899b199ec0b)‌

### 1. Initiate an authentication request‌ <a id="1-initiate-an-authentication-request"></a>

A third-party application initiates an authentication request, MYKEY adopts different processing strategies according to the channel.‌

（1）H5 Pages‌

MYKEY is compatible with the scatter protocol and the web3 protocol. You can use the methods of the scatter and web3 JS libraries to obtain account information and complete login.‌

（2）PC WEB‌

The third-party application encodes the request parameters into a QR Code and displays it according to the SimpleWallet or WalletConnect protocol. The code is scanned and authorized using MYKEY, then the third-party application is called back to complete the login.‌

（3）APP‌

After accessing the SDK, and the SDK is initialized, you can directly call the authority method to wake up MYKEY.‌

### 2. Sign / return receipt <a id="2-sign-return-receipt"></a>

MYKEY receives and parses parameters, and returns user account information based on the passed parameters.‌

（1）parameter: chain‌

The value of parameter chain could be ANY, EOS or ETH.‌

If the parameter chain is ANY, or the chain parameter is not passed, MYKEY selects the chain for authorizing in the default order \(EOS first, then ETH\).‌

If the parameter chain is specified, MYKEY will query the account of the specified chain and return account information.‌

If the user does not have an account on the targeted chain, the account returned by MYKEY is empty, and the third-party application may prompt the user to open an account on the chain.‌

### 3. Parse receipt, request public key <a id="3-parse-receipt-request-public-key"></a>

The third-party application parses the return receipt of MYKEY to obtain the user's mykeyId and the user account.‌

\(1\) Check if the mykeyId is registered‌

If the mykeyId has not been registered, then register mykeyId and the user address of the corresponding chain in the local database, and select a chain to query the user account.‌

If the mykeyId already registered, then directly select the account associated with the original mykeyId as the query address‌

\(2\) Query reserved key and its status‌

Third-party applications can choose to directly query the user's ETH public key through blockchain services, such as infura, and dfuse to query the user's EOS public key. The index 3 of public key is the reserved key. For the query method, please refer to Obtaining the ReservedKey in the sample [Verify signature at backend server](https://app.gitbook.com/@mykey/s/mykey-docs/~/drafts/-M4xqUNAZZV5VvERvh0L/v/English/third-party-login-based-on-mykey/verify-signature-on-server-backend).‌

Third-party applications that do not have high security requirements can also directly trust the public key returned by MYKEY.‌

### 4. Returns the reserved key and its status‌ <a id="4-returns-the-reserved-key-and-its-status"></a>

Blockchain service provider or MYKEY returns the reserved key information of the corresponding address, and its status.‌

### 5. Verify signature <a id="5-verify-signature"></a>

If the state of the reserved key is not available \(status != 0\), it will directly return failure.‌

If the reserved key is available, check \(1\) whether the signature corresponds to the user's reserved key; \(2\) whether the content of the signature is the same as the content of the request. If the verification succeeds, it returns success, otherwise it returns the authorization failure. For the sample code of the third-party application server to verify the signature, refer to the sample [Verify signature at backend server](https://app.gitbook.com/@mykey/s/mykey-docs/~/drafts/-M4xqUNAZZV5VvERvh0L/v/English/third-party-login-based-on-mykey/verify-signature-on-server-backend).



For more technical details, please refer to [More detailed tech and process of MYKEY login](more-detailed-tech-and-process-of-mykey-login.md).

