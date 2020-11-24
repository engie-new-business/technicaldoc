# Forwarder

I you want to pay the gas for your user you can use a forwarder contract.

 A user signs a message representing its transaction intent. The message is then sent to Rockside. There it gets wrapped in a new transaction to be sent and executed on chain by the Forwarder contract.

![Relay overview](https://raw.githubusercontent.com/rocksideio/technicaldoc/master/images/tx-relay-overview.png)

## User signs its message and sends it to Rockside

The parameters to be included on the signed message are:

* **signer**: address of the account who signed the message. The user who want to interact with the destination contract.
* **to**: if the destination contract has to interact with another account \(contract or EOA\), this field can be used \(for example to transfer tokens\).
* **data**: bytes to be executed.
* **nonce**: nonce value used by the signer. It's a 2 dimensional nonce represented as a 256 bit integer split in two.

At Rockside we follow [EIP-712](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-712.md) to structure the relay message.

When the message is created and signed, it's sent to Rockside with a chosen speed of inclusion in the blockchain.

### Rockside validates the transaction and sends it to the Forwarder

1. **Accept the transaction**: Rockside use EthGasStation as a reference for the gas prices. Depending on your given gas price limit, your requested speed and the current market gas prices, we decide whether or not to relay your transaction. We also verify that the Forwarder has enough ether to refund Rockside for the gas used by the transaction.
2. **Choose the appropriate EOA**: Rockside manages different pools of EOA to send transactions. A pool of EOA for each available speed. This way, we guarantee that a "fast" transaction will not be slowed down by a transaction with a "safelow" speed. It's like on the highway, each transaction has its own queue depending on its speed.
3. **The message is included within a transaction**: Using the corresponding EOA a transaction containing the signed message of the user is sent to the user's Forwarder. The gas price used by Rockside is in accordance with the speed requested.

The source code of Forwarder contract is available[ on Github](https://github.com/rocksideio/contracts/blob/master/contracts/Forwarder.sol).

### The Forwarder validates the message and calls the destination contract

1. **Message signature validation**: The Forwarder verifies that the signature corresponds to the signer and the parameters of the transaction.
2. **Check and update nonce**: To avoid replay attacks, the forwarder verifies that the nonce was not already used. Once done, the current nonce is incremented.
3. **Call the destination contract**: When all verifications are done, the destination contract is called with the parameters of the transactions.
4. **Refund Rockside relayer**: An amount of ether corresponding to the gas consumed and the gas price used \(limited by the gas price limit\) is sent from the Forwarder to the Rockside Relayer.

### The transaction is executed

The destination contract [is called with "data+signer"](https://github.com/rocksideio/contracts/blob/master/contracts/Forwarder.sol#L110). To be compatible with Rockside's forwarder, the destination contract must use a special function instead of the usual `msg.sender`

```text
function _msgSender() internal view returns (address ret) {
        address sender = msg.sender;
        if (msg.data.length >= 24 && msg.sender == authorizedForwarder) {
            assembly {
                sender := shr(96,calldataload(sub(calldatasize(),20)))
            }
        }
        return sender;
    }
```

This function is used for your contract to know if it was called by a meta transaction or a normal transaction.

You can see an example of implementation in [our SmartWallet contract](https://github.com/rocksideio/contracts/blob/master/contracts/SmartWallet.sol).

### **Replay Protection**

The Forwarder contract implements a two dimensional nonce from [EIP 2585](https://github.com/ethereum/EIPs/issues/2585) .

We think it's the less restrictive approach. It allows ordered but also concurrent meta-transactions. It provides different channels for nonce management. If you don't need concurrency, just use one channel. If you need concurrent transactions, use different channels. An extra gas fee will be charged the first time you use a channel.

In this implementation, the nonce is an `uint256` that is split in two 128 bit values. The higher bits represent the channel ID while the lower bits represent the nonce in the channel. The nonce to be sent is equal to the current nonce of a channel. For every use the current nonce get increased by one.

```javascript
mapping(address => mapping(uint128 => uint128)) public nonces;

    function checkAndUpdateNonce(address signer, uint256 nonce) internal returns (bool) {
        uint128 channelId = uint128(nonce % 2**128);
        uint128 channelNonce = uint128(nonce / 2**128);

        uint128 currentNonce = nonces[signer][channelId];
        if (channelNonce == currentNonce) {
            nonces[signer][channelId]++;
            return true;
        }
        return false;
    }
```

### Get an API KEY

To access the forwarder API you need an API Key.  It's available by registering on [Rockside Dashboard](https://dashboard.rockside.io/).

{% api-method method="post" host="https://api.rockside.io" path="/ethereum/:network/forwarders" %}
{% api-method-summary %}
Deploy a Forwarder contract
{% endapi-method-summary %}

{% api-method-description %}
Deploy a forwarder contract
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="network" type="string" required=true %}
Available networks are: ropsten, mainnet
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="apikey" type="string" required=true %}
Your API Key is available on Rockside Dashboard
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="owner" type="string" required=true %}
Ethereum address of the owner of the contract. Owner can modify the list of authorized senders and move the funds of the contract.
{% endapi-method-parameter %}

{% api-method-parameter name="destinations" type="array" required=false %}
List of authorized forward destination. If empty all destination are authorized. When forwarder deployed only owner can modify this list.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
You get the address of your forwarder and the txHash and the trackingID to to follow the status of the transaction.
{% endapi-method-response-example-description %}

```text
{
    "address": "0xa83E94cA4A9D92009C1Bf6dCA54b3E34D4463138",
    "transaction_hash": "0x6663a81a1a827c4bf2301eb169de900c51d2b6e4e2c26d503dce10888f8cdee9",
    "tracking_id": "01E9ZSDHMYYFMW3E1CVQ9ADVHK"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.rockside.io" path="/ethereum/:network/forwarders" %}
{% api-method-summary %}
List Forwarders
{% endapi-method-summary %}

{% api-method-description %}
Get the list of all your forwarders
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="network" type="string" required=true %}
Available networks are: mainnet, ropsten
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="apikey" type="string" required=true %}
Your API Key is available on Rockside Dashboard
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
List of your forwarders
{% endapi-method-response-example-description %}

```text
[
    "0x7Bb7703f2c601A54b484ADd52a07AFAD9C9F495e",
    "0x57732F6610623D944c2150C8E394e44f37b18357"
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.rockside.io" path="/ethereum/:network/forwarders/:forwarder\_address/relayParams" %}
{% api-method-summary %}
Get relay parameters
{% endapi-method-summary %}

{% api-method-description %}
Get the parameters required to create the relay message.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="network" type="string" required=true %}
Available networks are: ropsten, mainnet
{% endapi-method-parameter %}

{% api-method-parameter name="forwarder\_address" type="string" required=true %}
Address of the forwarder contract
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="apikey" type="string" required=true %}
Your API Key is available on Rockside Dashboard
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="account" type="string" required=true %}
The public address of the EOA that will sign the relay message.
{% endapi-method-parameter %}

