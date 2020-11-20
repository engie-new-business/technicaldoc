---
description: Unlock the best of Ethereum with Rockside relays.
---

# ⛽️ Relay API

## Introduction

The relay api is use the full potential of Rockside while providing a simple unauthenticated endpoint for users.

First get our relay params. That will give a list of `speed` associated to a `gas_price` and a `relayer`. Then use those to create the `data` required by the destination. And finally, send this `data` and the `speed` you choose to Rockside Relay service. 

If the transaction is accepted by Rockside \(enough ETH or ERC20 to refund Rockside for network fees\), we guarantee it will be included in the blockchain.

We are compatible with Gnosis smart wallet. More details are available [here](advanced/when-do-i-need-smart-wallet/gnosis.md).

[Reach us](https://twitter.com/rockside_io) if you want to integrate your standard in Rockside !

Choose a speed and make your data according to the related values \(`gasPrice`  and `refundReceiver` for gnosis\).



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

{% api-method method="post" host="https://api.rockside.io" path="/ethereum/:network/relay/:address" %}
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

{% api-method-parameter name="address" type="string" required=true %}
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

