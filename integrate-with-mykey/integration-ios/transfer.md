# Transfer

Pull up MYKEY to transfer. See the class definition for the parameters:[TransferRequest](../../dive-into-mykey/classes-and-methods/#class-transferrequest) and [MYKEYWalletCallback](../../dive-into-mykey/classes-and-methods/#class-mykeywalletcallback)

DApp server or client should query the user's ReserveKey from MYKEY SmartContract data to verify the signature, see detail in [KEYS in MYKEY](../../dive-into-mykey/dive-into-mykey-account.md#keydata表中的密钥).

```swift
let transferData = TransferData()
transferData.from = "mykeyhulu525"
transferData.to = "madaraxliang"
transferData.quantity = "1.31 EOS"
transferData.memo = "transfer-memo"

let transferActionData = TransferAction()
transferActionData.account = "eosio.token"
transferActionData.name = WalletActionConstants.TRANSFER.rawValue
transferActionData.info = "contract-transfer-info"
transferActionData.data = transferData

let contractActionData = ContractAction()
contractActionData.account = "eosio"
contractActionData.name = "buyram"
contractActionData.info = "contract-contract-info"
contractActionData.data = ["payer":"mykeyhulu123","receiver":"mykeyhulu111","quant":"1.01 EOS"]

let contractRequest = ContractRequest()
contractRequest.info = "Perform the mortgage REX operation"
contractRequest.orderId = "BH19004"
// param：{"protocol": "", "version": "", "tx_id": "", "ref": "", "account": ""}
contractRequest.callbackUrl = "https://dappserver.xxx.url"
contractRequest.actions = [transferActionData,contractActionData]

MYKEYWallet.shared.contract(contractRequest: contractRequest, response: MYKEYResponse.init(success: { (response) in
    self.view.makeToast("success")
}, failure: { (errorValue) in
    self.view.makeToast("failure")
}, cancelled: {
    self.view.makeToast("cancelled")
}))
```

