# FAQ

### How can I integrate my dApp contract with Rockside Relayer?

Your dApp contract need to implement a method _relayExecute \( address signer, address to , uint value, bytes memory data \)._ 

Because the forwarder validate the message signature and the nonce, you need to **only authorize the forwarder to call the relayExecute method** to be sure that the parameters are valid.

A simple example can be found on our [demo app contract](https://github.com/rocksideio/vote-showcase-app/blob/master/contracts/Vote.sol).

### How can I limit the contracts to which the forwarder can relay transactions?

When you deploy a forwarder, use the parameter _destinations_ to specify the list of contracts to which you want to authorize the relay. If no destination are specified, your forwarder will relay transactions without limitation.

```bash
curl --request POST 'https://api.rockside.io/ethereum/ropsten/forwarders' \
--header 'apikey: YOUR_API_KEY' \
--header 'Content-Type: application/json' \
-d '{
	"owner": "YOUR_ADMIN_ACCOUNT",
	"destinations": ["ADDRESS_OF_YOUR_DAPP_CONTRACT"]
}'
```

### 





