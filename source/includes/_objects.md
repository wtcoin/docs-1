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

```python
>>> from blockcypher import get_blockchain_overview
>>> get_blockchain_overview()
{
    "hash": "000000000000000006c488791bafc490efb365ad609ed39e6ee51d1afbc8ed83", 
    "height": 379780, 
    "high_fee_per_kb": 57607, 
    "last_fork_hash": "00000000000000000aef777b65b5d32301c914e7d9f2c4cf1f7366e1eb217124", 
    "last_fork_height": 280959, 
    "latest_url": "https://api.blockcypher.com/v1/btc/main/blocks/000000000000000006c488791bafc490efb365ad609ed39e6ee51d1afbc8ed83", 
    "low_fee_per_kb": 24554, 
    "medium_fee_per_kb": 28847, 
    "name": "BTC.main", 
    "peer_count": 817, 
    "previous_hash": "0000000000000000026727709d704d20de7d756427a10a86cd0b084066632e55", 
    "previous_url": "https://api.blockcypher.com/v1/btc/main/blocks/0000000000000000026727709d704d20de7d756427a10a86cd0b084066632e55", 
    "time": "datetime.datetime(2015, 10, 20, 19, 35, 50, 174235, tzinfo=tzutc())", 
    "unconfirmed_count": 100231
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
"relayed_by": "",
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

```python
>>> from blockcypher import get_block_overview
>>> get_block_overview('0000000000000000189bba3564a63772107b5673c940c16f12662b3e8546b412')
{
    "bits": 419486617, 
    "chain": "BTC.main", 
    "depth": 85570, 
    "fees": 130999, 
    "hash": "0000000000000000189bba3564a63772107b5673c940c16f12662b3e8546b412", 
    "height": 294322, 
    "mrkl_root": "359d624d37aee1efa5662b7f5dbc390e996d561afc8148e8d716cf6ad765a952", 
    "n_tx": 10, 
    "nonce": 1225187768, 
    "prev_block": "0000000000000000ced0958bd27720b71d32c5847e40660aaca39f33c298abb0", 
    "prev_block_url": "https://api.blockcypher.com/v1/btc/main/blocks/0000000000000000ced0958bd27720b71d32c5847e40660aaca39f33c298abb0", 
    "received_time": "datetime.datetime(2014, 4, 5, 7, 49, 18, 0, tzinfo=tzutc())", 
    "relayed_by": "", 
    "time": "datetime.datetime(2014, 4, 5, 7, 49, 18, 0, tzinfo=tzutc())", 
    "total": 1146652915, 
    "tx_url": "https://api.blockcypher.com/v1/btc/main/txs/", 
    "txids": [
        "32b3b86e40d996b1f281e24e8d4af2ceacbf874c4038369cc21baa807409b277", 
        "1579f716359ba1a207f70248135f5e5fadf539be1dcf5300613aedcb6577d287", 
        "dd1f183348eb41eaaa9ecf8012f9cca3ecbae41a6349f0cc4bfd2b1a497fa3d0", 
        "749d12ccd180968b82aef4c271ca4effdf981d9b5d12523264457c9d4e6fa78e", 
        "c4fe2ee16b8e3067d3d95caf7944011f4959781288b807df8bf853b7f80ed97c", 
        "5a2114675265522d2b7ce8a7874cfa7a22ccc3fb6566a8599d6432c6805b1b5f", 
        "077d851c8240671de80caa8be9f5285201c08a70edc5a45a9cd35fe7eaebf5e1", 
        "6202cc55fbd9130e065c9294a5b2e061c26f3d2c8df56c32da605d9f183103f9", 
        "ad3e7aa1c33f1d3e1c105d94f7b1542808da07bbe66b9621b050104a85dbf650", 
        "36cc61016b9d1bd69768666f287db1edaa9b292fb442f152af7099305677230e"
    ], 
    "ver": 2
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
**size** | *integer* | ***Optional*** Raw size of block (including header and all transactions) in bytes. Not returned for bitcoin blocks earlier than height 389104.
**ver** | *integer* | Block version. 
**time** | [*time*](https://tools.ietf.org/html/rfc3339) | Recorded time at which block was built. *Note: Miners rarely post accurate clock times.*
**received_time** | [*time*](https://tools.ietf.org/html/rfc3339) | The time BlockCypher's servers receive the block. Our servers' clock is continuously adjusted and accurate.
**relayed_by** | *string* | Address of the peer that sent BlockCypher's servers this block.
**bits** | *integer* | The block-encoded [difficulty target](https://en.bitcoin.it/wiki/Target).
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
**double_spend** | *bool* | *true* if this is an attempted double spend; *false* otherwise.
**vin_sz** | *integer* | Total number of inputs in the transaction.
**vout_sz** | *integer* | Total number of outputs in the transaction.
**confirmations** | *integer* | Number of subsequent blocks, including the block the transaction is in. Unconfirmed transactions have 0 confirmations.
**inputs** | *array[[TXInput](#txinput)]* | [TXInput](#txinput) Array, limited to 20 by default.
**outputs** | *array[[TXOutput](#txoutput)]* | [TXOutput](#txoutput) Array, limited to 20 by default.
**opt_in_rbf** | *bool* | ***Optional*** Returns **true** if this transaction has opted in to Replace-By-Fee (RBF), either **true** or not present. You can read more [about Opt-In RBF here.](https://bitcoincore.org/en/faq/optin_rbf/)
**confidence** | *float* | ***Optional*** The percentage chance this transaction will not be double-spent against, if unconfirmed. For more information, check the section on [Confidence Factor.](#confidence-factor)
**confirmed** | [*time*](https://tools.ietf.org/html/rfc3339) | ***Optional*** Time at which transaction was included in a block; only present for confirmed transactions.
**receive_count** | *integer* | ***Optional*** Number of peers that have sent this transaction to BlockCypher; only present for unconfirmed transactions.
**change_address** | *string* | ***Optional*** Address BlockCypher will use to send back your change, if you [constructed this transaction](#creating-transactions). If not set, defaults to the address from which the coins were originally sent.
**block_hash** | *string* | ***Optional***  Hash of the block that contains this transaction; only present for confirmed transactions.
**double_of** | *string* | ***Optional*** If this transaction is a double-spend (i.e. **double_spend** == *true*) then this is the hash of the transaction it's double-spending.
**data_protocol** | *string* | ***Optional*** Returned if this transaction contains an OP_RETURN associated with a known data protocol. Data protocols currently detected: *blockchainid* ; *openassets* ; *factom* ; *colu* ; *coinspark* ; *omni*
**hex** | *string* | ***Optional*** Hex-encoded bytes of the transaction, as sent over the network.
**next_inputs** | *url* | ***Optional*** If there are more transaction inptus that couldn't fit into the TXInput array, this is the BlockCypher URL to query the next set of TXInputs (within a TX object).
**next_outputs** | *url* | ***Optional*** If there are more transaction outputs that couldn't fit into the TXOutput array, this is the BlockCypher URL to query the next set of TXOutputs(within a TX object).

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
**wallet_name** | *string* | ***Optional*** Name of [Wallet](#wallet) or [HDWallet](#hdwallet) from which to derive inputs. Only used when constructing transactions via the [Creating Transactions](#creating-transactions) process.
**wallet_token** | *string* | ***Optional*** Token associated with [Wallet](#wallet) or [HDWallet](#hdwallet) used to derive inputs. Only used when constructing transactions via the [Creating Transactions](#creating-transactions) process.

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
**spent_by** | *string* | ***Optional*** The transaction hash that spent this output. Only returned for outputs that have been spent. The spending transaction may be unconfirmed.
**data_hex** | *string* | ***Optional*** A hex-encoded representation of an OP_RETURN data output, without any other script instructions. Only returned for outputs whose **script_type** is *null-data*.
**data_string** | *string* | ***Optional*** An ASCII representation of an OP_RETURN data output, without any other script instructions. Only returned for outputs whose **script_type** is *null-data* and if its data falls into the visible ASCII range.

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
**confidence** | *float* | A number from 0 to 1 representing BlockCypher's confidence that the transaction won't be double-spent against.
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
"ref_balance": 4433416,
"spent": false,
"confirmations": 54405,
"confirmed": "2014-05-22T03:46:25Z",
"double_spend": false
}
```

A TXRef object represents summarized data about a transaction input or output. Typically found in an array within an [Address](#address) object, which is usually returned from the standard [Address Endpoint](#address-endpoint).

Attribute | Type | Description
--------- | ---- | -----------
**address** | *string* | ***Optional*** The address associated with this transaction input/output. Only returned when querying an address endpoint via a wallet/HD wallet name.
**block_height** | *integer* | Height of the block that contains this transaction input/output. If it's unconfirmed, this will equal -1.
**tx_hash** | *string* | The hash of the transaction containing this input/output. While reasonably unique, using hashes as identifiers may be [unsafe](https://en.bitcoin.it/wiki/Transaction_Malleability).
**tx_input_n** | *integer* | Index of this input in the enclosing transaction. It's a negative number for an output.
**tx_output_n** | *integer* |	Index of this output in the enclosing transaction. It's a negative number for an input.
**value** | *integer* | The value transfered by this input/output in satoshis exchanged in the enclosing transaction.
**preference** | *string* | The likelihood that the enclosing transaction will make it to the next block; reflects the preference level miners have to include the enclosing transaction. Can be high, medium or low.
**spent** | *bool* | 	*true* if this is an output and was spent. If it's an input, or an unspent output, it will be *false*. 
**double_spend** | *bool* | *true* if this is an attempted double spend; *false* otherwise.
**confirmations** | *integer* | Number of subsequent blocks, including the block the transaction is in. Unconfirmed transactions have 0 confirmations.
**script** | *string* | ***Optional*** Raw, hex-encoded script of this input/output.
**ref_balance** | *integer* | ***Optional*** The past balance of the parent address the moment this transaction was confirmed. Not present for unconfirmed transactions.
**confidence** | *float* | ***Optional*** The percentage chance this transaction will not be double-spent against, if unconfirmed. For more information, check the section on [Confidence Factor.](#confidence-factor)
**confirmed** | [*time*](https://tools.ietf.org/html/rfc3339) | ***Optional*** Time at which transaction was included in a block; only present for confirmed transactions.
**spent_by** | *string* | ***Optional*** The transaction hash that spent this output. Only returned for outputs that have been spent. The spending transaction may be unconfirmed.
**received** | [*time*](https://tools.ietf.org/html/rfc3339) | ***Optional*** Time this transaction was received by BlockCypher's servers; only present for unconfirmed transactions.
**receive_count** | *integer* | ***Optional*** Number of peers that have sent this transaction to BlockCypher; only present for unconfirmed transactions.
**double_of** | *string* | ***Optional*** If this transaction is a double-spend (i.e. **double_spend** == *true*) then this is the hash of the transaction it's double-spending.

## TXSkeleton

```shell
# Note resource change to bcy/test instead of btc/main
curl -d '{"inputs":[{"addresses": ["CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9"]}],"outputs":[{"addresses": ["C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"], "value": 1000000}]}' https://api.blockcypher.com/v1/bcy/test/txs/new

{
"tx": {
	"block_height": -1,
	"block_index": 0,
	"hash": "4d6e32d71313fb548232642fd...",
	"addresses": [
		"CEztKBAYNoUEEaPYbkyFeXC5v8Jz9RoZH9",
		"C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn"
	],
	"total": 5419600,
	"fees": 12500,
	"size": 119,
	...
},
"tosign": [
	"97d81abd54cae1648951f49..."
],
"errors": [
	{ "error": "..." }
],
"signatures": [],
"pubkeys": []
}
```

A TXSkeleton is a convenience/wrapper Object that's used primarily when [Creating Transactions](#creating-transactions) through the [New](#new-transaction-endpoint) and [Send](#send-transaction-endpoint) endpoints.

Attribute | Type | Description
--------- | ---- | -----------
**tx** | *[TX](#tx)* | A temporary [TX](#tx), usually returned fully filled but missing input scripts.
**tosign** | *array[string]* | Array of hex-encoded data for you to sign, one for each input.
**signatures** | *array[string]* | Array of signatures corresponding to all the data in **tosign**, typically provided by you.
**pubkeys** | *array[string]* | Array of public keys corresponding to each signature. In general, these are provided by you, and correspond to the signatures you provide.
**tosign_tx** | *array[string]* | ***Optional*** Array of hex-encoded, work-in-progress transactions; optionally returned to validate the **tosign** data locally.
**errors** | *array["error":string]* | ***Optional*** Array of errors in the form *"error":"description-of-error"*. This is only returned if there was an error in any stage of transaction generation, and is usually accompanied by a HTTP 400 code.

## NullData

```shell
curl -d '{"data":"I am the walrus", "encoding":"string"}' https://api.blockcypher.com/v1/btc/main/txs/data?token=YOURTOKEN

{
  "data": "I am the walrus",
  "encoding": "string",
  "token": "YOURTOKEN",
  "hash": "cb6974e0fd57c91b70403e85ef48c840eecdca4804dfc4897b1321d5328e4f18"
}
```
A NullData Object is used exclusively by our [Data Endpoint](#data-endpoint) to embed small pieces of data on the blockchain. If your data is over 40 bytes, it cannot be embedded into the blockchain and will return an error

Attribute | Type | Description
--------- | ---- | -----------
**data** | *string* | The string representing the data to embed, can be either hex-encoded or plaintext.
**token** | *string* | ***Optional*** Your BlockCypher API token, can either be included here or as a URL Parameter in your request.
**encoding** | *string* | ***Optional*** The encoding of your data, can be either *string* (for plaintext) or *hex* (for hex-encoded). If not set, defaults to hex. 
**hash** | *string* | ***Optional*** The hash of the transaction containing your data; only part of return object.

## MicroTX

```shell
{
"from_pubkey": "03bb318b00de944086fad67ab78a832eb1bf26916053ecd3b14a3f48f9fbe0821f",
"to_address": "mrS82nWF3TCfhafSaKnmUUktGNYuTZn1Ap",
"value_satoshis": 5000,
"token": "YOURTOKEN",
"signatures": [
	"3045022100..."
],
"tosign" : [
	"cbac3d3105..."
],
"inputs": [
	{
	"prev_hash": "88e6f4fa0dea77e...",
	"output_index": 2
	},
	{
	"prev_hash": "a02ccb59238178e...",
	"output_index": 1
	}
],
"outputs": [
	{
	"address": "13My...",
	"value": 10000
	},
	{
	"address": "1bs15...",
	"value": 9995590
	}
],
"fees": 735
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
**inputs** | *array[[TXInput](#txinput)]* | ***Optional*** Partial list of inputs that will be used with this transaction. Inputs themsleves are heavily pared down, see cURL sample. Only returned when using **from_pubkey**.
**outputs** | *array[[TXOutput](#txoutput)]* | ***Optional*** Partial list of outputs that will be used with this transaction. Outputs themselves are heavily pared down, see cURL sample. Only returned when using **from_pubkey**.
**fees** | *integer* | ***Optional*** BlockCypher's optimally calculated fees for this MicroTX to guarantee swift 99% confirmation, only returned when using **from_pubkey**. BlockCypher pays these fees for the first 8,000 microtransactions, but like regular transactions, it is deducted from the source address thereafter.
**hash** | *string* | ***Optional*** The hash of the finalized transaction, once sent.

## Address

```shell
curl https://api.blockcypher.com/v1/btc/main/addrs/1DEP8i3QJCsomS4BSMY2RpU1upv62aGvhD

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

An Address represents a public address on a blockchain, and contains information about the state of balances and transactions related to this address. Typically returned from the [Address Balance](#address-balance-endpoint), [Address](#address-endpoint), and [Address Full Endpoint](#address-full-endpoint).

Attribute | Type | Description
--------- | ---- | -----------
**address** | *string* | ***Optional*** The requested address. Not returned if querying a wallet/HD wallet.
**wallet** | *[Wallet](#wallet)* | ***Optional*** The requested wallet object. Only returned if querying by wallet name instead of public address.
**hd_wallet** | *[HDWallet](#hdwallet)* | ***Optional*** The requested HD wallet object. Only returned if querying by HD wallet name instead of public address.
**total_received** | *integer* |Total amount of confirmed satoshis received by this address.
**total_sent** | *integer* | Total amount of confirmed satoshis sent by this address.
**balance**	| *integer* | Balance of confirmed satoshis on this address. This is the difference between outputs and inputs on this address, but only for transactions that have been included into a block (i.e., for transactions whose confirmations > 0).
**unconfirmed_balance**	| *integer* | Balance of unconfirmed satoshis on this address. Can be negative (if unconfirmed transactions are just spending outputs). Only unconfirmed transactions (haven't made it into a block) are included.
**final_balance**	| *integer* |	Total balance of satoshis, including confirmed and unconfirmed transactions, for this address.
**n_tx** | *integer* | Number of confirmed transactions on this address. Only transactions that have made it into a block (confirmations > 0) are counted.
**unconfirmed_n_tx** | *integer* | Number of unconfirmed transactions for this address. Only unconfirmed transactions (confirmations == 0) are counted.
**final_n_tx** | *integer* | Final number of transactions, including confirmed and unconfirmed transactions, for this address.
**tx_url** | *url* | ***Optional*** To retrieve base URL transactions. To get the full URL, concatenate this URL with a transaction's hash.
**txs** | *array[[TX](#tx)]* | ***Optional***  Array of full transaction details associated with this address. Usually only returned from the [Address Full Endpoint](#address-full-endpoint).
**txrefs** | *array[[TXRef](#txref)]* | ***Optional*** Array of transaction inputs and outputs for this address. Usually only returned from the standard [Address Endpoint](#address-endpoint).
**unconfirmed_txrefs** | *array[[TXRef](#txref)]* | ***Optional*** All unconfirmed transaction inputs and outputs for this address. Usually only returned from the standard [Address Endpoint](#address-endpoint).
**hasMore** | *bool* | ***Optional*** If *true*, then the Address object contains more transactions than shown. Useful for determining whether to poll the API for more transaction information.

## AddressKeychain

```shell
curl -X POST https://api.blockcypher.com/v1/btc/test3/addrs

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
**script_type**	| *string* | ***Optional*** If generating a multisig address, the type of multisig script; typically "multisig-n-of-m", where n and m are integers.
**original_address** | *string* | ***Optional*** If [generating an OAP address](#generate-asset-address-endpoint), this represents the parent blockchain's underlying address (the typical **address** listed above).
**oap_address** | *string* | ***Optional*** The OAP address, if generated using the [Generate Asset Address Endpoint](#generate-asset-address-endpoint). 

## Wallet

```shell
curl -d '{"name": "alice","addresses": ["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]}' https://api.blockcypher.com/v1/btc/main/wallets?token=YOURTOKEN

{
"token":"YOURTOKEN",
"name": "alice",
"addresses": ["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]
}
```
Attribute | Type | Description
--------- | ---- | -----------
**token** | *string* | User token associated with this wallet.
**name** | *string* | Name of the wallet.
**addresses** | *array[string]* | List of addresses associated with this wallet.

A Wallet contains a list of addresses associated by its name and the user's token. It can be used interchangeably with all the [Address API](#address-api) endpoints, and in many places that require addresses, like when [Creating Transactions](#creating-transactions).

The name of a wallet must be 1-25 characters long and cannot start with any characters that start an address for the currency contained in the wallet. For example, bitcoin wallet names cannot start with '1' or '3'.

## HDWallet

```shell
curl -d '{"name": "bob", "extended_public_key": "xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8"}' https://api.blockcypher.com/v1/btc/main/wallets/hd?token=YOURTOKEN

{"token": "YOURTOKEN",
"name": "bob",
"hd": true,
"extended_public_key": "xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8",
"chains": [
	{"chain_addresses": [
		{
			"address": "1FHz8bpEE5qUZ9XhfjzAbCCwo5bT1HMNAc",
			"path": "m/0"
		},
		{
			"address": "1J8QDN1u7iDMbJktbqXPSrAqruNjkmRFmT",
			"path": "m/1"
		},
		{
			"address": "1MWNKnYfE2LVdvAzFUioF3F3JXFpRfDCQb",
			"path": "m/2"
		}
	]}
]}
```
An HDWallet contains addresses derived from a single seed. Like normal wallets, it can be used interchangeably with all the [Address API](#address-api) endpoints, and in many places that require addresses, like when [Creating Transactions](#creating-transactions).

Attribute | Type | Description
--------- | ---- | -----------
**token** | *string* | User token associated with this HD wallet.
**name** | *string* | Name of the HD wallet.
**chains** | *array[[HDChain](#hdchain)]* | List of HD chains associated with this wallet, each containing [HDAddresses](#hdaddress). A single chain is returned if the wallet has no subchains.
**hd** | *bool* | *true* for HD wallets, not present for normal wallets.
**extended_public_key** | *string* | The extended public key all addresses in the HD wallet are derived from. It's encoded in [BIP32 format](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#serialization-format)
**subchain_indexes** | *array[integer]* | ***optional*** returned for HD wallets created with subchains.

The name of a wallet must be 1-25 characters long and cannot start with any characters that start an address for the currency contained in the wallet. For example, bitcoin wallet names cannot start with '1' or '3'.

## HDChain

```shell
curl https://api.blockcypher.com/v1/btc/main/wallet/hd/bob/addresses?token=YOURTOKEN

{"chains": [
	{"chain_addresses": [
		{
			"address": "1FHz8bpEE5qUZ9XhfjzAbCCwo5bT1HMNAc",
			"path": "m/0"
		},
		{
			"address": "1J8QDN1u7iDMbJktbqXPSrAqruNjkmRFmT",
			"path": "m/1"
		},
		{
			"address": "1MWNKnYfE2LVdvAzFUioF3F3JXFpRfDCQb",
			"path": "m/2"
		}]
	}
]}
```

An array of HDChains are included in every [HDWallet](#hdwallet) and returned from the [Get Wallet](#get-wallet-endpoint), [Get Wallet Addresses](#get-wallet-addresses-endpoint) and [Derive Address in Wallet](#derive-address-in-wallet-endpoint) endpoints.

Attribute | Type | Description
--------- | ---- | -----------
**chain_addresses** | *array[[HDAddress](#hdaddress)]* | Array of HDAddresses associated with this subchain.
**index** | *integer* | ***optional***  Index of the subchain, returned if the wallet has subchains.

## HDAddress

Attribute | Type | Description
--------- | ---- | -----------
**address** | *string* | Standard address representation.
**path** | *string* | The BIP32 path of the HD address.
**public** | *string* | ***optional*** Contains the hex-encoded public key if returned by [Derive Address in Wallet](#derive-address-in-wallet-endpoint) endpoint.

An HD Address object contains an address and its BIP32 HD path (location of the address in the HD tree). It also contains the hex-encoded public key when returned from the [Derive Address in Wallet](#derive-address-in-wallet-endpoint) endpoint.

## OAPIssue

```shell
{
"from_private": "0eb369746401c3369517239...", 
"to_address": "1C3nrGhUDxBbr393u2Wq4PiE8T6oEYjYhrK",
"amount": 200,
"metadata": "1a2b3c4d5e6f"
}
```

An OAPIssue represents a request for either issuance or transfer of new assets, as detailed in the [Asset API](#asset-api).

Attribute | Type | Description
--------- | ---- | -----------
**from_private** | *string* | The private key being used to issue or transfer assets.
**to_address** | *string* | The target OAP address assets for issue or transfer.
**amount** | *int* | The amount of assets being issued or transfered.
**metadata** | *string* | ***Optional*** Hex-encoded metadata that can optionally be encoded into the issue or transfer transaction.

## OAPTX

```shell
{
  "ver": 1,
  "assetid": "1Npqwstp55vgThp4pwAC9UhYkvPJ28b2Ui",
  "hash": "022e05bdfa2e148bc1882cb7a81506b8316fee6957b11625126d075a8cf8791b",
  "confirmed": "2015-10-25T04:50:06Z",
  "received": "2015-10-25T04:49:32.37Z",
  "oap_meta": "1a2b3c4d5e6f",
  "double_spend": false,
  "inputs": [
    {
      "prev_hash": "56253cffa1b3508d106391da3646cda2aee0bd080db427321c77ad11739e4239",
      "output_index": 0,
      "address": "1ByJUiocpifLPaYVTALpA7JYa9DxpGxXKKP",
      "output_value": 1000
    }
  ],
  "outputs": [
    {
      "address": "1C3nrGhUDxBbr393u2Wq4PiE8T6oEYjYhrK",
      "value": 200,
      "original_output_index": 1
    }
  ]
}
```

An OAPTX represents an [Open Assets Protocol](https://github.com/OpenAssets/open-assets-protocol) transaction, generated when issuing or transfering assets.

Attribute | Type | Description
--------- | ---- | -----------
**ver** | *int* | Version of Open Assets Protocol transaction. Typically 1.
**assetid** | *string* | Unique indentifier associated with this asset; can be used to query other transactions associated with this asset.
**hash** | *string* | This transaction's unique hash; same as the underlying transaction on the asset's parent blockchain.
**confirmed** | [*time*](https://tools.ietf.org/html/rfc3339) | ***Optional*** Time this transaction was confirmed; only returned for confirmed transactions.
**received** | [*time*](https://tools.ietf.org/html/rfc3339) | Time this transaction was received.
**oap_meta** | *string* | ***Optional*** Associated hex-encoded metadata with this transaction, if it exists.
**double_spend** | *bool* | *true* if this is an attempted double spend; *false* otherwise.
**inputs** | array[...] | Array of input data, which can be seen explicitly in the cURL example. Very similar to array of [TXInput](#txinput)s, but with values related to assets instead of satoshis.
**outputs** | array[...] | Array of output data, which can be seen explicitly in the cURL example. Very similar to array of [TXOutput](#txoutput)s, but with values related to assets instead of satoshis.

## Event

```shell
curl https://api.blockcypher.com/v1/btc/main/hooks/399d0923-e920-48ee-8928-2051cbfbc369

{
"id": "399d0923-e920-48ee-8928-2051cbfbc369"
"event": "unconfirmed-tx",
"address": "15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
"token": "YOURTOKEN",
"url": "https://my.domain.com/callbacks/new-tx",
"callback_errors": 0
}
```

An Event represents a WebHooks or WebSockets-based notification request, as detailed in the [Events & Hooks](#events-and-hooks) section of the documentation.

Attribute | Type | Description
--------- | ---- | -----------
**id** | *string* | Identifier of the event; generated when a new request is created.
**event** | *string* | Type of event; can be *unconfirmed-tx*, *new-block*, *confirmed-tx*, *tx-confirmation*, *double-spend-tx*, *tx-confidence*.
**hash**	| *string* | ***optional*** Only objects with a matching hash will be sent. The hash can either be for a block or a transaction.
**wallet_name**	|  *string* | ***optional*** Only transactions associated with the given wallet will be sent; can use a regular or HD wallet name. If used, requires a user token.
**token**	| *string* | ***optional*** Required if wallet_name is used, though generally we advise users to include it (as they can reach API throttling thresholds rapidly).
**address**	| *string* | ***optional*** Only transactions associated with the given address will be sent. A wallet name can also be used instead of an address, which will then match on any address in the wallet.
**confirmations** | *integer* | ***optional***  Used in concert with the *tx-confirmation* event type to set the number of confirmations desired for which to receive an update. You'll receive an updated [TX](#tx) for every confirmation up to this amount. The maximum allowed is 10; if not set, it will default to 6.
**confidence** | *float* | ***optional*** Used in concert with the *tx-confidence* event type to set the minimum [confidence](#confidence-factor) for which you'll receive a notification. You'll receive a [TX](#tx) once this threshold is met. Will accept any *float* between 0 and 1, exclusive; if not set, defaults to 0.99.
**script**	| *string* | ***optional*** Only transactions with an output script of the provided type will be sent. The recognized types of scripts are: *pay-to-pubkey-hash*, *pay-to-multi-pubkey-hash*, *pay-to-pubkey*, *pay-to-script-hash*, *null-data* (sometimes called OP_RETURN), *empty* or *unknown*.
**url** | *url* | ***optional*** Callback URL for this Event's WebHook; not applicable for WebSockets usage.
**callback_errors** | *int* | Number of errors when attempting to POST to callback URL; not applicable for WebSockets usage.

## PaymentForward

```shell
curl -d '{"destination":"15qx9ug952GWGTNn7Uiv6vode4RcGrRemh","callback_url": "https://my.domain.com/callbacks/new-pay","process_fees_address": "1LWw6FdzNUcX8bnekMMZ7eofcGF7SXmbrL", "process_fees_percent": 0.1,"token":"YOURTOKEN"}' https://api.blockcypher.com/v1/btc/main/payments

{
"input_address": "16uKw7GsQSzfMaVTcT7tpFQkd7Rh9qcXWX",
"destination": "15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
"callback_url": "https://my.domain.com/callbacks/new-pay"
"process_fees_address": "1LWw6FdzNUcX8bnekMMZ7eofcGF7SXmbrL",
"process_fees_percent": 0.1,
"token": "YOURTOKEN"
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


## Job

```shell
{
	"token": "YOURTOKEN",
	"analytics_engine": "payeeaddresses",
	"created_at": "2016-07-11T19:33:35.518605498Z",
	"completed_at": "2016-07-11T19:35:25.539002964Z",
	"finished": true,
	"started": true,
	"ticket": "50b77697-ff13-4a50-8a2b-cee605bc3000",
	"result_path": "https://api.blockcypher.com/v1/btc/main/analytics/job/50b77697-ff13-4a50-8a2b-cee605bc3000/results",
	"args": {
		"address": "1bones5gF1HJeiexQus6UtvhU4EUD4qfj",
		"value_threshold": 100000000
	}
}
```

A Job represents an analytics query set up through the [Analytics API](#analytics-api).

Attribute | Type | Description
--------- | ---- | -----------
**token** | *string* | The token that created this job.
**analytics_engine** | *string* | The *engine* used for the job query.
**created_at** | [*time*](https://tools.ietf.org/html/rfc3339) | The time this job was created.
**completed_at** | [*time*](https://tools.ietf.org/html/rfc3339) | ***Optional*** When this job was completed; only present on complete jobs.
**finished** | *bool* | **true** if this job is finished processing, **false** otherwise. 
**started** | *bool* | **true** if this job has begun processing, **false** otherwise.
**ticket** | *string* | Unique identifier for this job, used to get job status and results.
**result_path** | *url* | ***Optional*** URL to query job results; only present on complete jobs.
**args** | [JobArgs](#jobargs) | Query arguments for this job.

## JobArgs

```shell
{
	"address": "1bones5gF1HJeiexQus6UtvhU4EUD4qfj",
	"degree": 2,
	"limit": 100
}
```

A JobArgs represents the query parameters of a particular analytics job, used when [Creating an Analytics Job](#create-analytics-job) and returned within a [Job](#job). Note that the required and optional arguments can change depending on the *engine* you're using; for more specifics check the [Analytics Engine and Parameters](#analytics-engines-and-parameters) section.

Attribute | Type | Description
--------- | ---- | -----------
**address** | *string* | Address hash this job is querying.
**value_threshold** | *int* | Minimal/threshold value (in satoshis) to query.
**limit** | *int* | Limit of results to return.
**start** | [*time*](https://tools.ietf.org/html/rfc3339) | Beginning of time range to query.
**end** | [*time*](https://tools.ietf.org/html/rfc3339) | End of time range to query.
**degree** | *int* | Degree of connectiveness to query.
**source** | *string* | IP address and port, of the form "0.0.0.0:80". Ideally an IP and port combination found from another API lookup (for example, **relayed_by** from the [Transaction Hash Endpoint](#transaction-hash-endpoint))

## JobResults

```shell
{
	"page": 0,
	"more": true,
	"next_page": "https://api.blockcypher.com/v1/btc/main/analytics/job/50b77697-ff13-4a50-8a2b-cee605bc3000/results?page=1",
	"results": [
		{
			"DstAddr": "1changemCPo732F6oYUyhbyGtFcNVjprq",
			"SrcAddr": "1bones5gF1HJeiexQus6UtvhU4EUD4qfj",
			"TxHash": "0x3b06b1e9d70217d5e02644703fe79f54355b0ea05cd535787f5a6c627f1c
			43ef",
			"Value": 1e+08
		},
		....
	]
}
```

A JobResults represents the result of a particular analytics job, returned from [Get Analytics Job Results](#get-analytics-job-results). Note that the **results** field will depend largely on the *engine* used.

Attribute | Type | Description
--------- | ---- | -----------
**page** | *int* | Current page of results.
**more** | *bool* | *true* if there are more results in a separate page; *false* otherwise.
**next_page** | *url* | ***Optional*** URL to get the next page of results; only present if there are more results to show.
**results** | *array[...]* | Results of analytics job; structure of results are dependent on *engine*-type of query, but are generally either *strings* of address hashes or JSON objects.
