# Relay API

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
Available networks are: ropsten, mainnet
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
You get the address of your forwarder and the txHash and the trackingID to to follow the status of the transaction.
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

{% api-method-parameter name="channel\_id" type="string" required=false %}
The channel to use to .get the nonce. 
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns the nonce to add on your message and the address of the relayer that will be on charge of sending your meta transaction.
{% endapi-method-response-example-description %}

```
{
    "nonce": "0",
    "relayer": "0xdd0f36E17474E8CbF9C4e483D02a1CF34f41550A"
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
{% api-method-parameter name="to" type="string" required=true %}
Address of the contract that will receive the transaction.
{% endapi-method-parameter %}

{% api-method-parameter name="speed" type="string" required=true %}
Speed to relay your transaction: safelow \(around 30 min\), average \(around 5 min\), fast \(around 2 min\), fastest \(around 30 sec\). Higher speed required higher gas price limit.
{% endapi-method-parameter %}

{% api-method-parameter name="data" type="object" required=true %}
The parameters that the destination contract will receive. Object with to, value, data, gasPriceLimit, gasLimit, and nonce.
{% endapi-method-parameter %}

{% api-method-parameter name="signature" type="string" required=true %}
Signature of the parameters present in the data fields.
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

