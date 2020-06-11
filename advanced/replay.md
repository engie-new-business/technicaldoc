# Transaction replay

A pending transaction is a transaction that sits in the transaction pools of Ethereum nodes,waiting to be mined.

A pending transaction can be replayed by sending the same transaction, signed with the same account, but with a higher gas price.

Here would be an example of the steps to replay a transaction:

1. Send a transaction on the Ethereum network. This transaction is now waiting in transaction pools of various nodes.
2. The network gets congested and transactions with too low gas prices start to get stuck!
3. Let's say we want to avoid our transaction being stuck for hours.
4. Construct the same transaction but with a higher gas price \(the value of this new gas price would be based on various recommendations\).
5. Sign this second transaction with the same signer \(i.e. as in step 1\). Then I sent this new transaction on the Ethereum network.
6. With the right gas price the second transaction will be accepted accordingly.
7. The first transaction then become obsolete and is discarded quickly by the nodes since its nonce is now invalid.

Obviously Rockside relayer does it for you! Rockside monitors transactions to check those which are pending for too long.

We then auto replay transactions by carefully adapting to a new gas price, depending on how fast one wants the transaction included.

## Rockside Replay

Following [Eth Gas Station](https://ethgasstation.info) recommendations, we have four speeds in which a transaction can be played/replayed.

* `safe low`: mined in around 30 mins
* `average`, `standard`: mined in around 5 mins
* `fast`: mined in around 2 mins
* `fastest`: mined in around 30 seconds

The price recommended obviously varies according to the current congestion of the Ethereum network.

When you first send a transaction to Rockside, you specify a `speed` \(which translates to a deadline of inclusion in the blockchain\) and a `gas price limit` which is the maximum gas price you allow to apply to the transaction.

Rockside monitors your transaction. We then upgrade the gas price to be able to reach your deadline, Rockside paying for the extra costs.

Of course, Rockside paying for the extra costs \(for your transaction to be included in time\), we reserved the right to refuse a transaction when you first send it to us. Usually because the gas price limit your requested does not reflect the current state of the network. Our API would obviously explains why a your transaction was refused at the beginning of the process.