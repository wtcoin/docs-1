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

> An example Blockchain Object

```shell
{
"name": "BTC.main",
"height": 355987,
"hash": "00000000000000000571776041a7bbeaa4a0c3e2cd24b782a0bcb12df91ea4c4",
"time": "2015-05-11T22:50:16.462271831Z",
"latest_url": "https://api.blockcypher.com/v1/btc/main/blocks/00000000000000000571776041a7bbeaa4a0c3e2cd24b782a0bcb12df91ea4c4",
"previous_hash": "000000000000000016a6c97a4eeeac1ca238367262831f257d7e7552d211f55c",
"previous_url": "https://api.blockcypher.com/v1/btc/main/blocks/000000000000000016a6c97a4eeeac1ca238367262831f257d7e7552d211f55c",
"peer_count": 250,
"unconfirmed_count": 2135,
"last_fork_height": 355900,
"last_fork_hash": "00000000000000000427feb74d5784ec55a9e0f060e0328f7220433477d719bb"
}
```

A Blockchain represents the current state of a particular blockchain from the [Coin/Chain resources](#restful-resources:-coins-&-chains) that BlockCypher supports. Typically returned from the [Chain API endpoint](#chain-endpoint).

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
**unconfirmed_count** | *integer* | Number of unconfirmed transactions in memory pool (likely to be included in next block).
**last_fork_height** | *integer* | ***Optional*** The current height of the latest fork to the blockchain; when no competing blockchain fork present, not returned with endpoints that return Blockchains.
**last_fork_hash** | *string* | ***Optional*** The hash of the latest confirmed block in the latest fork of the blockchain; when no competing blockchain fork present, not returned with endpoints that return Blockchains.

## Block

> An example Block Object

```shell
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
**total** | *integer* | The total number of satoshis (or smallest, indivisible coin units in non-Bitcoin blockchains) transacted in this block.
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

## Transaction

> An example Transaction Object

```shell
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

A Transaction represents the current state of a particular transaction from either a [Block](#block) within a [Blockchain](#blockchain), or an unconfirmed transaction that has yet to be included in a [Block](#block). Typically returned from the [Unconfirmed Transactions](#unconfirmed-transactions-endpoint) and [Transaction Hash](#transaction-hash) endpoints.

Attribute | Type | Description
--------- | ---- | -----------
**block_height** | *integer* | Height of the block that contains this transaction. If this is an unconfirmed transaction, it will equal -1.
**hash** | *string* | The hash of the transaction. While reasonably unique, using hashes as identifiers may be [unsafe](https://en.bitcoin.it/wiki/Transaction_Malleability).
**addresses** | *array[string]* | Array of bitcoin public addresses involved in the transaction.
**total** | *integer* | The total number of satoshis (or smallest, indivisible coin units in non-Bitcoin blockchains) exchanged in this transaction.
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
**confidence** | *float* | The percentage chance this transaction will be included in the next block, if unconfirmed. For more information, check the section on [Zero Confirmation Confidence.](#zero-confirmation-confidence)
**inputs** | *array[[TXInput](#transactioninput)]* | Array of [Transaction Inputs](#transactioninput), limited to 20 by default.
**outputs** | *array[[TXOutput](#transactionoutput)]* | Array of [Transaction Outputs](#transactionoutput), limited to 20 by default.
**confirmed** | [*time*](https://tools.ietf.org/html/rfc3339) | ***Optional*** Time at which transaction was included in a block; only present for confirmed transactions.
**receive_count** | *integer* | ***Optional*** Number of peers that have sent this transaction to BlockCypher; only present for unconfirmed transactions.
**block_hash** | *string* | ***Optional***  Hash of the block that contains this transaction; only present for confirmed transactions.
**double_of** | *string* | ***Optional*** If this transaction is a double-spend (i.e. **double_spend** == true) then this is the hash of the transaction it's double-spending.
**hex** | *string* | ***Optional*** Hex-encoded bytes of the transaction, as sent over the network.

## TransactionInput

> An example Transaction Input Object

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

A TransactionInput represents an input consumed within a transaction. Typically found within an array in a [Transaction](#transaction). In most cases, Transaction Inputs are from previous [UTXOs](https://bitcoin.org/en/glossary/unspent-transaction-output), with the most prominent exceptions being attempted double-spend and [coinbase](https://bitcoin.org/en/glossary/coinbase) inputs.

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

## TransactionOutput

> An example Transaction Output Object

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

A TransactionOutput represents an output created by a transaction. Typically found with an array in a [Transaction](#transaction).

Attribute | Type | Description
--------- | ---- | -----------
**value** | *int* | Value in this transaction output, in satoshis (or smallest, indivisible coin units in non-Bitcoin blockchains).
**script** | *string* | Raw hexadecimal encoding of the encumbrance script for this output.
**addresses** | *array[string]* | Addresses that correspond to this output; typically this will only have a single address, and you can think of this output as having "sent" **value** to the address contained herein.
**script_type** | *string* | The type of encumbrance script used for this output.
**spent_by** | *string* | ***Optional*** The transaction hash that spent this output. Only returned for outputs that have been spent.

## TransactionTemplate

## Microtransaction

## Address

## AddressKeychain

## Wallet

## Event

## PaymentForward

## PaymentForwardCallback

