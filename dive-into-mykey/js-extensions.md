# JS功能扩展

JSBridge为MYKEY应用中心内嵌的浏览器环境中默认支持的JS注入库，其支持Scatter协议，也支持web3协议。

为了方便控制MYKEY应用中心浏览器，MYKEY还增加了以下方法：

包名：**window.MyKey.Browser**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x65B9;&#x6CD5;&#x540D;</th>
      <th style="text-align:left">&#x529F;&#x80FD;&#x8BF4;&#x660E;</th>
      <th style="text-align:left">&#x53C2;&#x6570;&#x683C;&#x5F0F;</th>
      <th style="text-align:left">&#x54CD;&#x5E94;&#x683C;&#x5F0F;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">closeWindow</td>
      <td style="text-align:left">&#x5173;&#x95ED;&#x5E94;&#x7528;&#x7A97;&#x53E3;&#x56DE;&#x5230;MYKEY</td>
      <td
      style="text-align:left">&#x65E0;</td>
        <td style="text-align:left">&#x65E0;</td>
    </tr>
    <tr>
      <td style="text-align:left">openFullScreen</td>
      <td style="text-align:left">&#x6253;&#x5F00;&#x5168;&#x5C4F;</td>
      <td style="text-align:left">isLandscape:true(&#x6A2A;&#x5C4F;)&#xFF0C;fasle(&#x7AD6;&#x5C4F;)</td>
      <td
      style="text-align:left">&#x65E0;</td>
    </tr>
    <tr>
      <td style="text-align:left">closeFullScreen</td>
      <td style="text-align:left">&#x5173;&#x95ED;&#x5168;&#x5C4F;</td>
      <td style="text-align:left">&#x65E0;</td>
      <td style="text-align:left">&#x65E0;</td>
    </tr>
    <tr>
      <td style="text-align:left">forbidPhysicalBack</td>
      <td style="text-align:left">&#x7981;&#x6B62;&#x7269;&#x7406;&#x8FD4;&#x56DE;&#x6309;&#x94AE;&#xFF08;Android&#x4E13;&#x5C5E;&#xFF09;</td>
      <td
      style="text-align:left">&#x65E0;</td>
        <td style="text-align:left">&#x65E0;</td>
    </tr>
    <tr>
      <td style="text-align:left">allowPhysicalBack</td>
      <td style="text-align:left">&#x5141;&#x8BB8;&#x7269;&#x7406;&#x8FD4;&#x56DE;&#x6309;&#x94AE;&#xFF08;Android&#x4E13;&#x5C5E;&#xFF09;</td>
      <td
      style="text-align:left">&#x65E0;</td>
        <td style="text-align:left">&#x65E0;</td>
    </tr>
    <tr>
      <td style="text-align:left">getAccountInfo</td>
      <td style="text-align:left">&#x83B7;&#x53D6;MYKEY&#x8D26;&#x6237;&#x4FE1;&#x606F;</td>
      <td style="text-align:left">&#x65E0;</td>
      <td style="text-align:left">{&quot;id&quot;:&quot;MYKEY&#x552F;&#x4E00;ID&quot;,&quot;accountName&quot;:&quot;MYKEY&#x5185;&#x8BBE;&#x7F6E;&#x7684;&#x6635;&#x79F0;&quot;,&quot;chainInfoList&quot;:[{&quot;chain&quot;:&quot;EOS&quot;,&quot;account&quot;:&quot;&quot;}],&quot;operationKeys&quot;:[&quot;&#x4E09;&#x628A;&#x64CD;&#x4F5C;&#x79D8;&#x94A5;&#x516C;&#x94A5;&quot;,&quot;&quot;,&quot;&quot;]}</td>
    </tr>
    <tr>
      <td style="text-align:left">getClientConfig</td>
      <td style="text-align:left">&#x83B7;&#x53D6;&#x5BA2;&#x6237;&#x7AEF;&#x90E8;&#x5206;&#x914D;&#x7F6E;</td>
      <td
      style="text-align:left">&#x65E0;</td>
        <td style="text-align:left">{&quot;currency&quot;:&quot;CNY|USD&quot;,&quot;locale&quot;:&quot;zh-CN|en-US|ko-KR|ja-JP&quot;,&quot;userAgent&quot;:&quot;&quot;,&quot;recaptchaUserKey&quot;:&quot;&quot;}</td>
    </tr>
    <tr>
      <td style="text-align:left">sendTransaction</td>
      <td style="text-align:left">&#x53D1;&#x9001;&#x4EA4;&#x6613;</td>
      <td style="text-align:left">{&quot;actions&quot;:[{&quot;account&quot;:&quot;eosio&quot;,&quot;name&quot;:&quot;buyram&quot;,&quot;data&quot;:{&quot;payer&quot;:&quot;&quot;,&quot;receiver&quot;:&quot;&quot;,&quot;quant&quot;:&quot;1.0000
        EOS&quot;}}],&quot;chain&quot;:&quot;EOS&quot;}</td>
      <td style="text-align:left">
        <p>promise:&#x65B9;&#x5F0F;&#x8FD4;&#x56DE;</p>
        <p>result:{&quot;errorCode&quot;:0,&quot;errorMsg&quot;:&quot;&quot;,&quot;data&quot;:{&quot;transactionId&quot;:&quot;&quot;,&quot;signature&quot;:&quot;&quot;}}</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">sign</td>
      <td style="text-align:left">&#x7533;&#x8BF7;MYKEY&#x7B7E;&#x540D;</td>
      <td style="text-align:left">message:(&#x5B57;&#x7B26;&#x4E32;&#x7C7B;&#x578B;&#x7B7E;&#x540D;&#x6570;&#x636E;)</td>
      <td
      style="text-align:left">
        <p>promise:&#x65B9;&#x5F0F;&#x8FD4;&#x56DE;</p>
        <p>result:{&quot;errorCode&quot;:0,&quot;errorMsg&quot;:&quot;&quot;,&quot;data&quot;:{&quot;signature&quot;:&quot;&quot;}}</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">setTitle</td>
      <td style="text-align:left">&#x8BBE;&#x7F6E;&#x5E94;&#x7528;&#x9876;&#x90E8;&#x6807;&#x9898;</td>
      <td
      style="text-align:left">title:(&#x5B57;&#x7B26;&#x4E32;&#x7C7B;&#x578B;&#x6807;&#x9898;&#x5185;&#x5BB9;)</td>
        <td
        style="text-align:left">&#x65E0;</td>
    </tr>
    <tr>
      <td style="text-align:left">showLoading</td>
      <td style="text-align:left">&#x663E;&#x793A;loading&#x52A8;&#x753B;</td>
      <td style="text-align:left">&#x65E0;</td>
      <td style="text-align:left">&#x65E0;</td>
    </tr>
    <tr>
      <td style="text-align:left">hiddenLoading</td>
      <td style="text-align:left">&#x53D6;&#x6D88;loading&#x52A8;&#x753B;</td>
      <td style="text-align:left">&#x65E0;</td>
      <td style="text-align:left">&#x65E0;</td>
    </tr>
    <tr>
      <td style="text-align:left">copyToClipboard</td>
      <td style="text-align:left">&#x590D;&#x5236;&#x5185;&#x5BB9;&#x5230;&#x7CFB;&#x7EDF;&#x526A;&#x8D34;&#x677F;</td>
      <td
      style="text-align:left">message:(&#x5B57;&#x7B26;&#x4E32;&#x7C7B;&#x578B;&#x8981;&#x590D;&#x5236;&#x7684;&#x5185;&#x5BB9;)</td>
        <td
        style="text-align:left">
          <p>promise:&#x65B9;&#x5F0F;&#x8FD4;&#x56DE;</p>
          <p>result:{&quot;errorCode&quot;:0,&quot;errorMsg&quot;:&quot;&quot;}</p>
          </td>
    </tr>
    <tr>
      <td style="text-align:left">showOpenChain</td>
      <td style="text-align:left">&#x6253;&#x5F00;&#x521B;&#x5EFA;&#x94FE;&#x7684;&#x9875;&#x9762;</td>
      <td
      style="text-align:left">{&quot;chain&quot;:&quot;EOS|ETH&quot;}</td>
        <td style="text-align:left">&#x65E0;</td>
    </tr>
    <tr>
      <td style="text-align:left">showLessNetworkFee</td>
      <td style="text-align:left">&#x663E;&#x793A;&#x7F51;&#x7EDC;&#x8D39;&#x4F59;&#x989D;&#x4E0D;&#x8DB3;&#x5F39;&#x6846;</td>
      <td
      style="text-align:left">&#x65E0;</td>
        <td style="text-align:left">&#x65E0;</td>
    </tr>
    <tr>
      <td style="text-align:left">checkAppVersion</td>
      <td style="text-align:left">&#x68C0;&#x6D4B;&#x5E94;&#x7528;&#x7248;&#x672C;</td>
      <td style="text-align:left">&#x65E0;</td>
      <td style="text-align:left">&#x65E0;</td>
    </tr>
    <tr>
      <td style="text-align:left">printLog</td>
      <td style="text-align:left">&#x6253;&#x5370;&#x65E5;&#x5FD7;&#xFF08;&#x5BA2;&#x6237;&#x7AEF;&#x6709;&#x65E5;&#x5FD7;&#x4E0A;&#x62A5;&#x529F;&#x80FD;&#xFF0C;&#x53EF;&#x534F;&#x52A9;&#x5206;&#x6790;&#x95EE;&#x9898;&#xFF09;</td>
      <td
      style="text-align:left">log:(&#x5B57;&#x7B26;&#x4E32;&#x7C7B;&#x578B;&#x65E5;&#x5FD7;)</td>
        <td
        style="text-align:left">&#x65E0;</td>
    </tr>
    <tr>
      <td style="text-align:left">encodeFunctionCall</td>
      <td style="text-align:left">&#x5BF9;&#x65B9;&#x6CD5;&#x8FDB;&#x884C;&#x5E8F;&#x5217;&#x5316;</td>
      <td
      style="text-align:left">
        <p>abi:&#x8BE5;&#x65B9;&#x6CD5;&#x7684;abi&#x63CF;&#x8FF0;,&#x5B57;&#x7B26;&#x4E32;&#x7C7B;&#x578B;,eg:&quot;[{\&quot;constant\&quot;:false,\&quot;inputs\&quot;:[{\&quot;name\&quot;:\&quot;_to\&quot;,\&quot;type\&quot;:\&quot;address\&quot;},{\&quot;name\&quot;:\&quot;_value\&quot;,\&quot;type\&quot;:\&quot;uint256\&quot;}],\&quot;name\&quot;:\&quot;transfer\&quot;,\&quot;outputs\&quot;:[{\&quot;name\&quot;:\&quot;\&quot;,\&quot;type\&quot;:\&quot;bool\&quot;}],\&quot;payable\&quot;:false,\&quot;stateMutability\&quot;:\&quot;nonpayable\&quot;,\&quot;type\&quot;:\&quot;function\&quot;}]&quot;</p>
        <p>method&#xFF1A;&#x65B9;&#x6CD5;&#x540D;, &#x5B57;&#x7B26;&#x4E32;&#x7C7B;&#x578B;&#xFF0C;eg:transfer&#x3002;</p>
        <p>param&#xFF1A;&#x53C2;&#x6570;&#xFF0C;eg:{_to:&quot;0xc4ED1B3f31acadbE3c14B20fA766B6C4B1FAB208&quot;,_value:&quot;20000000000000000000&quot;}</p>
        </td>
        <td style="text-align:left">
          <p>promise:&#x65B9;&#x5F0F;&#x8FD4;&#x56DE;</p>
          <p>result:{&quot;errorCode&quot;:0,&quot;errorMsg&quot;:&quot;&quot;,&quot;data&quot;:&quot;[&#x5E8F;&#x5217;&#x5316;&#x540E;&#x7684;&#x503C;]&quot;}</p>
        </td>
    </tr>
  </tbody>
</table>

