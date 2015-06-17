# Address API

BlockCypher's Address API allows you to look up information about public addresses on the blockchain, generate single-use, low-value key pairs with corresponding addresses, help generate multisig addresses, and collect multiple addresses into a single shortcut for address viewing, all [based on the coin/chain resource](#restful-resources) you've selected for your endpoints.

If you're new to blockchains, you can think of public addresses as similar to bank account numbers in a traditional ledger. The biggest differences:

- Anyone can generate a public address themselves (through [ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm) in Bitcoin). No single authority is needed to generate new addresses; it's just public-private key cryptography.
- Public addresses are significantly more lightweight. Consequently, and unlike traditional bank accounts, you can (and should!) generate new addresses for every transaction.
- Addresses can also leverage [*pay-to-script-hash*](https://en.bitcoin.it/wiki/Pay_to_script_hash), which means they can represent exotic things beyond a single private-public key pair; the most prominent example being multi-signature addresses that require n-of-m signatures to spend.

## Address Balance Endpoint

```shell
curl http://api.blockcypher.com/v1/btc/main/addrs/1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD/balance

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

```javascript
$.get('https://api.blockcypher.com/v1/btc/main/addrs/1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD/balance')
  .then(function(d) {console.log(d)});
> {
>   "address": "1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD",
>   "total_received": 4433416,
>   "total_sent": 0,
>   "balance": 4433416,
>   "unconfirmed_balance": 0,
>   "final_balance": 4433416,
>   "n_tx": 7,
>   "unconfirmed_n_tx": 0,
>   "final_n_tx": 7
> }
```

```ruby
# Full balance
> block_cypher.address_balance("1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD")
=> {"address"=>"1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD",
 "total_received"=>4433416,
 "total_sent"=>0,
 "balance"=>4433416,
 "unconfirmed_balance"=>0,
 "final_balance"=>4433416,
 "n_tx"=>7,
 "unconfirmed_n_tx"=>0,
 "final_n_tx"=>7}

# Just final balance
> block_cypher.address_final_balance("1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD")
=> 4433416
```

```python
>>> import requests
>>> r = requests.get('http://api.blockcypher.com/v1/btc/main/addrs/1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD/balance')
>>> r.json()
{'n_tx': 7,
 'total_received': 4433416,
 'final_n_tx': 7,
 'total_sent': 0,
 'balance': 4433416,
 'final_balance': 4433416,
 'unconfirmed_balance': 0,
 'address': '1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD',
 'unconfirmed_n_tx': 0}
```

The Address Balance Endpoint is the simplest---and fastest---method to get a subset of information on a public address.

Resource | Method | Return Object
-------- | ------ | -------------
/addrs/$ADDRESS/balance | GET | [Address](#address)

ADDRESS is a *string* representing the public address (or wallet name) you're interested in querying, for example:

`1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD`

The returned object contains information about the address, including its balance in satoshis and the number of transactions associated with it. The endpoint omits any detailed transaction information, but if that isn't required by your application, then it's the fastest and preferred way to get public address information.

## Address Endpoint

```shell
curl http://api.blockcypher.com/v1/btc/main/addrs/1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD

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

```javascript
$.get('https://api.blockcypher.com/v1/btc/main/addrs/1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD')
  .then(function(d) {console.log(d)});
> {
> "address": "1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD",
> "total_received": 4433416,
> "total_sent": 0,
> "balance": 4433416,
> "unconfirmed_balance": 0,
> "final_balance": 4433416,
> "n_tx": 7,
> "unconfirmed_n_tx": 0,
> "final_n_tx": 7,
> "txrefs": [
> 	{
> 	"tx_hash": "14b1052855bbf6561bc4db8aa501762...",
> 	"block_height": 302013,
> 	"tx_input_n": -1,
> 	"tx_output_n": 0,
> 	"value": 20213,
> 	"spent": false,
> 	"confirmations": 55061,
> 	"confirmed": "2014-05-22T03:46:25Z",
> 	"double_spend": false
> 	},
> 	{
> 	"tx_hash": "4cff011ec53022f2ae47197d1a2fd4a6...",
> 	"block_height": 302002,
> 	"tx_input_n": -1,
> 	"tx_output_n": 0,
> 	"value": 40596,
> 	"spent": false,
> 	"confirmations": 55072,
> 	"confirmed": "2014-05-22T02:56:08Z",
> 	"double_spend": false
> 	},
> ...
> ],
> "tx_url": "https://api.blockcypher.com/v1/btc/main/txs/"
> }
```

```ruby
> block_cypher.address_details("1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD")
=> {"address"=>"1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD", 
 "total_received"=>4433416,
 "total_sent"=>0,
 "balance"=>4433416,
 "unconfirmed_balance"=>0,
 "final_balance"=>4433416,
 "n_tx"=>7,
 "unconfirmed_n_tx"=>0,
 "final_n_tx"=>7,
 "txrefs"=> [
  {"tx_hash"=>"14b1052855bbf6561bc4db8aa501762e7cc1e86994dda9e782a6b73b1ce0dc1e",
	 "block_height"=>302013,
	 "tx_input_n"=>-1,
	 "tx_output_n"=>0,
	 "value"=>20213,
	 "spent"=>false,
	 "confirmations"=>59220,
	 "confirmed"=>"2014-05-22T03:46:25Z",
	 "double_spend"=>false},
	{"tx_hash"=>"4cff011ec53022f2ae47197d1a2fd4a6ac2a80139f4d0131c1fed625ed5dc869",
	 "block_height"=>302002,
	 "tx_input_n"=>-1,
	 "tx_output_n"=>0,
	 "value"=>40596,
	 "spent"=>false,
	 "confirmations"=>59231,
	 "confirmed"=>"2014-05-22T02:56:08Z",
	 "double_spend"=>false},
	...],
 "tx_url"=>"https://api.blockcypher.com/v1/btc/main/txs/"}
```

```python
>>> import requests
>>> r = requests.get('http://api.blockcypher.com/v1/btc/main/addrs/1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD')
>>> r.json()
{'txrefs': [{'spent': False,
   'value': 20213,
   'block_height': 302013,
   'tx_output_n': 0,
   'double_spend': False,
   'tx_input_n': -1,
   'tx_hash': '14b1052855bbf6561bc4db8aa501762e7cc1e86994dda9e782a6b73b1ce0dc1e',
   'confirmed': '2014-05-22T03:46:25Z',
   'confirmations': 58033},
   ..., ],
 'n_tx': 7,
 'total_received': 4433416,
 'final_n_tx': 7,
 'total_sent': 0,
 'balance': 4433416,
 'final_balance': 4433416,
 'tx_url': 'https://api.blockcypher.com/v1/btc/main/txs/',
 'unconfirmed_balance': 0,
 'address': '1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD',
 'unconfirmed_n_tx': 0}
```

The default Address Endpoint strikes a balance between speed of response and data on Addresses. It returns more information about an address' transactions than the [Address Balance Endpoint](#address-balance-endpoint) but doesn't return full transaction information (like the [Address Full Endpoint](#address-full-endpoint)).

Resource | Method | Return Object
-------- | ------ | -------------
/addrs/$ADDRESS | GET | [Address](#address)

Flag | Type | Effect
---- | ---- | ------
**unspentOnly** | *bool* | If **unspentOnly** is *true*, filters response to only include unspent transaction outputs (UTXOs).
**before** | *integer* | Filters response to only include transactions below **before** height in the blockchain.

ADDRESS is a *string* representing the public address (or wallet name) you're interested in querying, for example:

`1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD`

The returned object contains information about the address, including its balance in satoshis, the number of transactions associated with it, and transaction inputs/outputs.

## Address Full Endpoint

```shell
curl http://api.blockcypher.com/v1/btc/main/addrs/1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD/full?before=300000

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

```javascript
$.get('https://api.blockcypher.com/v1/btc/main/addrs/1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD/full?before=300000')
  .then(function(d) {console.log(d)});
> {
> "address": "1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD",
> "total_received": 4433416,
> "total_sent": 0,
> "balance": 4433416,
> "unconfirmed_balance": 0,
> "final_balance": 4433416,
> "n_tx": 7,
> "unconfirmed_n_tx": 0,
> "final_n_tx": 7,
> "txs": [
> 	{
> 	"block_hash": "0000000000000000af64802c79...",
> 	"block_height": 292586,
> 	"hash": "b4735a0690dab16b8789fceaf81c511f...",
> 	"addresses": [
> 		"18KXZzuC3xvz6upUMQpsZzXrBwNPWZjdSa",
> 		"1AAuRETEcHDqL4VM3R97aZHP8DSUHxpkFV",
> 		"1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD",
> 		"1VxsEDjo6ZLMT99dpcLu4RQonMDVEQQTG"
> 	],
> 	"total": 3537488,
> 	"fees": 20000,
> 	"size": 438,
> 	"preference": "medium",
> 	"relayed_by": "",
> 	"confirmed": "2014-03-26T17:08:04Z",
> 	"received": "2014-03-26T17:08:04Z",
> 	"ver": 1,
> 	"lock_time": 0,
> 	"double_spend": false,
> 	"vin_sz": 2,
> 	"vout_sz": 2,
> 	"confirmations": 64492,
> 	"confidence": 1,
> 	"inputs": [
> 		{
> 		"prev_hash": "729f6469b59fea5da7...",
> 		"output_index": 0,
> 		"script": "483045022100d06cdad1a...",
> 		"output_value": 3500000,
> 		"sequence": 4294967295,
> 		"addresses": [
> 			"1VxsEDjo6ZLMT99dpcLu4RQonMDVEQQTG"
> 		],
> 		"script_type": "pay-to-pubkey-hash"
> 		},
> 		...
> 	],
> 	"outputs": [
> 		{
> 		"value": 3500000,
> 		"script": "76a9148629647bd642a237...",
> 		"addresses": [
> 			"1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD"
> 		],
> 		"script_type": "pay-to-pubkey-hash"
> 		},
> 		{
> 		"value": 37488,
> 		"script": "76a9145049e2ad94ed9c68...",
> 		"spent_by": "3ebe4bb294beaed58aca83...",
> 		"addresses": [
> 			"18KXZzuC3xvz6upUMQpsZzXrBwNPWZjdSa"
> 		],
> 		"script_type": "pay-to-pubkey-hash"
> 		}
> 	]
> 	},
> 	...,
> ]
> }
```

```ruby
> block_cypher.address_full_txs("1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD")
=> {"address"=>"1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD",
 "total_received"=>4433416,
 "total_sent"=>0,
 "balance"=>4433416,
 "unconfirmed_balance"=>0,
 "final_balance"=>4433416,
 "n_tx"=>7,
 "unconfirmed_n_tx"=>0,
 "final_n_tx"=>7,
 "txs"=>[
  {"block_hash"=>"00000000000000006548ac8dc283c97e8165023dc1fdbbca2eaa75f0143f4a8c",
  "block_height"=>302013,
  "hash"=>"14b1052855bbf6561bc4db8aa501762e7cc1e86994dda9e782a6b73b1ce0dc1e",
  "addresses"=>["17astdTmG8zzVmry8mV8A7at Ar3XefEgRX", "1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD"],
  "total"=>8835413,
  "fees"=>10000, 
  "size"=>258,
  "preference"=>"medium",
  "relayed_by"=>"",
  "confirmed"=>"2014-05-22T03:46:25Z",
  "received"=>"2014-05-22T03:46:25Z",
  "ver"=>1,
  "lock_time"=>0,
  "double_spend"=>false,
	"vin_sz"=>1,
	"vout_sz"=>2,
	"confirmations"=>59221,
	"confidence"=>1,
	"inputs"=>[
	 {"prev_hash"=>"4cff011ec53022f2ae47197d1a2fd4a6ac2a80139f4d0131c1fed625ed5dc869",
	 "output_index"=>1,
	 "script" =>"483045022035695e3b237733c70a5...",
	 "output_value"=> 8845413,
	 "sequence"=>4294967295,
	 "addresses"=>["17astdTmG8zzVmry8mV8A7atAr3XefEgRX"],
	 "script_type"=>"pay-to-pubkey-hash"}],
	"outputs"=>[
	 {"value"=>20213,
	 "script"=>"76a9148629647bd642a2372d846a7660e210c8414f047c88ac",
	 "addresses"=>["1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD"],
	 "script_type"=>"pay-to-pubkey-hash"},
	 ... ]}, 
  ...
 ]}
```

```python
>>> import requests, json
>>> data = {'before': 300000}
>>> r = requests.get('http://api.blockcypher.com/v1/btc/main/addrs/1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD/full', data=json.dumps(data))
>>> r.json()
{'txs': [{'size': 438,
   'double_spend': False,
   'total': 3537488,
   'preference': 'medium',
   'confidence': 1,
   'fees': 20000,
   'block_hash': '0000000000000000af64802c79f9b22e9091eb5548b4b662d5e444e61885923b',
   'vin_sz': 2,
   'ver': 1,
   'relayed_by': '',
   'block_height': 292586,
   'lock_time': 0,
   'vout_sz': 2,
   'hash': 'b4735a0690dab16b8789fceaf81c511f3be484e319f684cc214380eaa2851030',
   'addresses': ['18KXZzuC3xvz6upUMQpsZzXrBwNPWZjdSa',
    '1AAuRETEcHDqL4VM3R97aZHP8DSUHxpkFV',
    '1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD',
    '1VxsEDjo6ZLMT99dpcLu4RQonMDVEQQTG'],
   'inputs': [{'script_type': 'pay-to-pubkey-hash',
     'prev_hash': '729f6469b59fea5da77457f3291e2623c2516e3e8e7afc782687c6d59f4c5e41',
     'script': '483045022100d06cdad1a54081e8499a4117f9f52d7fbc83c679dda7e3c22c08e964915b7354022010a2d6af1601d28d33a456dab2bccf3fbde35b2f3a9db82f72d675c90d015571014104672a00c8ee6fa23d68094dd98188ea1491848498554a10e13194851b614168b225b28b7f5a1c6ba98b5463438ef030c48b60533031ff2de84104e549d8d06ea9',
     'addresses': ['1VxsEDjo6ZLMT99dpcLu4RQonMDVEQQTG'],
     'sequence': 4294967295,
     'output_value': 3500000,
     'output_index': 0},
     ...],
   'confirmations': 67460,
   'received': '2014-03-26T17:08:04Z',
   'confirmed': '2014-03-26T17:08:04Z',
   'outputs': [{'addresses': ['1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD'],
     'value': 3500000,
     'script': '76a9148629647bd642a2372d846a7660e210c8414f047c88ac',
     'script_type': 'pay-to-pubkey-hash'},
     ...]},
   ...,],
 'n_tx': 7,
 'total_received': 4433416,
 'final_n_tx': 7,
 'total_sent': 0,
 'balance': 4433416,
 'final_balance': 4433416,
 'unconfirmed_balance': 0,
 'address': '1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD',
 'unconfirmed_n_tx': 0}
```

The Address Full Endpoint returns all information available about a particular address, including an array of complete [transactions](#tx) instead of just transaction inputs and outputs. Unfortunately, because of the amount of data returned, it is the slowest of the address endpoints, but it returns the most detailed data record.

Resource | Method | Return Object
-------- | ------ | -------------
/addrs/$ADDRESS/full | GET | [Address](#address)

Flag | Type | Effect
---- | ---- | ------
**before** | *integer* | Filters response to only include transactions before *integer* height in the blockchain.

ADDRESS is a *string* representing the public address (or wallet name) you're interested in querying, for example:

`1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD`

The returned object contains information about the address, including its balance in satoshis, the number of transactions associated with it, and the corresponding full transaction records.

<aside class="notice">
If your returned <a href="#address">Address</a> object includes the <b>hasMore</b> attribute, there are more transactions associated with the address than transfered through this endpoint. If this happens, note the block height of the last transaction in the array, and then you can use the <b>before</b> flag to page through results.
</aside>

## Generate Address Endpoint

```shell
curl -X POST http://api.blockcypher.com/v1/btc/test3/addrs

{
"private": "81ee75559d37cbe4b7cbbfb9931ab1ba32172c5cdfc3ac2d020259b4c1104198",
"public": "0231ff9ec76820cb36b69061f6ffb125db3793b4aced468a1261b0680e1ef4883a",
"address": "mvpW7fMSi1nbZhJJDySNS2PUau8ppnu4kY",
"wif": "cRwGhRjCuuNtPgLcoYd1CuAqjFXCV5YNCQ1LB8RsFCvu61VfSsgR"
}
```

```javascript
$.post('https://api.blockcypher.com/v1/btc/test3/addrs')
  .then(function(d) {console.log(d)});
> {
> "private": "81ee75559d37cbe4b7cbbfb9931ab1ba32172c5cdfc3ac2d020259b4c1104198",
> "public": "0231ff9ec76820cb36b69061f6ffb125db3793b4aced468a1261b0680e1ef4883a",
> "address": "mvpW7fMSi1nbZhJJDySNS2PUau8ppnu4kY",
> "wif": "cRwGhRjCuuNtPgLcoYd1CuAqjFXCV5YNCQ1LB8RsFCvu61VfSsgR"
> }
```

```ruby
> block_cypher.address_generate
=> {"private"=>"39c8788ef9815d061a2a4528817b3c23730b1129e5ca60032138061f35483951",
 "public"=>"039dc801dd76a7fa03789e0fe8efdd7e0857e73fa4ae38a294a60002ef949e83c4",
 "address"=>"1FdQDEnHXibwMMMKaaNGrhrcujJNZyszUN",
 "wif"=>"KyA2uUUMZkiRcSTx3PE1pBeMFgyifpxswcJu3Q7fVwXXZmvh48JA"}
```

```python
# Please use the python library at https://github.com/blockcypher/blockcypher-python to securely generate an address client-side
```

The Generate Address endpoint allows you to generate private-public key-pairs along with an associated public address. No information is required with this POST request.

<aside class="success">
The private key returned is immediately discarded by our servers, but we advise that these keys should not be used for any high-value---or long-term storage---addresses.
</aside>

<aside class="warning">
Always use HTTPS for Address Generation requests. Otherwise, your generated private keys will be sent over insecure channels and could be MITM'd.
</aside>

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/addrs | POST | *nil* | [AddressKeychain](#addresskeychain)

The returned object contains a private key in hex-encoded and wif-encoded format, a public key, and a public address.

## Generate Multisig Address Endpoint

```shell
curl -d '{"pubkeys": ["02c716d071a76cbf0d29c29cacfec76e0ef8116b37389fb7a3e76d6d32cf59f4d3", "033ef4d5165637d99b673bcdbb7ead359cee6afd7aaf78d3da9d2392ee4102c8ea", "022b8934cc41e76cb4286b9f3ed57e2d27798395b04dd23711981a77dc216df8ca"], "script_type": "multisig-2-of-3"}' https://api.blockcypher.com/v1/btc/main/addrs

{
"private": "",
"public": "",
"address": "3BF1M1PnTge94QewuWh3B8mRVw8U4SVnb4",
"wif": "",
"pubkeys": [
	"02c716d071a76cbf0d29c29cacfec76e0ef8116b37389fb7a3e76d6d32cf59f4d3",
	"033ef4d5165637d99b673bcdbb7ead359cee6afd7aaf78d3da9d2392ee4102c8ea",
	"022b8934cc41e76cb4286b9f3ed57e2d27798395b04dd23711981a77dc216df8ca"
],
"script_type": "multisig-2-of-3"
}
```

```javascript
var data = {
  "pubkeys": [
    "02c716d071a76cbf0d29c29cacfec76e0ef8116b37389fb7a3e76d6d32cf59f4d3",
    "033ef4d5165637d99b673bcdbb7ead359cee6afd7aaf78d3da9d2392ee4102c8ea",
    "022b8934cc41e76cb4286b9f3ed57e2d27798395b04dd23711981a77dc216df8ca"
  ],
  "script_type": "multisig-2-of-3"
};
$.post('https://api.blockcypher.com/v1/btc/test3/addrs', data)
  .then(function(d) {console.log(d)});
> {
> "private": "",
> "public": "",
> "address": "3BF1M1PnTge94QewuWh3B8mRVw8U4SVnb4",
> "wif": "",
> "pubkeys": [
> 	"02c716d071a76cbf0d29c29cacfec76e0ef8116b37389fb7a3e76d6d32cf59f4d3",
> 	"033ef4d5165637d99b673bcdbb7ead359cee6afd7aaf78d3da9d2392ee4102c8ea",
> 	"022b8934cc41e76cb4286b9f3ed57e2d27798395b04dd23711981a77dc216df8ca"
> ],
> "script_type": "multisig-2-of-3"
> }
```

```ruby
> block_cypher.address_generate_multi(["02c716d071a76cbf0d29c29cacfec76e0ef8116b37389fb7a3e76d6d32cf59f4d3", "033ef4d5165637d99b673bcdbb7ead359cee6afd7aaf78d3da9d2392ee4102c8ea", "022b8934cc41e76cb4286b9f3ed57e2d27798395b04dd23711981a77dc216df8ca"], "multisig-2-of-3")
=> {"private"=>"",
 "public"=>"",
 "address"=>"3BF1M1PnTge94QewuWh3B8mRVw8U4SVnb4",
 "wif"=>"",
 "pubkeys"=>
  ["02c716d071a76cbf0d29c29cacfec76e0ef8116b37389fb7a3e76d6d32cf59f4d3",
   "033ef4d5165637d99b673bcdbb7ead359cee6afd7aaf78d3da9d2392ee4102c8ea",
   "022b8934cc41e76cb4286b9f3ed57e2d27798395b04dd23711981a77dc216df8ca"],
 "script_type"=>"multisig-2-of-3"}
```

```python
>>> import requests, json
>>> data = {'pubkeys': ['02c716d071a76cbf0d29c29cacfec76e0ef8116b37389fb7a3e76d6d32cf59f4d3', '033ef4d5165637d99b673bcdbb7ead359cee6afd7aaf78d3da9d2392ee4102c8ea', '022b8934cc41e76cb4286b9f3ed57e2d27798395b04dd23711981a77dc216df8ca'], 'script_type': 'multisig-2-of-3'}
>>> r = requests.post('https://api.blockcypher.com/v1/btc/main/addrs', data=json.dumps(data))
>>> r.json()
{'public': '',
 'private': '',
 'script_type': 'multisig-2-of-3',
 'pubkeys': ['02c716d071a76cbf0d29c29cacfec76e0ef8116b37389fb7a3e76d6d32cf59f4d3',
  '033ef4d5165637d99b673bcdbb7ead359cee6afd7aaf78d3da9d2392ee4102c8ea',
  '022b8934cc41e76cb4286b9f3ed57e2d27798395b04dd23711981a77dc216df8ca'],
 'address': '3BF1M1PnTge94QewuWh3B8mRVw8U4SVnb4',
 'wif': ''}
```

The Generate Multisig Address Endpoint is a convenience method to help you generate multisig addresses from multiple public keys. After supplying a partially filled-out [AddressKeychain](#addresskeychain) object (including only an array of hex-encoded public keys and the script type), the returned object includes the computed public address.

<aside class="notice">
This endpoint is the same resource as the <a href="#generateadressendpoint">Generate Address Endpoint</a>, but with data in the request body.
</aside>

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/addrs | POST | [AddressKeychain](#addresskeychain) | [AddressKeychain](#addresskeychain)

## Wallets

The Wallet API allows you to group multiple addresses under a single name. It only holds public address information and never requires any private keys. 

<aside class="notice">
Don't be confused: this Wallet API has nothing to do with private key management. The best way to think of the Wallet API is a "Set of Public Addresses to Query Together" API, but that name refused to fit into any of our marketing materials.
</aside>

A wallet can be created, deleted, and have addresses added and removed. The wallet itself can have any custom name as long as it does not start with the standard address prefix (1 or 3 for Bitcoin).

Wallets can be leveraged by the [Address API](#address-api), just by using their name instead of $ADDRESS. They can also be used with [Events](#events-and-hooks) and with the [Transaction API](#transaction-api). In general, using a wallet instead of an address in an API will have the effect of [batching the set of addresses](#batching) contained in the wallet.

<aside class="warning">
You are required to authenticate with your <a href="#rate-limits-and-tokens">user token</a> when using a wallet in any API endpoint. You can register <a href="http://accounts.blockcypher.com/">for a token here.</a>
</aside>

The following code examples should be considered serially; that is to say, the results will appear as if each API call were done sequentially. Also, $NAME is a *string* representing the name of your wallet, for example:

`alice`
<br />

### Create Wallet Endpoint

```shell
curl -d '{"name": "alice","addresses": ["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]}' https://api.blockcypher.com/v1/btc/main/wallets?token=YOURTOKEN

{"token": "YOURTOKEN",
"name": "alice",
"addresses": [
	"1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
]}
```

```javascript
var data = {"name": "alice","addresses": ["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]};
$.post('https://api.blockcypher.com/v1/btc/main/wallets?token=USERTOKEN', data)
  .then(function(d) {console.log(d)});
> {"token": "USERTOKEN",
> "name": "alice",
> "addresses": [
> 	"1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
> ]}
```

```python
>>> import requests, json
>>> data = {'name': 'alice', 'addresses': ['1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e', ]}
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.post('https://api.blockcypher.com/v1/btc/main/wallets', data=json.dumps(data), params=params)
>>> r.json()
{'name': 'alice',
 'token': 'YOUR_TOKEN',
 'addresses': ['1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e']}
```

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/wallets | POST | [Wallet](#wallet) | [Wallet](#wallet)

This endpoint allows you to create a new wallet, by POSTing a partially filled out [Wallet](#wallet) object. At minimum, you must include the **name** attribute and at least one public address in the **addresses** array. If successful, it will return the same [Wallet](#wallet) object you requested, appended with your user token.

<aside class="warning">
If the named wallet already exists under your token, attempting to create a new wallet will return an error.
</aside>

<br />

### Get Wallet Endpoint

```shell
curl https://api.blockcypher.com/v1/btc/main/wallets/alice?token=YOURTOKEN

{"token":"YOURTOKEN",
"name":"alice",
"addresses": [ "1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]}
```

```javascript
$.get('https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses?token=YOURTOKEN')
  .then(function(d) {console.log(d)});
> {
> "token":"YOURTOKEN",
> "name":"alice",
> "addresses": [
> 	"1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
> ]}
```

```python
>>> import requests
>>> params = {'token': 'YOURTOKEN'}
>>> r = requests.get('https://api.blockcypher.com/v1/btc/main/wallets/alice', params=params)
>>> r.json()
{'name': 'alice',
 'token': 'YOURTOKEN',
 'addresses': ['1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e']}
```

Resource | Method | Return Object
-------- | ------ | -------------
/wallets/$NAME | GET | [Wallet](#wallet)

This endpoint returns a [Wallet](#wallet) based on its $NAME.

### Add Addresses to Wallet Endpoint

```shell
curl -d '{"addresses": ["13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j"]}' https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses?token=YOURTOKEN

{"token": "YOURTOKEN",
"name": "alice",
"addresses": [
	"1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e",
	"13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j"
]}
```

```javascript
var data = {"addresses": ["13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j"]};
$.post('https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses?token=USERTOKEN', data)
  .then(function(d) {console.log(d)});
> {
>   "token": "USERTOKEN",
>   "name": "alice",
>   "addresses": [
>   	"1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e",
>   	"13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j"
> ]}
```

```python
>>> import requests, json
>>> data = {'addresses': ['13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j', ]}
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.post('https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses', data=json.dumps(data), params=params)
>>> r.json()
{'name': 'alice',
 'token': 'YOUR_TOKEN',
 'addresses': ['1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e', '13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j']}
```

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/wallets/$NAME/addresses | POST | [Wallet](#wallet) | [Wallet](#wallet)

This endpoint allows you to add public addresses to the $NAME wallet, by POSTing a partially filled out [Wallet](#wallet) object. You only need to include the additional addresses in a new **addresses** array in the object. If successful, it will return the newly modified [Wallet](#wallet), including an up-to-date, complete listing of addresses.

<br />

### Get Wallet Addresses Endpoint

```shell
curl https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses?token=YOURTOKEN

{"addresses": [
	"13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j",
	"1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
]}
```

```javascript
$.get('https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses?token=USERTOKEN')
  .then(function(d) {console.log(d)});
> {
> "addresses": [
> 	"13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j",
> 	"1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
> ]}
```

```python
>>> import requests
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.get('https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses', params=params)
>>> r.json()
{'addresses': ['13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j',
  '1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e']}
```

Resource | Method | Return Object
-------- | ------ | -------------
/wallets/$NAME/addresses | GET | [Wallet](#wallet)

This endpoint returns a list of the addresses associated with the $NAME wallet. It returns the addresses in a partially filled out [Wallet](#wallet) object, which you'll find under the **addresses** attribute.

<br />

### Remove Addresses from Wallet Endpoint

```shell
$ curl -X DELETE https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses?token=USERTOKEN&address=1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e

```

```javascript
$.ajax({
  url: "https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses?token=USERTOKEN&address=1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e",
  method: "DELETE"
})
```

```python
>>> import requests, json
>>> data = {'token': 'YOUR_TOKEN', 'address': '1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e'}
>>> r = requests.delete('https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses', data=json.dumps(data))
# returns nothing, let's just check the status code to be sure
>>> assert r.status_code == 204
```

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/wallets/$NAME/addresses?address=$ADDRESS | DELETE | *nil* | *nil*

This endpoint allows you to delete an $ADDRESS associated with the $NAME wallet. As a reminder, you [can batch](#http://dev.blockcypher.com/#batching) multiple addresses by appending them with semicolons within the $ADDRESS URL parameter. If successful, it will return an HTTP 204 status code with no return object.

<br />

### Generate Address in Wallet Endpoint

```shell
curl -X POST https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses/generate?token=YOURTOKEN

{"token": "YOURTOKEN",
"name": "alice",
"addresses": [
	"13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j",
	"14LcPtRSGjYb1s8kfxsVDbXvA7VYCmoFho"
],
"private": "6238efeb679d75ec3b1a43e76cc0ed33abdf56e30bb5bb95e4793134a7958609",
"public": "03e4f273521a30373a639f60da836f2308a5d53853ec18f903dd235c73e6e26e4a",
"address": "14LcPtRSGjYb1s8kfxsVDbXvA7VYCmoFho",
"wif": "KzWeDL7sysRay7pZUm6hQQLaDVjmN1jUZzeZuq6ru5FtN1RhPrLX"}
```

```javascript
$.post('https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses/generate?token=USERTOKEN')
  .then(function(d) {console.log(d)});
> {
>   "token": "USERTOKEN",
>   "name": "alice",
>   "addresses": [
>   	"13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j",
>   	"14LcPtRSGjYb1s8kfxsVDbXvA7VYCmoFho"
>   ],
>   "private": "6238efeb679d75ec3b1a43e76cc0ed33abdf56e30bb5bb95e4793134a7958609",
>   "public": "03e4f273521a30373a639f60da836f2308a5d53853ec18f903dd235c73e6e26e4a",
>   "address": "14LcPtRSGjYb1s8kfxsVDbXvA7VYCmoFho",
>   "wif": "KzWeDL7sysRay7pZUm6hQQLaDVjmN1jUZzeZuq6ru5FtN1RhPrLX"
> }
```

```python
# Please use the python library at https://github.com/blockcypher/blockcypher-python to securely generate an address client-side
```

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/wallets/$NAME/addresses/generate | POST | *nil* | [Wallet](#Wallet) + [AddressKeychain](#AddressKeychain)

This endpoint allows you to generate a new address associated with the $NAME wallet, similar to the [Generate Address Endpoint](#generate-address-endpoint). If successful, it will returned the newly modified [Wallet](#wallet) composed with an [AddressKeychain](#AddressKeychain).

<br />
<br />
<br />
<br />
<br />

### Delete Wallet Endpoint

```shell
curl -X DELETE https://api.blockcypher.com/v1/btc/main/wallets/alice?token=YOURTOKEN

```

```javascript
$.ajax({
  url: "https://api.blockcypher.com/v1/btc/main/wallets/alice?token=USERTOKEN",
  method: "DELETE"
});
```

```python
>>> import requests
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.delete('https://api.blockcypher.com/v1/btc/main/wallets/alice', params=params)
# returns nothing, let's just check the status code to be sure
>>> assert r.status_code == 204
```

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/wallets/$NAME | DELETE | [Wallet](#wallet) | *nil*

This endpoint deletes the wallet with $NAME. If successful, it will return an HTTP 204 status code with no return object.
