# BTP Message Verifier (BMV):

This section provides an interface of a general BMV in which it could be deployed to other blockchains that supports Ethereum Virtual Machine (EVM). The BMV, written by Solidity language, consists of two contracts:

### BMV ([IBMV](./interfaces/IBMV.sol))

#### **Interface**

<span style="color:gray">**getMTA()**</span>

```solidity
function getMTA() external view returns (string memory);
```

- Description:
  - Get base64 encoding of Merkle Tree
  - Caller can be ANY
- Params:
  - None
- Returns:
  - _mta: string ( Base64 encode of Merkle Tree )

<span style="color:gray">**getMTA()**</span>

```solidity
function getConnectedBMC() external view returns (string memory);
```

- Description:
  - Get BTP address of BMCPeriphery contract
  - Caller can be ANY
- Params:
  - None
- Returns:
  - _bmc: address ( Address of linked BMCPeriphery )

<span style="color:gray">**getNetAddress()**</span>

```solidity
function getNetAddress() external view returns (string memory);
```

- Description:
  - Get a network address of a supporting blockchain
  - Caller can be ANY
- Params:
  - None
- Returns:
  - _net: string ( Network address of a blockchain )

<span style="color:gray">**getValidators()**</span>

```solidity
function getValidators() external view returns (bytes32, address[] memory);
```

- Description:
  - Get a network address of a supporting blockchain
  - Caller can be ANY
- Params:
  - None
- Returns:
  - _hash: bytes32 ( Hash of RLP encode from a given list of `Validators` )
  - _addrs: address[] ( A list of `Validators` )

<span style="color:gray">**getStatus()**</span>

```solidity
function getValidators() external view returns (bytes32, address[] memory);
```

- Description:
  - Used by Relay to resolve a next BTP Message to send
  - Caller MUST be BMCPeriphery contract
- Params:
  - None
- Returns:
  - _height: uint256 ( Height of Merkle Trie Accumulator )
  - _offset: uint256 ( Offset of Merkle Trie Accumulator )
  - _lastHeight: uint256 ( Block height of the last BTP Message )

<span style="color:gray">**handleRelayMessage()**</span>

```solidity
function handleRelayMessage(
    string memory _bmc,
    string memory _prev,
    uint256 _seq,
    string calldata _msg
) external returns (bytes[] memory);
```

- Description:
  - Decodes Relay Messages and processes BTP Messages
  - If there is an error, it sends a BTP Message containing an Error Message
  - BTP Messages with old sequence numbers are ignored
  - BTP Message contains future sequence number will likely fail
- Params:
  - _bmc: string ( BTP address of a BMC handling this message )
  - _prev: string ( BTP address of a previous BMC )
  - _seq: uint256 ( Next sequence number to get a message )
  - _msg: string ( Serialized of Relay Message )
- Returns:
  - _rlp: bytes[] ( List of serialized bytes of a BTP Message )

### DataValidator ([IDatavalidator](./interfaces/IDataValidator.sol))

#### **Interface**

<span style="color:gray">**handleRelayMessage()**</span>

```solidity
function handleRelayMessage(
    string memory _bmc,
    string memory _prev,
    uint256 _seq,
    string calldata _msg
) external returns (bytes[] memory);
```

- Description:
  - Validate receipt proofs and return btp messages
- Params:
  - _bmc: string ( BTP address of a BMC handling this message )
  - _prev: string ( BTP address of a previous BMC )
  - _seq: uint256 ( Next sequence number to get a message )
  - _serializedMsg: bytes ( Serialized bytes of Relay Message )
  - _receiptHash: bytes ( Receipt root hash of MPT )
- Returns:
  - _rlp: bytes[] ( List of serialized bytes of a BTP Message )
