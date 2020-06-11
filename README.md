# 🚀Getting started

Rockside relayer is a non-custodial transaction delivery service. When sending a transaction to Rockside, you provide:

* the `gas price limit` which is the maximum price you want to apply to the transaction
* a chosen `speed` of inclusion in the blockchain

We make sure your transaction is then executed at the best price in respect to the delay.

## What problem are we solving?

Sometimes Ethereum transactions are stuck or lost. Often to solve this, developers overpay transactions and must remain on-call to unblock them.

Our solution is a new approach to deal with this situation.

## Transaction Relay overview

![Relay overview](https://raw.githubusercontent.com/rocksideio/technicaldoc/master/images/tx-relay-overview.png)

## How does it work?

Depending on your given gas price limit, your requested speed and the current market gas prices, we decide whether or not to relay your transaction.

Once accepted, we ensure your transaction is validated at the best price at your speed.

* **API and SDK**: Send your transactions using our API and open source SDK \(JS, iOS\)
* **Speed**: available speeds are `safelow` \(around 30 minutes\), `average` or `standard`\(around 5 minutes\), `fast` \(around 2 minutes\), `fastest` \(around 30 seconds\)
* **Gas price limit**: Maximum gas price value in wei, you are willing to apply to your transaction.
* **Meta-transactions**: To relay your transactions we use the concept of meta-transactions, wrapping your signed message in a new transaction. More  on [meta-transactions page](https://github.com/rocksideio/technicaldoc/tree/1976743e9a75dae477fe57fe11e634ee36992cc2/advanced/meta-transaction.md).
* **Gasless transaction:** Thanks to meta-transactions, Rockside allows you to pay gas fees for your DApp's users. They no longer need ethers to interact with your DApp.
* **Transaction auto replay**: We monitor your transaction and we replace it with one with a higher gas price when necessary to validate your transaction according to your requested speed. More on [transaction replay page](advanced/replay.md).
* **Pool of signers**: We manage a pool of signers with a [multi-dimensionnal replay protection](advanced/replay-protection.md) to guarantee no stuck transactions.
* **Transaction batch** You can group your transactions in a batch. You then have a atomic operation and your transactions will be executed on chain within a single transaction.
* **Tracking ID**: When sending a transaction to Rockside, we also provide you with its tracking ID. Use it to have the latest and automatically updated status of your transaction even in case of replays and other actions taken.

## Getting Started

### Connect to Rockside

Go to [dashboard.rockside.io](https://dashboard.rockside.io) and connect using your github account. You will get your API key that will allow you to access our API.

### Deploy your Forwarder

To relay your transaction, you need to deploy a Forwarder contract.

You must specify an owner account. Only the owner can transfer funds from the Forwarder contract.

By default forwarders only accept transactions coming from Rockside signers. Only the owner can modify the list of authorized senders.

To deploy a forwarder execute this request:

```bash
curl --request POST 'https://api.rockside.io/ethereum/ropsten/forwarders' \
--header 'apikey: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data '{"owner": "0xf845b2501A69eF480aC577b99e96796c2B6AE88E"}'
```

You get:

```text
{
    "address": "0xa83E94cA4A9D92009C1Bf6dCA54b3E34D4463138",
    "transaction_hash": "0x6663a81a1a827c4bf2301eb169de900c51d2b6e4e2c26d503dce10888f8cdee9",
    "tracking_id": "01E9ZSDHMYYFMW3E1CVQ9ADVHK"
}
```

### Send ether to your Forwarder

Because the forwarder will payback for the transaction fees it needs to be funded in ether.

On Ropsten, we fund your forwarder with 0.01 ETH when it's deployed. When your credit is consumed you have to fund it by sending ether to your Forwarder contract address.

### Create your relay message

As an example, let's call a smart contract that simply stores the address of the account that signed the relay message.

The contract is deployed at: [0xa8F87be466D1bDff91E6A8E44Be47bF767432638](https://ropsten.etherscan.io/address/0xa8f87be466d1bdff91e6a8e44be47bf767432638) on the Ropsten network.

We create a node project to be able to build the relay message.

```bash
npm init
```

Install Rockside Wallet SDK

```bash
npm install @rocksideio/rockside-wallet-sdk
```

On index.js add:

```javascript
const  Wallet = require('@rocksideio/rockside-wallet-sdk/lib/wallet.js')
const  Hash = require('@rocksideio/rockside-wallet-sdk/lib/hash.js')

// A new wallet is created. It will be used to sign the message
const wallet = Wallet.BaseWallet.createRandom();


// Following are the different parts of the message.

// The domain we are calling with the chainID and the address of the contract we are calling.
const domain = { chainId: 3, verifyingContract: '0xa8F87be466D1bDff91E6A8E44Be47bF767432638' };

// Here, all the parameters that can be provided to the dApps to execute the requested transaction.
// In our case only the signer is required. When we call the contract, it adds the signer to an array of caller.
const relayMessage = {
  relayer: "",
  signer: wallet.getAddress(),
  to: "",
  value: 0,
  data: [],
  nonce: 0,
  gasLimit: 0,
  gasPrice: 0
};

// Create the HASH of the message
const hash = Hash.executeMessageHash(domain, relayMessage);

// Sign the Hash with the wallet.
wallet.sign(hash).then((value) => {
  console.log("Wallet address (signer): "+wallet.getAddress())
  console.log("Signature: "+value)
});
```

Run the script

```bash
npm index.js
```

You get:

```bash
Wallet address (signer): 0xeaD23030fe26C5965FDf6D5C72C575689E2F51D2
Signature: 0xf68b22a18b28ec88751a270ba1575634134f09847ed36587fbc51d5d2de1aef927d8cec7d7d0f870c7fc5ecfd59e9407f5b2c0ce0824dc988de427aaede89f681c
```

Keep those two parameters, you will use it to call Rockside API.

### Relay your transaction

To use Rockside API, you need to provide a speed and a gas price limit.

Available speeds are:

* safelow \(around 30 minutes\)
* average/standard \(around 5 minutes\)
* fast \(around 2 minutes\)
* fastest \(around 30 seconds\)

Depending on your choice, you have to specify your gas price limit. It defines the maximum gas price you agree to pay to execute your transaction at the requested speed.

If you want to have an idea of the price to provide you can use [EthGasStation](https://ethgasstation.info).

![EthGasStation](https://raw.githubusercontent.com/rocksideio/technicaldoc/master/images/ethGasStation.png)

**Note**: EthGasStation gas prices are provided on Gwei on their front page, and as 10xGwei when using their API. Make sure to convert it to wei before sending the value to Rockside \(ex: 39 Gwei -&gt; 39000000000 Wei\)

Let's use curl to relay your transaction:

```bash
curl --request POST 'https://api.rockside.io/ethereum/ropsten/forwarders/FORWARDER_ADDRESS/forward' \
--header 'apikey: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data '{
  "to": "0xa8F87be466D1bDff91E6A8E44Be47bF767432638",
  "speed": "average",
  "data": {
    "gas_price_limit": "39000000000",
    "signer": "YOUR_WALLET_ADDRESS",
    "value": "0",
    "nonce": "0"
  },
  "signature": "YOUR_SIGNATURE"
}'
```

You will get:

```javascript
{
    "transaction_hash": "0x2968698cb90ec9d95f8656d28bb29593029c79bcd22b42dc6b9469cb03729e2a",
    "tracking_id": "01EA266P6PKN0ZY01P0Q6G7WR5"
}
```

You can follow your transaction on Etherscan with the `transaction_hash`. But since Rockside is susceptible to replay a stuck transaction, this hash value can changes.

So to keep track of your transaction - whatever happens to it - use the `tracking_id` as follow:

```bash
curl --request GET 'https://api.rockside.io/ethereum/ropsten/transactions/TX_TRACKING_ID' \
--header 'apikey: YOUR_API_KEY' \
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

