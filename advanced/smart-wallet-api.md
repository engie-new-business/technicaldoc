# 📖 Smart wallet API

{% api-method method="post" host="https://api.rockside.io/" path="ethereum/:network/smartwallets" %}
{% api-method-summary %}
Deploy a SmartWallet
{% endapi-method-summary %}

{% api-method-description %}
Deploy a smart-wallet to be used with a forwarder
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="network" type="string" required=true %}
Available networks are mainnet, ropsten
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="apikey" type="string" required=true %}
Your API KEY is available on Rockside Dashboard
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="account" type="string" required=true %}
Owner account of the smartwallet. Account that will sign relay message
{% endapi-method-parameter %}

{% api-method-parameter name="forwarder" type="string" required=true %}
Address of the forwarder to trust for the relay.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "address": "0x07672cf263BeB920B34b2740596b8B4a28b25D47",
    "transaction_hash": "0xf3ad76a9879a60c1bde76f806d463d0f378e2d1ace78eb62bbde40561f77df36",
    "tracking_id": "01ED9ER94VM4K0V7K7AF8NFGHV"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

