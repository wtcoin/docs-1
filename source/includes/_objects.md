# Objects

```shell
# All cURL'd Objects are JSON:

{
	key: value,
	key: value,
}
```

Before diving into BlockCypher's endpoints, this section details all the Objects exposed and expected by the API. Some of you might be more interested in the endpoints themselves, in which case, feel free to [skip to the next section.](#blockchain-api) But for others, a section dedicated to all of BlockCypher's Objects might prove a useful overview, especially if you're new to Blockchain development in general. And in either case, this section provides a comprehensive reference for Objects in the API. For each Object there's a description and a link to a germane API endpoint.

<aside class="notice">
Objects sometimes contain <b>attributes</b> that are <b><i>optional.</i></b> These are always noted in the <b>description</b>. In these cases, the Object may not be guaranteed to include that <b>attribute</b> in its response.
</aside>

## Blockchain

```shell
curl https://api.blockcypher.com/v1/btc/main

{
  "name": "BTC.main",
  "height": 360060,
  "hash": "000000000000000000bf56ff4a81e399374a68344a64d6681039412de78366b8",
  "time": "2015-06-08T22:57:08.260165627Z",
  "latest_url": "https://api.blockcypher.com/v1/btc/main/blocks/000000000000000000bf56ff4a81e399374a68344a64d6681039412de78366b8",
  "previous_hash": "000000000000000011c9511ae1265d34d3c16fff6e8f94380425833b3d0ae5d8",
  "previous_url": "https://api.blockcypher.com/v1/btc/main/blocks/000000000000000011c9511ae1265d34d3c16fff6e8f94380425833b3d0ae5d8",
  "peer_count": 239,
  "unconfirmed_count": 617,
  "high_fee_per_kb": 46086,
  "medium_fee_per_kb": 29422,
  "low_fee_per_kb": 12045,
  "last_fork_height": 359865,
  "last_fork_hash": "00000000000000000aa6462fd9faf94712ce1b5a944dc666f491101c996beab9"
}
```

