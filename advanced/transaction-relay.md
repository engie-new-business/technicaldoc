To relay your transactions we use the concept of meta-transactions. Meta transaction are based on the principle of off-chain message signing for on-chain use. A user will sign a message representing its transaction intent. Then the message is sent to a relayer that will have the responsability to relay the message that will be executed on chain by a smart contract.

![relay overview](https://raw.githubusercontent.com/rocksideio/technicaldoc/master/images/tx-relay-overview.png "Relay overview")


## Forwarder contract
// TO DO EXPLAIN What is the role of the Forwarder Contract

## User sign its message and send it to Rockside Relay API

The paramaters that have to be included on the signed message are:

* **signer**: The address of the account who signed the message. The user who want to interact with the destination contract.
* **to**: If the destination contract should interract with other account (contract or EOA), this field can be used (for example to tranfert tokens)
* **value**:  Can be used in some use case, to allow user owning Ether or Token to perform transaction.
* **data**: The bytes to be executed.
* **nonce**: The nonce value used by the signer. It's a 2 dimensional nonce represented as a 256 bit integer split in two. See [replay protection page](replay-protection.md)
* **gasLimit**: The maximun gas aggreed for this transaction to be relayed. Use 0 if not limit **# Really Need it ?**
* **gasPriceLimit**: The maximun gas price to use to refund the gas consumed by the transaction.

At Rockside we follow [EIP-712](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-712.md) to structure the relay message.

When the message is created and signed, it's sent to Rockside Relayer API with a chosen speed of inclusion in the blockchain.

### The Relayer validate the transaction and send it to the Forwarder

1. **Accept the transaction**: Rockside use EthGasStation as a reference for the gas prices. Depending on your given gas price limit, your requested speed and the current market gas prices, we decide whether or not to relay your transaction. We also verify that the Forwarder have enough ETH to refund Rockside for the gas used by the transaction.

2. **Choose the appropriate EOA**: Rockside manage different pools of EOA to send transactions. A pool of EOA for each available speeds. By this way, we guarantee that a transaction with a "fast" speed will not be slowed down by a transaction with a "safelow" speed. It's like on the highway, each transaction has its own queue depending on its speed.

3. **The message is included on a transaction**: Using the chosen EAO a transaction containing the signed message of the user is sent to the Forwarder. The gas price used by Rockside is in accordance with the speed requested.


### The Forwarder validate the transaction and call the destination contract

1. **Message signature validation**:
2. **Check and update nonce**:
3. **Call the destination contract**:

### The transaction is executed

The destination contract has received the different parameters of the transaction (signer, to, value, data). It can now execute the transaction on behalf of the signer.
