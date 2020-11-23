---
description: Unlock the best of Ethereum with Rockside relays.
---

# ⛽️ Relay API

To relay a transaction, send the **destination contract address, the data and the speed** you want**.**  Available speeds are `safelow` \(around 30 minutes\), `average` or `standard`\(around 5 minutes\), `fast` \(around 2 minutes\), `fastest` \(around 30 seconds\). 

On **Testnet** we accept to relay transactions to any destination and we do not check the refund Rockside will receive. 

On **Mainnet** we accept to relay transactions to any [Gnosis smart wallet](https://docs.rockside.io/advanced/when-do-i-need-smart-wallet/gnosis). To authorize your destination contract, please [contact us](https://twitter.com/rockside_io). We check the data sent to the destination contract to determine if we will be sufficiently refunded for network fees.

To know our expectations for **gasprice** and **refund address**, call our[ relay params AP](https://docs.rockside.io/relay#get-relay-params)I. That will give a list of `speed` associated to a `gas_price` and a `relayer`. Then use those to create the `data` required by the destination. 

When the transaction is accepted, it will be included in the blockchain even if we have to pay more than the gas price that was specified.

Because Rockside is susceptible to **replay a stuck transaction**, the Tx Hash associated to your transaction changes.

To keep track of your transaction - whatever happens to it - use the `tracking_id`- to call our [Transaction infos API](https://docs.rockside.io/relay#follow-your-transaction).

{% api-method method="post" host="https://api.rockside.io" path="/ethereum/:network/relay/:destination\_address" %}
{% api-method-summary %}
Relay transaction
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="network" type="string" required=true %}
Available networks: ropsten, mainnet
{% endapi-method-parameter %}

{% api-method-parameter name="destination\_address" type="string" required=true %}
Address of your contract.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="data" type="string" required=true %}
Hexadecimal data of the call.
{% endapi-method-parameter %}

{% api-method-parameter name="speed" type="string" required=true %}
Speed of your choosing between safelow, standard, fast and fastest.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "transaction_hash": "0x6663a81a1a827c4bf2301eb169de900c51d2b6e4e2c26d503dce10888f8cdee9",
    "tracking_id": "01E9ZSDHMYYFMW3E1CVQ9ADVHK"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.rockside.io" path="/ethereum/:network/relay/:address/params" %}
{% api-method-summary %}
Get Relay Params
{% endapi-method-summary %}

{% api-method-description %}
You need to choose a speed and a the appropriate relayer before sending your transaction. For that, all we need is your smart contract address.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="network" type="string" required=true %}
Available networks: ropsten, mainnet
{% endapi-method-parameter %}

{% api-method-parameter name="address" type="string" required=true %}
Address of your contract.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Gas price in wei for each speed and relayer associated.
{% endapi-method-response-example-description %}

```
{
    "speeds": {
        "fast": {
            "gas_price": "1400000000",
            "relayer": "0x1000000000000000000000000000000000000000"
        },
        "fastest": {
            "gas_price": "1900000000",
            "relayer": "0x2000000000000000000000000000000000000000"
        },
        "safelow": {
            "gas_price": "600000000",
            "relayer": "0x3000000000000000000000000000000000000000"
        },
        "standard": {
            "gas_price": "1000000000",
            "relayer": "0x4000000000000000000000000000000000000000"
        }
    }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.rockside.io" path="/ethereum/:network/transactions/:id" %}
{% api-method-summary %}
Follow your transaction
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
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
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

