# Transaction replay

A pending transaction is a transaction that is waiting in the transactions pool of the Ethereum nodes to be mined.

A pending transaction can be replayed by transmitting the same transaction, signed with the same account, but with a higher gas price.

Here would be an example of the steps to replay a transaction:

1. I send a transaction on the Ethereum network. This transaction is now waiting in transactions pool of the node.
2. The network is congested and transactions with too low gas prices start to get stuck!
3. I decide that I want this transaction to pass now to avoid being stuck for potentially hours.
4. I construct the same transaction but with a higher gas price \(the value of this new gas price would be based on heuristics/recommendations\).
5. I sign this second transaction with the same signer \(i.e. as in step 1\). Then I sent this new transaction on the Ethereum network.
6. With the right gas price the second transaction will be accepted accordingly.
7. The first transaction then become obsolete and is discarded by the nodes since its nonce is in the past and now invalid.

Obviously Rockside relayer does it for you!

Rockside monitors transactions to check those which are pending for too long.

We then auto replay them by carefully adapting to a new gas price, depending on how fast one wants the transaction included.

## Rockside Replay

Following [Eth Gas Station](https://ethgasstation.info) recommendations, we have roughly four speeds in which a transaction can be played/replayed.

* `safe low`: mined in around 30 mins
* `average`: mined in around 5 mins
* `fast`: mined in around 2 mins
* `fastest`: mined in around 30 seconds

The price for each of those speeds obviously varies according to the current congestion of the Ethereum network.

When you first send a transaction to Rockside, you specify a `speed` \(which translates to a deadline of inclusion in the blockchain\) and a `maximum gas price` you are willing to pay.

Monitoring your transaction, if Rockside sees it is not going to make it. we upgrade the gas price to be able to reach your deadline, Rockside paying for the extra costs.

Of course, Rockside paying for the extra costs \(for your transaction to be included in time\), we reserved the right to refuse a transaction when you first send it to us. Usually because the maximum gas price your are willing to pay does not reflect the current state of the network. Our API would obviously explains why a transaction was refused.

