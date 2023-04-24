# Registering and voting network proposals

### Registering Proposal in the Network

Registering a network proposal requires signing a transaction calling the `registerProposal` method of the governance contract. The ABI of the `registerProposal` method is as follow:

```json
{ 
     "inputs": [
       { "name": "title", "type": "str" },
       { "name": "description", "type": "str" },
       { "name": "value", "type": "bytes" }
     ],
     "name": "registerProposal", 
     "outputs": [],
     "payable": "0x1",
     "type": "function"
}
```

The `registerProposal` method has 3 defined inputs:

* title: this input receives a `string` and defines the title of the proposal.
* description: this input recieves a `string` and defines a description for the proposal.
* value: this input receives a value of type `byte` and it is the stringified JSON object of your type of proposal converted into a hex hash.

Both `title` and `description` input are values of type `string`, for the case of the `value` input depending on your type of proposal (refer to the previous section [Types of Proposals](types-of-proposals.md)) you would need to create the correct hex hash depending on the proposal type.\
\
For example a text proposal with the following structure:

```json
{
  "name": "text",
  "value": {
    "text": "This is a text proposal"
  }
}
```

After converting into a hex hash the resulting value would be:

```
0x5b7b226e616d65223a2274657874222c2276616c7565223a7b2274657874223a2254686973206973206120746578742070726f706f73616c227d7d5d
```

The signed RPC call for registering this sample text proposal would be the following:

```jsdoc
{
  to: 'cx0000000000000000000000000000000000000001', // governance contract
  from: 'hx1223f323..', // Validator wallet registering the proposal
  nid: '0x2',  // network identifier
  version: '0x3',
  timestamp: '0x5f9b6322ab100',
  stepLimit: '0x7a1200',
  value: '0x56bc75e2d63100000', // cost for submitting proposal (100 icx)
  nonce: '0x1',
  dataType: 'call',
  data: {
    method: 'registerProposal', // method name
    params: {
      title: 'Proposal title',
      description: 'Proposal description',
      value: '0x5b7b226e616d65223a2274657874222c2276616c7565223a7b2274657874223a2254686973206973206120746578742070726f706f73616c227d7d5d'
    }
  },
  signature: '/moH05QRHTofZKACOG25Wn18Ep7Xm3TCkZLgL5Lv4B1D1+tRzkiEUTCOyoucxMBGac7+pgadPjmULICMWtV8tAA='
}
```

### Fetching Proposals from the Network

To fetch a proposal from the chain we need the id of the proposal (this is the transaction hash generated when the proposal was registered) and we call the `getProposal` method of the governance contract using this id as an input.

The following is an example of a call with the `getProposal` method

```bash
curl -X POST -H 'Content-Type' --data '{"jsonrpc":"2.0","method":"icx_call","id":365,"params":{"to":"cx0000000000000000000000000000000000000001","dataType":"call","data":{"method":"getProposal","params": {"id": "0x08a4eccb51132579d992752c477a677e60e9e25fec3bbf3dbc2cacff42499efd"}}}}' https://api.espanicon.team/api/v3
```

response

```json
{                                                                               
  "jsonrpc": "2.0",                                                             
  "result": {                                                                   
    "apply": {                                                                  
      "address": "hxfba37e91ccc13ec1dab115811f73e429cde44d48",                  
      "id": "0x351ed564f4bed35ad9656f224c1eb2252635cecb7865c225bb5391da897f615c"
,                                                                               
      "name": "Lydia Labs",                                                     
      "timestamp": "0x5eda71f4287ea"                                            
    },                                                                          
    "contents": {                                                               
      "description": "CPS Treasury SCORE network update to support new features"
,                                                                               
      "title": "CPS Treasury SCORE network update",                             
      "type": "0x9",                                                            
                                                                                
      "value": {                                                                
        "data": "[{\"name\":\"networkScoreUpdate\",\"value\":{\"address\":\"cxdca1178010b5368aea929ad5c06abee64b91acc2\",\"content\":\"0x000232...223\"}}]"     
      }                                                                         
    },                                                                          
    "endBlockHeight": "0x377ca12",                                              
    "id": "0x08a4eccb51132579d992752c477a677e60e9e25fec3bbf3dbc2cacff42499efd", 
    "proposer": "hxfba37e91ccc13ec1dab115811f73e429cde44d48",                   
    "proposerName": "Lydia Labs",                                               
    "startBlockHeight": "0x37497de",                                            
    "status": "0x1",                                                            
    "vote": {                                                                   
      "agree": {
        "amount": "0xb4c744f99775c3b654fb04",
        "list": [
          {
            "address": "hxfba37e91ccc13ec1dab115811f73e429cde44d48",
            "amount": "0x13219c178e102236bc0000",
            "id": "0x43c6db10526eca375e688b9e644b11547734d32dd56fe2839d6a72770f35ca72",
            "name": "Lydia Labs",
            "timestamp": "0x5ed7be9e09aa0"
          },
          ...
        ]
      },
      "disagree": { "amount": "0x0", "list": [] },
      "noVote": {
        "amount": "0x3972c0b5127f554c01dd76",
        "list": [
          "hxf680ae11372c48afebbce77c9d8de455e1acc18a",
          ...
        ]
      }
    }
  },
  "id": 365
}
```

### Voting on Network Proposals

To cast a vote for a network proposal as a validator we need to sign a transaction using the validator wallet calling the `voteProposal` method of the governance contract using the id of the proposal as a value in the `id` input and either `0x1` or `0x0` (to approve or reject) as the values for the `vote` input.

The following is the RPC call signing to approve a Network Proposal:

```jsdoc
{
  to: 'cx0000000000000000000000000000000000000001', // governance contract
  from: 'hxe9d...', // validator wallet voting on the proposal
  nid: '0x2',
  version: '0x3',
  timestamp: '0x5f9b6631c9940',
  stepLimit: '0x7a1200',
  nonce: '0x1',
  dataType: 'call',
  data: {
    method: 'voteProposal',
    params: {
      id: '0xa46f6bb8...', // proposal id
      vote: '0x1' // vote to approve
    }
  },
  signature: 'NYey6haxr/KM9RLvfz3g6UC/kawjTz0kc2DwG0n5mQIK18usz7xaR55LcGmfirmPZgPkY+ZbkACbjE3m13eqTwE='
}
```

### Resources:

* [https://github.com/icon-project/governance2#registerproposal](https://github.com/icon-project/governance2#registerproposal)
