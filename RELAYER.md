# Rockside Relayer

Rockside relayer is a transaction deliverery service. Give us your max gas price and your deadline, we makes sure your transaction is executed on time and at the best price.

## The problem we solve
Sometimes Ethereum transactions are stuck or lost.
To attempt to solve this problem, developers overpay transactions and must remain on-call to unblock them.
We designed a solution with a new approach to get rid of this situation

## How we do that

* **API and SDK**: Send your transactions using our API and SDK (JS, iOS)
* **Deadline and gas price limit**: Depending on your gas price limit, your deadline and current network gas prices, we accept your transaction. We make sure that your transaction is executed on time and at the best price.
* **Meta transaction**: To relay your transaction we use Meta Transaction and wraps signed message in a new transaction. By the way Rockside allows you to pay gas fees for your DApp's users. They no longer need ETH to interact with your Dapp.
* **Transaction auto replay**: We monitor your transactions to increase the price of gas when necessary to meet the deadline.
* **Pool of signers**: We manage a pool of signer with a multi-dimentionnal replay protection to garanty no stuck transaction.
* **Transaction batch** Send us more than one transaction in a call. All your transactions are executed onchain within a single transaction.
* **Tracking ID**: When sending a transaction with Rockside, you get it's transaction hash but also a trancking ID. Use it to follow the status of your transaction even it's replayed with higher gas price.


## Getting Started

### Connect to Rockside

Go to dashboard.rockside.io and connect using your github account.
You will get your API KEY that will allow you to access Rockside API.

### Deploy your forwarder contract

To deploy a forwarder contract you have to specify an owner account.

By default forwarders only accept transactions comming from rockside signers. Only the owner of the forwarder can modify the list of authorized sender.

Execute this request:

```
curl --location --request POST 'https://api-integration.rockside.io/ethereum/ropsten/forwarder' \
--header 'apikey: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
--data-raw '{
	"owner": "0xf845b2501A69eF480aC577b99e96796c2B6AE88E"
}'
```

You get:

```
{
    "address": "0xa83E94cA4A9D92009C1Bf6dCA54b3E34D4463138",
    "transaction_hash": "0x6663a81a1a827c4bf2301eb169de900c51d2b6e4e2c26d503dce10888f8cdee9",
    "tracking_id": "01E9ZSDHMYYFMW3E1CVQ9ADVHK"
}
```

### Send ETH to your Forwarder contract

Send funds directly to your Forwarder contract address. When your Forwarder execute a relay, the gas consumed is sent back to Rockside relayer using a gas price lower or equal to the specified Gas Price Limit.


### Sign your relay message


### Use rockside to Relay your transaction.



