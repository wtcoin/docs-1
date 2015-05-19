# Address API

BlockCypher's Address API allows you to look up information about public addresses on the blockchain, generate single-use, low-value key pairs with corresponding addresses, help generate multisig addresses, and collect multiple addresses into a single shortcut for address viewing, all [based on the coin/chain resource](#restful-resources:-coins-&-chains) you've selected for your endpoints.

If you're new to blockchains, you can think of public addresses as similar to bank account numbers in a traditional ledger. The biggest differences:

- Anyone can generate a public address themselves (through [ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm) in Bitcoin). No single authority is needed to generate new addresses; it's just public-private key cryptography.
- Public addresses are significantly more lightweight. Consequently, and unlike traditional bank accounts, you can (and should!) generate new addresses for every transaction.
- Addresses can also leverage [*pay-to-script-hash*](https://en.bitcoin.it/wiki/Pay_to_script_hash), which means they can represent exotic things beyond a single private-public key pair; the most prominent example being multi-signature addresses that require n-of-m signatures to spend.

## Address Balance Endpoint

```shell
$ curl http://api.blockcypher.com/v1/btc/main/addrs/1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD/balance

{
"address": "1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD",
"total_received": 4433416,
"total_sent": 0,
"balance": 4433416,
"unconfirmed_balance": 0,
"final_balance": 4433416,
"n_tx": 7,
"unconfirmed_n_tx": 0,
"final_n_tx": 7
}
```

The Address Balance Endpoint is the simplest---and fastest---method to get a subset of information on a public address.

Resource | Method | Return Object
-------- | ------ | -------------
/addrs/$ADDRESS/balance | GET | [Address](#Address)

$ADDRESS is a *string* representing the public address you're interested in querying, for example:

`1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD`

The returned object contains information about the address, including its balance in satoshis and the number of transactions associated with it. The endpoint omits any detailed transaction information, but if that isn't required by your application, then it's the fastest and preferred way to get public address information.

## Address Endpoint

```shell
$ curl http://api.blockcypher.com/v1/btc/main/addrs/1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD

{
"address": "1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD",
"total_received": 4433416,
"total_sent": 0,
"balance": 4433416,
"unconfirmed_balance": 0,
"final_balance": 4433416,
"n_tx": 7,
"unconfirmed_n_tx": 0,
"final_n_tx": 7,
"txrefs": [
	{
	"tx_hash": "14b1052855bbf6561bc4db8aa501762...",
	"block_height": 302013,
	"tx_input_n": -1,
	"tx_output_n": 0,
	"value": 20213,
	"spent": false,
	"confirmations": 55061,
	"confirmed": "2014-05-22T03:46:25Z",
	"double_spend": false
	},
	{
	"tx_hash": "4cff011ec53022f2ae47197d1a2fd4a6...",
	"block_height": 302002,
	"tx_input_n": -1,
	"tx_output_n": 0,
	"value": 40596,
	"spent": false,
	"confirmations": 55072,
	"confirmed": "2014-05-22T02:56:08Z",
	"double_spend": false
	},
...
],
"tx_url": "https://api.blockcypher.com/v1/btc/main/txs/"
}
```

The default Address Endpoint strikes a balance between speed of response and data on Addresses. It returns more information about an address' transactions than the [Address Balance Endpoint](#address-balance-endpoint) but doesn't return full transaction information (like the [Address Full Endpoint](#address-full-endpoint)).

Resource | Method | Return Object
-------- | ------ | -------------
/addrs/$ADDRESS | GET | [Address](#Address)

Flag | Type | Effect
---- | ---- | ------
**unspentOnly** | *bool* | If *true*, filters response to only include unspent transaction outputs (UTXOs).
**before** | *integer* | Filters response to only include transactions before *integer* height in the blockchain.

$ADDRESS is a *string* representing the public address you're interested in querying, for example:

`1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD`

The returned object contains information about the address, including its balance in satoshis, the number of transactions associated with it, and transaction inputs/outputs.

## Address Full Endpoint

```shell
$ curl http://api.blockcypher.com/v1/btc/main/addrs/1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD/full?before=300000

{
"address": "1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD",
"total_received": 4433416,
"total_sent": 0,
"balance": 4433416,
"unconfirmed_balance": 0,
"final_balance": 4433416,
"n_tx": 7,
"unconfirmed_n_tx": 0,
"final_n_tx": 7,
"txs": [
	{
	"block_hash": "0000000000000000af64802c79...",
	"block_height": 292586,
	"hash": "b4735a0690dab16b8789fceaf81c511f...",
	"addresses": [
		"18KXZzuC3xvz6upUMQpsZzXrBwNPWZjdSa",
		"1AAuRETEcHDqL4VM3R97aZHP8DSUHxpkFV",
		"1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD",
		"1VxsEDjo6ZLMT99dpcLu4RQonMDVEQQTG"
	],
	"total": 3537488,
	"fees": 20000,
	"size": 438,
	"preference": "medium",
	"relayed_by": "",
	"confirmed": "2014-03-26T17:08:04Z",
	"received": "2014-03-26T17:08:04Z",
	"ver": 1,
	"lock_time": 0,
	"double_spend": false,
	"vin_sz": 2,
	"vout_sz": 2,
	"confirmations": 64492,
	"confidence": 1,
	"inputs": [
		{
		"prev_hash": "729f6469b59fea5da7...",
		"output_index": 0,
		"script": "483045022100d06cdad1a...",
		"output_value": 3500000,
		"sequence": 4294967295,
		"addresses": [
			"1VxsEDjo6ZLMT99dpcLu4RQonMDVEQQTG"
		],
		"script_type": "pay-to-pubkey-hash"
		},
		...
	],
	"outputs": [
		{
		"value": 3500000,
		"script": "76a9148629647bd642a237...",
		"addresses": [
			"1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD"
		],
		"script_type": "pay-to-pubkey-hash"
		},
		{
		"value": 37488,
		"script": "76a9145049e2ad94ed9c68...",
		"spent_by": "3ebe4bb294beaed58aca83...",
		"addresses": [
			"18KXZzuC3xvz6upUMQpsZzXrBwNPWZjdSa"
		],
		"script_type": "pay-to-pubkey-hash"
		}
	]
	},
	...,
]
}
```

The Address Full Endpoint returns all information available about a particular address, including an array of complete [Transactions](#transaction) instead of just transaction inputs and outputs. Unfortunately, because of the amount of data returned, it is the slowest of the address endpoints, but it returns the most detailed data record.

<aside class="notice">
If your returned [Address](#address) object includes the **hasMore** attribute, there are more transactions than can be displayed. If this is the case, note the block height of the last transaction in the array, and then you can use the **before** flag to page through results.
</aside>

Resource | Method | Return Object
-------- | ------ | -------------
/addrs/$ADDRESS/full | GET | [Address](#Address)

Flag | Type | Effect
---- | ---- | ------
**before** | *integer* | Filters response to only include transactions before *integer* height in the blockchain.

$ADDRESS is a *string* representing the public address you're interested in querying, for example:

`1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD`

The returned object contains information about the address, including its balance in satoshis, the number of transactions associated with it, and the corresponding full transaction records.

## Generate Address Endpoint

## Generate Multisig Address Endpoint

## Wallets
