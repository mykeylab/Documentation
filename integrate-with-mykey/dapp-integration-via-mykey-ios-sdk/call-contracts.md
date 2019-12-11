# Call contracts

Pull up MYKEY for contract calls, support multiple action combination calls, support ContractAction and TransferAction two types of action types. Please refer to the class definition for the parameters [ContractRequest](),[ContractAction](),[TransferAction]() and [MYKEYResponse]().

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

