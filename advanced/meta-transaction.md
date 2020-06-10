Meta transaction are based on the principle of off-chain message signing for on-chain use. A user will sign a message representing its transaction intent. So its identity will be garantee.

The message is then send to a relayer that will have the responsability to relay the message to be executed on chain on a smart contract.

![relay overview](https://raw.githubusercontent.com/rocksideio/technicaldoc/master/images/tx-relay-overview.png "Relay overview")

## User sign its message

The structure of a relay message is defined like this on Rockside:

* relayer:
* signer:
* to:
* value:
* data:
* nonce:
* gasLimit:
* gasPrice:

Its should allow the destination contract to have enough element to understand the user intent.


## The message is sent to the Relayer

## The Relayer send the transaction

## The transaction is executed on chain


