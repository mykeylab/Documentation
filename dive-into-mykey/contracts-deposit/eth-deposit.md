# ETH充值

### 交易所识别

使用Parity/GEth 配置tracing， 识别合约内部交易。以Parity为例, 可以使用trace\_transaction 拿到交易的所有traces。\([https://wiki.parity.io/JSONRPC-trace-module\#trace\_transaction](https://wiki.parity.io/JSONRPC-trace-module#trace_transaction)\)

Parity 参数示例:

parity --cache-size=1024 --max-peers 100 --db-path /root/.parity --jsonrpc-interface "[0.0.0.0](https://confluence.inner-bihu.com/0.0.0.0)" --jsonrpc-port 7001 --tracing on --jsonrpc-apis eth,net,personal,web3,traces

所需硬件配置:

[https://wiki.parity.io/FAQ\#what-are-the-parity-ethereum-disk-space-needs-and-overall-hardware-requirements](https://wiki.parity.io/FAQ#what-are-the-parity-ethereum-disk-space-needs-and-overall-hardware-requirements)， [https://etherscan.io/chartsync/chaindefault](https://etherscan.io/chartsync/chaindefault)

