# Types of Proposals

### Type of proposals (schema of a proposal)

For the section that allows validators to submit network proposals the following types of forms must be created.

*   Proposal of type "text"



    | Key        | Value Type                                                                                               | Description          | Required field? |
    | ---------- | -------------------------------------------------------------------------------------------------------- | -------------------- | --------------- |
    | name       | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR) | "text" (fixed value) | Y               |
    | value      | T\_DICT                                                                                                  |                      | Y               |
    | value.text | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR) | Text value           | Y               |



    ```json
    {
      "name": "text",
      "value": {
        "text": "This is a text proposal"
      }
    }
    ```



*   Proposal of type "revision"



    | Key            | Value Type                                                                                               | Description              | Required field? |
    | -------------- | -------------------------------------------------------------------------------------------------------- | ------------------------ | --------------- |
    | name           | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR) | "revision" (fixed value) | Y               |
    | value          | T\_DICT                                                                                                  |                          | Y               |
    | value.revision | [https://github.com/icon-project/governance2#T\_INT](https://github.com/icon-project/governance2#T\_INT) | Revision code            | Y               |



    ```json
    {
      "name": "revision",
      "value": {
        "revision": "0x11"
      }
    }
    ```



*   Proposal of type "maliciousScore"



    | Key           | Value Type                                                                                                               | Description                    | Required field? |
    | ------------- | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------ | --------------- |
    | name          | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR)                 | "maliciousScore" (fixed value) | Y               |
    | value         | T\_DICT                                                                                                                  |                                | Y               |
    | value.address | [https://github.com/icon-project/governance2#T\_ADDR\_SCORE](https://github.com/icon-project/governance2#T\_ADDR\_SCORE) | SCORE address                  | Y               |
    | value.type    | [https://github.com/icon-project/governance2#T\_INT](https://github.com/icon-project/governance2#T\_INT)                 | 0x0: Freeze, 0x1: Unfreeze     | Y               |



    ```json
    {
      "name": "maliciousScore",
      "value": {
        "address": "cx7cc546bf908018b5602b66fa65ff5fdacef45fe0",
        "type": "0x0"
      }
    }
    ```



*   Proposal of type "prepDisqualification"



    | Key           | Value Type                                                                                                           | Description                          | Required field? |
    | ------------- | -------------------------------------------------------------------------------------------------------------------- | ------------------------------------ | --------------- |
    | name          | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR)             | "prepDisqualification" (fixed value) | Y               |
    | value         | T\_DICT                                                                                                              |                                      | Y               |
    | value.address | [https://github.com/icon-project/governance2#T\_ADDR\_EOA](https://github.com/icon-project/governance2#T\_ADDR\_EOA) | EOA address of main/sub P-Rep        | Y               |



    ```json
    {
      "name": "prepDisqualification",
      "value": {
        "address": "hx7cc546bf908018b5602b66fa65ff5fdacef45fe0"
      }
    }
    ```



*   Proposal of type "stepPrice"



    | Key             | Value Type                                                                                               | Description                          | Required field? |
    | --------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------ | --------------- |
    | name            | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR) | "stepPrice" (fixed value)            | Y               |
    | value           | T\_DICT                                                                                                  |                                      | Y               |
    | value.stepPrice | [https://github.com/icon-project/governance2#T\_INT](https://github.com/icon-project/governance2#T\_INT) | An integer of the step price in loop | Y               |



    ```json
    {
      "name": "stepPrice",
      "value": {
        "stepPrice": "0x2e90edd00"
      }
    }
    ```



*   Proposal of type "stepCosts"



    | name                       | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR)   | "stepCosts" (fixed value)                                                                        | Required field? |
    | -------------------------- | ---------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | --------------- |
    | value                      | T\_DICT                                                                                                    |                                                                                                  | Y               |
    | value.costs                | [https://github.com/icon-project/governance2#T\_DICT](https://github.com/icon-project/governance2#T\_DICT) | Step costs to set as a dict. All fields are optional but at least one field should be specified. | Y               |
    | value.costs.schema         | T\_INT                                                                                                     | Schema version (currently fixed at 1)                                                            | N               |
    | value.costs.default        | T\_INT                                                                                                     | Default cost charged each time transaction is executed                                           | N               |
    | value.costs.contractCall   | T\_INT                                                                                                     | Cost to call the smart contract function                                                         | N               |
    | value.costs.contractCreate | T\_INT                                                                                                     | Cost to call the smart contract code generation function                                         | N               |
    | value.costs.contractUpdate | T\_INT                                                                                                     | Cost to call the smart contract code update function                                             | N               |
    | value.costs.contractSet    | T\_INT                                                                                                     | Cost to store the generated/updated smart contract code per byte                                 | N               |
    | value.costs.get            | T\_INT                                                                                                     | Cost to get values from the state database per byte                                              | N               |
    | value.costs.getBase        | T\_INT                                                                                                     | Default cost charged each time get is called                                                     | N               |
    | value.costs.set            | T\_INT                                                                                                     | Cost to set values newly in the state database per byte                                          | N               |
    | value.costs.setBase        | T\_INT                                                                                                     | Default cost charged each time set is called                                                     | N               |
    | value.costs.delete         | T\_INT                                                                                                     | Cost to delete values in the state database per byte                                             | N               |
    | value.costs.deleteBase     | T\_INT                                                                                                     | Default cost charged each time delete is called                                                  | N               |
    | value.costs.input          | T\_INT                                                                                                     | Cost charged for input data included in transaction per byte                                     | N               |
    | value.costs.log            | T\_INT                                                                                                     | Cost to emit event logs per byte                                                                 | N               |
    | value.costs.logBase        | T\_INT                                                                                                     | Default cost charged each time log is called                                                     | N               |
    | value.costs.apiCall        | T\_INT                                                                                                     | Cost charged for heavy API calls (e.g. hash functions)                                           | N               |



    ```json
    {
      "name": "stepCosts",
      "value": {
        "costs": {"default": "0x186a0", "set": "0x140"}
      }
    }
    ```



*   Proposal of type "rewardFund"



    | Key           | Value Type                                                                                               | Description                                     | Required field? |
    | ------------- | -------------------------------------------------------------------------------------------------------- | ----------------------------------------------- | --------------- |
    | name          | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR) | "rewardFund" (fixed value)                      | Y               |
    | value         | T\_DICT                                                                                                  |                                                 | Y               |
    | value.iglobal | [https://github.com/icon-project/governance2#T\_INT](https://github.com/icon-project/governance2#T\_INT) | The total amount of monthly reward fund in loop | Y               |



    ```json
    {
        "name": "rewardFund",
        "value": {
            "iglobal": "0x27b46536c66c8e3000000"
        }
    }
    ```



*   Proposal of type "rewardFundsAllocation"



    | Key                      | Value Type                                                                                                 | Description                                                            | Required field? |
    | ------------------------ | ---------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- | --------------- |
    | name                     | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR)   | "rewardFundsAllocation" (fixed value)                                  | Y               |
    | value                    | T\_DICT                                                                                                    |                                                                        | Y               |
    | value.rewardFunds        | [https://github.com/icon-project/governance2#T\_DICT](https://github.com/icon-project/governance2#T\_DICT) | Reward fund values information to set. All values are required.        | Y               |
    | value.rewardFunds.iprep  | T\_INT                                                                                                     | The percentage allocated to the P-Rep from the monthly reward fund     | Y               |
    | value.rewardFunds.icps   | T\_INT                                                                                                     | The percentage allocated to the CPS from the monthly reward fund       | Y               |
    | value.rewardFunds.irelay | T\_INT                                                                                                     | The percentage allocated to the BTP relay from the monthly reward fund | Y               |
    | value.rewardFunds.ivoter | T\_INT                                                                                                     | The percentage allocated to the Voter from the monthly reward fund     | Y               |



    ```json
    {
      "name": "rewardFundsAllocation",
      "value": {
        "rewardFunds": {
          "iprep": "0x10",
          "icps": "0xa",
          "irelay": "0xa",
          "ivoter": "0x40"
        }
      }
    }
    ```



*   Proposal of type "networkScoreDesignation"



    | Key                         | Value Type                                                                                                               | Description                                                                               | Required field? |
    | --------------------------- | ------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------- | --------------- |
    | name                        | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR)                 | "networkScoreDesignation" (fixed value)                                                   | Y               |
    | value                       | T\_DICT                                                                                                                  |                                                                                           | Y               |
    | value.networkScores         | T\_LIST\[T\_DICT]                                                                                                        | network SCORE values to set. If the address is an empty string, deallocate network SCORE. | Y               |
    | value.networkScores.role    | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR)                 | "cps" or "relay"                                                                          | Y               |
    | value.networkScores.address | [https://github.com/icon-project/governance2#T\_ADDR\_SCORE](https://github.com/icon-project/governance2#T\_ADDR\_SCORE) | network SCORE address                                                                     | Y               |



    ```json
    {
      "name": "networkScoreDesignation",
      "value": {
        "networkScores": [
          {
            "role": "cps",
            "address": "cx7cc546bf908018b5602b66fa65ff5fdacef45fe0"
          }
        ]
      }
    }
    ```



*   Proposal of type "networkScoreUpdate"



    | Key           | Value Type                                                                                                               | Description                          | Required field? |
    | ------------- | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------ | --------------- |
    | name          | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR)                 | "networkScoreUpdate" (fixed value)   | Y               |
    | value         | T\_DICT                                                                                                                  |                                      | Y               |
    | value.address | [https://github.com/icon-project/governance2#T\_ADDR\_SCORE](https://github.com/icon-project/governance2#T\_ADDR\_SCORE) | network SCORE address to update      | Y               |
    | value.content | [https://github.com/icon-project/governance2#T\_BIN\_DATA](https://github.com/icon-project/governance2#T\_BIN\_DATA)     | SCORE code in hexadecimal string     | Y               |
    | value.params  | [https://github.com/icon-project/governance2#T\_LIST](https://github.com/icon-project/governance2#T\_LIST)               | Parameters passed to score on update | N               |



    ```json
    {
      "name": "networkScoreUpdate",
      "value": {
        "address": "cx7cc546bf908018b5602b66fa65ff5fdacef45fe0",
        "content": "0x504b0304107082bc2bf352a000000280...00000504b03041400080808000000210000000",
        "params": ["0x10", "Hello"]
      }
    }
    ```



*   Proposal of type "accumulatedValidationFailureSlashingRate"



    | Key                | Value Type                                                                                               | Description                                              | Required field? |
    | ------------------ | -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- | --------------- |
    | name               | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR) | "accumulatedValidationFailureSlashingRate" (fixed value) | Y               |
    | value              | T\_DICT                                                                                                  |                                                          | Y               |
    | value.slashingRate | [https://github.com/icon-project/governance2#T\_INT](https://github.com/icon-project/governance2#T\_INT) | slashing rate \[0 \~ 100]                                | Y               |



    ```json
    {
      "name": "accumulatedValidationFailureSlashingRate",
      "value": {
        "slashingRate": "0x5"
      }
    }
    ```



*   Proposal of type "missedNetworkProposalVoteSlashingRate"



    | Key                | Value Type                                                                                               | Description                                           | Required field? |
    | ------------------ | -------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- | --------------- |
    | name               | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR) | "missedNetworkProposalVoteSlashingRate" (fixed value) | Y               |
    | value              | T\_DICT                                                                                                  |                                                       | Y               |
    | value.slashingRate | [https://github.com/icon-project/governance2#T\_INT](https://github.com/icon-project/governance2#T\_INT) | slashing rate \[0 \~ 100]                             | Y               |



    ```json
    {
      "name": "missedNetworkProposalVoteSlashingRate",
      "value": {
        "slashingRate": "0x5"
      }
    }
    ```



*   Proposal of type "call"

    | Key                 | Value Type                                                                                                                     | Description                                                                                                                   | Required field? |
    | ------------------- | ------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------- | --------------- |
    | name                | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR)                       | "customCall" (fixed value)                                                                                                    | Y               |
    | value               | T\_DICT                                                                                                                        |                                                                                                                               | Y               |
    | value.to            | [https://github.com/icon-project/governance2#T\_ADDRESS\_SCORE](https://github.com/icon-project/governance2#T\_ADDRESS\_SCORE) | SCORE address                                                                                                                 | Y               |
    | value.method        | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR)                       | method name                                                                                                                   | Y               |
    | value.params        | [https://github.com/icon-project/governance2#T\_LIST](https://github.com/icon-project/governance2#T\_LIST)                     | parameters                                                                                                                    | ?               |
    | value.params.type   | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR)                       | type of parameter                                                                                                             | ?               |
    | value.params.value  | [https://github.com/icon-project/governance2#T\_STR](https://github.com/icon-project/governance2#T\_STR)                       | value                                                                                                                         | ?               |
    | value.params.fields | T\_DICT\[T\_STR]                                                                                                               | empty if parameter type is not struct or \[]struct. It has the keys of the struct as keys, and the types of values as values. | ?               |



    ```json
    {
      "name": "call",
      "value": {
        "to": "cx0000000000000000000000000000000000000000",
        "method": "someMethod",
        "params": [
          {
            "type": "str",
            "value": "Alice"
          },
          {
            "type": "struct",
            "value": {
              "nickName": "Bob",
              "address": "hxb6b5791be0b5ef67063b3c10b840fb81514db2fd"
            },
            "fields": {
              "nickName": "str",
              "address": "Address"
            }
          }
        ]
      }
    }
    ```

### Resources:

* [https://github.com/icon-project/governance2](https://github.com/icon-project/governance2#registerproposal)
