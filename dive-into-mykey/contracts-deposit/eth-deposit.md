# ETH deposit

### Exchange identify MYKEY transaction

Use Parity / Geth to configure tracing to identify contract internal transactions. Take Parity as an example, you can use trace\_transaction to get all traces of a transaction.\([https://wiki.parity.io/JSONRPC-trace-module\#trace\_transaction](https://wiki.parity.io/JSONRPC-trace-module#trace_transaction)\)

Parity parameters example:

parity --cache-size=1024 --max-peers 100 --db-path /root/.parity --jsonrpc-interface "[0.0.0.0](https://confluence.inner-bihu.com/0.0.0.0)" --jsonrpc-port 7001 --tracing on --jsonrpc-apis eth,net,personal,web3,traces

Required hardware configuration: 

[https://wiki.parity.io/FAQ\#what-are-the-parity-ethereum-disk-space-needs-and-overall-hardware-requirements](https://wiki.parity.io/FAQ#what-are-the-parity-ethereum-disk-space-needs-and-overall-hardware-requirements)， [https://etherscan.io/chartsync/chaindefault](https://etherscan.io/chartsync/chaindefault)

