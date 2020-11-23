# About Rockside

Rockside relayer is a non-custodial transaction delivery service. When sending a transaction to Rockside, you provide:

* the `gas price limit` which is the maximum price you want to apply to the transaction
* a chosen `speed` of inclusion in the blockchain

We make sure your transaction is then executed at the best price in respect to the delay.

{% embed url="https://www.youtube.com/watch?v=psR6ala0ksI" %}

## What problem are we solving?

Sometimes Ethereum transactions are stuck or lost. To attempt to solve this problem, developers overpay transactions and must remain on-call to unblock them. Rockside is a transaction relayer for sending Blockchain transactions at a fixed and predetermined price and time. Rockside ensure the validation of your transactions and bears any additional gas.

## How does it work?

Depending on your given gas price limit, your requested speed and the current market gas prices, we decide whether or not to relay your transaction.

Once accepted, we ensure your transaction is validated at the best price at your speed.

* **API and SDK**: Send your transactions using our API and open source SDK \(JS, iOS\)
* **Speed**: available speeds are `safelow` \(around 30 minutes\), `average` or `standard`\(around 5 minutes\), `fast` \(around 2 minutes\), `fastest` \(around 30 seconds\)
* **Gas price limit**: Maximum gas price value in wei, you are willing to apply to your transaction.
* **Meta-transactions**: To relay your transactions we use the concept of meta-transactions, wrapping your signed message in a new transaction. 
* **Gasless transaction:** Thanks to meta-transactions, Rockside allows you to pay gas fees for your DApp's users. They no longer need ethers to interact with your DApp.
* **Transaction auto replay**: We monitor your transaction and we replace it with one with a higher gas price when necessary to validate your transaction according to your requested speed. More on [transaction replay page](https://github.com/rocksideio/technicaldoc/tree/9cfac19c42deb1f642ed99865999106aaf5c8c75/advanced/replay.md).
* **Pool of signers**: We manage a pool of signers to guarantee no stuck transactions.
* **Transaction batch** You can group your transactions in a batch. You then have an atomic operation and your transactions will be executed on chain within a single transaction.
* **Tracking ID**: When sending a transaction to Rockside, we also provide you with its tracking ID. Use it to have the latest and automatically updated status of your transaction even in case of replays and other actions taken.

## 

