# 合约调用

唤起MYKEY进行合约调用, 支持多Action组合调用, 支持ContractAction和TransferAction两种形式的action类型。 参数请详见类定义:[ContractRequest](../../dive-into-mykey/classes-and-methods/#lei-contractrequest),[ContractAction](../../dive-into-mykey/classes-and-methods/#lei-contractaction),[TransferAction](../../dive-into-mykey/classes-and-methods/#lei-transferaction) 和 [MYKEYResponse](../../dive-into-mykey/classes-and-methods/#lei-mykeyresponse).

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
// return: same as SimpleWallet {"code": [0-2], "message": ""}
contractRequest.callbackUrl = "https://dappserver.xxx.url"
contractRequest.actions = [transferActionData,contractActionData]

MYKEYWallet.shared.contract(contractRequest: contractRequest, response: MYKEYResponse.init(success: { (response) in
    self.presentDataView(data: response)
    self.view.makeToast("success")
}, failure: { (errorValue) in
    self.presentDataView(data: errorValue)
    self.view.makeToast("failure")
}, cancelled: {
    self.view.makeToast("cancelled")
}))
```

