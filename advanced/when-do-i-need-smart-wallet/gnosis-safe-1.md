---
description: The most trusted platform to store digital assets on Ethereum
---

# Gnosis Safe

## Presentation <a id="presentation"></a>

Gnosis Safe is one of the best smart contract based wallet available on Ethereum. Fully tested, audited and trusted by the ecosystem.‌

It can manage all token standard and more, has a fully configurable multi-signature and compatible with all the client side wallets.‌

It's the perfect wallet for your client to manage their assets.‌

Checkout the [official website](https://gnosis-safe.io/), the [documentation](https://docs.gnosis.io/safe/) or the [source code](https://github.com/gnosis/safe-contracts) for more informations.‌

### Integration example <a id="integration-example"></a>

A simple example on how to use Rockside with Gnosis Safe Wallet is available [here](https://github.com/rocksideio/rockside-integration-examples/tree/master/gnosis-safe).

{% api-method method="get" host="https://api.rockside.io" path="/ethereum/:network/relay/:address/params" %}
{% api-method-summary %}
Get Relay Params
{% endapi-method-summary %}

{% api-method-description %}
You need to choose a speed and a the appropriate relayer before sending your transaction. For that all we need is the address of your Gnosis Safe.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="network" type="string" required=true %}
Available networks: ropsten, mainnet
{% endapi-method-parameter %}

{% api-method-parameter name="address" type="string" required=true %}
Address of your Gnosis.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="apikey" type="string" required=true %}
Your API key.
{% endapi-method-parameter %}
{% endapi-method-headers %}
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

Now you need to make your gnosis transaction with `gasPrice`  and `refundReceiver` set to the respective value of the speed of your choosing.

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
Address of your Gnosis.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="apikey" type="string" required=true %}
Your API key.
{% endapi-method-parameter %}
{% endapi-method-headers %}

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



