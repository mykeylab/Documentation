# Sign in with MYKEY

Sign in with MYKEY, that is, sign in to the APP or websites through MYKEY accounts, and enjoy the benefits of the MYKEY accounts: free use, support for multiple chains.

Currently MYKEY supports two mainstream public chains, ETH and EOS. Some users only have an account on one chain, that is, only one chain account and its public key addresses; some users have accounts on two public chains, when authorizing login, you can choose any public chain to sign and verify the signature.

The data transmitted by MYKEY to third-party applications include: the user's MYKEY ID, blockchain account and corresponding public key addresses.

Sign in with MYKEY, **similar to the traditional OpenID technology of the Internet**, third-party applications can identify users through MYKEY ID, can obtain the user's balance  through the account on the blockchain, and determine the user's operating authority of the account through signature and verification.

## Process to sign in with MYKEY

Different third-party applications use different methods to sign in with MYKEY as a third-party login method.

{% tabs %}
{% tab title="H5 Page" %}
H5 page, compatible with Scatter or Web3 protocol, then you can get the user's MYKEY ID and blockchain account to complete the login.

Take the H5 page of a decentralized exchange as an example:

{% embed url="https://youtu.be/eVoZUKLeSdQ" %}

Steps:

a. For the Scatter protocol, first use the method scatter.connect, then use the login method to obtain the user's account information. For sample code, see: [H5 EOS Login](../integrate-with-mykey/h5/h5-eos.md#login)

b. For the Web3 protocol, use web3.eth's givenProvider method to get the user's account information. For sample code, see: [H5 ETH Login](../integrate-with-mykey/h5/h5-eth.md#login)
{% endtab %}

{% tab title="Web Applications" %}
Web applications comply with SimpleWallet protocol \(EOS\) or WalletConnect protocol \(ETH\). MYKEY will automatically recognize the protocol by scanning the QR code to complete the authorized login.

Take a decentralized exchange using MYKEY scan code to sign in as an example:

{% embed url="https://youtu.be/SjQ2-5CdXpI" %}

Steps: 

1. Web applications generate QR codes based on the JSON data that needs to be transferred. See the data structure to be transferred [Web Application sign in with MYKEY](../integrate-with-mykey/scan.md#login)
2. MYKEY users scan the QR code to authorize login.
3. MYKEY callback web application
{% endtab %}

{% tab title="Android APP" %}
Android application, integrate with [MYKEY Android SDK](https://github.com/mykeylab/MYKEY-Client-SDK/tree/master/Android), call the authorize method of SDK to complete the login.

Take a large-scale blockchain game as an example:

{% embed url="https://youtu.be/7Gjx5\_fx4x8" %}

Steps:

1. Environment preparation, add MYKEY Android SDK
2. Initialize the SDK using the initSimple method
3. Use the authorize method to authorize login
4. The APP receives the user's MYKEY ID and address on the blockchain
5. \(Optional\) The APP queries the address from the public chain according to the user account and performs signature verification, also called signature verification. For different programming languages, we provide [sample code for verification at backend server](verify-signature-on-server-backend.md).
{% endtab %}

{% tab title="iOS APP" %}
iOS application, integrate with [MYKEY iOS SDK](https://github.com/mykeylab/MYKEY-Client-SDK/tree/master/iOS), call the authorize method of SDK to complete the login.

Steps:

1. Environment preparation, add MYKEY iOS SDK
2. Initialize the SDK using the initSimple method
3. Use the authorize method to authorize login
4. The APP receives the user's MYKEY ID and address on the blockchain
5. \(Optional\) The APP queries the address from the public chain according to the user account and performs signature verification, also called signature verification. For different programming languages, we provide [sample code for verification at backend server](verify-signature-on-server-backend.md).
{% endtab %}
{% endtabs %}







