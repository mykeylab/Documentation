# EOS

## Compatible with Scatter

MYKEY is compatible with Scatter protocol. You can develop dapp with Scatter protocol, and access it with MYKEY's default browser. 

You can also refer below link to get more details about Scatter protocol:

[https://get-scatter.com/developers/settingupforwebapps](https://get-scatter.com/developers/settingupforwebapps)

## Login

Use scatter.connect method, then login method to get user account information.

```javascript
import ScatterJS from "@scatterjs/core";

ScatterJS.connect("My DAPP", { network }).then(connected => {
  if (!connected) return alert("no scatter");

  const eos = ScatterJS.eos(network, Api, { rpc });
  this.setState({ eos });

  ScatterJS.login().then(id => {
    if (!id) return alert("no identity");
    const account = ScatterJS.account("eos");
    this.setState({ account }, this.getVote);
  });
});
```

## Verify the signature from MYKEY

[MYKEY account structure](../../dive-into-mykey/dive-into-mykey-account.md#mykey有啥不同？) is different with other EOS account, if dapp verify signature in their server side, should use the ReservedKey, more details see this [Document.](../../dive-into-mykey/dive-into-mykey-account.md#keydata表中的密钥)

The Reserved Key can be queried through the keydata table of the mykeymanager smart contract. When querying, the specified range is the MYKEY account and the index is 3.

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

