# Transaction API

BlockCypher's Transaction API allows you to look up information about unconfirmed transactions, query transactions based on hash, and create and propagate your own transactions, including multisignature transactions---all [based on the coin/chain resource](#restful-resources) you've selected for your endpoints.

If you're new to blockchains, the idea of transactions is relatively self-explanatory. Here's what's going on underneath the hood: a transaction takes previous "unspent transaction outputs" (also known as [UTXOs](https://bitcoin.org/en/glossary/unspent-transaction-output)) as "transaction inputs" and creates new "locking scripts" on those inputs such that they are "sent" to new addresses (to become new UTXOs). While most of these public addresses are reference points for single private keys that can "unlock" the newly created UTXOs, occasionally they are sent to more exotic addresses through *pay-to-script-hash*, typically multisignature addresses.

Generally speaking, UTXOs are generated from previous transactions (except for [Coinbase inputs](https://bitcoin.org/en/glossary/coinbase)).

<aside class="success">
Even if you don't use the API to assist in creating transactions, we highly recommend using BlockCypher to push your raw transactions. BlockCypher maintains connections to 20% of all nodes on all blockchains it supports, which means we can propagate transactions faster than almost anyone else. Given our speed and reliability, this is especially powerful when used in concert with our <a href="#confidence-factor">Confidence Factor.</a>
</aside>

## Transaction Hash Endpoint

```shell
curl https://api.blockcypher.com/v1/btc/main/txs/f854aebae95150b379cc1187d848d58225f3c4157fe992bcd166f58bd5063449

{
"block_hash": "0000000000000000c504bdea36e5...",
"block_height": 293000,
"hash": "f854aebae95150b379cc1187d848d58225f...",
"addresses": [
	"13XXaBufpMvqRqLkyDty1AXqueZHVe6iyy",
	"19YtzZdcfs1V2ZCgyRWo8i2wLT8ND1Tu4L",
	"1BNiazBzCxJacAKo2yL83Wq1VJ18AYzNHy",
	"1GbMfYui17L5m6sAy3L3WXAtf1P32bxJXq",
	"1N2f642sbgCMbNtXFajz9XDACDFnFzdXzV"
],
"total": 70320221545,
"fees": 0,
"size": 636,
"preference": "low",
"relayed_by": "",
"confirmed": "2014-03-29T01:29:19Z",
"received": "2014-03-29T01:29:19Z",
"ver": 1,
"lock_time": 0,
"double_spend": false,
"vin_sz": 4,
"vout_sz": 1,
"confirmations": 64373,
"confidence": 1,
"inputs": [
	{
	"prev_hash": "583910b7bf90ab802e22e5c25a89b59...",
	"output_index": 1,
	"script": "4830450220504b1ccfddf508422bdd8b0f...",
	"output_value": 16450000,
	"sequence": 4294967295,
	"addresses": [
		"1GbMfYui17L5m6sAy3L3WXAtf1P32bxJXq"
	],
	"script_type": "pay-to-pubkey-hash"
	},
	...
],
"outputs": [
	{
	"value": 70320221545,
	"script": "76a914e6aad9d712c419ea8febf009a3f3...",
	"spent_by": "35832d6c70b98b54e9a53ab2d51176eb...",
	"addresses": [
		"1N2f642sbgCMbNtXFajz9XDACDFnFzdXzV"
	],
	"script_type": "pay-to-pubkey-hash"
	}
]
}
```

```javascript
$.get('https://api.blockcypher.com/v1/btc/main/txs/f854aebae95150b379cc1187d848d58225f3c4157fe992bcd166f58bd5063449').then(function(d) {console.log(d)});
> {
> "block_hash": "0000000000000000c504bdea36e5...",
> "block_height": 293000,
> "hash": "f854aebae95150b379cc1187d848d58225f...",
> "addresses": [
> 	"13XXaBufpMvqRqLkyDty1AXqueZHVe6iyy",
> 	"19YtzZdcfs1V2ZCgyRWo8i2wLT8ND1Tu4L",
> 	"1BNiazBzCxJacAKo2yL83Wq1VJ18AYzNHy",
> 	"1GbMfYui17L5m6sAy3L3WXAtf1P32bxJXq",
> 	"1N2f642sbgCMbNtXFajz9XDACDFnFzdXzV"
> ],
> "total": 70320221545,
> "fees": 0,
> "size": 636,
> "preference": "low",
> "relayed_by": "",
> "confirmed": "2014-03-29T01:29:19Z",
> "received": "2014-03-29T01:29:19Z",
> "ver": 1,
> "lock_time": 0,
> "double_spend": false,
> "vin_sz": 4,
> "vout_sz": 1,
> "confirmations": 64373,
> "confidence": 1,
> "inputs": [
> 	{
> 	"prev_hash": "583910b7bf90ab802e22e5c25a89b59...",
> 	"output_index": 1,
> 	"script": "4830450220504b1ccfddf508422bdd8b0f...",
> 	"output_value": 16450000,
> 	"sequence": 4294967295,
> 	"addresses": [
> 		"1GbMfYui17L5m6sAy3L3WXAtf1P32bxJXq"
> 	],
> 	"script_type": "pay-to-pubkey-hash"
> 	},
> 	...
> ],
> "outputs": [
> 	{
> 	"value": 70320221545,
> 	"script": "76a914e6aad9d712c419ea8febf009a3f3...",
> 	"spent_by": "35832d6c70b98b54e9a53ab2d51176eb...",
> 	"addresses": [
> 		"1N2f642sbgCMbNtXFajz9XDACDFnFzdXzV"
> 	],
> 	"script_type": "pay-to-pubkey-hash"
> 	}
> ]
> }
```

```python
>>> import requests
>>> r = requests.get('https://api.blockcypher.com/v1/btc/main/txs/f854aebae95150b379cc1187d848d58225f3c4157fe992bcd166f58bd5063449')
>>> r.json()
{'confirmed': '2014-03-29T01:29:19Z',
 'ver': 1,
 'double_spend': False,
 'outputs': [{'value': 70320221545,
   'addresses': ['1N2f642sbgCMbNtXFajz9XDACDFnFzdXzV'],
   'script_type': 'pay-to-pubkey-hash',
   'script': '76a914e6aad9d712c419ea8febf009a3f3bfdd8d222fac88ac',
   'spent_by': '35832d6c70b98b54e9a53ab2d51176eb19ad11bc4505d6bb1ea6c51a68cb92ee'}],
 'block_height': 293000,
 'confidence': 1,
 'total': 70320221545,
 'block_hash': '0000000000000000c504bdea36e531d8089d324f2d936c86e3274f97f8a44328',
 'fees': 0,
 'inputs': [{'output_value': 16450000,
   'sequence': 4294967295,
   'prev_hash': '583910b7bf90ab802e22e5c25a89b59862b20c8c1aeb24dfb94e7a508a70f121',
   'script': '4830450220504b1ccfddf508422bdd8b0fcda2b1483e87aee1b486c0130bc29226bbce3b4e022100b5befcfcf0d3bf6ebf0ac2f93badb19e3042c7bed456c398e743b885e782466c012103b1feb40b99e8ff18469484a50e8b52cc478d5f4f773a341fbd920a4ceaedd4bf',
   'addresses': ['1GbMfYui17L5m6sAy3L3WXAtf1P32bxJXq'],
   'output_index': 1,
   'script_type': 'pay-to-pubkey-hash'},
   ...
],
 'preference': 'low',
 'vout_sz': 1,
 'vin_sz': 4,
 'relayed_by': '',
 'received': '2014-03-29T01:29:19Z',
 'hash': 'f854aebae95150b379cc1187d848d58225f3c4157fe992bcd166f58bd5063449',
 'addresses': ['13XXaBufpMvqRqLkyDty1AXqueZHVe6iyy',
  '19YtzZdcfs1V2ZCgyRWo8i2wLT8ND1Tu4L',
  '1BNiazBzCxJacAKo2yL83Wq1VJ18AYzNHy',
  '1GbMfYui17L5m6sAy3L3WXAtf1P32bxJXq',
  '1N2f642sbgCMbNtXFajz9XDACDFnFzdXzV'],
 'lock_time': 0,
 'size': 636,
 'confirmations': 67378}
```


The Transaction Hash Endpoint returns detailed information about a given transaction based on its hash.


Resource | Method | Return Object
-------- | ------ | -------------
/txs/$TXHASH | GET | [TX](#tx)

TXHASH is a *string* representing the hex-encoded transaction hash you're interested in querying, for example:

`f854aebae95150b379cc1187d848d58225f3c4157fe992bcd166f58bd5063449`

The returned object contains detailed information about the transaction, including the value transfered, date received, and a full listing of inputs and outputs.

## Unconfirmed Transactions Endpoint

```shell
curl http://api.blockcypher.com/v1/btc/main/txs

[
	{
	"block_height": -1,
	"hash": "7f3af38d988e479f651520b13b85...",
	"addresses": [
		"19ZZonTmBD3VBhgFSWrtNis6Wo4aNRyjAV",
		"1GD2Gnt3Vk51FpUhZWrFpSa24KE2ZGPaWG",
		"1HEJiW8jxvUvVQ5cuButRmFj6ZzKzM9Fp6"
	],
	"total": 46977000,
	"fees": 10000,
	"size": 225,
	"preference": "medium",
	"relayed_by": "54.209.56.58:8333",
	...
	},
	{
	"block_height": -1,
	"hash": "7e2d8b4e95c8246e81f297539409...",
	"addresses": [
		"12u8EFJR6QJpNYbuQcYt2iMsAtPdnCFwQb",
		"1MUbYoqSadRSEipwcmKnH37NaKakDCmgwW"
	],
	"total": 1070000,
	"fees": 10000,
	"size": 225,
	"preference": "medium",
	"relayed_by": "52.1.229.213:8333",
	"received": "2015-05-21T05:44:13.878Z",
	...
	},
...
]
```

```javascript
$.get('https://api.blockcypher.com/v1/btc/main/txs').then(function(d) {console.log(d)});
> [
> 	{
> 	"block_height": -1,
> 	"hash": "7f3af38d988e479f651520b13b85...",
> 	"addresses": [
> 		"19ZZonTmBD3VBhgFSWrtNis6Wo4aNRyjAV",
> 		"1GD2Gnt3Vk51FpUhZWrFpSa24KE2ZGPaWG",
> 		"1HEJiW8jxvUvVQ5cuButRmFj6ZzKzM9Fp6"
> 	],
> 	"total": 46977000,
> 	"fees": 10000,
> 	"size": 225,
> 	"preference": "medium",
> 	"relayed_by": "54.209.56.58:8333",
> 	...
> 	},
> 	{
> 	"block_height": -1,
> 	"hash": "7e2d8b4e95c8246e81f297539409...",
> 	"addresses": [
> 		"12u8EFJR6QJpNYbuQcYt2iMsAtPdnCFwQb",
> 		"1MUbYoqSadRSEipwcmKnH37NaKakDCmgwW"
> 	],
> 	"total": 1070000,
> 	"fees": 10000,
> 	"size": 225,
> 	"preference": "medium",
> 	"relayed_by": "52.1.229.213:8333",
> 	"received": "2015-05-21T05:44:13.878Z",
> 	...
> 	},
> ...
> ]
```

```python
# Fund existing address with faucet
>>> import requests
>>> r = requests.get('http://api.blockcypher.com/v1/btc/main/txs')
>>> r.json()
[{'received': '2015-06-10T23:10:31.534Z',
  'ver': 1,
  'double_spend': False,
  'outputs': [{'value': 131910000,
    'addresses': ['1Gb4TAU4GD73akvt4V7tg2WdqadcpQSvhH'],
    'script_type': 'pay-to-pubkey-hash',
    'script': '76a914aafae908428a1778d0e76f0dca4cb800f731873e88ac'},
    ...
    ],
  'block_height': -1,
  'confidence': 0.36208077122201,
  'receive_count': 109,
  'total': 131940196,
  'fees': 13754,
  'inputs': [{'output_value': 9467,
    'sequence': 4294967295,
    'prev_hash': '3c65f3bdec16c55fcedec8159ca8b7decf12393e3a7088febbfacfd752534ee0',
    'script': '47304402207510efc3ff2ed868478ef627c4fd53a27254874e61c1ff1be6fb5221844c08ac02204a8a50c35d3fac8ec0c15f468c98abb629cadf3ac898f7be5e6f464ba9e4fba00121035649100ba29d0b34df8a928c2ed7ab1389d017b4ed206f6df5c212a461a764f2',
    'age': 38,
    'addresses': ['1MBuZ5ZxhytsjRZmec5rLr1WhDWm9DgFUJ'],
    'output_index': 1,
    'script_type': 'pay-to-pubkey-hash'},
    ...
   ],
  'preference': 'medium',
  'vout_sz': 2,
  'vin_sz': 3,
  'relayed_by': '167.114.118.213:8333',
  'hash': '97f551c7d200f3acf322160d6ada87830d2ad8d935909630a35a87d61bc8fa74',
  'addresses': ['1DLDFKuAWGUJqZoYrevG8bHufuZCjCdSFZ',
   '1Gb4TAU4GD73akvt4V7tg2WdqadcpQSvhH',
   ...
   ]
  'lock_time': 0,
  'size': 521,
  'confirmations': 0},
 ...
],
```

The Unconfirmed Transactions Endpoint returns an array of the latest transactions relayed by nodes in a blockchain that haven't been included in any blocks.

Resource | Method | Return Object
-------- | ------ | -------------
/txs | GET | Array[[TX](#TX)]

The returned object is an array of transactions that haven't been included in blocks, arranged in reverse chronological order (latest is first, then older transactions follow).

<aside class="notice">
Due to <a href="https://bitcoin.org/en/glossary/malleability">transaction malleability</a> it can be difficult to deal with transaction hashes before they've been confirmed in blocks. Use caution, and consider applying our <a href="#confidence-factor">Confidence Factor</a> to mitigate potential issues.
</aside>

## Creating Transactions

Using BlockCypher's API, you can push transactions to blockchains one of two ways:

- Use a third party library to create your transactions and [push raw transactions](#push-raw-transaction-endpoint)
- Use our two-endpoint process outlined below, wherein we generate a [TXSkeleton](#txskeleton) based on your input address, output address, and value to transfer.

In either case, for security reasons, we never take possession of your private keys. We do use private keys with our [Microtransaction API](#microtransaction-api), but they are for low-value transactions and we discard them immediately from our servers' memory.

<aside class="warning">
Always use HTTPS for creating and pushing transactions.
</aside>

### New Transaction Endpoint

```shell
curl -d '{"inputs":[{"addresses": ["CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"]}],"outputs":[{"addresses": ["C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"], "value": 1000000}]}' https://api.blockcypher.com/v1/bcy/test/txs/new

{
"tx": {
	"block_height": -1,
	"hash": "c2b350b273b3bf04791d8e59fc9c021fd91fa423c50c29473dc079150f5a778a",
	"addresses": [
		"CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9",
		"C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
	],
	"total": 4988000,
	"fees": 12000,
	"size": 119,
	"preference": "high",
	...,
	"confirmations": 0,
	"inputs": [
		{
		"prev_hash": "c8ea8b221580ebb2f1cabc8b40797bffec742b97c82a329df96d93121db43519",
		"output_value": 5000000,
		"addresses": ["CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"],
		"script": "",
		"script_type": "",
		...
		}
	],
	"outputs": [
		{
		"value": 1000000,
		"script": "76a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac",
		"addresses": ["C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"],
		"script_type": "pay-to-pubkey-hash"
		},
		{
		"value": 3988000,
		"script": "76a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac",
		"addresses": ["CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"],
		"script_type": "pay-to-pubkey-hash"
		}
	]
},
"tosign": ["32b5ea64c253b6b466366647458cfd60de9cd29d7dc542293aa0b8b7300cd827"]
}
```

```javascript
var newtx = {
  inputs: [{addresses: ['CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9']}],
  outputs: [{addresses: ['C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn'], value: 100000}]
};
$.post('https://api.blockcypher.com/v1/bcy/test/txs/new', JSON.stringify(newtx))
  .then(function(d) {console.log(d)});
> {
>   "tx": {
>     "block_height": -1,
>     "hash": "c2b350b273b3bf04791d8e59fc9c021fd91fa423c50c29473dc079150f5a778a",
>     "addresses": [
>       "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9",
>       "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
>     ],
>     "total": 4988000,
>     "fees": 12000,
>     "size": 119,
>     "preference": "high",
>     "relayed_by": "208.71.159.84",
>     "received": "2015-05-21T19:04:14.492743867Z",
>     "ver": 1,
>     "lock_time": 0,
>     "double_spend": false,
>     "vin_sz": 1,
>     "vout_sz": 2,
>     "confirmations": 0,
>     "inputs": [
>       {
>         "prev_hash": "c8ea8b221580ebb2f1cabc8b40797bffec742b97c82a329df96d93121db43519",
>         "output_index": 0,
>         "script": "",
>         "output_value": 5000000,
>         "sequence": 4294967295,
>         "addresses": [
>           "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
>         ],
>         "script_type": "",
>         "age": 4
>       }
>     ],
>     "outputs": [
>       {
>         "value": 1000000,
>         "script": "76a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac",
>         "addresses": [
>           "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
>         ],
>         "script_type": "pay-to-pubkey-hash"
>       },
>       {
>         "value": 3988000,
>         "script": "76a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac",
>         "addresses": [
>           "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
>         ],
>         "script_type": "pay-to-pubkey-hash"
>       }
>     ]
>   },
>   "tosign": [
>     "32b5ea64c253b6b466366647458cfd60de9cd29d7dc542293aa0b8b7300cd827"
>   ]
> }
```

```python
>>> import requests, json
>>> data = {'inputs': [{'addresses': ['CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9',]}], 'outputs': [{'addresses': ['C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn', ], 'value': 1000000}]}
>>> r = requests.post('https://api.blockcypher.com/v1/bcy/test/txs/new', data=json.dumps(data))
>>> r.json()
{'tosign': ['d65bd083f968a2002e6d71044c0aaa601ec45dfdd6dc9168907141bda4f31fbd'],
 'tx': {'received': '2015-06-10T23:16:15.28508806Z',
  'ver': 1,
  'double_spend': False,
  'outputs': [{'value': 1000000,
    'addresses': ['C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn'],
    'script_type': 'pay-to-pubkey-hash',
    'script': '76a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac'},
   {'value': 2856000,
    'addresses': ['CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9'],
    'script_type': 'pay-to-pubkey-hash',
    'script': '76a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac'}],
  'block_height': -1,
  'total': 3856000,
  'fees': 12000,
  'inputs': [{'output_value': 3868000,
    'sequence': 4294967295,
    'prev_hash': '6b1c30cad97df956cfcb47b4cd471bb69112fb726b0fb129575337e3fb9f2c1a',
    'script': '',
    'age': 294,
    'addresses': ['CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9'],
    'output_index': 2,
    'script_type': ''}],
  'preference': 'high',
  'vout_sz': 2,
  'vin_sz': 1,
  'relayed_by': '207.38.134.25',
  'hash': '4508ebb031af94173c58d3848683308ccb8d67c08952feb3d36406066c030bf5',
  'addresses': ['CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9',
   'C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn'],
  'lock_time': 0,
  'size': 119,
  'confirmations': 0}}
```

To use BlockCypher's two-endpoint transaction creation tool, first you need to provide the input address(es), output address, and value to transfer (in satoshis). Provide this in a partially-filled out [TX](#tx) request object.

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/txs/new | POST | [TX](#tx) | [TXSkeleton](#txskeleton)

As you can see from the code example, you only need to provide a single public address within the **addresses** array of both the **input** and **output** of your [TX](#tx) request object. You also need to fill in the **value** with the amount you'd like to transfer from one address to another.

If you'd like, you can even use a [Wallet](#wallet) instead of addresses as your input. You just need to use two non-standard fields (your **wallet_name** and **wallet_token**) within the **inputs** array in your transaction, instead of **addresses**:

`{inputs:[{"wallet_name":"alice", "wallet_token":"YOURTOKEN"}], value: 5000000}`

While this particular usage will differ between client libraries, the result is the same: the addresses within your wallet will be used as the **inputs**, as if all of them had been placed within the **addresses** array.

As a return object, you'll receive a [TXSkeleton](#txskeleton) containing a slightly-more complete [TX](#tx) alongside data you need to sign in the **tosign** array. You'll need this object for the next steps of the transaction creation process.

<aside class="notice">
If you want to automatically empty your input address(es) without knowing their exact value, your <a href="#tx">TX</a> request object's <b>value</b> can be set to -1 to <i>sweep</i> all value from your input address(es) to your output address.
</aside>

<aside class="notice">
There are many manually configurable options available via your <a href="#tx">TX</a> request object. You can read about them in more detail in the <a href="#customizing-transaction-requests">Customizing Transaction Requests</a> section below.
</aside>

<aside class="warning">
The <b>hash</b> assigned to <a href="#tx">TX</a> within the return object is only temporary. A final <b>hash</b> will be provided after the transaction is sent to the network through the Send Transaction Endpoint.
</aside>

```shell
# next, you sign the data returned in the tosign array locally
# here we're using our signer tool (https://github.com/blockcypher/btcutils/tree/master/signer), but any ECDSA secp256k1 signing tool should work
# $PRIVATEKEY here is a hex-encoded private key corresponding to the input from address CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9 

./signer 32b5ea64c253b6b466366647458cfd60de9cd29d7dc542293aa0b8b7300cd827 $PRIVATEKEY

3045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca
```

```javascript
// next, you sign the data returned in the tosign array locally
// here we're sing bitcoinjs built to expose bigi, buffer and require or directly:
//   https://blockcypher.github.io/documentation/js/samples/bitcoinjs-min.js

var bitcoin = require("bitcoinjs-lib");
var bigi    = require("bigi");
var buffer  = require('buffer');
var key     = new bitcoin.ECKey(bigi.fromHex(my_hex_private_key), true);

var newtx = {
  inputs: [{addresses: ['CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9']}],
  outputs: [{addresses: ['C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn'], value: 100000}]
};
// calling the new endpoint, same as above
$.post('https://api.blockcypher.com/v1/bcy/test/txs/new', JSON.stringify(newtx))
  .then(function(tmptx) {
    // signing each of the hex-encoded string required to finalize the transaction
    tmptx.pubkeys = [];
    tmptx.signatures = tmptx.tosign.map(function(tosign, n) {
      tmptx.pubkeys.push(key.pub.toHex());
      return key.sign(new buffer.Buffer(tosign, "hex")).toDER().toString("hex");
    });
    // sending back the transaction with all the signatures to broadcast
    $.post(url, tmptx).then(function(finaltx) {
      console.log(finaltx);
    })
  });

```

### Locally Sign Your Transaction

With your [TXSkeleton](#txskeleton) returned from the New Transaction Endpoint, you now need to use your private key(s) to sign the data provided in the **tosign** array.

Digital signing can be a difficult process, and is where the majority of issues arise when dealing with cryptocurrency transactions. We are working on integrating client-side signing solutions into our libraries to make this process easier. You can read more about [signing here.](https://bitcoin.org/en/developer-guide#term-signature) In the mean time, if you want to experiment with client-side signing, consider using our [signer tool](https://github.com/blockcypher/btcutils/tree/master/signer).

<aside class="notice">
One of the most common errors in the signing process is a data format mismatch. We always return and expect hex-encoded data, but oftentimes, standard signing libraries require byte arrays. Remember to convert your data, and always send hex-encoded signatures to BlockCypher.
</aside>

```shell
# the request body is truncated because it's huge, but it's the same as the returned object from above plus the signatures and public keys
curl -d '{"tx": {...}, "tosign": [ "32b5ea64c253b6b466366647458cfd60de9cd29d7dc542293aa0b8b7300cd827" ], "signatures": [ "3045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca" ], "pubkeys": [ "02152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044" ] }' https://api.blockcypher.com/v1/bcy/test/txs/send

{
"tx": {
	"block_height": -1,
	"hash": "4e6dfb1415b4fba5bd257c129847c70fbd4e45e41828079c4a282680528f3a50",
	"addresses": [
		"CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9",
		"C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
	],
	"total": 4988000,
	"fees": 12000,
	"size": 226,
	"preference": "high",
	...,
	"confirmations": 0,
	"inputs": [
		{
		"prev_hash": "c8ea8b221580ebb2f1cabc8b40797bffec742b97c82a329df96d93121db43519",
		"output_value": 5000000,
		"addresses": ["CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"],
		"script": "483045022100921fc36b911094280f...",
		"script_type": "pay-to-pubkey-hash",
		...
		}
	],
	"outputs": [
		{
		"value": 1000000,
		"script": "76a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac",
		"addresses": ["C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"],
		"script_type": "pay-to-pubkey-hash"
		},
		{
		"value": 3988000,
		"script": "76a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac",
		"addresses": ["CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"],
		"script_type": "pay-to-pubkey-hash"
		}
	]
},
"tosign": [ "" ]
}
```

```javascript
// this shows just the last step of sending your transaction, see the signing section
// for the fully integrated sample
// the request body is truncated because it's huge, but it's the same as the returned
// object from calling the new endpoint plus the signatures and public keys added in the
// signing section
var sendtx = {
  tx: {...},
  tosign: [
    "32b5ea64c253b6b466366647458cfd60de9cd29d7dc542293aa0b8b7300cd827"
  ],
  signatures: [
    "3045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca"
  ],
  pubkeys: [
    "02152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044"
  ]
};
$.post('https://api.blockcypher.com/v1/bcy/test/txs/send', JSON.stringify(sendtx))
  .then(function(d) {console.log(d)});
> {
>   "tx": {
>     "block_height": -1,
>     "hash": "4e6dfb1415b4fba5bd257c129847c70fbd4e45e41828079c4a282680528f3a50",
>     "addresses": [
>       "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9",
>       "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
>     ],
>     "total": 4988000,
>     "fees": 12000,
>     "size": 226,
>     "preference": "high",
>     "relayed_by": "73.162.198.68",
>     "received": "2015-05-22T04:38:57.470017042Z",
>     "ver": 1,
>     "lock_time": 0,
>     "double_spend": false,
>     "vin_sz": 1,
>     "vout_sz": 2,
>     "confirmations": 0,
>     "inputs": [
>       {
>         "prev_hash": "c8ea8b221580ebb2f1cabc8b40797bffec742b97c82a329df96d93121db43519",
>         "output_index": 0,
>         "script": "483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044",
>         "output_value": 5000000,
>         "sequence": 4294967295,
>         "addresses": [
>           "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
>         ],
>         "script_type": "pay-to-pubkey-hash",
>         "age": 546
>       }
>     ],
>     "outputs": [
>       {
>         "value": 1000000,
>         "script": "76a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac",
>         "addresses": [
>           "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
>         ],
>         "script_type": "pay-to-pubkey-hash"
>       },
>       {
>         "value": 3988000,
>         "script": "76a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac",
>         "addresses": [
>           "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
>         ],
>         "script_type": "pay-to-pubkey-hash"
>       }
>     ]
>   },
>   "tosign": [
>     ""
>   ]
> }
```

### Send Transaction Endpoint

Once you've finished signing the **tosign** array locally, put that (hex-encoded) data into the **signatures** array of the [TXSkeleton](#txskeleton). You also need to include the corresponding (hex-encoded) public key(s) in the **pubkeys** array, in the order of the addresses/inputs provided. Signature and public key order matters, so make sure they are returned in the same order as the inputs you provided.

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/txs/send | POST | [TXSkeleton](#txskeleton) | [TXSkeleton](#txskeleton)

If the transaction was successful, you'll receive a [TXSkeleton](#TXskeleton) with the completed [TX](#tx) (which contains its final **hash**) and an HTTP Status Code 201.

### Dealing with Errors

Signing and creating transactions can be one of the trickiest parts of using blockchains in your applications. Consequently, when an error is encountered when [Creating Transactions](#creating-transactions), we send back an HTTP Status Code 400 alongside a descriptive array of **errors** within the [TXSkeleton](#txskeleton).

<aside class="notice">
One of the most common errors we see are users who use uncompressed public keys when compressed public keys were used to generate the address; remember to be careful to use the right keys!
</aside>

## Customizing Transaction Requests

While we demonstrated the simplest use of our [two-endpoint process to create transactions](#creating-transactions), you can have finer-grain control by modifying the [TX](#tx) request object before sending to `/txs/new`.

By default, we allow unconfirmed UTXOs as inputs when creating transactions. If you only want to allow confirmed UTXOs, set the **confirmations** value in your [TX](#tx) request object to 1.

Instead of providing **addresses**, you can use **prev_hash** and **output_index** within the **inputs** array of your request object, in which case, we'll use the inputs as provided and not attempt to generate them from a list of addresses. We will compute change and fees the same way.

BlockCypher will set the change address to the first transaction input/address listed in the transaction. To redirect this default behavior, you can set an optional **change_address** field within the [TX](#tx) request object.

Fees in cryptocurrencies can be complex. We provide 2 different ways for you to control the fees included in your transactions:

1. Set the **preference** property in your [TX](#tx) request object to "high", "medium", or "low". This will calculate and include appropriate fees for your transaction to be included in the next 1-2 blocks, 3-6 blocks or 7 or more blocks respectively. You can see the explicit estimates per kilobyte for these high, medium, and low ranges by calling your base resource through the [Chain Endpoint.](#chain-endpoint) The default fee calculation is based on a "high" **preference**. A preference set to "zero" will set no fee.
1. Manually set the fee to a desired amount by setting the **fees** property in your [TX](#tx) request object. Note that a fee too low may result in an error for some transactions that would require it.

To learn more about fees, [bitcoinfees.com](http://bitcoinfees.com/) is a good resource.

For even more control, you can also change the **script_type** in the **output** of your partially filled [TX](#tx). You'll notice this used to powerful effect in the section on [Multisig Transactions](#multisig-transactions). These are the possible script types:

1. *pay-to-pubkey-hash* (most common transaction transferring to a public key hash, and the default behavior if no out)
1. *pay-to-multi-pubkey-hash* (multi-signatures transaction, now actually less used than *pay-to-script-hash* for this purpose)
1. *pay-to-pubkey* (used for mining transactions)
1. *pay-to-script-hash* (used for transactions relying on arbitrary scripts, now used primarily for multi-sig transactions)
1. *null-data* (sometimes called op-return; used to embed small chunks of data in the blockchain)
1. *empty* (no script present, mostly used for mining transaction inputs)
1. *unknown* (non-standard script)

## Push Raw Transaction Endpoint

```shell
curl -d '{"tx":"01000000011935b41d12936df99d322ac8972b74ecff7b79408bbccaf1b2eb8015228beac8000000006b483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044ffffffff0240420f00000000001976a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac20da3c00000000001976a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac00000000"}' https://api.blockcypher.com/v1/bcy/test/txs/push

{
  "block_height": -1,
  "hash": "4e6dfb1415b4fba5bd257c129847c70fbd4e45e41828079c4a282680528f3a50",
  "addresses": [
    "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9",
    "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
  ],
  "total": 4988000,
  "fees": 12000,
  "size": 226,
  "preference": "high",
  "relayed_by": "73.162.198.68",
  "received": "2015-05-22T05:10:00.305308666Z",
  "ver": 1,
  "lock_time": 0,
  "double_spend": false,
  "vin_sz": 1,
  "vout_sz": 2,
  "confirmations": 0,
  "inputs": [
    {
      "prev_hash": "c8ea8b221580ebb2f1cabc8b40797bffec742b97c82a329df96d93121db43519",
      "output_index": 0,
      "script": "483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044",
      "output_value": 5000000,
      "sequence": 4294967295,
      "addresses": [
        "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
      ],
      "script_type": "pay-to-pubkey-hash",
      "age": 576
    }
  ],
  "outputs": [
    {
      "value": 1000000,
      "script": "76a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac",
      "addresses": [
        "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
      ],
      "script_type": "pay-to-pubkey-hash"
    },
    {
      "value": 3988000,
      "script": "76a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac",
      "addresses": [
        "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
      ],
      "script_type": "pay-to-pubkey-hash"
    }
  ]
}
```

```javascript
var pushtx = {
  tx: "01000000011935b41d12936df99d322ac8972b74ecff7b79408bbccaf1b2eb8015228beac8000000006b483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044ffffffff0240420f00000000001976a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac20da3c00000000001976a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac00000000"
};
$.post('https://api.blockcypher.com/v1/bcy/test/txs/send', JSON.stringify(pushtx))
  .then(function(d) {console.log(d)});
> {
>   "block_height": -1,
>   "hash": "4e6dfb1415b4fba5bd257c129847c70fbd4e45e41828079c4a282680528f3a50",
>   "addresses": [
>     "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9",
>     "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
>   ],
>   "total": 4988000,
>   "fees": 12000,
>   "size": 226,
>   "preference": "high",
>   "relayed_by": "73.162.198.68",
>   "received": "2015-05-22T05:10:00.305308666Z",
>   "ver": 1,
>   "lock_time": 0,
>   "double_spend": false,
>   "vin_sz": 1,
>   "vout_sz": 2,
>   "confirmations": 0,
>   "inputs": [
>     {
>       "prev_hash": "c8ea8b221580ebb2f1cabc8b40797bffec742b97c82a329df96d93121db43519",
>       "output_index": 0,
>       "script": "483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044",
>       "output_value": 5000000,
>       "sequence": 4294967295,
>       "addresses": [
>         "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
>       ],
>       "script_type": "pay-to-pubkey-hash",
>       "age": 576
>     }
>   ],
>   "outputs": [
>     {
>       "value": 1000000,
>       "script": "76a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac",
>       "addresses": [
>         "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
>       ],
>       "script_type": "pay-to-pubkey-hash"
>     },
>     {
>       "value": 3988000,
>       "script": "76a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac",
>       "addresses": [
>         "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
>       ],
>       "script_type": "pay-to-pubkey-hash"
>     }
>   ]
> }
```

```python
>>> import requests, json
>>> data = {'tx': '01000000011935b41d12936df99d322ac8972b74ecff7b79408bbccaf1b2eb8015228beac8000000006b483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044ffffffff0240420f00000000001976a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac20da3c00000000001976a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac00000000'}
>>> r = requests.post('https://api.blockcypher.com/v1/bcy/test/txs/push', data=json.dumps(data))
>>> r.json()
{
  "block_height": -1,
  "hash": "4e6dfb1415b4fba5bd257c129847c70fbd4e45e41828079c4a282680528f3a50",
  "addresses": [
    "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9",
    "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
  ],
  "total": 4988000,
  "fees": 12000,
  "size": 226,
  "preference": "high",
  "relayed_by": "73.162.198.68",
  "received": "2015-05-22T05:10:00.305308666Z",
  "ver": 1,
  "lock_time": 0,
  "double_spend": false,
  "vin_sz": 1,
  "vout_sz": 2,
  "confirmations": 0,
  "inputs": [
    {
      "prev_hash": "c8ea8b221580ebb2f1cabc8b40797bffec742b97c82a329df96d93121db43519",
      "output_index": 0,
      "script": "483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044",
      "output_value": 5000000,
      "sequence": 4294967295,
      "addresses": [
        "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
      ],
      "script_type": "pay-to-pubkey-hash",
      "age": 576
    }
  ],
  "outputs": [
    {
      "value": 1000000,
      "script": "76a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac",
      "addresses": [
        "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
      ],
      "script_type": "pay-to-pubkey-hash"
    },
    {
      "value": 3988000,
      "script": "76a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac",
      "addresses": [
        "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
      ],
      "script_type": "pay-to-pubkey-hash"
    }
  ]
}
```

If you'd prefer to use your own transaction library instead of the recommended path of our two-endpoint [transaction generation](#creating-transactions) we're still happy to help you propagate your raw transactions. Simply send your raw hex-encoded transaction to this endpoint and we'll leverage our huge network of nodes to propagate your transaction faster than anywhere else.

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/txs/push | POST | {"tx":$TXHEX} | [TX](#tx)

TXHEX is a hex-encoded raw representation of your transaction, for example:

`01000000011935b41d12936df99d322ac8972b74ecff7b79408bbccaf1b2eb8015228beac8000000006b483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044ffffffff0240420f00000000001976a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac20da3c00000000001976a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac00000000`

If it succeeds, you'll receive a decoded [TX](#tx) object and an HTTP Status Code 201. If you'd like, you can use the decoded transaction **hash** alongside an [Event](#events-and-hooks) to track its progress in the network.

## Decode Raw Transaction Endpoint

```shell
curl -d '{"tx":"01000000011935b41d12936df99d322ac8972b74ecff7b79408bbccaf1b2eb8015228beac8000000006b483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044ffffffff0240420f00000000001976a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac20da3c00000000001976a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac00000000"}' https://api.blockcypher.com/v1/bcy/test/txs/decode

{
  "block_height": -1,
  "hash": "4e6dfb1415b4fba5bd257c129847c70fbd4e45e41828079c4a282680528f3a50",
  "addresses": [
    "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9",
    "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
  ],
  "total": 4988000,
  "fees": 12000,
  "size": 226,
  "preference": "high",
  "relayed_by": "73.162.198.68",
  "received": "2015-05-22T05:10:00.305308666Z",
  "ver": 1,
  "lock_time": 0,
  "double_spend": false,
  "vin_sz": 1,
  "vout_sz": 2,
  "confirmations": 0,
  "inputs": [
    {
      "prev_hash": "c8ea8b221580ebb2f1cabc8b40797bffec742b97c82a329df96d93121db43519",
      "output_index": 0,
      "script": "483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044",
      "output_value": 5000000,
      "sequence": 4294967295,
      "addresses": [
        "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
      ],
      "script_type": "pay-to-pubkey-hash",
      "age": 576
    }
  ],
  "outputs": [
    {
      "value": 1000000,
      "script": "76a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac",
      "addresses": [
        "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
      ],
      "script_type": "pay-to-pubkey-hash"
    },
    {
      "value": 3988000,
      "script": "76a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac",
      "addresses": [
        "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
      ],
      "script_type": "pay-to-pubkey-hash"
    }
  ]
}
```

```javascript
var decodetx = {
  tx: "01000000011935b41d12936df99d322ac8972b74ecff7b79408bbccaf1b2eb8015228beac8000000006b483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044ffffffff0240420f00000000001976a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac20da3c00000000001976a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac00000000"
};
$.post('https://api.blockcypher.com/v1/bcy/test/txs/decode', JSON.stringify(decodetx))
  .then(function(d) {console.log(d)});
> {
>   "block_height": -1,
>   "hash": "4e6dfb1415b4fba5bd257c129847c70fbd4e45e41828079c4a282680528f3a50",
>   "addresses": [
>     "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9",
>     "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
>   ],
>   "total": 4988000,
>   "fees": 12000,
>   "size": 226,
>   "preference": "high",
>   "relayed_by": "73.162.198.68",
>   "received": "2015-05-22T05:10:00.305308666Z",
>   "ver": 1,
>   "lock_time": 0,
>   "double_spend": false,
>   "vin_sz": 1,
>   "vout_sz": 2,
>   "confirmations": 0,
>   "inputs": [
>     {
>       "prev_hash": "c8ea8b221580ebb2f1cabc8b40797bffec742b97c82a329df96d93121db43519",
>       "output_index": 0,
>       "script": "483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044",
>       "output_value": 5000000,
>       "sequence": 4294967295,
>       "addresses": [
>         "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
>       ],
>       "script_type": "pay-to-pubkey-hash",
>       "age": 576
>     }
>   ],
>   "outputs": [
>     {
>       "value": 1000000,
>       "script": "76a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac",
>       "addresses": [
>         "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
>       ],
>       "script_type": "pay-to-pubkey-hash"
>     },
>     {
>       "value": 3988000,
>       "script": "76a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac",
>       "addresses": [
>         "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
>       ],
>       "script_type": "pay-to-pubkey-hash"
>     }
>   ]
> }
```

```python
# Fund existing address with faucet
>>> import requests, json
>>> data = {'tx': '01000000011935b41d12936df99d322ac8972b74ecff7b79408bbccaf1b2eb8015228beac8000000006b483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044ffffffff0240420f00000000001976a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac20da3c00000000001976a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac00000000'}
>>> r = requests.post('https://api.blockcypher.com/v1/bcy/test/txs/decode', data=json.dumps(data))
>>> r.json()
{'received': '2015-06-10T23:37:05.211843254Z',
 'ver': 1,
 'double_spend': False,
 'outputs': [{'value': 1000000,
   'addresses': ['C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn'],
   'script_type': 'pay-to-pubkey-hash',
   'script': '76a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac'},
  {'value': 3988000,
   'addresses': ['CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9'],
   'script_type': 'pay-to-pubkey-hash',
   'script': '76a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac',
   'spent_by': '892fd7b36c1c3a2e5edb9b4a5d4ffd9ba74d78d3acf3b249991bd8d10a287dbd'}],
 'block_height': -1,
 'total': 4988000,
 'fees': 12000,
 'inputs': [{'output_value': 5000000,
   'sequence': 4294967295,
   'prev_hash': 'c8ea8b221580ebb2f1cabc8b40797bffec742b97c82a329df96d93121db43519',
   'script': '483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044',
   'age': 28188,
   'addresses': ['CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9'],
   'output_index': 0,
   'script_type': 'pay-to-pubkey-hash'}],
 'preference': 'high',
 'vout_sz': 2,
 'vin_sz': 1,
 'relayed_by': '207.38.134.25',
 'hash': '4e6dfb1415b4fba5bd257c129847c70fbd4e45e41828079c4a282680528f3a50',
 'addresses': ['CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9',
  'C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn'],
 'lock_time': 0,
 'size': 226,
 'confirmations': 0}
```

We also offer the ability to decode raw transactions without sending propagating them to the network; perhaps you want to double-check another client library or confirm that another service is sending proper transactions. 

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/txs/decode | POST | {"tx":$TXHEX} | [TX](#tx)

TXHEX is a hex-encoded raw representation of your transaction, for example:

`01000000011935b41d12936df99d322ac8972b74ecff7b79408bbccaf1b2eb8015228beac8000000006b483045022100921fc36b911094280f07d8504a80fbab9b823a25f102e2bc69b14bcd369dfc7902200d07067d47f040e724b556e5bc3061af132d5a47bd96e901429d53c41e0f8cca012102152e2bb5b273561ece7bbe8b1df51a4c44f5ab0bc940c105045e2cc77e618044ffffffff0240420f00000000001976a9145fb1af31edd2aa5a2bbaa24f6043d6ec31f7e63288ac20da3c00000000001976a914efec6de6c253e657a9d5506a78ee48d89762fb3188ac00000000`

If it succeeds, you'll receive your decoded [TX](#tx) object.

## Multisig Transactions

```shell
{
	"inputs": [{"addresses": [sourceAddr]}],
	"outputs": [{
		"addresses"   : [pubkey1, pubkey2, pubkey3],
		"script_type" : "multisig-2-of-3",
		"value"       : 250000,
	}]
}
```

```javascript
{
	"inputs": [{"addresses": [sourceAddr]}],
	"outputs": [{
		"addresses"   : [pubkey1, pubkey2, pubkey3],
		"script_type" : "multisig-2-of-3",
		"value"       : 250000,
	}]
}
```

Multisignature transactions are made simple by the method described in the [Creating Transactions](#creating-transactions) section, but they deserve special mention. In order to use them, you first need to fund a multisignature address. You use the `/txs/new` endpoint as before, but instead of the **outputs** **addresses** array containing public addresses, it instead contains the public keys associated with the new address. In addition, you must select a **script_type** of *mutlisig-n-of-m*, where *n* and *m* are numbers (e.g., *multisig-2-of-3*). The code example demonstrates how the partially filled [TX request object](#tx) would appear.

After you've set up your request object, you send to `/txs/new` and receive a partially filled [TXSkeleton](#txskeleton) as before, but with data to sign from the source address. Sign this data and include the public key(s) of the source address---as demonstrated in the [Creating Transactions](#creating-transactions)---then send along to the `/txs/send` endpoint. If it returns with an HTTP Status Code 201, then your multisignature address (via a *pay-to-script-hash* address) is funded.

<aside class="notice">
If you only need a <i>pay-to-script-hash</i> address corresponding to N-of-M multisig and don't want---or need---to fund it immediately, you should use the comparatively easier <a href="#generate-multisig-address-endpoint">Generate Multisig Address Endpoint.</a>
</aside>

### Spending Multisig Funds

```shell
{
	"inputs": [{
		"addresses"   : [pubkey1, pubkey2, pubkey3],
		"script_type" : "multisig-2-of-3"
	}],
	"outputs": [{
		"addresses" : [destAddr],
		"value"     : 150000
	}]
}
```

```javascript
{
	"inputs": [{
		"addresses"   : [pubkey1, pubkey2, pubkey3],
		"script_type" : "multisig-2-of-3"
	}],
	"outputs": [{
		"addresses" : [destAddr],
		"value"     : 150000
	}]
}
```

Once funded, you might want to programmatically spend the money in the address at some point. Here the process is similar, but with the inputs and outputs reversed. As you can see in the code sample, you need to provide the public keys within the **inputs** **addresses** array, and the corresponding **script_type** of *multisig-n-of-m* (e.g., *multisig-2-of-3*). Then you follow the same process of sending to `/txs/new` and getting an array of data to be signed.

Each party can send their signed data individually to `/txs/send` and we can correlate the signatures to the public keys; once we have enough signatures we'll propagate the transaction. Or you can send all needed signatures alongside ordered public keys with a single call to `/txs/send`.
