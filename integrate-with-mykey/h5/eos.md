# EOS

**Special Notice**: [MYKEY account structure](../../dive-into-mykey/mykey-on-eos.md#mykey-account-structure) is different with other EOS account, if dapp verify signature in their server side, should use the public key of Reserved, more details see this [Document](../../dive-into-mykey/mykey-on-eos.md#integrate-eos-dapps-with-mykey).

## Compatible with Scatter

MYKEY is compatible with Scatter protocol. You can develop dapp with Scatter protocol, and access it with MYKEY's default browser. 

You can also refer below link to get more details about Scatter protocol:

[https://get-scatter.com/developers/settingupforwebapps](https://get-scatter.com/developers/settingupforwebapps)

## Verify signing with MYKEY

In MYKEY, please use [authorize](../integration-android/authorize.md) to sign with MYKEY.  

MYKEY generate signed message, and return the account info and signed message to dapp server, dapp server needs to verify the signature.

