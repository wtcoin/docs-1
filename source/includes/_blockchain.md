# Blockchain API

The first component---and highest level---of the BlockCypher API allows you to query general information about blockchain and blocks [based on the coin/chain resource](#restful-resources) you've selected for your endpoints.

If you're new to blockchains, you can think of the blockchain itself as an immutable, distributed ledger. Each block in the blockchain is like a "page" in the ledger containing information about transactions between parties. A great place to start understanding the mechanics behind blockchains is the [original Bitcoin whitepaper.](http://bitcoin.org/bitcoin.pdf)

## Chain Endpoint

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
>>> import requests
>>> r = requests.get('https://api.blockcypher.com/v1/btc/main')
>>> r.json()
{'last_fork_height': 359865,
 'time': '2015-06-08T23:03:49.126001425Z',
 'height': 360061,
 'low_fee_per_kb': 12045,
 'peer_count': 250,
 'previous_hash': '000000000000000000bf56ff4a81e399374a68344a64d6681039412de78366b8',
 'hash': '00000000000000000b51219d4c56be4262dd7d1ca34045f6f5389634bab61e94',
 'name': 'BTC.main',
 'latest_url': 'https://api.blockcypher.com/v1/btc/main/blocks/00000000000000000b51219d4c56be4262dd7d1ca34045f6f5389634bab61e94',
 'last_fork_hash': '00000000000000000aa6462fd9faf94712ce1b5a944dc666f491101c996beab9',
 'unconfirmed_count': 693,
 'previous_url': 'https://api.blockcypher.com/v1/btc/main/blocks/000000000000000000bf56ff4a81e399374a68344a64d6681039412de78366b8',
 'high_fee_per_kb': 45997,
 'medium_fee_per_kb': 29422}
```

General information about a blockchain is available by GET-ing the [base resource](#restful-resources).

Resource | Method | Return Object
-------- | ------ | -------------
/ | GET | [Blockchain](#blockchain)

The returned object contains a litany of information about the blockchain, including its height, the time/hash of the latest block, and more.

For more detailed information about the data returned, check the [Blockchain](#blockchain) object.

## Block Hash Endpoint

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

```python
>>> import requests
>>> r = requests.get('https://api.blockcypher.com/v1/btc/main/blocks/0000000000000000189bba3564a63772107b5673c940c16f12662b3e8546b412')
>>> r.json()
{'tx_url': 'https://api.blockcypher.com/v1/btc/main/txs/',
 'prev_block': '0000000000000000ced0958bd27720b71d32c5847e40660aaca39f33c298abb0',
 'depth': 65739,
 'nonce': 1225187768,
 'height': 294322,
 'received_time': '2014-04-05T07:49:18Z',
 'mrkl_root': '359d624d37aee1efa5662b7f5dbc390e996d561afc8148e8d716cf6ad765a952',
 'ver': 2,
 'n_tx': 10,
 'prev_block_url': 'https://api.blockcypher.com/v1/btc/main/blocks/0000000000000000ced0958bd27720b71d32c5847e40660aaca39f33c298abb0',
 'hash': '0000000000000000189bba3564a63772107b5673c940c16f12662b3e8546b412',
 'chain': 'BTC.main',
 'txids': ['32b3b86e40d996b1f281e24e8d4af2ceacbf874c4038369cc21baa807409b277',
  '1579f716359ba1a207f70248135f5e5fadf539be1dcf5300613aedcb6577d287',
  'dd1f183348eb41eaaa9ecf8012f9cca3ecbae41a6349f0cc4bfd2b1a497fa3d0',
  ...,
 'total': 1146652915,
 'fees': 130999,
 'bits': 419486617,
 'time': '2014-04-05T07:49:18Z'}
```

If you want more data on a particular block, you can use the Block Hash endpoint.

Resource | Method | Return Object
-------- | ------ | -------------
/blocks/$BLOCK_HASH | GET | [Block](#block)

Flag | Type | Effect
---- | ---- | ------
**txstart** | *integer* | Filters response to only include transaction hashes after **txstart** in the block.
**limit** | *integer* | Filters response to only include a maximum of **limit** transactions hashes in the block. Maximum value allowed is 200.

BLOCK_HASH is a *string* representing the hash of the block you're interested in querying, for example:

`0000000000000000189bba3564a63772107b5673c940c16f12662b3e8546b412`

The returned object contains information about the block, including its height, the total amount of satoshis transacted within it, the number of transactions in it, and much more. For more detail on the data returned, check the [Block](#block) object.

## Block Height Endpoint

```shell
curl https://api.blockcypher.com/v1/btc/main/blocks/294322?txstart=1&limit=1

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
	"1579f716359ba1a207f70248135f5e5fadf539be1dcf53...",
],
"depth": 61793,
"prev_block_url": "https://api.blockcypher.com/v1/btc/main/blocks/0000000000000000ced0958bd27720b71d32c5847e40660aaca39f33c298abb0",
"tx_url": "https://api.blockcypher.com/v1/btc/main/txs/"
}
```

```python
# Fund existing address with faucet
>>> import requests
>>> params = {'txstart': 1, 'limit': 1}
>>> r = requests.get('https://api.blockcypher.com/v1/btc/main/blocks/294322', params=params)
>>> r.json()
{'tx_url': 'https://api.blockcypher.com/v1/btc/main/txs/',
 'prev_block': '0000000000000000ced0958bd27720b71d32c5847e40660aaca39f33c298abb0',
 'depth': 65739,
 'nonce': 1225187768,
 'height': 294322,
 'received_time': '2014-04-05T07:49:18Z',
 'mrkl_root': '359d624d37aee1efa5662b7f5dbc390e996d561afc8148e8d716cf6ad765a952',
 'ver': 2,
 'n_tx': 10,
 'prev_block_url': 'https://api.blockcypher.com/v1/btc/main/blocks/0000000000000000ced0958bd27720b71d32c5847e40660aaca39f33c298abb0',
 'hash': '0000000000000000189bba3564a63772107b5673c940c16f12662b3e8546b412',
 'chain': 'BTC.main',
 'next_txids': 'https://api.blockcypher.com/v1/btc/main/blocks/0000000000000000189bba3564a63772107b5673c940c16f12662b3e8546b412?txstart=2&limit=1',
 'txids': ['1579f716359ba1a207f70248135f5e5fadf539be1dcf5300613aedcb6577d287'],
 'total': 1146652915,
 'fees': 130999,
 'bits': 419486617,
 'time': '2014-04-05T07:49:18Z'}
```

You can also query for information on a block using its height, using the same resource but with a different variable type.

Resource | Method | Return Object
-------- | ------ | -------------
/blocks/$BLOCK_HEIGHT | GET | [Block](#block)

Flag | Type | Effect
---- | ---- | ------
**txstart** | *integer* | Filters response to only include transaction hashes after **txstart** in the block.
**limit** | *integer* | Filters response to only include a maximum of **limit** transactions hashes in the block. Maximum value allowed is 200.

BLOCK_HEIGHT is an *integer* representing the height of the block you're interested in querying, for example:

`294322`

As above, the returned object contains information about the block, including its hash, the total amount of satoshis transacted within it, the number of transactions in it, and much more. For more detail on the data returned, check the [Block](#block) object.

<aside class="warning">
With recent blocks, $BLOCK_HEIGHT is not always a unique identifier to a particular block (due to the possibility of soft forks and the nature of the consensus model with blockchains). If you're querying blocks with <b>depth above 10</b> the height should be a safe identifier.
</aside>