A Blockchain represents the current state of a particular blockchain from the [Coin/Chain resources](#restful-resources) that BlockCypher supports. Typically returned from the [Chain API endpoint](#chain-endpoint).

Attribute | Type | Description
--------- | ---- | -----------
**name** | *string* | The name of the blockchain represented, in the form of $COIN.$CHAIN.
**height** | *integer* | The current height of the blockchain; i.e., the number of blocks in the blockchain.
**hash** | *string* | The hash of the latest confirmed block in the blockchain; in Bitcoin, the [hashing function is SHA256(SHA256(block))](https://en.bitcoin.it/wiki/Block_hashing_algorithm).
**time** | [*time*](https://tools.ietf.org/html/rfc3339) | The time of the latest update to the blockchain; typically when the latest block was added.
**latest_url** | *url* | The BlockCypher URL to query for more information on the latest confirmed block; returns a [Block](#block).
**previous_hash** | *string* | The hash of the second-to-latest confirmed block in the blockchain.
**previous_url** | *url* | The BlockCypher URL to query for more information on the second-to-latest confirmed block; returns a [Block](#block).
**peer_count** | *integer* | *N/A, will be deprecated soon*.
**high_fee_per_kb** | *integer* | A rolling average of the fee (in satoshis) paid per kilobyte for transactions to be confirmed within 1 to 2 blocks.
**medium_fee_per_kb** | *integer* | A rolling average of the fee (in satoshis) paid per kilobyte for transactions to be confirmed within 3 to 6 blocks.
**low_fee_per_kb** | *integer* | A rolling average of the fee (in satoshis) paid per kilobyte for transactions to be confirmed in 7 or more blocks.
**unconfirmed_count** | *integer* | Number of unconfirmed transactions in memory pool (likely to be included in next block).
**last_fork_height** | *integer* | ***Optional*** The current height of the latest fork to the blockchain; when no competing blockchain fork present, not returned with endpoints that return Blockchains.
**last_fork_hash** | *string* | ***Optional*** The hash of the latest confirmed block in the latest fork of the blockchain; when no competing blockchain fork present, not returned with endpoints that return Blockchains.

## Block

```shell
curl https://api.blockcypher.com/v1/btc/main/blocks/0000000000000000189bba3564a63772107b5673c940c16f12662b3e8546b412

{
"hash": "0000000000000000189bba3564a63772107b567...",
"height": 294322,
"chain": "BTC.main",
"total": 1146652915,
"fees": 130999,
"ver": 2,
"time": "2014-04-05T07:49:18Z",
"received_time": "2014-04-05T07:49:18Z",
"bits": 419486617,
"nonce": 1225187768,
"n_tx": 10,
"prev_block": "0000000000000000ced0958bd27720b71d3...",
"mrkl_root": "359d624d37aee1efa5662b7f5dbc390e996d...",
"txids": [
	"32b3b86e40d996b1f281e24e8d4af2ceacbf874c403836...",
	"1579f716359ba1a207f70248135f5e5fadf539be1dcf53...",
	...,
],
"depth": 61793,
"prev_block_url": "https://api.blockcypher.com/v1/btc/main/blocks/0000000000000000ced0958bd27720b71d32c5847e40660aaca39f33c298abb0",
"tx_url": "https://api.blockcypher.com/v1/btc/main/txs/"
}
```

A Block represents the current state of a particular block from a [Blockchain](#blockchain). Typically returned from the [Block Hash](#block-hash-endpoint) and [Block Height](#block-height-endpoint) endpoints.

Attribute | Type | Description
--------- | ---- | -----------
**hash** | *string* | The hash of the block; in Bitcoin, the [hashing function is SHA256(SHA256(block))](https://en.bitcoin.it/wiki/Block_hashing_algorithm)
**height** | *integer* | The height of the block in the blockchain; i.e., there are **height** earlier blocks in its blockchain.
**depth** | *integer* | The depth of the block in the blockchain; i.e., there are **depth** later blocks in its blockchain.
**chain** | *string* | The name of the blockchain represented, in the form of $COIN.$CHAIN
**total** | *integer* | The total number of satoshis transacted in this block.
**fees** | *integer* | The total number of fees---in satoshis---collected by miners in this block.
**ver** | *integer* | Block version. In Bitcoin, per [BIP34](https://github.com/bitcoin/bips/blob/master/bip-0034.mediawiki) the last version 1 block was at height 227835.
**time** | [*time*](https://tools.ietf.org/html/rfc3339) | Recorded time at which block was built. *Note: Miners rarely post accurate clock times.*
**received_time** | [*time*](https://tools.ietf.org/html/rfc3339) | The time BlockCypher's servers receive the block. Our servers' clock is continuously adjusted and accurate.
**bits** | *integer* | The block-encoded [difficulty target](https://en.bitcoin.it/wiki/Difficulty).
**nonce** | *integer* | The [number used by a miner](https://en.bitcoin.it/wiki/Nonce) to generate this block.
**n_tx** | *integer* | Number of transactions in this block.
**prev_block** | *string* | The hash of the previous block in the blockchain.
**prev_block_url** | *url* | The BlockCypher URL to query for more information on the previous block.
**tx_url** | *url* | The base BlockCypher URL to receive transaction details. To get more details about specific transactions, you must concatenate this URL with the desired transaction hash(es).
**mrkl_root** | *string* | The [Merkle root](https://bitcoin.stackexchange.com/questions/10479/what-is-the-merkle-root) of this block.
**txids** | *array[string]* | An array of transaction hashes in this block. By default, only 20 are included.
**next_txids** | *url* | ***Optional*** If there are more transactions that couldn't fit in the **txids** array, this is the BlockCypher URL to query the next set of transactions (within a Block object).

## TX

```shell
curl https://api.blockcypher.com/v1/btc/main/txs/f854aebae95150b379cc1187d848d58225f3c4157fe992bcd166f58bd5063449

{
"block_hash": "0000000000000000c504bdea36e531d80...",
"block_height": 293000,
"hash": "f854aebae95150b379cc1187d848d58225f3c41...",
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
"confirmations": 63171,
"confidence": 1,
"inputs": [
	{
		"prev_hash": "583910b7bf90ab802e22e5c25a89...",
		"output_index": 1,
		"script": "4830450220504b1ccfddf508422bdd8...",
		"output_value": 16450000,
		"sequence": 4294967295,
		"addresses": [
			"1GbMfYui17L5m6sAy3L3WXAtf1P32bxJXq"
		],
		"script_type": "pay-to-pubkey-hash"
	},
	...,
	...,
],
"outputs": [
	{
		"value": 70320221545,
		"script": "76a914e6aad9d712c419ea8febf009a...",
		"spent_by": "35832d6c70b98b54e9a53ab2d5117...",
		"addresses": [
			"1N2f642sbgCMbNtXFajz9XDACDFnFzdXzV"
		],
		"script_type": "pay-to-pubkey-hash"
	}
]
}
```

A TX represents the current state of a particular transaction from either a [Block](#block) within a [Blockchain](#blockchain), or an unconfirmed transaction that has yet to be included in a [Block](#block). Typically returned from the [Unconfirmed Transactions](#unconfirmed-transactions-endpoint) and [Transaction Hash](#transaction-hash) endpoints.

Attribute | Type | Description
--------- | ---- | -----------
**block_height** | *integer* | Height of the block that contains this transaction. If this is an unconfirmed transaction, it will equal -1.
**hash** | *string* | The hash of the transaction. While reasonably unique, using hashes as identifiers may be [unsafe](https://en.bitcoin.it/wiki/Transaction_Malleability).
**addresses** | *array[string]* | Array of bitcoin public addresses involved in the transaction.
**total** | *integer* | The total number of satoshis exchanged in this transaction.
**fees** | *integer* | The total number of fees---in satoshis---collected by miners in this transaction.
**size** | *integer* | The size of the transaction in bytes.
**preference** | *string* | The likelihood that this transaction will make it to the next block; reflects the preference level miners have to include this transaction. Can be high, medium or low.
**relayed_by** | *string* | Address of the peer that sent BlockCypher's servers this transaction.
**received** | [*time*](https://tools.ietf.org/html/rfc3339) | Time this transaction was received by BlockCypher's servers.
**ver** | *integer* | Version number, [typically 1 for Bitcoin transactions.](https://en.bitcoin.it/wiki/Transaction)
**lock_time** | *integer* | Time when transaction can be valid. Can be interpreted in two ways: if less than 500 million, refers to block height. If more, refers to Unix epoch time.
**double_spend** | *bool* | True if this is an attempted double spend; false otherwise.
**vin_sz** | *integer* | Total number of inputs in the transaction.
**vout_sz** | *integer* | Total number of outputs in the transaction.
**confirmations** | *integer* | Number of subsequent blocks, including the block the transaction is in. Unconfirmed transactions have 0 confirmations.
**confidence** | *float* | The percentage chance this transaction will be included in the next block, if unconfirmed. For more information, check the section on [Confidence Factor.](#confidence-factor)
**inputs** | *array[[TXInput](#txinput)]* | [TXInput](#txinput) Array, limited to 20 by default.
**outputs** | *array[[TXOutput](#txoutput)]* | [TXOutput](#txoutput) Array, limited to 20 by default.
**confirmed** | [*time*](https://tools.ietf.org/html/rfc3339) | ***Optional*** Time at which transaction was included in a block; only present for confirmed transactions.
**receive_count** | *integer* | ***Optional*** Number of peers that have sent this transaction to BlockCypher; only present for unconfirmed transactions.
**change_address** | *string* | ***Optional*** Address BlockCypher will use to send back your change, if you [constructed this transaction](#creating-transactions). If not set, defaults to the address from which the coins were originally sent.
**block_hash** | *string* | ***Optional***  Hash of the block that contains this transaction; only present for confirmed transactions.
**double_of** | *string* | ***Optional*** If this transaction is a double-spend (i.e. **double_spend** == true) then this is the hash of the transaction it's double-spending.
**hex** | *string* | ***Optional*** Hex-encoded bytes of the transaction, as sent over the network.

## TXInput

```shell
{
"prev_hash": "e5c9be87798b0fa8ad55a22b5d731d6f50d72bffaa6179f9999499e57388cc33",
"output_index": 1,
"script": "004730440220689409a16c98fa1659cae4c20...",
"output_value": 2450698356,
"sequence": 4294967295,
"addresses": [
	"34ch2jrhYXDFsLK1FZQ3SjqgFRnX8mGC5W"
],
"script_type": "pay-to-script-hash",
"age": 5
}
```

A TXInput represents an input consumed within a transaction. Typically found within an array in a [TX](#tx). In most cases, TXInputs are from previous [UTXOs](https://bitcoin.org/en/glossary/unspent-transaction-output), with the most prominent exceptions being attempted double-spend and [coinbase](https://bitcoin.org/en/glossary/coinbase) inputs.

Attribute | Type | Description
--------- | ---- | -----------
**prev_hash** | *string* | The previous transaction hash where this input was an output. Not present for [coinbase](https://bitcoin.org/en/glossary/coinbase) transactions.
**output_index** | *integer* | The index of the output being spent within the previous transaction. Not present for [coinbase](https://bitcoin.org/en/glossary/coinbase) transactions.
**output_value** | *integer* | The value of the output being spent within the previous transaction. Not present for [coinbase](https://bitcoin.org/en/glossary/coinbase) transactions.
**script_type** | *string* | The type of script that encumbers the output corresponding to this input.
**script** | *string* | Raw hexadecimal encoding of the script.
**addresses** | *array[string]* | An array of public addresses associated with the output of the previous transaction.
**sequence** | *integer* | Legacy 4-byte [sequence number](https://bitcoin.org/en/glossary/sequence-number), not usually relevant unless dealing with locktime encumbrances.
**age** | *integer* | ***Optional*** Number of confirmations of the previous transaction for which this input was an output. Currently, only returned in unconfirmed transactions.

## TXOutput

```shell
{
"value": 70320221545,
"script": "76a914e6aad9d712c419ea8febf009a3f3bfdd8d222fac88ac",
"spent_by": "35832d6c70b98b54e9a53ab2d51176eb19ad11bc4505d6bb1ea6c51a68cb92ee",
"addresses": [
	"1N2f642sbgCMbNtXFajz9XDACDFnFzdXzV"
],
"script_type": "pay-to-pubkey-hash"
}
```

A TXOutput represents an output created by a transaction. Typically found within an array in a [TX](#tx).

Attribute | Type | Description
--------- | ---- | -----------
**value** | *int* | Value in this transaction output, in satoshis.
**script** | *string* | Raw hexadecimal encoding of the encumbrance script for this output.
**addresses** | *array[string]* | Addresses that correspond to this output; typically this will only have a single address, and you can think of this output as having "sent" **value** to the address contained herein.
**script_type** | *string* | The type of encumbrance script used for this output.
**spent_by** | *string* | ***Optional*** The transaction hash that spent this output. Only returned for outputs that have been spent.

## TXConfidence

```shell
curl https://api.blockcypher.com/v1/btc/main/txs/43fa951e1bea87c282f6725cf8bdc08bb48761396c3af8dd5a41a085ab62acc9/confidence

{
"age_millis": 12725,
"receive_count": 666,
"confidence": 0.9901509730004237,
"txhash": "43fa951e1bea87c282f6725cf8bdc08bb48761396c3af8dd5a41a085ab62acc9",
"txurl": "https://api.blockcypher.com/v1/btc/main/txs/43fa951e1bea87c282f6725cf8bdc08bb48761396c3af8dd5a41a085ab62acc9"
}
```

A TXConfidence represents information about the confidence that an unconfirmed transaction will make it into the next block. Typically used as a return object from the [Transaction Confidence Endpoint.](#transaction-confidence-endpoint)

Attribute | Type | Description
--------- | ---- | -----------
**age_millis** | *integer* | The age of the transaction in milliseconds, based on the earliest time BlockCypher saw it relayed in the network.
**receive_count** | *integer* | Number of peers that have sent this transaction to BlockCypher; only positive for unconfirmed transactions. -1 for confirmed transactions.
**confidence** | *float* | A number from 0 to 1 representing BlockCypher's confidence that the transaction will make it into the next block.
**txhash** | *string* | The hash of the transaction. While reasonably unique, using hashes as identifiers may be [unsafe](https://en.bitcoin.it/wiki/Transaction_Malleability).
**txurl** | *url* | The BlockCypher URL one can use to query more detailed information about this transaction.

## TXRef

```shell
{
"tx_hash": "14b1052855bbf6561bc4db8aa501762e7cc1e86994dda9e782a6b73b1ce0dc1e",
"block_height": 302013,
"tx_input_n": -1,
"tx_output_n": 0,
"value": 20213,
"spent": false,
"confirmations": 54405,
"confirmed": "2014-05-22T03:46:25Z",
"double_spend": false
}
```

A TXRef object represents summarized data about a transaction input or output. Typically found in an array within an [Address](#address) object, which is usually returned from the standard [Address Endpoint](#address-endpoint).

Attribute | Type | Description
--------- | ---- | -----------
**block_height** | *integer* | Height of the block that contains this transaction. If this is an unconfirmed transaction, it will equal -1.
**tx_hash** | *string* | The hash of the transaction containing this input or output. While reasonably unique, using hashes as identifiers may be [unsafe](https://en.bitcoin.it/wiki/Transaction_Malleability).
**tx_input_n** | *integer* | Index of the input in the transaction. It's a negative number for an output.
**tx_output_n** | *integer* |	Index of the output in the transaction. It's a negative number for an input.
**value** | *integer* | The value transfered by this input/output in satoshis exchanged in this transaction.
**preference** | *string* | The likelihood that this transaction will make it to the next block; reflects the preference level miners have to include this transaction. Can be high, medium or low.
**spent** | *bool* | 	True if this output was spent; false otherwise.
**double_spend** | *bool* | True if this is an attempted double spend; false otherwise.
**confirmations** | *integer* | Number of subsequent blocks, including the block the transaction is in. Unconfirmed transactions have 0 confirmations.
**confidence** | *float* | ***Optional*** The percentage chance this transaction will be included in the next block, if unconfirmed. For more information, check the section on [Confidence Factor.](#confidence-factor)
**confirmed** | [*time*](https://tools.ietf.org/html/rfc3339) | ***Optional*** Time at which transaction was included in a block; only present for confirmed transactions.
**spent_by** | *string* | ***Optional*** The transaction hash that spent this output. Only returned for outputs that have been spent.
**received** | [*time*](https://tools.ietf.org/html/rfc3339) | ***Optional*** Time this transaction was received by BlockCypher's servers; only present for unconfirmed transactions.
**receive_count** | *integer* | ***Optional*** Number of peers that have sent this transaction to BlockCypher; only present for unconfirmed transactions.
**double_of** | *string* | ***Optional*** If this transaction is a double-spend (i.e. **double_spend** == true) then this is the hash of the transaction it's double-spending.

## TXSkeleton

```shell
curl -d '{"inputs":[{"addresses": ["CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"]}],"outputs":[{"addresses": ["C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"], "value": 1000000}]}' https://api.blockcypher.com/v1/bcy/test/txs/new

{
"tx": {
	"block_height": -1,
	"hash": "f961dea839fd69653547a0308360...",
	"addresses": [
		"1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD",
		"1FGAsJFNgWvFz2tWQAnRq8S6fVX9Zmuxje",
		],
	"total": 661116,
	"fees": 10000,
	"received": "2014-04-20T23:52:21.069978821Z",
	...
},
"tosign": [
	"04779733bba8085dd86c21d86c...",
	"0396ea6f1bf7493e738339bd72..."
],
"errors": [
	{ "error": "..." }
],
"signatures": [
	"a832bd21afc21...",
	"0123ba82a273b..."
],
"pubkeys": [
	"02c8d29d...",
	"02c9d27d...",
]
}
```

A TXSkeleton is a convenience/wrapper Object that's used primarily when [Creating Transactions](#creating-transactions) through the [New](#new-transaction-endpoint) and [Send](#send-transaction-endpoint) endpoints.

Attribute | Type | Description
--------- | ---- | -----------
**tx** | *[TX](#tx)* | A temporary [TX](#tx), usually returned fully filled but missing input scripts.
**tosign** | *array[string]* | Array of hex-encoded data for you to sign, one for each input.
**signatures** | *array[string]* | Array of signatures corresponding to all the data in **tosign**, typically provided by you.
**pubkeys** | *array[string]* | Array of public keys corresponding to each signature. In general, these are provided by you, and correspond to the signatures you provide.
**errors** | *array["error":string]* | ***Optional*** Array of errors in the form *"error":"description-of-error"*. This is only returned if there was an error in any stage of transaction generation, and is usually accompanied by a HTTP 400 code.

## MicroTX

```shell
{
"from_pubkey": "03bb318b00de944086fad67ab78a832eb1bf26916053ecd3b14a3f48f9fbe0821f",
"to_address": "mrS82nWF3TCfhafSaKnmUUktGNYuTZn1Ap",
"value_satoshis": 5000,
"token": "477a17a15d55b408038404a46c059ef9",
"signatures": [
	"3045022100..."
]
}
```

A MicroTX represents a streamlined---and typically much lower value---microtransaction, one which BlockCypher can sign for you if you send your private key. MicroTXs can also be signed on the client-side without ever sending your private key. You'll find these objects used in the [Microtransaction API.](#microtransaction-api)

<aside class="notice">
Only one of these three fields is required for the microtransaction endpoint: <b>from_pubkey</b>, <b>from_private</b>, <b>from_wif</b> . If you send more than one, the API will return an error.
</aside>

Attribute | Type | Description
--------- | ---- | -----------
**from_pubkey** | *string* | Hex-encoded public key from which you're sending coins.
**from_private** | *string* | Hex-encoded private key from which you're sending coins.
**from_wif** | *string* | WIF-encoded private key from which you're sending coins.
**to_address** | *string* | The target address to which you're sending coins.
**value_satoshis** | *int* | Value you're sending/you've sent in satoshis.
**token** | *string* | Your [BlockCypher API token](http://accounts.blockcypher.com/)
**change_address** | *string* | ***Optional*** Address BlockCypher will use to send back your change. If not set, defaults to the address from which the coins were originally sent. While not required, we recommend that you set a change address.
**wait_guarantee** | *bool* | ***Optional*** If not set, defaults to *true*, which means the API will wait for BlockCypher to guarantee the transaction, using our [Confidence Factor](#confidence-factor). The guarantee usually takes around 8 seconds. If manually set to *false*, the Microtransaction endpoint will return as soon as the transaction is broadcast.
**tosign** | *array[string]* | ***Optional*** Hex-encoded data for you to sign after initiating the microtransaction. Sent in reply to a microtransaction generated using **from_pubkey**/a public key.
**signatures** | *array[string]* | ***Optional*** Hex-encoded signatures for you to send back after having received (and signed) **tosign**.
**inputs** | *array[[TXInput](#txinput)]* | ***Optional*** Partial list of inputs that will be used with this transaction. Only returned when using **from_pubkey**.
**outputs** | *array[[TXOutput](#txoutput)]* | ***Optional*** Partial list of outputs that will be used with this transaction. Only returned when using **from_pubkey**.
**fees** | *integer* | ***Optional*** BlockCypher's optimally calculated fees for this MicroTX to guarantee swift 99% confirmation, only returned when using **from_pubkey**. BlockCypher pays these fees for the first 8,000 microtransactions, but like regular transactions, it is deducted from the source address thereafter.
**hash** | *string* | ***Optional*** The hash of the finalized transaction, once sent.

## Address

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
	...
],
"unconfirmed_txrefs": [
	...
]
}
```

An Address represents a public address on a blockchain, and contains information about the state of balances and transactions related to this address. Typically returned from the [Address Balance](#address-balance-endpoint), [Address](#address-endpoint), and [Address Detail Endpoint](#address-detail-endpoint).

Attribute | Type | Description
--------- | ---- | -----------
**address** | *string* | The requested address.
**total_received** | *integer* |Total amount of satoshis received by this address.
**total_sent** | *integer* | Total amount of satoshis sent by this address.
**balance**	| *integer* | Balance of confirmed satoshis on this address. This is the difference between outputs and inputs on this address, but only for transactions that have been included into a block (i.e., for transactions whose confirmations > 0).
**unconfirmed_balance**	| *integer* | Balance of unconfirmed satoshis on this address. Can be negative (if unconfirmed transactions are just spending outputs). Only unconfirmed transactions (haven't made it into a block) are included.
**final_balance**	| *integer* |	Total balance of satoshis, including confirmed and unconfirmed transactions, for this address.
**n_tx** | *integer* | Number of confirmed transactions on this address. Only transactions that have made it into a block (confirmations > 0) are counted.
**unconfirmed_n_tx** | *integer* | Number of unconfirmed transactions for this address. Only unconfirmed transactions (confirmations == 0) are counted.
**final_n_tx** | *integer* | Final number of transactions, including confirmed and unconfirmed transactions, for this address.
**tx_url** | *url* | ***Optional*** To retrieve base URL transactions. To get the full URL, concatenate this URL with a transaction's hash.
**txs** | *array[[TX](#tx)]* | ***Optional***  Array of full transaction details associated with this address. Usually only returned from the [Address Detail Endpoint](#address-detail).
**txrefs** | *array[[TXRef](#txref)]* | ***Optional*** Array of transaction inputs and outputs for this address. Usually only returned from the standard [Address Endpoint](#address-endpoint).
**unconfirmed_txrefs** | *array[[TXRef](#txref)]* | ***Optional*** All unconfirmed transaction inputs and outputs for this address. Usually only returned from the standard [Address Endpoint](#address-endpoint).
**hasMore** | *bool* | ***Optional*** If *true*, then the Address object contains more transactions than shown. Useful for determining whether to poll the API for more transaction information.

## AddressKeychain

```shell
curl -X POST http://api.blockcypher.com/v1/btc/test3/addrs

{
"private": "86751cb880a9a1addcc3b67979976158dd800afe9d14b68349921299b20c94dd",
"public": "03866586fbe3652eb219c5ed99c3fc72d125472248183f966e0673be08a1c543de",
"address": "n1ucSDLByN5GLLQuE7BMrtTWHwHtkaVkfA",
"wif": "cS64ygfjWjN73S78oUbJQeikDn9uS7KNWS1PL7NqeBUuF4UobnAy"
}
```

An AddressKeychain represents an associated collection of public and private keys alongside their respective public address. Generally returned and used with the [Generate Address Endpoint](#generate-address-endpoint).

Attribute | Type | Description
--------- | ---- | -----------
**address**	| *string* | Standard address representation.
**public** | *string* | Hex-encoded Public key.
**private**	| *string* | Hex-encoded Private key.
**wif**	| *string* | [Wallet import format](https://en.bitcoin.it/wiki/Wallet_import_format), a common encoding for the private key.
**pubkeys**	| *array[string]* | ***Optional*** Array of public keys to provide to generate a multisig address.
**script_type**	| *array[string]* | ***Optional*** If generating a multisig address, the type of multisig script; typically "multisig-n-of-m", where n and m are integers.

## Wallet

```shell
curl -d '{"name": "alice","addresses": ["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]}' https://api.blockcypher.com/v1/btc/main/wallets?token=USERTOKEN

{
"token":"USERTOKEN",
"name": "alice",
"addresses": ["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]
}
```

A Wallet represents a list of addresses, and can be used interchangeably with all the [Address API](#address-api) endpoints.

Attribute | Type | Description
--------- | ---- | -----------
**token** | *string* | User token associated with this wallet.
**name** | *string* | Name of the wallet.
**addresses** | *array[string]* | List of addresses associated with this wallet.

## Event

```shell
curl https://api.blockcypher.com/v1/btc/main/hooks/399d0923-e920-48ee-8928-2051cbfbc369

{
"id": "399d0923-e920-48ee-8928-2051cbfbc369"
"event": "unconfirmed-tx",
"address": "15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
"token": "USERTOKEN"
"url": "https://my.domain.com/callbacks/new-tx"
}
```

An Event represents a WebHooks or WebSockets-based notification request, as detailed in the [Events & Hooks](#events-and-hooks) section of the documentation.

Attribute | Type | Description
--------- | ---- | -----------
**id** | *string* | Identifier of the event; generated when a new request is created.
**event** | *string* | Type of event; can be *unconfirmed-tx*, *new-block*, *confirmed-tx*, *tx-confirmation*, *double-spend-tx*.
**hash**	| *string* | ***optional*** Only objects with a matching hash will be sent. The hash can either be for a block or a transaction.
**wallet_name**	|  *string* | ***optional*** Only transactions associated with the given wallet will be sent. If used, requires a user token.
**token**	| *string* | ***optional*** Required if wallet_name is used, though generally we advise users to include it (as they can reach API throttling thresholds rapidly).
**address**	| *string* | ***optional*** Only transactions associated with the given address will be sent. A wallet name can also be used instead of an address, which will then match on any address in the wallet.
**confirmations** | *integer* | ***optional***  Used in concert with the *tx-confirmation* event type to set the number of confirmations desired for which to receive an update. You'll receive an updated [TX](#tx) for every confirmation up to this amount. The maximum allowed is 10; if not set, it will default to 6.
**script**	| *string* | ***optional*** Only transactions with an output script of the provided type will be sent. The recognized types of scripts are: *pay-to-pubkey-hash*, *pay-to-multi-pubkey-hash*, *pay-to-pubkey*, *pay-to-script-hash*, *null-data* (sometimes called OP_RETURN), *empty* or *unknown*.
**url** | *url* | ***optional*** Callback URL for this Event's WebHook; not applicable for WebSockets usage.

## PaymentForward

```shell
curl -d '{"destination":"15qx9ug952GWGTNn7Uiv6vode4RcGrRemh","callback_url": "https://my.domain.com/callbacks/new-pay","process_fees_address": "1LWw6FdzNUcX8bnekMMZ7eofcGF7SXmbrL", "process_fees_percent": 0.1,"token":"USERTOKEN"}' http://api.blockcypher.com/v1/btc/main/payments

{
"input_address": "16uKw7GsQSzfMaVTcT7tpFQkd7Rh9qcXWX",
"destination": "15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
"callback_url": "https://my.domain.com/callbacks/new-pay"
"process_fees_address": "1LWw6FdzNUcX8bnekMMZ7eofcGF7SXmbrL",
"process_fees_percent": 0.1,
"token": "USERTOKEN"
}
```

A PaymentForward object represents a request set up through the [Payment Forwarding](#payment-forwarding) service.

Attribute | Type | Description
--------- | ---- | -----------
**id** | *string* | Identifier of the payment forwarding request; generated when a new request is created.
**token**	| *string* | The mandatory user token.
**destination**	| *string* | The required destination address for payment forwarding.
**input_address**	| *string* | The address which will automatically forward to **destination**; generated when a new request is created.
**process_fees_address** | *string* | ***Optional*** Address to forward processing fees, if specified. Allows you to receive a fee for your own services.
**process_fees_satoshis**	| *int* | ***Optional*** Fixed processing fee amount to be sent to the fee address. A fixed satoshi amount or a percentage is required if a **process_fees_address** has been specified.
**process_fees_percent** | *float* | ***Optional*** Percentage of the transaction to be sent to the fee address. A fixed satoshi amount or a percentage is required if a **process_fees_address** has been specified.
**callback_url** | *url* | ***Optional*** The URL to call anytime a new payment is forwarded.
**enable_confirmations** | *bool* | ***Optional*** Whether to also call the **callback_url** with subsequent confirmations of the forwarding transactions. Automatically sets up a WebHook.
**mining_fees_satoshis** | *int* | ***Optional*** Mining fee amount to include in the forwarding transaction, in satoshis. If not set, defaults to 10,000.
**transactions** | *array[string]* | ***Optional*** History of forwarding transaction hashes for this payment forwarding request.

## PaymentForwardCallback

```shell
{
"value": 100000000,
"input_address": "16uKw7GsQSzfMaVTcT7tpFQkd7Rh9qcXWX",
"destination": "15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
"input_transaction_hash": "39bed5d...",
"transaction_hash": "1aa6103..."
}
```

A PaymentForwardCallback object represents the payload delivered to the optional **callback_url** in a [PaymentForward](#payment-forward) request.

Attribute | Type | Description
--------- | ---- | -----------
**value** | *int* | Amount sent to the destination address, in satoshis.
**input_address** | *string* | The intermediate address to which the payment was originally sent.
**destination** | *string* |The final destination address to which the payment will eventually be sent.
**input_transaction_hash** | *string* | The transaction hash representing the initial payment to the **input_address**.
**transaction_hash** | *string* | The transaction hash of the generated transaction that forwards the payment from the **input_address** to the **destination.**
