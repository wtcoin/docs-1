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

## TransactionInput

## TransactionOutput

## TransactionTemplate

## Microtransaction

## Wallet

## Address

## AddressKeychain

## Event

## PaymentForward

## PaymentForwardCallback

