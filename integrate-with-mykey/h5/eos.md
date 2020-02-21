# EOS

## 兼容Scatter

MYKEY兼容Scatter协议，您可以直接开发兼容Scatter协议的dapp，再通过mykey内置浏览器访问。也可以参考以下链接，了解更多Scatter协议的内容：

[https://get-scatter.com/developers/settingupforwebapps](https://get-scatter.com/developers/settingupforwebapps)

## MYKEY验签方式

[MYKEY的账号体系](../../dive-into-mykey/mykey-on-eos.md#mykey帐户结构)与其他的EOS账号有所差异，需要在服务端验签时使用**Reserved公钥**进行验签。客户端使用MYKEY账号index为3的公钥，用scatter.getArbitrarySignature方法进行签名。

Reserved Key的公钥，可以通过智能合约mykeymanager的表keydata查询到。查询时，指定范围为 MYKEY账号。

```javascript
/**
 * 获取ReservedKey
 * 详情参考 https://docs.mykey.org/v/English/dive-into-mykey/mykey-on-eos#mykey-account-structure
 * @param  {String}  name  MYKEY账号
 * @return {String}  ReservedKey/the 3rd Operation Key 
 */
async getReservedKey(name) {
	const mgrcontract = await this.getMykeyMgr(name)
    const mykey_signkey_index = 3
    const keydata = await this.eosJsonRpc.get_table_rows({json:true, code:mgrcontract, scope:name, table:'keydata', lower_bound: mykey_signkey_index, limit:1})
    if(!keydata) return '';
    return keydata.rows[0].key.pubkey;
}

/**
 * 验证已签名的数据
 * Parameters
 *  signature (string | Buffer) buffer or hex string
 *  data (string | Buffer)
 *  pubkey (pubkey | PublicKey)
 *  encoding (optional, default 'utf8')
 *  hashData boolean sha256 hash data before verify (optional, default true)
 */
ecc.verify(signature, data, pubkey) === true

```

