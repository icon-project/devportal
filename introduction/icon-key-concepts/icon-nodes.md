# ICON Nodes

Nodes are the essential entities of ICON network. They have roles to produce and validate the blocks, which contains transactions transmitted into ICON network. All transactions sent to ICON network are relayed or directed to nodes. 

There are two types of nodes in ICON network.

### Public Representative \(P-Rep\)

Public Representatives are the consensus nodes that produce, verify blocks and participate in network policy decisions on the ICON Network. P-Rep nodes are ordered by the amount of vote their receive.

All P-Rep nodes receives a representative reward based on the amount of vote their receive.

#### Main P-Rep

The top 22 P-Reps are considered as Main P-Rep and can produce block turn by turn, and receive block reward. One of P-Rep nodes becomes a leader node, who has the right to propose a block on its turn. And the other P-Rep nodes validate the proposed block. The block is confirmed when 2/3 of P-Rep nodes agree on that block. P-Rep nodes will become a leader node in a pre-defined order, and produce 10 blocks on their turn.

#### Sub P-Rep

The other 78 P-Reps are considered as Sub P-Rep and only receive the representative reward. They also verify blocks.

### Citizen

Citizen nodes synchronize the blockchain data from Peer nodes. Citizen is not just a simple data replication store but verifies every block data by executing the transactions in the block. Therefore, we can trust the sanity of the block data downloaded from the Peer nodes. In addition, because Citizen does not participate in block generation, when it receives the transaction requests, Citizen relays the transaction requests to the Peer nodes. With the above two main functions, Citizen is typically deployed as a service end-point of ICON Network. Citizen answers the queries from the users and relays transactions to the Peer nodes. It is designed that no transactions and queries are supposed to be sent to the Peer nodes directly. This architecture keeps the Peer nodes focusing on consensus, that is, producing and validating blocks. In addition, limited access to Peer nodes makes the ICON network safer from attacks. Since Citizen nodes verify the blocks and transactions, Exchanges or DApp operators are recommended to setup own Citizen nodes inside their network, rather than use publicly opened Citizen nodes outside their network.