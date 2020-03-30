# Third-Party Login based on MYKEY

MYKEY provides SDKs and APIs to help third-party applications implement convenient user authentication and reduce the barriers to entry for users.

MYKEY login allows the use of MYKEY account login to access third-party applications and send blockchain transactions, with the following characteristics:

* Protected unmanaged blockchain account 
* Free to use blockchain
* Multi-chain support and multi-language services

## technology principle of login with MYKEY 

MYKEY login relies on ECDSA-secp256k1's signature and verification methods. A third-party application initiates a login request. The user provides a specified signature through MYKEY. The third-party application verifies the validity of the signature and completes the authentication.

## authentication process of login with MYKEY

![](../.gitbook/assets/image%20%285%29.png)

### 1. Initiate an authentication request

A third-party application initiates an authentication request, MYKEY adopts different processing strategies according to the channel.

（1）H5 Pages

MYKEY is compatible with the scatter protocol and the web3 protocol. You can use the methods of the scatter and web3 JS libraries to obtain account information and complete login.

（2）PC WEB

The third-party application encodes the request parameters into a QR Code and displays it according to the SimpleWallet protocol. The code is scanned and authorized using MYKEY, then the third-party application is called back to complete the login.

（3）APP

After accessing the SDK, and the SDK is initialized, you can directly call the authority method to wake up MYKEY; otherwise, pass parameters through the api method.

### 2. Sign / return receipt

MYKEY receives and parses parameters, and returns user account information based on the passed parameters.

（1）parameter: chain

The value of parameter chain could be ANY, EOS or ETH.

If the parameter chain is ANY, or the chain parameter is not passed, MYKEY selects the chain for authorizing in the default order \(EOS first, then ETH\).

If the parameter chain is specified, MYKEY will query the account of the specified chain and return account information.

If the user does not have an account on the targeted chain, the account returned by MYKEY is empty, and the third-party application may prompt the user to open an account on the chain.

### 3. Parse receipt, request public key

The third-party application parses the return receipt of MYKEY to obtain the user's mykey\_id and the user account.

\(1\) Check if the mykey\_id is registered

If the mykey\_id has not been registered, then register mykey\_id and the user address of the corresponding chain in the local database, and select a chain to query the user account.

If the mykey\_id already registered, then directly select the account associated with the original mykey\_id as the query address

\(2\) Query reserved key and its status

Third-party applications can choose to directly query the user's ETH public key through blockchain services, such as infura, and dfuse to query the user's EOS public key. The index 3 of public key is the reserved key. For the query method, please refer to Obtaining the ReservedKey in the sample [Verify signature at backend server](verify-signature-on-server-backend.md).

Third-party applications that do not have high security requirements can also directly trust the public key returned by MYKEY.

### 4. Returns the reserved key and its status

Blockchain service provider or MYKEY returns the reserved key information of the corresponding address, and its status.

### 5. Verify signature

If the state of the reserved key is not available \(status != 0\), it will directly return failure. 

If the reserved key is available, check \(1\) whether the signature corresponds to the user's reserved key; \(2\) whether the content of the signature is the same as the content of the request. If the verification succeeds, it returns success, otherwise it returns the authorization failure. For the sample code of the third-party application server to verify the signature, refer to the sample [Verify signature at backend server](verify-signature-on-server-backend.md).

## Authentication methods for integration methods

MYKEY supports the following signing methods:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Integration methods</th>
      <th style="text-align:left">Verify signatures methods</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">MYKEY SDK</td>
      <td style="text-align:left">
        <p>&#x200B;<a href="https://app.gitbook.com/@mykey/s/mykey-docs/~/drafts/-M0YWbq9MRyudOOMs8Aw/v/English/integrate-with-mykey/integration-android/sign">Android SDK</a>&#x200B;</p>
        <p>&#x200B;<a href="https://app.gitbook.com/@mykey/s/mykey-docs/~/drafts/-M0YWbq9MRyudOOMs8Aw/v/English/integrate-with-mykey/integration-ios/sign">iOS SDK</a>&#x200B;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">H5</td>
      <td style="text-align:left">
        <p><a href="../integrate-with-mykey/h5/eth.md#verify-signing-with-mykey">&#x200B;ETH Web3&#x200B;</a>
        </p>
        <p>&#x200B;<a href="../integrate-with-mykey/h5/eos.md#verify-signing-with-mykey">EOS Scatter&#x200B;</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">SimpleWallet Protocol</td>
      <td style="text-align:left">&#x200B;<a href="https://app.gitbook.com/@mykey/s/mykey-docs/~/drafts/-M0YWbq9MRyudOOMs8Aw/v/English/integrate-with-mykey/simplewallet#sign">SimpleWallet</a>&#x200B;</td>
    </tr>
    <tr>
      <td style="text-align:left">Scan QRCode for Web on PC</td>
      <td style="text-align:left">&#x200B;<a href="https://app.gitbook.com/@mykey/s/mykey-docs/~/drafts/-M0YWbq9MRyudOOMs8Aw/v/English/integrate-with-mykey/simplewallet/scan#sign">Scan QRCode</a>&#x200B;</td>
    </tr>
    <tr>
      <td style="text-align:left">Deeplink Protocol</td>
      <td style="text-align:left">&#x200B;</td>
    </tr>
  </tbody>
</table>

