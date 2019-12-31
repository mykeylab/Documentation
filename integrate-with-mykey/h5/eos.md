# EOS

## Compatible with Scatter

MYKEY is compatible with Scatter protocol. You can develop dapp with Scatter protocol, and access it with MYKEY's default browser. 

You can also refer below link to get more details about Scatter protocol:

[https://get-scatter.com/developers/settingupforwebapps](https://get-scatter.com/developers/settingupforwebapps)

## Verify signing with MYKEY

\*\*\*\*[MYKEY account structure](../../dive-into-mykey/mykey-on-eos.md#mykey-account-structure) is different with other EOS account, if dapp verify signature in their server side, should use the public key of Reserved, more details see this [Document](../../dive-into-mykey/mykey-on-eos.md#integrate-eos-dapps-with-mykey).

MYKEY uses the reserved key which index is 3 to sign through scatter.getArbitrarySignature. Server side of dapps need use the corresponding public key of reserved key to verify signature, it can be queried by backend code in table keydata of contract mykeymanager by scope ACCOUNT\_NAME.

```javascript
/**
 * Get ReservedKey in mykey account which is for other actions without specific operation keys, details in
 * https://docs.mykey.org/v/English/dive-into-mykey/mykey-on-eos#mykey-account-structure
 * @param  {String}  name account name
 * @return {String}      ReservedKey/the 3rd Operation Key
 */
async getReservedKey(name) {
	const mgrcontract = await this.getMykeyMgr(name)
    const mykey_signkey_index = 3
    const keydata = await this.eosJsonRpc.get_table_rows({json:true, code:mgrcontract, scope:name, table:'keydata', lower_bound: mykey_signkey_index, limit:1})
    if(!keydata) return '';
    return keydata.rows[0].key.pubkey;
}

/**
 * Verify signed data
 * Parameters
 *  signature (string | Buffer) buffer or hex string
 *  data (string | Buffer)
 *  pubkey (pubkey | PublicKey)
 *  encoding (optional, default 'utf8')
 *  hashData boolean sha256 hash data before verify (optional, default true)
 */
ecc.verify(signature, data, pubkey) === true

```

