# Summary of verifying MYKEY signature

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
        <p><a href="integration-android/sign.md">Android SDK</a>
        </p>
        <p><a href="integration-ios/sign.md">iOS SDK</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">H5</td>
      <td style="text-align:left">
        <p><a href="h5/eth.md#verify-signing-with-mykey">ETH Web3</a>
        </p>
        <p><a href="h5/eos.md#verify-signing-with-mykey">EOS Scatter</a>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">SimpleWallet Protocol</td>
      <td style="text-align:left"><a href="simplewallet/#sign">SimpleWallet</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Scan QRCode for Web on PC</td>
      <td style="text-align:left"><a href="simplewallet/scan.md#sign">Scan QRCode</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Deeplink Protocol</td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>