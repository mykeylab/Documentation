# 数据存储模块

AccountStorage合约中有4个多层mapping，用来存储数据。

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x6570;&#x636E;</th>
      <th style="text-align:left">&#x5B9A;&#x4E49;</th>
      <th style="text-align:left">&#x542B;&#x4E49;</th>
      <th style="text-align:left">&#x5907;&#x6CE8;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&#x4E00;&#x7EC4;&#x516C;&#x94A5;</td>
      <td style="text-align:left">
        <p>mapping (<b>address</b> =&gt; mapping(<b>uint256</b> =&gt;<b> KeyItem</b>))
          keyData;</p>
        <p>struct KeyItem {
          <br />address pubKey;
          <br />uint256 status;
          <br />}</p>
      </td>
      <td style="text-align:left">MYKEY&#x8D26;&#x6237;&#x5730;&#x5740;=&gt;&#x5E8F;&#x53F7;index=&gt;&#x516C;&#x94A5;&#x7ED3;&#x6784;&#x4F53;&#xFF08;&#x5305;&#x542B;&#x516C;&#x94A5;&#x548C;&#x72B6;&#x6001;&#xFF09;</td>
      <td
      style="text-align:left">
        <p>&#x7B2C;0&#x4F4D;&#x662F;admin key&#xFF0C;&#x7B2C;1&#x4F4D;&#x5F80;&#x540E;&#x662F;operation
          key&#x3002;</p>
        <p>&#x9ED8;&#x8BA4;&#x72B6;&#x6001;&#x4E3A;0&#xFF1B;&#x72B6;&#x6001;&#x4E3A;1&#x662F;&#x51BB;&#x7ED3;&#x72B6;&#x6001;&#x3002;</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x7D27;&#x6025;&#x8054;&#x7CFB;&#x4EBA;</td>
      <td style="text-align:left">
        <p>mapping (<b>address</b> =&gt; mapping(<b>uint256</b> =&gt;<b> BackupAccount</b>))
          backupData;</p>
        <p>struct BackupAccount {
          <br />address backup;
          <br />uint256 effectiveDate;
          <br />uint256 expiryDate;
          <br />}</p>
      </td>
      <td style="text-align:left">MYKEY&#x8D26;&#x6237;&#x5730;&#x5740;=&gt;&#x5E8F;&#x53F7;index=&gt;&#x7D27;&#x6025;&#x8054;&#x7CFB;&#x4EBA;&#x7ED3;&#x6784;&#x4F53;&#xFF08;&#x5305;&#x542B;&#x7D27;&#x6025;&#x8054;&#x7CFB;&#x4EBA;&#x8D26;&#x6237;&#x5730;&#x5740;&#xFF0C;&#x751F;&#x6548;&#x65F6;&#x95F4;&#xFF0C;&#x5931;&#x6548;&#x65F6;&#x95F4;&#xFF09;</td>
      <td
      style="text-align:left">
        <p>index&#x4E3A;0&#xFF5E;5&#xFF0C;&#x6700;&#x591A;6&#x4E2A;&#x7D27;&#x6025;&#x8054;&#x7CFB;&#x4EBA;&#x3002;</p>
        <p>&#x6DFB;&#x52A0;&#x7D27;&#x6025;&#x8054;&#x7CFB;&#x4EBA;(addBackup)&#x548C;&#x5220;&#x9664;&#x7D27;&#x6025;&#x8054;&#x7CFB;&#x4EBA;(removeBackup)&#x90FD;&#x6709;21&#x5929;&#x7684;&#x5EF6;&#x65F6;&#xFF0C;&#x56E0;&#x6B64;</p>
        <p>addBackup&#x64CD;&#x4F5C;&#x4F1A;&#x8BBE;&#x7F6E;&#x751F;&#x6548;&#x65F6;&#x95F4;&#xFF1B;</p>
        <p>removeBackup&#x64CD;&#x4F5C;&#x4F1A;&#x8BBE;&#x7F6E;&#x5931;&#x6548;&#x65F6;&#x95F4;&#xFF1B;</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x5EF6;&#x65F6;&#x52A8;&#x4F5C;</td>
      <td style="text-align:left">
        <p>mapping (<b>address</b> =&gt; mapping(<b>bytes4</b> =&gt;<b> DelayItem</b>))
          delayData;</p>
        <p>struct DelayItem {
          <br />bytes32 hash;
          <br />uint256 dueTime;
          <br />}</p>
      </td>
      <td style="text-align:left">MYKEY&#x8D26;&#x6237;&#x5730;&#x5740;=&gt;&#x52A8;&#x4F5C;ID=&gt;&#x5EF6;&#x65F6;&#x7ED3;&#x6784;&#x4F53;&#xFF08;&#x5305;&#x542B;&#x52A8;&#x4F5C;&#x54C8;&#x5E0C;&#xFF0C;&#x5230;&#x671F;&#x65F6;&#x95F4;&#xFF09;</td>
      <td
      style="text-align:left">
        <p>&#x52A8;&#x4F5C;ID&#x662F;&#x52A8;&#x4F5C;&#x65B9;&#x6CD5;&#x7684;&#x552F;&#x4E00;&#x6807;&#x5FD7;&#xFF0C;&#x4E0E;&#x52A8;&#x4F5C;&#x65B9;&#x6CD5;&#x4E00;&#x4E00;&#x5BF9;&#x5E94;&#xFF0C;&#x957F;&#x5EA6;4&#x5B57;&#x8282;&#x3002;</p>
        <p>&#x5982;bytes4(keccak256(&quot;changeAdminKey(address,address)&quot;))</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x591A;&#x7B7E;&#x63D0;&#x6848;</td>
      <td style="text-align:left">
        <p>mapping (<b>address</b> =&gt; mapping(<b>address</b> =&gt; mapping(<b>bytes4</b> =&gt; <b>Proposal</b>)))
          proposalData;</p>
        <p>struct Proposal {
          <br />bytes32 hash;
          <br />address[] approval;
          <br />}</p>
      </td>
      <td style="text-align:left">
        <p>client&#xFF1A;&#x591A;&#x7B7E;&#x63D0;&#x6848;&#x5F53;&#x4E8B;&#x4EBA;&#xFF1B;</p>
        <p>backup&#xFF1A;&#x5F53;&#x4E8B;&#x4EBA;&#x7684;&#x7D27;&#x6025;&#x8054;&#x7CFB;&#x4EBA;&#xFF1B;</p>
        <p>proposer&#xFF1A;&#x591A;&#x7B7E;&#x63D0;&#x6848;&#x53D1;&#x8D77;&#x4EBA;&#xFF1B;</p>
        <p>client&#x8D26;&#x6237;&#x5730;&#x5740;=&gt;proposer&#x8D26;&#x6237;&#x5730;&#x5740;=&gt;&#x52A8;&#x4F5C;ID=&gt;&#x63D0;&#x6848;&#x7ED3;&#x6784;&#x4F53;&#xFF08;&#x5305;&#x542B;&#x52A8;&#x4F5C;&#x54C8;&#x5E0C;&#xFF0C;&#x5DF2;&#x8D5E;&#x6210;&#x7684;&#x7D27;&#x6025;&#x8054;&#x7CFB;&#x4EBA;&#x7684;&#x6570;&#x7EC4;&#xFF09;</p>
      </td>
      <td style="text-align:left">
        <p>&#x52A8;&#x4F5C;&#x54C8;&#x5E0C;&#x662F;&#x63D0;&#x6848;&#x901A;&#x8FC7;&#x540E;&#x5C06;&#x8981;&#x8C03;&#x7528;&#x7684;&#x51FD;&#x6570;&#x53CA;&#x5176;&#x53C2;&#x6570;&#x7684;&#x54C8;&#x5E0C;&#xFF0C;&#x5982;keccak256(abi.encodePacked(&apos;changeAdminKey&apos;,
          account, pkNew))&#x3002;</p>
        <p>&#x63D0;&#x6848;&#x5206;&#x4E24;&#x79CD;&#xFF1A;</p>
        <ol>
          <li>proposeByBoth&#xFF1A;client&#x548C;&#x67D0;&#x4E00;backup&#x5171;&#x540C;&#x53D1;&#x8D77;&#xFF0C;&#x9700;&#x8981;&#x53CC;&#x65B9;&#x7B7E;&#x540D;&#xFF0C;&#x6B64;&#x65F6;proposer&#x4E3A;client&#x672C;&#x4EBA;&#xFF1B;</li>
          <li>proposeAsBackup&#xFF1A;&#x67D0;&#x4E00;backup&#x5355;&#x72EC;&#x53D1;&#x8D77;&#xFF08;&#x5F53;client&#x4E22;&#x5931;admin
            key&#x65F6;&#xFF09;&#xFF0C;&#x6B64;&#x65F6;proposer&#x4E3A;backup&#xFF1B;</li>
        </ol>
      </td>
    </tr>
  </tbody>
</table>

