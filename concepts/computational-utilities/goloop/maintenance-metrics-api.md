# Maintenance metrics API

Provide metrics by HTTP GET ‘[http://SERVER\_IP:RPC\_PORT/metrics’](http://server\_ip/:RPC\_PORT/metrics%E2%80%99)

Integrated with [prometheus](https://prometheus.io)

* suffix-rule of metric
  * \_duration : time value (unit : msec)
  * \_cnt : number of events
  * \_sum : sum of values

## Consensus

| Metric                      | Description                          |
| --------------------------- | ------------------------------------ |
| consensus\_height           | Height of Propose-Block              |
| consensus\_height\_duration | Consensus Duration of Previous Block |
| consensus\_round            | Current Consensus Round              |
| consensus\_round\_duration  | Duration of Previous Consensus Round |

## Transaction Latency

| Metric              | Description                                                  |
| ------------------- | ------------------------------------------------------------ |
| txlatency\_commit   | Average of commit latency (msec) of transactions from user   |
| txlatency\_finalize | Average of finalize latency (msec) of transactions from user |

## Transaction Pool

Accumulated number and bytes of processed transactions

### From any

Received transactions via p2p and json-rpc

| Metric              | Description                                     |
| ------------------- | ----------------------------------------------- |
| txpool\_add\_cnt    | accumulated number of add transactions          |
| txpool\_add\_sum    | accumulated bytes of add transactions           |
| txpool\_drop\_cnt   | accumulated number of drop invalid-transactions |
| txpool\_drop\_sum   | accumulated bytes of drop invalid-transactions  |
| txpool\_remove\_cnt | accumulated number of remove valid-transactions |
| txpool\_remove\_sum | accumulated bytes of remove valid-transactions  |

### From user

Received transactions via json-rpc

| Metric                    | Description                                     |
| ------------------------- | ----------------------------------------------- |
| txpool\_user\_add\_cnt    | accumulated number of add transactions          |
| txpool\_user\_add\_sum    | accumulated bytes of add transactions           |
| txpool\_user\_drop\_cnt   | accumulated number of drop invalid-transactions |
| txpool\_user\_drop\_sum   | accumulated bytes of drop invalid-transactions  |
| txpool\_user\_remove\_cnt | accumulated number of remove valid-transactions |
| txpool\_user\_remove\_sum | accumulated bytes of remove valid-transactions  |

## Network traffic

Accumulated number and bytes of network packets

| Metric             | Description                           |
| ------------------ | ------------------------------------- |
| network\_recv\_cnt | accumulated number of receive packets |
| network\_recv\_sum | accumulated bytes of receive packets  |
| network\_send\_cnt | accumulated number of send packets    |
| network\_send\_sum | accumulated bytes of send packets     |
