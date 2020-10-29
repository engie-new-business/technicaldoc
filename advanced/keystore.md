---
description: Keys management system
---

# Keystore

{% api-method method="post" host="https://api.rockside.io" path="/ethereum/eoa" %}
{% api-method-summary %}
Create EOA Account
{% endapi-method-summary %}

{% api-method-description %}
Create an account \(EOA\) on your rockside wallet. Those accounts will need ether to send transaction.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="apikey" type="string" required=true %}
Your API key is available on Rockside Dashboard.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "address": "0x273e3d3ae721f0137d16cbd168c495e70e046dd4"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

**Example**:

```bash
curl -X POST \
  https://api.rockside.io/ethereum/eoa \
  -H 'Content-Length: 0' \
  -H 'apikey: YOUR_API_KEY'
```

{% api-method method="get" host="https://api.rockside.io" path="/ethereum/eoa" %}
{% api-method-summary %}
List  EOA Accounts
{% endapi-method-summary %}

{% api-method-description %}
List all your rockside wallet accounts \(EOA\).
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="apikey" type="string" required=true %}
Your API key is available on Rockside Dashboard.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
[
    "0x60d44d28c826cff5d05ee57526e20103cdac1c9a",
    "0x642aa2e540bb7172c487d5e2c315ad81b27070af",
    "0x55f42ec257a6ec1b77735f5c9428d954e36c62c2",
    "0xfb56d1a1ec09c9dac3870db4ab9516e646f167fc"
]
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

**Example**:

```bash
curl -X GET \
  https://api.rockside.io/ethereum/eoa \
  -H 'apikey: YOUR_API_KEY'
```

{% api-method method="post" host="https://api.rockside.io" path="/ethereum/eoa/EOA\_ADDRESS/sign" %}
{% api-method-summary %}
Sign transaction
{% endapi-method-summary %}

{% api-method-description %}
Sign a transaction to send it with sendRawTransaction
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="EOA\_ADDRESS" type="string" required=true %}
address of the EOA to use to sign the transaction
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="apikey" type="string" required=true %}
Your API Key is available on Rockside dashboard
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="network\_id" type="string" required=true %}
Blockchain network id
{% endapi-method-parameter %}

{% api-method-parameter name="nonce" type="string" required=true %}
Sender nonce to use
{% endapi-method-parameter %}

{% api-method-parameter name="to" type="string" required=false %}
Destination address
{% endapi-method-parameter %}

{% api-method-parameter name="value" type="string" required=false %}
Value of transaction
{% endapi-method-parameter %}

{% api-method-parameter name="data" type="string" required=false %}
Data of the transaction
{% endapi-method-parameter %}

{% api-method-parameter name="gasPrice" type="string" required=false %}
GasPrice of the transaction
{% endapi-method-parameter %}

{% api-method-parameter name="gas" type="string" required=false %}
Gas of the transaction 
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "signed_transaction": "0xf85f01808276c0947c869a955e4846e5f6e28fb525b0200d03159e1680801ba09ca643357e2e3e4c0ee9f8a61b1ffb377b6acd453b5a1582fde4360888b31f34a072718acf37e7bcc4ad46e22a8eb6e386e1ad8629e57a3c5ceaccf55a325367a7"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

**Example**:

```bash
curl -X POST \
  https://api.rockside.io/ethereum/eoa/0x7e4470895cc2815cb09b05ad480574d518b4a92b/sign \
  -H 'apikey: YOUR_API_KEY'
  -d '{"from":"0x7e4470895cc2815cb09b05ad480574d518b4a92b",
  "to":"0x7c869a955e4846e5f6e28fb525b0200d03159e16",
  "gas":"0x76c0",
  "gasPrice":"0x0","value":"0x0", "nonce":"1" }'
```

{% api-method method="post" host="https://api.rockside.io" path="/ethereum/eoa/EOA\_ADDRESS/sign-message" %}
{% api-method-summary %}
Sign Message
{% endapi-method-summary %}

{% api-method-description %}
Sign a message
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="EOA\_ADDRESS" type="string" required=true %}
address of the EOA to use to sign the transaction
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="apikey" type="string" required=true %}
Your API Key is available on Rockside dashboard
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="message" type="string" required=true %}
Keccak hash of the message to sign
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
    "signed_message": "0xc352bbfc7049da9890467b6254faf4dc3f2743ad5531ded061df4814295f185365daca00a3b1fc60a0cbf88039466bb70c071d142d89ab45d9c15c4e77fae28001"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

**Example**:

```bash
curl -X POST \
  https://api.rockside.io/ethereum/eoa/PUBLIC_ADDRESS/sign-messsage \
  -H 'apikey: YOUR_API_KEY'
  -d '{"message":"0x3877c10b1c024084aef6141a712640a7fad4bf9cd7ba195f112e2543e229c8bb"}'
```

