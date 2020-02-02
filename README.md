# Introduction

Welcome to MYKEY developer center.

MYKEY is a self-sovereign identity system, which is the first implementation of KEY ID, to lower the threshold of public blockchains and DAPPs, work for mass adoption and be an important part of Web3.0 trust network.

As a multi-chain wallet, MYKEY, will support multiple smart contract platform. Since the MYKEY accounts saved in **smart contract** on each blockchain, MYKEY temporarily does not support blockchains without smart contract. Now MYKEY support **EOS** and **ETH**.

Here you can learn how to quickly integrate with the smart wallet - MYKEY, and get the most useful development resources. This website including:

* [Integrated with MYKEY](integrate-with-mykey/integration-android/) Demo how to integrate with MYKEY by using MYKEY SDK，SimpleWallet, Scatter protocol, web3 protocol etc.
* [Dive into MYKEY](dive-into-mykey/mykey-on-eos.md) Detailed design, extensions of MYKEY.
  * [Deposit with MYKEY](dive-into-mykey/contracts-deposit/)  how to identify MYKEY transactions and actions.
* [Dev resources](development-resources/eos.md) The most practical EOS、ETH development resources.



Unlike ordinary EOS / ETH accounts, MYKEY accounts have multiple permissions by default. MYKEY uses a permission called **Reserved Key** for signature. When DApp verifies the signature, it needs to find the Reserved Key of the MYKEY account from the smart contract first, then verifies the signature. MYKEY supports the following signing methods: 

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
        <p><a href="integrate-with-mykey/integration-android/sign.md">Android SDK</a>
        </p>
        <p><a href="integrate-with-mykey/integration-ios/sign.md">iOS SDK</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">H5</td>
      <td style="text-align:left">
        <p><a href="integrate-with-mykey/h5/eth.md#verify-signing-with-mykey">ETH Web3</a>
        </p>
        <p><a href="integrate-with-mykey/h5/eos.md#verify-signing-with-mykey">EOS Scatter</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">SimpleWallet Protocol</td>
      <td style="text-align:left"><a href="integrate-with-mykey/simplewallet/#sign">SimpleWallet</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Scan QRCode for Web on PC</td>
      <td style="text-align:left"><a href="integrate-with-mykey/simplewallet/scan.md#sign">Scan QRCode</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Deeplink Protocol</td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>