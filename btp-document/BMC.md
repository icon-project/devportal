# BTP Message Center (BMC):

This section provides an interface of a general BMC in which it could be deployed to other blockchains that supports Ethereum Virtual Machine (EVM). The BMC, written by Solidity language, consists of two contracts:

### BMCManagement

____
`BMCManagement` ([IBMCManagement](./interfaces/IBMCManagement.sol)) is a smart contract that holds important logic implementations to manage authorization, and information of supporting networks

#### **Interface**

<span style="color:gray">**setBMCPeriphery()**</span>

```solidity
function setBMCPeriphery(address _addr) external;
```

- Description:
  - Update an address of BMCPeriphery contract
  - Caller MUST have an ownership role
  - A new address of BMCPeriphery contract MUST be different from a current one (also not `address(0)`)
- Params:
  - _addr: address ( Address of a new BMCPeriphery contract )

<span style="color:gray">**addOwner()**</span>

```solidity
function addOwner(address _owner) external;
```

- Description:
  - Allow a current owner to add an additional owner
  - Caller MUST have an ownership role
- Params:
  - _owner: address ( Address of a new Owner )

<span style="color:gray">**removeOwner()**</span>

```solidity
function removeOwner(address _owner) external;
```

- Description:
  - Allow a current owner to remove an existing owner
  - Caller MUST have an ownership role
  - Unable to remove the last Owner
- Params:
  - _owner: address ( Address of an Owner to be removed )  

<span style="color:gray">**isOwner()**</span>

```solidity
function isOwner(address _owner) external view returns (bool);
```

- Description:
  - Checking whether one specific address has an ownership role
  - Caller can be ANY
- Params:
  - _owner: address ( Address needs to be verified )
- Returns:
  - `true` or `false`: bool 

<span style="color:gray">**addService()**</span>

```solidity
function addService(string memory _svc, address _addr) external;
```

- Description:
  - Register a BSH smart contract for one service
  - Caller MUST have an ownership role
- Params:
  - _svc: string ( Name of a service )
  - _addr: address ( Address of a BSH service contract)

<span style="color:gray">**removeService()**</span>

```solidity
function removeService(string calldata _svc) external;
```

- Description:
  - Unregister a BSH smart contract of one service
  - Caller MUST have an ownership role
- Params:
  - _svc: string ( Name of a service )

<span style="color:gray">**addVerifier()**</span>

```solidity
function addVerifier(string calldata _net, address _addr) external;
```

- Description:
  - Register BMV contract for one network
  - Caller MUST have an ownership role
- Params:
  - _net: string ( Network Address of a connecting blockchain )
  - _addr: address ( Address of BMV contract )

<span style="color:gray">**removeVerifier()**</span>

```solidity
function removeVerifier(string calldata _net) external;
```

- Description:
  - Unregister BMV contract for one network
  - Caller MUST have an ownership role
- Params:
  - _net: string ( Network Address of a connecting blockchain )

<span style="color:gray">**addLink()**</span>

```solidity
function addLink(string calldata _link) external;
```

- Description:
  - Initializes status information of one link
  - Caller MUST have an ownership role
- Params:
  - _link: string ( BTP Address of a connecting BMC )

<span style="color:gray">**setLink()**</span>

```solidity
function setLink(
    string calldata _link,
    uint256 _blockInterval,
    uint256 _maxAggregation,
    uint256 _delayLimit
) external;
```

- Description:
  - Set/Update status information of one link
  - Caller MUST have an ownership role
- Params:
  - _link: string ( BTP Address of a connected BMC )
  - _blockInterval: uint256 ( Block interval of a connected link )
  - _maxAggregation: uint256 ( Max aggreation of a connected link )
  - _delayLimit: uint256 ( Delay limit of a connected link )

<span style="color:gray">**removeLink()**</span>

```solidity
function removeLink(string calldata _link) external;
```

- Description:
  - Remove one link and its status information
  - Caller MUST have an ownership role
- Params:
  - _link: string ( BTP Address of a connected BMC )

<span style="color:gray">**addRoute()**</span>

