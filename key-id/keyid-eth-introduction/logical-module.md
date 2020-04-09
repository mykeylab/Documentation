# 逻辑模块

四个初始逻辑模块是：AccountLogic, DualsigsLogic, TransferLogic, DappLogic。

后续LogicManager合约可以添加逻辑模块，也可以关闭原有的模块，但添加和关闭都有延时，不是立即生效。

逻辑模块都继承自BaseLogic。一些公用的函数（比如验签逻辑）都在BaseLogic中。

<table>
  <thead>
    <tr>
      <th style="text-align:left">Logic&#x6A21;&#x5757;</th>
      <th style="text-align:left">&#x7EDF;&#x4E00;&#x5165;&#x53E3;</th>
      <th style="text-align:left">&#x5305;&#x542B;&#x52A8;&#x4F5C;(&#x65B9;&#x6CD5;&#x7684;&#x7B2C;&#x4E00;&#x4E2A;&#x53C2;&#x6570;&#x5FC5;&#x987B;&#x662F;&#x7B7E;&#x540D;&#x8005;)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">AccountLogic</td>
      <td style="text-align:left">
        <p>function enter(bytes calldata data, bytes calldata signature, uint256
          nonce) external {}</p>
        <p>&#x53C2;&#x6570;&#xFF1A;&#x52A8;&#x4F5C;&#x6570;&#x636E;&#xFF0C;&#x7B7E;&#x540D;&#x8D26;&#x6237;&#xFF0C;&#x7B7E;&#x540D;&#xFF0C;nonce</p>
      </td>
      <td style="text-align:left">
        <p>&#x4FEE;&#x6539;admin key&#xFF0C;&#x6DFB;&#x52A0;operation key&#xFF0C;</p>
        <p>&#x6279;&#x91CF;&#x4FEE;&#x6539;&#x6240;&#x6709;operation key&#xFF0C;&#x51BB;&#x7ED3;/&#x89E3;&#x51BB;&#x8D26;&#x6237;&#xFF0C;&#x5220;&#x9664;&#x7D27;&#x6025;&#x8054;&#x7CFB;&#x4EBA;&#xFF0C;&#x53D6;&#x6D88;&#x5EF6;&#x65F6;&#x52A8;&#x4F5C;&#xFF0C;</p>
        <p>&#x4EE5;&#x7D27;&#x6025;&#x8054;&#x7CFB;&#x4EBA;(backup)&#x7684;&#x8EAB;&#x4EFD;&#x53D1;&#x8D77;&#x63D0;&#x6848;/&#x8D5E;&#x6210;&#x63D0;&#x6848;&#xFF0C;&#x4EE5;&#x5F53;&#x4E8B;&#x4EBA;(client)&#x7684;&#x8EAB;&#x4EFD;&#x53D6;&#x6D88;&#x63D0;&#x6848;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">TransferLogic</td>
      <td style="text-align:left">&#x53D1;&#x9001;ETH&#xFF0C;ERC20&#xFF0C;NFT&#x7B49;&#x4EE3;&#x5E01;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">DappLogic</td>
      <td style="text-align:left">&#x8C03;&#x7528;&#x5176;&#x4ED6;&#x5408;&#x7EA6;</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">DualsigsLogic</td>
      <td style="text-align:left">function enter(bytes calldata data, bytes calldata clientSig, bytes calldata
        backupSig, uint256 clientNonce, uint256 backupNonce) external {}&#x53C2;&#x6570;&#xFF1A;&#x52A8;&#x4F5C;&#x6570;&#x636E;&#xFF0C;client&#x7B7E;&#x540D;&#xFF0C;backup&#x7B7E;&#x540D;&#xFF0C;client
        nonce&#xFF0C;backup nonce</td>
      <td style="text-align:left">
        <p>&#x9700;&#x8981;&#x53CC;&#x7B7E;&#x540D;&#x7684;&#x52A8;&#x4F5C;&#xFF0C;&#x5373;client&#x548C;backup&#x53CC;&#x65B9;&#x7B7E;&#x540D;&#xFF1A;</p>
        <ol>
          <li>&#x6DFB;&#x52A0;&#x7D27;&#x6025;&#x8054;&#x7CFB;&#x4EBA;&#xFF08;&#x9700;&#x8981;&#x5F81;&#x5F97;&#x540C;&#x610F;&#xFF0C;&#x6240;&#x4EE5;&#x7D27;&#x6025;&#x8054;&#x7CFB;&#x4EBA;&#x4E5F;&#x8981;&#x7B7E;&#x540D;&#xFF09;&#xFF1B;</li>
        </ol>
        <p>2. &#x7531;client&#x548C;backup&#x5171;&#x540C;&#x53D1;&#x8D77;&#x63D0;&#x6848;(&#x6B64;&#x7C7B;&#x63D0;&#x6848;&#x901A;&#x8FC7;&#x540E;&#xFF0C;&#x52A8;&#x4F5C;&#x7ACB;&#x5373;&#x6267;&#x884C;&#xFF0C;&#x65E0;&#x5EF6;&#x65F6;)&#xFF1B;</p>
      </td>
    </tr>
  </tbody>
</table>与EOS合约的主要不同点：

1. **延时交易：** 只存储延时的动作哈希（EOS上存储了完整的动作数据）和对应的到期时间，MYKEY后端会monitor到期状态，自动trigger。
2. **签名raw message**  
   遵循EIP191，签名的raw message拼接方式为：0x1900 + 逻辑合约地址 + **动作数据\(包含函数名ID和参数\)** + nonce。  
   其中，动作数据的前四个字节是动作ID\(Method ID\)，其后依此是各个参数，我们规定，**第一个参数必须是签名者\(signer account address\)**，否则验签会失败。  
   例如，下面是transferEth这个动作的数据，黑色部分是动作ID，红色部分为参数from\(签名者\)，蓝色部分为参数to，绿色部分为参数amount。

   0xd765925d000000000000000000000000996b09ecabdb73219fd3e156985bc98acc552b6900000000000000000000000005f9a32dc3155d95a28fa3934edb843d7e2ef48a00000000000000000000000000000000000000000000000000038d7ea4c68000

3. **nonce的设计：** 每一个Logic模块都会维护一个keyNonce map，key/value分别是已通过验签的公钥和nonce。 nonce是当前时间戳（单位是微秒，即1/1000000秒），是一个uint256，调用Logic模块入口函数时以参数的形式传入。 验签时，要求nonce大于keyNonce中存储的旧nonce，且nonce不大于当前时间戳加24小时。 验签通过后，nonce会存入keyNonce，覆盖旧nonce。

