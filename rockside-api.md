# 📖 Forward API

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