```solidity
function addRoute(string calldata _dst, string calldata _link) external;
```

- Description:
  - Add route to a BMC contract
  - Caller MUST have an ownership role
- Params:
  - _dst: string ( BTP Address of a destination BMC )
  - _link: string ( BTP Address of a next BMC for this destination )

<span style="color:gray">**removeRoute()**</span>

```solidity
function removeRoute(string calldata _dst) external;
```

- Description:
  - Remove route to a BMC contract
  - Caller MUST have an ownership role
- Params:
  - _dst: string ( BTP Address of a destination BMC )

<span style="color:gray">**addRelay()**</span>

```solidity
function addRelay(string calldata _link, address[] memory _addrs) external;
```

- Description:
  - Register/Update `Relays` for a network
  - Caller MUST have an ownership role
- Params:
  - _link: string ( BTP Address of a connected BMC )
  - _addrs: address[] ( An array of addresses of `Relays` )

<span style="color:gray">**removeRelay()**</span>

```solidity
function removeRelay(string calldata _link, address _addr) external;
```

- Description:
  - Unregister `Relay` for a network
  - Caller MUST have an ownership role
  - This method should be used to remove one `Relay` out from a current list
  - In case of removing multiple `Relays`, `addRelay()` method is a better choice
- Params:
  - _link: string ( BTP Address of a connected BMC )
  - _addr: address ( Address of `Relays` to remove )

<span style="color:gray">**getServices()**</span>

```solidity
struct Service {
    string svc;
    address addr;
}

function getServices() external view returns (Service[] memory _servicers);
```

- Description:
  - Query registered services (BSH)
  - Caller can be ANY
- Params:
  - None
- Returns:
  - _servicers: Service[] ( An array of Service )

<span style="color:gray">**getVerifiers()**</span>

```solidity
struct Verifier {
    string net;
    address addr;
}

function getVerifiers() external view returns (Verifier[] memory _verifiers);
```

- Description:
  - Query registered Verifiers (BMV)
  - Caller can be ANY
- Params:
  - None
- Returns:
  - _verifiers: Verifier[] ( An array of Verifier )

<span style="color:gray">**getLinks()**</span>

```solidity
function getLinks() external view returns (string[] memory _links);
```

- Description:
  - Query registered links (connected BMCs)
  - Caller can be ANY
- Params:
  - None
- Returns:
  - _links: string[] ( BTP Addresses of connected BMCs )

<span style="color:gray">**getRoutes()**</span>

```solidity
struct Route {
    string dst;     
    string next;
}
function getRoutes() external view returns (Route[] memory _routes);
```

- Description:
  - Query routing information
  - Caller can be ANY
- Params:
  - None
- Returns:
  - _routes: Route[] ( An array of Route )

<span style="color:gray">**getRelays()**</span>

```solidity
function getRelays(string calldata _link) external view returns (address[] memory _relayes);
```

- Description:
  - Query registered `Relays` of one link
  - Caller can be ANY
- Params:
  - _link: string ( BTP Address of a connected BMC )
- Returns:
  - _relayes: address[] ( A list of `Relays` )

<span style="color:gray">**getBshServiceByName()**</span>

```solidity
function getBshServiceByName(string memory _serviceName) external view returns (address);
```

- Description:
  - Query BSH service by name
  - Only called by BMCPeriphery contract
- Params:
  - _serviceName: string ( BSH service name )
- Returns:
  - _bsh: address ( Address of BSH service )

<span style="color:gray">**getBmvServiceByNet()**</span>

```solidity
function getBmvServiceByNet(string memory _net) external view returns (address);
```

- Description:
  - Query BMV service by net
  - Only called by BMCPeriphery contract
- Params:
  - _net: string ( A network address of a connected chain )
- Returns:
  - _bsh: address ( Address of BMV contract )

<span style="color:gray">**getPendingRequest()**</span>

```solidity
struct Request {
    string serviceName;
    address bsh;
}
function getPendingRequest() external view returns (Request[] memory);
```

- Description:
  - Query all pending service requests
  - Only called by BMCPeriphery contract
- Params:
  - None
