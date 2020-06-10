To relay your transactions we use the concept of meta-transactions. Meta transaction are based on the principle of off-chain message signing for on-chain use. A user will sign a message representing its transaction intent. Then the message is sent to a relayer that will have the responsability to relay the message that will be executed on chain by a smart contract.

![relay overview](https://raw.githubusercontent.com/rocksideio/technicaldoc/master/images/tx-relay-overview.png "Relay overview")

## User sign its message and send it to the Relayer

The paramaters that have to be included on the signed message are:

* **signer**: The address of the account who signed the message. The user who want to interact with the destination contract.
* **to**: If the destination should interract with other contract, this field can be used (for example to tranfert tokens)
* **value**:  Can be used in some use case, to allow user owning Ether or Token to perform transaction having effect on it.
* **data**: The bytes to be executed.
* **nonce**: The nonce value used by the signer. It's a 2 dimensional nonce represented as a 256 bit integer split in two. See [replay protection page](replay-protection.md)
* **gasLimit**: The maximun gas aggreed for this transaction to be relayed. Use 0 if not limit **# Really Need it ?**
* **gasPriceLimit**: The maximun gas price to use to refund the gas consumed by the transaction.

At Rockside we follow [EIP-712](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-712.md) to structure the relay message.

When the message is created and signed, it's sent to Rockside Relayer API with a chosen speed of inclusion in the blockchain.

### The Relayer validate the transaction and send it to the Forwarder



### The Forwarder validate the transaction and forward it

### The transaction is executed


