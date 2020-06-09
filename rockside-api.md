# Rockside API

{% api-method method="post" host="https://api.rockside.io" path="/ethereum/:network/forwarder" %}
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
Available network are: ropsten, mainnet
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="apikey" type="string" required=true %}
Your API Key is available on  Rockside Dashboard
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
Forwarder deployed
{% endapi-method-response-example-description %}

```
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

{% api-method method="get" host="https://api.rockside.io" path="/ethereum/:network/relayParams" %}
{% api-method-summary %}
Get Forwarder relay parameters
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="" type="string" required=false %}

{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