- Returns:
  - _requests: Request[] ( List of all pending requests )

<span style="color:gray">**getLink()**</span>

```solidity
struct Link {
    address[] relays; //  Address of multiple Relays handle for this link network
    string[] reachable; //  A BTP Address of the next BMC that can be reach using this link
    uint256 rxSeq;
    uint256 txSeq;
    uint256 blockIntervalSrc;
    uint256 blockIntervalDst;
    uint256 maxAggregation;
    uint256 delayLimit;
    uint256 relayIdx;
    uint256 rotateHeight;
    uint256 rxHeight;
    uint256 rxHeightSrc;
    bool isConnected;
}

function getLink(string memory _to) external view returns (Link memory);
```

- Description:
  - Query link information
  - Only called by BMCPeriphery contract
- Params:
  - _to: string ( BTP Address of a link )
- Returns:
  - _info: Link ( Information of a link )

<span style="color:gray">**getLinkRxSeq()**</span>

```solidity
function getLinkRxSeq(string calldata _prev) external view returns (uint256);
```

- Description:
  - Query rotation sequence by link
  - Only called by BMCPeriphery contract
- Params:
  - _prev: string ( BTP address of a previous BMC )
- Returns:
  - _seq: uint256 ( Rotation sequence )

<span style="color:gray">**getLinkTxSeq()**</span>

```solidity
function getLinkTxSeq(string calldata _prev) external view returns (uint256);
```

- Description:
  - Query transaction sequence by link
  - Only called by BMCPeriphery contract
- Params:
  - _prev: string ( BTP address of a previous BMC )
- Returns:
  - _seq: uint256 ( Transaction sequence )

<span style="color:gray">**getLinkRelays()**</span>

```solidity
function getLinkRelays(string calldata _prev) external view returns (address[] memory);
```

- Description:
  - Query `Relays` by link
  - Only called by BMCPeriphery contract
- Params:
  - _prev: string ( BTP address of a previous BMC )
- Returns:
  - _relays: address[] ( A list of `Relays` )

<span style="color:gray">**getRelayStatusByLink()**</span>

```solidity
struct RelayStats {
    address addr;
    uint256 blockCount;
    uint256 msgCount;
}

function getRelayStatusByLink(string memory _prev) external view returns (RelayStats[] memory);
```

- Description:
  - Query status of `Relays` by link
  - Only called by BMCPeriphery contract
- Params:
  - _prev: string ( BTP address of a previous BMC )
- Returns:
  - _stats: RelayStats[] ( status of all `Relays` )

<span style="color:gray">**updatePendingReq()**</span>

```solidity
function updatePendingReq(Request memory _req) external;
```

- Description:
  - Update a pending request
  - Only called by BMCPeriphery contract
- Params:
  - _req: Request ( A service request )

<span style="color:gray">**updateLinkRxSeq()**</span>

```solidity
function updateLinkRxSeq(string calldata _prev, uint256 _val) external;
```

- Description:
  - Update rotation sequence by link
  - Only called by BMCPeriphery contract
- Params:
  - _prev: string ( BTP address of a previous BMC )
  - _val: Request ( Increment value )

<span style="color:gray">**updateLinkTxSeq()**</span>

```solidity
function updateLinkTxSeq(string calldata _prev) external;
```

- Description:
  - Increase transaction sequence by 1
  - Only called by BMCPeriphery contract
- Params:
  - _prev: string ( BTP address of a previous BMC )

<span style="color:gray">**updateLinkReachable()**</span>

```solidity
function updateLinkReachable(string memory _prev, string memory _to) external;
```

- Description:
  - Add a reachable BTP address to link
  - Only called by BMCPeriphery contract
- Params:
  - _prev: string ( BTP address of a previous BMC )
  - _to: string ( BTP address of a BMC can be reachable from `_prev` )

<span style="color:gray">**deleteLinkReachable()**</span>

```solidity
function deleteLinkReachable(string memory _prev, uint256 _index) external;
```

- Description:
  - Remove a reachable BTP address
  - Only called by BMCPeriphery contract
- Params:
  - _prev: string ( BTP address of a previous BMC )
  - _index: uint256 ( An index of reachable BTP address in a list to be removed )

