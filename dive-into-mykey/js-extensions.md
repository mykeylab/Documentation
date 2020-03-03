# JS Extensions

JSBridge is injected javascript code in MYKEY dapp browser enviroment by default, which support Scatter protocol, and web3js protocol.

In order to control the MYKEY application conveniently, MYKEY adds the following methods:

package name: **window.MyKey.Browser**

<table>
  <thead>
    <tr>
      <th style="text-align:left">Methods</th>
      <th style="text-align:left">Method description</th>
      <th style="text-align:left">Params</th>
      <th style="text-align:left">Response</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">closeWindow</td>
      <td style="text-align:left">Close App window and return to MYKEY</td>
      <td style="text-align:left">None</td>
      <td style="text-align:left">None</td>
    </tr>
    <tr>
      <td style="text-align:left">openFullScreen</td>
      <td style="text-align:left">Open full screen</td>
      <td style="text-align:left">isLandscape:true, fasle</td>
      <td style="text-align:left">None</td>
    </tr>
    <tr>
      <td style="text-align:left">closeFullScreen</td>
      <td style="text-align:left">Close full screen</td>
      <td style="text-align:left">None</td>
      <td style="text-align:left">None</td>
    </tr>
    <tr>
      <td style="text-align:left">forbidPhysicalBack</td>
      <td style="text-align:left">Disable physical back button(Android Only)</td>
      <td style="text-align:left">None</td>
      <td style="text-align:left">None</td>
    </tr>
    <tr>
      <td style="text-align:left">allowPhysicalBack</td>
      <td style="text-align:left">Allow physical back button(Android Only)</td>
      <td style="text-align:left">None</td>
      <td style="text-align:left">None</td>
    </tr>
    <tr>
      <td style="text-align:left">getAccountInfo</td>
      <td style="text-align:left">Get MYKEY Account Information</td>
      <td style="text-align:left">None</td>
      <td style="text-align:left">{&quot;id&quot;:&quot;MYKEY UUID&quot;,&quot;accountName&quot;:&quot;Nickname
        in MYKEY&quot;,&quot;chainInfoList&quot;:[{&quot;chain&quot;:&quot;EOS&quot;,&quot;account&quot;:&quot;&quot;}],&quot;operationKeys&quot;:[&quot;Three
        operation keys&quot;,&quot;&quot;,&quot;&quot;]}</td>
    </tr>
    <tr>
      <td style="text-align:left">getClientConfig</td>
      <td style="text-align:left">Get client configuration</td>
      <td style="text-align:left">None</td>
      <td style="text-align:left">{&quot;currency&quot;:&quot;CNY|USD&quot;,&quot;locale&quot;:&quot;zh-CN|en-US|ko-KR|ja-JP&quot;,&quot;userAgent&quot;:&quot;&quot;,&quot;recaptchaUserKey&quot;:&quot;&quot;}</td>
    </tr>
    <tr>
      <td style="text-align:left">sendTransaction</td>
      <td style="text-align:left">Send transaction</td>
      <td style="text-align:left">{&quot;actions&quot;:[{&quot;account&quot;:&quot;eosio&quot;,&quot;name&quot;:&quot;buyram&quot;,&quot;data&quot;:{&quot;payer&quot;:&quot;&quot;,&quot;receiver&quot;:&quot;&quot;,&quot;quant&quot;:&quot;1.0000
        EOS&quot;}}],&quot;chain&quot;:&quot;EOS&quot;}</td>
      <td style="text-align:left">
        <p>return promise object</p>
        <p>result:{&quot;errorCode&quot;:0,&quot;errorMsg&quot;:&quot;&quot;,&quot;data&quot;:{&quot;transactionId&quot;:&quot;&quot;,&quot;signature&quot;:&quot;&quot;}}</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">sign</td>
      <td style="text-align:left">Apply MYKEY sign transactions</td>
      <td style="text-align:left">message:(unsignatured data in string)</td>
      <td style="text-align:left">
        <p>return promise object</p>
        <p>result:{&quot;errorCode&quot;:0,&quot;errorMsg&quot;:&quot;&quot;,&quot;data&quot;:{&quot;signature&quot;:&quot;&quot;}}</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">setTitle</td>
      <td style="text-align:left">Set application title</td>
      <td style="text-align:left">title:(title in string)</td>
      <td style="text-align:left">None</td>
    </tr>
    <tr>
      <td style="text-align:left">showLoading</td>
      <td style="text-align:left">Show loading animation</td>
      <td style="text-align:left">None</td>
      <td style="text-align:left">None</td>
    </tr>
    <tr>
      <td style="text-align:left">hiddenLoading</td>
      <td style="text-align:left">hidden loading animation</td>
      <td style="text-align:left">None</td>
      <td style="text-align:left">None</td>
    </tr>
    <tr>
      <td style="text-align:left">copyToClipboard</td>
      <td style="text-align:left">Copy content to system clipboard</td>
      <td style="text-align:left">message:(content to be copied in string)</td>
      <td style="text-align:left">
        <p>return promise object</p>
        <p>result:{&quot;errorCode&quot;:0,&quot;errorMsg&quot;:&quot;&quot;}</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">showOpenChain</td>
      <td style="text-align:left">Show the page to open accounts with chains</td>
      <td style="text-align:left">{&quot;chain&quot;:&quot;EOS|ETH&quot;}</td>
      <td style="text-align:left">None</td>
    </tr>
    <tr>
      <td style="text-align:left">showLessNetworkFee</td>
      <td style="text-align:left">Pop-up box showing insufficient network fee balance</td>
      <td style="text-align:left">None</td>
      <td style="text-align:left">None</td>
    </tr>
    <tr>
      <td style="text-align:left">checkAppVersion</td>
      <td style="text-align:left">Check App version</td>
      <td style="text-align:left">None</td>
      <td style="text-align:left">None</td>
    </tr>
    <tr>
      <td style="text-align:left">printLog</td>
      <td style="text-align:left">Print log(client can push logs to servers to help debug issues)</td>
      <td
      style="text-align:left">log:(log in string)</td>
        <td style="text-align:left">None</td>
    </tr>
    <tr>
      <td style="text-align:left">encodeFunctionCall</td>
      <td style="text-align:left">serialize specified function call</td>
      <td style="text-align:left">
        <p>abi: abi for the function, type string, eg:&quot;[{\&quot;constant\&quot;:false,\&quot;inputs\&quot;:[{\&quot;name\&quot;:\&quot;_to\&quot;,\&quot;type\&quot;:\&quot;address\&quot;},{\&quot;name\&quot;:\&quot;_value\&quot;,\&quot;type\&quot;:\&quot;uint256\&quot;}],\&quot;name\&quot;:\&quot;transfer\&quot;,\&quot;outputs\&quot;:[{\&quot;name\&quot;:\&quot;\&quot;,\&quot;type\&quot;:\&quot;bool\&quot;}],\&quot;payable\&quot;:false,\&quot;stateMutability\&quot;:\&quot;nonpayable\&quot;,\&quot;type\&quot;:\&quot;function\&quot;}]&quot;</p>
        <p>method: method name, type string, eg:transfer&#x3002;</p>
        <p>param&#xFF1A;params of method, eg:{_to:&quot;0xc4ED1B3f31acadbE3c14B20fA766B6C4B1FAB208&quot;,_value:&quot;20000000000000000000&quot;}</p>
      </td>
      <td style="text-align:left">
        <p>return promise object</p>
        <p>result:{&quot;errorCode&quot;:0,&quot;errorMsg&quot;:&quot;&quot;,&quot;data&quot;:&quot;[serialized
          string]&quot;}</p>
      </td>
    </tr>
  </tbody>
</table>