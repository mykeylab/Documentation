# Third-Party Login based on MYKEY

MYKEY provides SDKs and APIs to help third-party applications implement convenient user authentication and reduce the barriers to entry for users.

MYKEY login allows the use of MYKEY account login to access third-party applications and send blockchain transactions, with the following characteristics:

* Protected unmanaged blockchain account 
* Free to use blockchain
* Multi-chain support and multi-language services

## technology principle of login with MYKEY 

MYKEY login relies on ECDSA-secp256k1's signature and verification methods. A third-party application initiates a login request. The user provides a specified signature through MYKEY. The third-party application verifies the validity of the signature and completes the authentication.

## authentication process of login with MYKEY

![](.gitbook/assets/image%20%285%29.png)

### 1. Initiate an authentication request

When a third-party application initiates an authentication request, it needs to pass five parameters: action, digest of action details, timestamp, application name / id, callback URL, designated blockchain \(optional\), and adopt different processing strategies according to the channel.

For example: request={login, hash{login}, 1581410193, bihu.com, bihu\_callback\_url, ETH}

（1）H5 Pages

Communicate with MYKEY through JS Bridge.

（2）PC WEB

Encode request parameters into QR Code and display.

（3）APP

If you integrate with MYKEY SDK, you can directly call the relevant method to wake up MYKEY; otherwise, pass parameters through the API method.  


### 2. Sign / return receipt

MYKEY receives and parses parameters to show users the names and actions of third-party applications.

（1）Analyzing the accounts on chains 

If a third-party application specifies a blockchain and the user has not created an account on the chain, it will prompt to open an account. If not specified or activated, it will proceed to the next step.

（2）Verify permissions and signatures

The user enters the password or verifies the fingerprint / face, and calls the Reserved Key to sign the request.

（3）Return receipt

In addition to the signature, the receipt also needs to contain the user UUID and user address information. If the third-party application does not specify a blockchain, all on-chain addresses are returned, which is convenient for the third-party application to identify the user.

For example:

ETH：receipt={sig, uuid:ETH:0xFFFF }

No specific chains：receipt={sig, uuid:{EOS:null; ETH:0xFFFF...; Tron:0xEEEE}  }

### 3. Parse receipt, request public key

The third-party application parses the request to obtain the user uuid and user address.

（1）Check if uuid is registered

If the uuid has not been registered, register the uuid and the user address of the corresponding chain in the local database, and select a chain as the query address.

If already registered, directly select the user address associated with the original uuid as the query address.

（2）Querying the Public Key

Third-party applications can choose to directly query the user's public key through blockchain services, such as infura, or they can query the public key of the corresponding address through the API provided by MYKEY.

### 4. Returns the public key

The blockchain service provider or MYKEY returns the public key information of the corresponding address.

![\(warning\)](https://confluence.inner-bihu.com/s/en_US/7901/04c8b7bf0a5b4889210956b8230224e43d124b25/_/images/icons/emoticons/warning.svg)Note: Public keys may be frozen.

### 5. Verify signature

If the status of the public key is not available, it will directly return failure.

If the public key is available, check \(1\) whether the signature corresponds to the user's public key; \(2\) whether the content of the signature is the same as the content of the request. If the verification passes, it returns success, otherwise it returns the signing failure.

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
        <p>&#x200B;<a href="https://app.gitbook.com/@mykey/s/mykey-docs/~/drafts/-M0YWbq9MRyudOOMs8Aw/v/English/integrate-with-mykey/h5/eth#verify-signing-with-mykey">ETH Web3</a>&#x200B;</p>
        <p>&#x200B;<a href="https://app.gitbook.com/@mykey/s/mykey-docs/~/drafts/-M0YWbq9MRyudOOMs8Aw/v/English/integrate-with-mykey/h5/eos#verify-signing-with-mykey">EOS Scatter</a>&#x200B;</p>
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

