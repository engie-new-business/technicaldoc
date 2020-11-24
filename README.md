# 🚀 Getting started

## Getting Started

As an example, let's call the method "hello" of a smart contract that simply stores "hello" messages.

The **helloRockside** contract is deployed at: [0x4FD167973185AD2a968339172A936641fD31F3CD](https://ropsten.etherscan.io/address/0x4fd167973185ad2a968339172a936641fd31f3cd#code) on Ropsten network.

We will call the **hello**  method with "**hello rockside**" as parameter.

The data that represent this call is:

```bash
DATA: 0xa777d0dc0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000e68656c6c6f20726f636b73696465000000000000000000000000000000000000
```

To generate the ABI encoded data, we used Web3 as follows:

```javascript
const Web3 = require('web3')
const web3 = new Web3()

const helloContractAddress = "0xFb428d37AcC708F37A40c8D95d723e1Aea49cc07"
const helloContractABI = [{"inputs":[{"internalType":"string","name":"helloMsg","type":"string"}],"name":"hello","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"uint256","name":"","type":"uint256"}],"name":"helloMessages","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"view","type":"function"}]

var helloContract =  new web3.eth.Contract(helloContractABI, helloContractAddress)
const data = helloContract.methods.hello("hello rockside").encodeABI();

console.log("DATA: "+data);
```

### Call Rockside Relay API

Pass in the URL the **address of the destination contract** \(in our case the helloRockside contract\), in the body specify the **speed** and the **data.**  For more infos, check our [Relay API](https://docs.rockside.io/relay).

```bash
curl --request POST 'https://api.rockside.io/ethereum/ropsten/relay/0x4FD167973185AD2a968339172A936641fD31F3CD' \
--header 'Content-Type: application/json' \
--data '{"speed": "fast","data": "0xa777d0dc0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000e68656c6c6f20726f636b73696465000000000000000000000000000000000000"}'

```

You will get:

```javascript
{
    "transaction_hash": "0x2968698cb90ec9d95f8656d28bb29593029c79bcd22b42dc6b9469cb03729e2a",
    "tracking_id": "01EA266P6PKN0ZY01P0Q6G7WR5"
}
```

### Follow your transaction status

You can follow your transaction on Etherscan with the `transaction_hash`. But because Rockside is susceptible to replay a stuck transaction, this hash value can changes.

To keep track of your transaction - whatever happens to it - use the `tracking_id` as follow:

```bash
curl --request GET 'https://api.rockside.io/ethereum/ropsten/transactions/TX_TRACKING_ID'
```

You will get:

```javascript
{
    "transaction_hash": "0x2968698cb90ec9d95f8656d28bb29593029c79bcd22b42dc6b9469cb03729e2a",
    "tracking_id": "01EA266P6PKN0ZY01P0Q6G7WR5",
    "from": "0xdd0f36e17474e8cbf9c4e483d02a1cf34f41550a",
    "to": "0x7bb7703f2c601a54b484add52a07afad9c9f495e",
    "data_length": 484,
    "value": 0,
    "gas": 87601,
    "gas_price": 19270,
    "chain_id": 3,
    "deadline": "2020-06-05T12:15:03.337Z",
    "receipt": {
        "status": 1,
        "cumulative_gas_used": 6332041,
        "logs": [],
        "transaction_hash": "0x2968698cb90ec9d95f8656d28bb29593029c79bcd22b42dc6b9469cb03729e2a",
        "contract_address": "0x0000000000000000000000000000000000000000",
        "gas_used": 86687,
        "block_hash": "0xd21a10cc344cb84faaab6725de8dedf51ae8deaacba62c6e0a570dc2578481f2",
        "block_number": 8035552,
        "transaction_index": 8
    },
    "created": "2020-06-05T12:10:03.337Z",
    "updated": "2020-06-05T12:11:23.632Z",
    "status": "success",
}
```

### Go further

This example run on Ropsten. For Mainnet you will have to implement in your contract a mechanism to refund Rockside for the gas fees.

An example of refund can be found in [Gnosis Safe contract](https://github.com/gnosis/safe-contracts/blob/development/contracts/GnosisSafe.sol), method `handlePayment` or in our [Forwarder contrac](https://github.com/rocksideio/contracts/blob/master/contracts/Forwarder.sol)t, method `Forward`.

