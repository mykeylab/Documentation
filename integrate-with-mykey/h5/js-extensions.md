# JS功能扩展

JSBridge为MYKEY应用中心内嵌的浏览器环境中默认支持的JS注入库，其支持Scatter协议，也支持web3协议。

为了方便控制MYKEY应用中心浏览器，MYKEY还增加了以下方法：

| 方法 | 描述 |
| :--- | :--- |
| [isMYKEY](js-extensions.md#jian-cha-dang-qian-qian-bao-shi-fou-wei-mykey) | 检查当前钱包是否为MYKEY |
| [closeWindow](js-extensions.md#guan-bi-chuang-kou) | 关闭应用窗口回到MYKEY |
| [openFullScreen](js-extensions.md#da-kai-quan-ping) | 打开全屏 |
| [closeFullScreen](js-extensions.md#guan-bi-quan-ping) | 关闭全屏 |
| [forbidPhysicalBack](js-extensions.md#jin-zhi-wu-li-fan-hui) | 禁止物理返回按钮（Android专属） |
| [allowPhysicalBack](js-extensions.md#yun-xu-wu-li-fan-hui) | 允许物理返回按钮（Android专属） |
| [getAccountInfo](js-extensions.md#huo-qu-mykey-zhang-hu-xin-xi) | 获取MYKEY账户信息 |
| [getClientConfig](js-extensions.md#huo-qu-ke-hu-duan-bu-fen-pei-zhi) | 获取客户端部分配置 |
| [sendTransaction](js-extensions.md#fa-song-jiao-yi) | 发送交易 |
| [sign](js-extensions.md#qian-ming) | 申请MYKEY签名 |
| [setTitle](js-extensions.md#she-zhi-ying-yong-ding-bu-biao-ti) | 设置应用顶部标题 |
| [showLoading](js-extensions.md#xian-shi-loading-dong-hua) | 显示loading动画 |
| [hiddenLoading](js-extensions.md#qu-xiao-loading-dong-hua) | 取消loading动画 |
| [encodeFunctionCall](js-extensions.md#dui-fang-fa-jin-hang-xu-lie-hua) | 对方法进行序列化 |

### 检查当前钱包是否为MYKEY

```javascript
window.ethereum.isMYKEY
```

### 关闭窗口

关闭应用窗口回到MYKEY。

```javascript
window.MyKey.Browser.closeWindow
```

### 

### 打开全屏

打开全屏**。**

```javascript
window.MyKey.Browser.openFullScreen
```

#### **参数**

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x63CF;&#x8FF0;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">isLandscape</td>
      <td style="text-align:left">boolean</td>
      <td style="text-align:left">
        <p>true(&#x6A2A;&#x5C4F;)</p>
        <p>fasle(&#x7AD6;&#x5C4F;)</p>
      </td>
    </tr>
  </tbody>
</table>

### \*\*\*\*

### **关闭全屏**

关闭全屏。

```javascript
window.MyKey.Browser.closeFullScreen
```

###  ****

### **禁止物理返回**

禁止物理返回按钮，限Android

```javascript
window.MyKey.Browser.forbidPhysicalBack
```

### 

### 允许物理返回

允许物理返回按钮，限Android

```javascript
window.MyKey.Browser.allowPhysicalBack
```

### 

### 获取MYKEY账户信息

获取MYKEY账户信息

```javascript
window.MyKey.Browser.getAccountInfo
```

#### 参数

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x63CF;&#x8FF0;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">openChain</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>true|fasle</p>
        <p>true: &#x5982;&#x679C;&#x6CA1;&#x6709;&#x94FE;&#x4F1A;&#x6253;&#x5F00;&#x521B;&#x5EFA;&#x9875;&#x9762;</p>
      </td>
    </tr>
  </tbody>
</table>

#### 返回值

```javascript
返回格式：Promise
{
	"data": {
		"accountName": "",     //MYKEY内设置的昵称
		"chainInfoList": ["account": "", "chain": ""],
		"id": "",    //MYKEY唯一ID
		"operationKeys": ["","",""],   //三把操作权限的公钥
		"xpubOperationKeys": ["","",""]  //三把操作秘钥公钥Xpub形式
	},
	"errorCode": 0
}
```



### 获取客户端部分配置

获取客户端部分配置

```javascript
window.MyKey.Browser.getClientConfig
```

#### 返回值

```javascript
返回方式：Promise
{
	"data": {
		"currency": "CNY",
		"locale": "zh-CN",
		"maxKycBindAccount": 1,
		"regioin": "CN",
		"userAgent": "",       //包含 channel:MYKEY
		"recaptchaUserKey":""  //MYKEY红包用到
	},
	"errorCode": 0
}
```



### 发送交易

发送交易

```javascript
window.MyKey.Browser.sendTransaction(transaction)
```

#### 参数

| 参数名 | 类型 | 描述 |
| :--- | :--- | :--- |
| transaction | string | 指定链以及交易的actions |

#### 返回值

```javascript
返回方式：Promise
result: {
	"errorCode": 0,
	"errorMsg": "",
	"data": {
		"transactionId": "",
		"signature": ""
	}
}
```

#### 举例：

EOS链上购买内存。

```javascript
window.MyKey.Browser.sendTransaction('{"actions":[{"account":"eosio","name":"buyram","data":{"payer":"","receiver":"","quant":"1.0000 EOS"}}],"chain":"EOS","extra":{"key":"value"}}')
```

新增的extra字段，生效版本为：Android:2.5.0，IOS:2.5.0



### 签名

申请MYKEY签名

```javascript
window.MyKey.Browser.sign(message)
```

#### 参数

| 参数名 | 类型 | 描述 |
| :--- | :--- | :--- |
| message | string | 待签名的数据 |

#### 返回值

```javascript
返回格式：Promise
result: {
	"errorCode": 0,
	"errorMsg": "",
	"data": {
		"signature": ""
	}
}
```



### 获取交易进度

```javascript
window.MyKey.Browser.getTransactionProgress
```

#### 参数

| 参数名 | 类型 | 描述 |
| :--- | :--- | :--- |
| chain | string | EOS\|ETH |
| transactionId | string |  |
| blockNum | int | 当前交易的blockNum，非必填 |

#### 返回值

```javascript
返回格式：Promise
result: {
	"errorCode": 0,
	"errorMsg": "",
	"data": {
		"percent": ,   //[0 - 100]
		"blockNum":    //[当前交易的blockNum]
	}
}
```

生效版本：Android:2.5.0，iOS:2.5.0



### 设置应用顶部标题

设置应用顶部标题

```javascript
window.MyKey.Browser.setTitle(title)
```

#### 参数

| 参数名 | 类型 | 描述 |
| :--- | :--- | :--- |
| title | string | 标题内容 |

### 

### 显示loading动画

显示loading动画

```javascript
window.MyKey.Browser.showLoading
```



### 取消loading动画

取消loading动画

```javascript
window.MyKey.Browser.hiddenLoading
```

### 

### 对方法进行序列化

对方法进行序列化

```javascript
window.MyKey.Browser.encodeFunctionCall(abi, method, param)
```

#### 参数

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x63CF;&#x8FF0;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">abi</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>&#x8BE5;&#x65B9;&#x6CD5;&#x7684;abi&#x63CF;&#x8FF0;,&#x5B57;&#x7B26;&#x4E32;&#x7C7B;&#x578B;,eg:&quot;[{\&quot;constant\&quot;:false,\&quot;inputs\&quot;:[</p>
        <p>{\&quot;name\&quot;:\&quot;_to\&quot;,\&quot;type\&quot;:\&quot;address\&quot;},{\&quot;name\&quot;:\&quot;_value\&quot;,\&quot;type\&quot;:\&quot;uint256\&quot;}],\&quot;name\&quot;:\&quot;transfer\&quot;,\&quot;outputs\&quot;:[{\&quot;name\&quot;:\&quot;\&quot;,\&quot;type\&quot;:\&quot;bool\&quot;}],\&quot;payable\&quot;:false,</p>
        <p>\&quot;stateMutability\&quot;:\&quot;nonpayable\&quot;,\&quot;type\&quot;:\&quot;function\&quot;}]&quot;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">method</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">&#x65B9;&#x6CD5;&#x540D;, &#x5B57;&#x7B26;&#x4E32;&#x7C7B;&#x578B;&#xFF0C;eg:transfer&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">param</td>
      <td style="text-align:left">string</td>
      <td style="text-align:left">
        <p>&#x53C2;&#x6570;&#xFF0C;eg:{_to:&quot;0xc4ED1B3f31acadbE3c14B20fA766B6C4B1FAB208&quot;,</p>
        <p>_value:&quot;20000000000000000000&quot;}</p>
      </td>
    </tr>
  </tbody>
</table>

#### 返回值

```javascript
返回方式：Promise
result: {
	"errorCode": 0,
	"errorMsg": "",
	"data": ""     //[序列化后的值]
}
```





