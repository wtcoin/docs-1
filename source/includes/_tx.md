# Transactions API

BlockCypher's Transactions API allows you to look up information about unconfirmed transactions, query transactions based on hash, and create and propagate your own transactions, including multisignature transactions---all [based on the coin/chain resource](#restful-resources:-coins-&-chains) you've selected for your endpoints.

If you're new to blockchains, the idea of transactions is relatively self-explanatory. Here's what's going on underneath the hood: a transaction takes previous "unspent transaction outputs" (also known as [UTXOs](https://bitcoin.org/en/glossary/unspent-transaction-output)) as "transaction inputs" and creates new "locking scripts" on those inputs such that they are "sent" to new addresses (to become new UTXOs). While most of these public addresses are reference points for single private keys that can "unlock" the newly created UTXOs, occasionally they are sent to more exotic addresses through *pay-to-script-hash*, typically multisignature addresses.

Generally speaking, UTXOs are generated from previous transactions (except for [Coinbase inputs](https://bitcoin.org/en/glossary/coinbase)).

<aside class="success">
Even if you don't use the API to assist in creating transactions, we highly recommend using BlockCypher to push your raw transactions. BlockCypher maintains connections to 20% of all nodes on all blockchains it supports, which means we can propagate transactions faster than almost anyone else. Given our speed and reliability, this is especially powerful when used in concert with our <a href="#zero-confirmation-confidence">Zero Confirmation Confidence factor.</a>
</aside>

## Unconfirmed Transactions Endpoint

```shell
$ curl http://api.blockcypher.com/v1/btc/main/txs

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

The Unconfirmed Transactions Endpoint returns an array of the latest transactions relayed by nodes in a blockchain that haven't been included in any blocks.

Resource | Method | Return Object
-------- | ------ | -------------
/txs | GET | Array[[Transactions](#transactions)]

The returned object is an array of [Transactions](#transactions) that haven't been included in blocks, arranged in reverse chronological order (latest is first, then older transactions follow).

<aside class="notice">
Due to <a href="https://bitcoin.org/en/glossary/malleability">transaction malleability</a> it can be difficult to deal with transaction hashes before they've been confirmed in blocks. Use caution, and consider applying our <a href="#zero-confirmation-confidence">Zero Confirmation Confidence</a> factor to mitigate potential issues.
</aside>

## Transaction Hash Endpoint

```shell
$ curl https://api.blockcypher.com/v1/btc/main/txs/f854aebae95150b379cc1187d848d58225f3c4157fe992bcd166f58bd5063449

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

The Transaction Hash Endpoint returns detailed information about a given transaction based on its hash.


Resource | Method | Return Object
-------- | ------ | -------------
/txs/$TXHASH | GET | [Transactions](#transactions)

$TXHASH is a *string* representing the hex-encoded transaction hash you're interested in querying, for example:

`f854aebae95150b379cc1187d848d58225f3c4157fe992bcd166f58bd5063449`

The returned object contains detailed information about the transaction, including the value transfered, date received, and a full listing of inputs and outputs.

## Creating Transactions

Within BlockCypher's API, you can push transactions to blockchains one of two ways:

- Use a third party library to create your transactions and [push raw transactions](#push-raw-transaction-endpoint)
- Use our two-endpoint process outlined below, wherein we generate a [Transaction Skeleton](#transactionskeleton) based on your input address, output address, and value to transfer.

In either case, for security reasons, we never take possession of your private keys. We do use private keys with our [Microtransactions API](#microtransactions-api), but they are for low-value transactions and we discard them immediately from our servers' memory.

<aside class="warning">
Always use HTTPS for creating and pushing transactions.
</aside>

### New Transaction Endpoint

```shell
$ curl -d '{"inputs":[{"addresses": ["CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"]}],"outputs":[{"addresses": ["C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"], "value": 1000000}]}' https://api.blockcypher.com/v1/bcy/test/txs/new

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
    "relayed_by": "208.71.159.84",
    "received": "2015-05-21T19:04:14.492743867Z",
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
        "script": "",
        "output_value": 5000000,
        "sequence": 4294967295,
        "addresses": [
          "CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"
        ],
        "script_type": "",
        "age": 4
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
  },
  "tosign": [
    "32b5ea64c253b6b466366647458cfd60de9cd29d7dc542293aa0b8b7300cd827"
  ]
}
```

To use BlockCypher's two-endpoint transaction creation tool, first you need to provide the input address(es), output address, and value to transfer. This is provided in the form of a partially-filled out [Transaction](#transaction) request object.

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/tx/new | POST | [Transaction](#transaction) | [TransactionSkeleton](#transactionskeleton)

As you can see from the code example, you only need to provide a single public address within the **addresses** array of both the **input** and **output** of your [Transaction](#transaction) request object. You also need to fill in the **value** with the amount you'd like to transfer from one address to another.

<aside class="notice">
The **value** can be set to -1 to *sweep* all value from your input address(es) to your output address.
</aside>

### Send Transaction Endpoint

## Push Raw Transaction Endpoint

## Decode Raw Transaction Endpoint

## Multisig Transactions

