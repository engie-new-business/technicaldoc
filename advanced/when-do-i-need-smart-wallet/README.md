# Smart wallet

### When do I need a smart wallet ?

Meta transaction and message relay is really great, it allows to separate the intention of the user \(example to interact with a dApp\) and the transmission of his intentions via a transaction. The consequence is that the transaction sender is not the one who interact with the dApp.

In some use cases, your users need to interact with contracts that do not implement meta-transaction and use the `message.sender` to determinate the identity of the user.  For example you cannot relay a message to call the approve method of an ERC20 contract.

For those use cases the use of a smart wallet can be relevant. In order to use your platform you can deploy a smart wallet for your user. Then your user will have to deposit ETH and Token on this smart wallet. Then you can relay message of your user that can control their smart wallet to interact with the ERC20 contract. This way you can provide your user gasless transaction and all benefits of a relayer \(optimized gas price, auto-replay ... \).

### How Rockside works with Smart Wallet ?

With a smart wallet you don't need to go through a Forwarder contract because smart wallets are already doing what a Forwarder would do. But in that scenario, it's on the user behalf to refund Rockside after a relay. 

The only thing you need is a Gnosis Safe for each of your client with funds for refund.

Rockside support only Gnosis Safe for the moment but more are coming.

![](../../.gitbook/assets/aaa2%20%281%29.png)

User signs messages in order to control their smart-wallet so they can approve or send ERC20 tokens. The user with its Smart Wallet contract relay the transaction and refund the gas to Rockside. The signature and the nonce are checked in the Smart Wallet contract.

An example of calling HEX Smartcontract with a Gnosis Safe with and without a Forwarder is [available here](https://github.com/rocksideio/Demo-Smartwallet-Hex).