{% api-method-parameter name="channel\_id" type="string" required=true %}
The channel to use to .get the nonce.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns the nonce and the gas price prices. You can choose a gas price and use it has `gas_price_limit` for "relay a transaction" route.
{% endapi-method-response-example-description %}

```text
{
    "nonce": "4",
    "gas_prices": {
        "fast": "81000000000",
        "fastest": "98000000000",
        "safelow": "57000000000",
        "standard": "66000000000"
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.rockside.io" path="/ethereum/:network/forwarders/:forwarder\_address/" %}
{% api-method-summary %}
Relay a transaction
{% endapi-method-summary %}

{% api-method-description %}
Relay your transaction by providing signed message, speed and gasprice limit.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="network" type="string" required=true %}
Available networks are: ropsten, mainnet.
{% endapi-method-parameter %}

{% api-method-parameter name="forwarder\_address" type="string" required=true %}
Address of the forwarder contract
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="apikey" type="string" required=true %}
Your API Key is available on Rockside Dashboard
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="gas\_price\_limit" type="string" required=true %}
Maximum gas price to use to relay the transaction.
{% endapi-method-parameter %}

{% api-method-parameter name="speed" type="string" required=true %}
Speed to relay your transaction: safelow \(around 30 min\), average \(around 5 min\), fast \(around 2 min\), fastest \(around 30 sec\). Higher speed required higher gas price limit.
{% endapi-method-parameter %}

{% api-method-parameter name="message" type="object" required=true %}
The parameters that the destination contract will receive. Object with `signer`, `to`, `data` and `nonce`.
{% endapi-method-parameter %}

{% api-method-parameter name="signature" type="string" required=true %}
Signature of the parameters present in the message fields.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
    "transaction_hash": "0x6663a81a1a827c4bf2301eb169de900c51d2b6e4e2c26d503dce10888f8cdee9",
    "tracking_id": "01E9ZSDHMYYFMW3E1CVQ9ADVHK"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.rockside.io" path="/ethereum/:network/transactions/:id" %}
{% api-method-summary %}
Get transaction infos
{% endapi-method-summary %}

{% api-method-description %}
Retrieve informations about a transaction relayed.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="network" type="string" required=true %}
Available network are: mainnet, ropsten
{% endapi-method-parameter %}

{% api-method-parameter name="id" type="string" required=true %}
`transaction_hash` or `tracking_id`. Returned by the relay service when sending your transaction. Use the tracking ID to have the latest and automatically updated status of your transaction even in case of replays and other actions taken.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="apikey" type="string" required=true %}
Your API Key is available on Rockside Dashboard
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns infos for a transaction.
{% endapi-method-response-example-description %}

```text
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
    "status": "success",
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

