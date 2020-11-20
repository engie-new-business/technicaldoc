# Gnosis

## Presentation <a id="presentation"></a>

Gnosis Safe is one of the best smart contract based wallet available on Ethereum. Fully tested, audited and trusted by the ecosystem.‌

It can manage all token standard and more, has a fully configurable multi-signature and compatible with all the client side wallets.‌

It's the perfect wallet for your client to manage their assets.‌

Checkout the [official website](https://gnosis-safe.io/), the [documentation](https://docs.gnosis.io/safe/) or the [source code](https://github.com/gnosis/safe-contracts) for more informations.‌

### Integration example

A simple example on how to use Rockside with Gnosis Safe Wallet is available [here](https://github.com/rocksideio/rockside-integration-examples/tree/master/gnosis-safe).

## Refund

### Refund with DAI

Rockside enables a Gnosis Safe to refund the relayer with DAI. 

In order to enable the refund in DAI,  when calling the `execTransaction` on the gnosis, you need to specify parameters  `gasToken` to `0x6b175474e89094c44da98b954eedeac495271d0f`  and `gasPrice` to the amount of DAI for each unit of gas.

Rockside checks that the amount of DAI is equal or superior to the gas price \(in ether\) multiplied by the ether value in USD \(retrieved via the Coingecko API\). 

For example, if the gas price is 10 GWEI and the ether value is $400, the the `gasPrice` must be set to a value equal or superior to `0.00000001*400` . Here `0.00000001` is 10 GWEI represented in ETH.