<span style="color:gray">**updateRelayStats()**</span>

```solidity
function updateRelayStats(
    address _relay,
    uint256 _blockCountVal,
    uint256 _msgCountVal
) external;
```

- Description:
  - Update relay status
  - Only called by BMCPeriphery contract
- Params:
  - _relay: address ( Address of Relay )
  - _blockCountVal: uint256 ( Increment value for block counter )
  - _msgCountVal: uint256 ( Increment value for message counter )

<span style="color:gray">**resolveRoute()**</span>

```solidity
function resolveRoute(string memory _dstNet) external view returns (string memory, string memory);
```

- Description:
  - Find a next BMC
  - Only called by BMCPeriphery contract
- Params:
  - _dstNet: string ( A network address of a destination chain )
- Returns:
  - _next: string ( BTP Address of a next BMC )
  - _dst: string ( BTP Address of a destination BMC )

<span style="color:gray">**rotateRelay()**</span>

```solidity
function rotateRelay(
    string memory _link,
    uint256 _currentHeight,
    uint256 _relayMsgHeight,
    bool _hasMsg
) external returns (address);
```

- Description:
  - Determine a Relay that takes a responsibility to submit Relay message
  - Only called by BMCPeriphery contract
- Params:
  - _link: string ( BTP address of a connected BMC )
  - _currentHeight: uint256 ( Current block height of MTA from BMV contract )
  - _relayMsgHeight: uint256 ( Block height of the last BTP Message )
  - _hasMsg: bool ( check if Relay message exists - `true`/`false`)
- Returns:
  - _relay: address ( Address of Relay )

### BMCPeriphery

____
`BMCPeriphery` ([IMCPeriphery](./interfaces/IBMCPeriphery.sol)) is a smart contract that handles communications among `BSHPeriphery`, `BMV` contracts, and `Relays`

#### **Interface**

<span style="color:gray">**getBmcBtpAddress()**</span>

```solidity
function getBmcBtpAddress() external view returns (string memory);
```

- Description:
  - Get BTP address of a BMCPeriphery contract
  - Caller can be ANY
- Params:
  - None
- Returns:
  - _btpAddr: string ( BTP address of a BMCPeriphery contract )

<span style="color:gray">**handleRelayMessage()**</span>

```solidity
function handleRelayMessage(string calldata _prev, string calldata _msg) external;
```

- Description:
  - Verify and decode RelayMessage with BMV
  - Dispatch BTP Messages to registered BSHs
  - Caller MUST be a registered Relay
- Params:
  - _prev: string ( BTP Address of a BMC generating a message )
  - _msg: address ( Base64 encoded string of serialized bytes of Relay Message )

<span style="color:gray">**sendMessage()**</span>

```solidity
function sendMessage(
    string calldata _to,
    string calldata _svc,
    uint256 _sn,
    bytes calldata _msg
) external;
```

- Description:
  - Send a Service Message to a specific network
  - Caller MUST be a registered BSH or BMCManagement
- Params:
  - _to: string ( Network Address of a destination network )
  - _svc: string ( Name of a service )
  - _sn: uint256 ( Serial number of a Service Message )
  - _msg: bytes ( Serialized bytes of a Service Message )

<span style="color:gray">**getStatus()**</span>

```solidity
struct LinkStats {
    uint256 rxSeq;
    uint256 txSeq;
    VerifierStats verifier;
    RelayStats[] relays;
    uint256 relayIdx;
    uint256 rotateHeight;
    uint256 rotateTerm;
    uint256 delayLimit;
    uint256 maxAggregation;
    uint256 rxHeightSrc;
    uint256 rxHeight;
    uint256 blockIntervalSrc;
    uint256 blockIntervalDst;
    uint256 currentHeight;
}

function getStatus(string calldata _link) external view returns (LinkStats memory _linkStats);
```

- Description:
  - Get status of BMC
  - Caller can be ANY
- Params:
  - _link: string ( BTP Address of a connected BMC )
- Returns:
  - _linkStats: LinkStats ( Information status of a link )
