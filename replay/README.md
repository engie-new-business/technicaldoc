# Replay

A pending transaction is a transaction that is waiting in the transactions pool of the Ethereum nodes to be mined.

A pending transaction can be replayed by transmitting the same transaction but with a higher gas price.

Here would be an example of the steps to replay a transaction:

1. I send a transaction to a node on Ethereum
2. This transaction is in the node transactions pool
3. The network is congested and transactions with too low gas prices start to get stuck.
4. I decide that I want this transaction to pass now to avoid being stuck for potentially hours
5. I construct the same transaction but with a higher gas prices (the value of this new gas price is based on external recommendations)
6. I sign this second transaction with the same signer (i.e. as in step 1). Then I sent this new transaction on the Ethereum network.
7. With the right gas price my second transaction will be accepted shortly
8. The first transaction will become obsolete and is discarded by the node since the nonce it has is in the past and now invalid

Obviously Rockside relayer does it for you!

Rockside monitors transactions to check those which are pending for too long. The relayer then auto replay them by carefully adapting to a new gas price, depending on how fast one wants the transaction included.