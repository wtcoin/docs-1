# Wallet API

The Wallet API allows you to group multiple addresses under a single name. It only holds public address information and never requires any private keys.

<aside class="notice">
Don't be confused: this Wallet API has nothing to do with private key management. The best way to think of the Wallet API is a "Set of Public Addresses to Query Together" API, but that name refused to fit into any of our marketing materials.
</aside>

A normal Wallet can be created, deleted, and have addresses added and removed. The Wallet itself can have any custom name as long as it does not start with the standard address prefix (1 or 3 for Bitcoin).

## Hierarchical Deterministic (HD) Wallets

We also offer support for HD Wallets, which make it easy to manage multiple addresses under a single name. All HD wallet addresses are derived from a single seed. Please see [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) for more background on HD wallets.

HD Wallets can be created, deleted, and have new addresses generated. However, unlike normal Wallets, addresses cannot be removed.

When creating a wallet, one can optionally include one or more "subchain" indexes. These subchains can later be referenced when generating new addresses or sending txs. If none are provided in wallet creation, the wallet will derive & use addresses straight from the given extended pubkey. If no index is given when using the wallet with other APIs, it defaults to using the wallet's first (sub) chain.

In BIP32 notation, the wallet layout is m/0, m/1, ... and m/**i**/0, m/**i**/1, ... for each subchain **i** if the wallet has subchains. For example, the path of the fourth address generated is m/3 for a non-subchain wallet. The path of the fourth address at subchain index two is m/2/3. Note that this is different from the default BIP32 wallet layout.

If you want to use [BIP32 default wallet layout](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#The_default_wallet_layout) you can submit the extended public key of m/0' (which can only be derived from your master private key) with subchain indexes = [0, 1]. Subchain index 0 represents the external chain (of account 0) and will discover all k keypairs that look like: m/0'/0/k. Subchain index 1 represents the internal chain (of account 0) and will discover all k keypairs in m/0'/1/k.

If you want to use [BIP 44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) layout (for BTC), you can submit the extended public key of m/44'/0'/0'. (which can only be derived from your master private key) with subchain indexes = [0, 1]. Subchain index 0 represents the external chain (of account 0) and will discover all k keypairs in m/44'/0'/0'/0/k. Subchain index 1 represents the internal chain (of account 0) and will discover all k keypairs in m/44'/0'/0'/1/k.

If an address ahead of current addresses listed in an HD Wallet receives a transaction, it will be added, along with any addresses between the new address and the last used one. We limit looking ahead to 10 addresses; if more than 10 addresses are skipped for the next payment to an HD Wallet, it won't be detected/added to the HD Wallet.

## Using Wallets

Both HD Wallets and normal Wallets can be leveraged by the [Address API](#address-api), just by using their $NAME instead of $ADDRESS. They can also be used with [Events](#events-and-hooks) and with the [Transaction API](#transaction-api). In general, using a wallet instead of an address in an API will have the effect of [batching the set of addresses](#batching) contained in the wallet.

<aside class="notice">
You are required to authenticate with your <a href="#rate-limits-and-tokens">user token</a> when using a Wallet or HD Wallet in any API endpoint. You can register <a href="http://accounts.blockcypher.com/">for a token here.</a>
</aside>

The following code examples should be considered serially; that is to say, the results will appear as if each API call were done sequentially. Also, $NAME is a *string* representing the name of your wallet, for example:

`alice`

As you'll see in the examples, if you're using HD Wallets, take care to use the appropriate resource (e.g. `/wallets/hd` instead of `/wallets` ).

<aside class="warning">
HD Wallets and Wallets share the same namespace under a particular coin/chain. For example, under a single token, you cannot have both an HD Wallet and normal Wallet named "alice" on Bitcoin Main.
</aside>

## Create Wallet Endpoint

```shell
# normal wallet
curl -d '{"name": "alice","addresses": ["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]}' https://api.blockcypher.com/v1/btc/main/wallets?token=YOURTOKEN

{"token": "YOURTOKEN",
"name": "alice",
"addresses": [
	"1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
]}

# hd wallet
curl -d '{"name": "bob", "extended_public_key": "xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8", "subchain_indexes": [1, 3]}' https://api.blockcypher.com/v1/btc/main/wallets/hd?token=YOURTOKEN

{"token": "YOURTOKEN",
"name": "bob",
"hd": true,
"extended_public_key": "xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8",
"subchain_indexes": [ 1, 3 ]}
```

```javascript
//normal wallet
var data = {"name": "alice","addresses": ["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]};
$.post('https://api.blockcypher.com/v1/btc/main/wallets?token=USERTOKEN', data)
  .then(function(d) {console.log(d)});
> {"token": "USERTOKEN",
> "name": "alice",
> "addresses": [
> 	"1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
> ]}

//hd wallet
var data = {"name": "bob", "extended_public_key": "xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8, "subchain_indexes": [ 1, 3 ]};
$.post('https://api.blockcypher.com/v1/btc/main/wallets?token=USERTOKEN', data)
  .then(function(d) {console.log(d)});
> {"token": "YOURTOKEN",
> "name": "bob",
> "hd" : true,
> "extended_public_key": "xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoC> u1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8", "subchain_indexes": [ 1, 3 ]}
> }
```

```ruby
> block_cypher.wallet_create("alice",["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"])
=> {"token"=>"YOURTOKEN", "name"=>"alice", "addresses"=>["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]}

# TODO: ruby hd wallet example
```

```python
//normal wallet
>>> import requests, json
>>> data = {'name': 'alice', 'addresses': ['1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e', ]}
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.post('https://api.blockcypher.com/v1/btc/main/wallets', data=json.dumps(data), params=params)
>>> r.json()
{'name': 'alice',
 'token': 'YOUR_TOKEN',
 'addresses': ['1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e']}

//hd wallet
>>> data = {'name': 'bob', 'extended_public_key': 'xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoC> u1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8', 'subchain_indexes': [1, 3]}
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.post('https://api.blockcypher.com/v1/btc/main/wallets/hd', data=json.dumps(data), params=params)
>>> r.json()
{'name': 'bob',
 'token': 'YOUR_TOKEN',
 'extended_public_key': 'xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoC> u1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8',
 'subchain_indexes': [1, 3]}
```

```php
<?php
// normal wallet
// Run on console:
// php -f .\sample\wallet-api\CreateWalletEndpoint.php

// Create a new instance of Wallet object
$wallet = new Wallet();
$wallet->setName('alice');
$wallet->setAddresses(array(
    "1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
));

$walletClient = new WalletClient($apiContext);
$createdWallet = $walletClient->create($wallet);

{
  "token":"c0afcccdde5081d6429de37d16166ead",
  "name":"alice",
  "addresses":[
    "1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
  ]
}

// hd wallet
// Run on console:
// php -f .\sample\wallet-api\CreateHDWalletEndpoint.php

// Create a new instance of HDWallet object
$wallet = new HDWallet();
$wallet->setName('bob');
$wallet->setExtendedPublicKey('xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8');
$wallet->setSubchainIndexes(array(1, 3));

$walletClient = new HDWalletClient($apiContext);
$createdWallet = $walletClient->create($wallet);

{
  "token":"c0afcccdde5081d6429de37d16166ead",
  "name":"bob",
  "hd":true,
  "extended_public_key":"xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8",
  "subchain_indexes":[
    1,
    3
  ]
}

```

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/wallets | POST | [Wallet](#wallet) | [Wallet](#wallet)
/wallets/hd | POST | [HDWallet](#hdwallet) | [HDWallet](#hdwallet)

This endpoint allows you to create a new wallet, by POSTing a partially filled out [Wallet](#wallet) or [HDWallet](#hdwallet) object, depending on the endpoint. 

For normal wallets, at minimum, you must include the **name** attribute and at least one public address in the **addresses** array.

For HD wallets, at minimum, you must include the **name** and the **extended_public_key** attributes. The encoding of the key is [documented here.](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#serialization-format) You can optionally include **subchain_indexes** to initialize the wallet with one or more subchains. If not given, the wallet will derive address straight from the given extended pubkey. See [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#The_default_wallet_layout) for more info.

If successful, it will return the same [Wallet](#wallet) or [HDWallet](#hdwallet) object you requested, appended with your user token.

<aside class="warning">
If the named wallet already exists under your token, attempting to create a new wallet will return an error. As mentioned above, Wallets and HDWallets share the same namespace, so an error will be returned if you attempt to create a named HD Wallet that shares a name with a normal Wallet under your token.
</aside>

## Get Wallet Endpoint

```shell
# normal wallet
curl https://api.blockcypher.com/v1/btc/main/wallets/alice?token=YOURTOKEN

{"token":"YOURTOKEN",
"name":"alice",
"addresses": [ "1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]}

# hd wallet
curl https://api.blockcypher.com/v1/btc/main/wallets/hd/bob?token=YOURTOKEN

{"token": "YOURTOKEN",
"name": "bob",
"hd": true,
"extended_public_key": "xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8",
"subchain_indexes": [ 1, 3 ]}
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

//TODO: hd wallet example
```

```ruby
> block_cypher.wallet_get("alice")
=> {"token"=>"YOURTOKEN", "name"=>"alice", "addresses"=>["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]}

//TODO: hd wallet example
```

```python
>>> import requests
>>> params = {'token': 'YOURTOKEN'}
>>> r = requests.get('https://api.blockcypher.com/v1/btc/main/wallets/alice', params=params)
>>> r.json()
{'name': 'alice',
 'token': 'YOURTOKEN',
 'addresses': ['1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e']}

//TODO: hd wallet example
```

```php
<?php
// normal wallet
// Run on console:
// php -f .\sample\wallet-api\GetWalletEndpoint.php

$walletClient = new WalletClient($apiContext);
$wallet = $walletClient->get('alice');

{
  "token":"c0afcccdde5081d6429de37d16166ead",
  "name":"alice",
  "addresses":[
    "1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
  ]
}

// hd wallet
// Run on console:
// php -f .\sample\wallet-api\GetHDWalletEndpoint.php

$walletClient = new HDWalletClient($apiContext);
$wallet = $walletClient->get('bob');

{
  "token":"c0afcccdde5081d6429de37d16166ead",
  "name":"bob",
  "hd":true,
  "extended_public_key":"xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8",
  "subchain_indexes":[
    1,
    3
  ]
}
```

Resource | Method | Return Object
-------- | ------ | -------------
/wallets/$NAME | GET | [Wallet](#wallet)
/wallets/hd/$NAME | GET | [HDWallet](#hdwallet)

This endpoint returns a [Wallet](#wallet) or [HDWallet](#hdwallet) based on its $NAME.

## Add Addresses to Wallet Endpoint

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

```ruby
> block_cypher.wallet_add_addr("alice",["13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j"])
=> {"token"=>"YOURTOKEN", "name"=>"alice", "addresses"=>["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e", "13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j"]}
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

```php
<?php
// Run on console:
// php -f .\sample\wallet-api\AddAddressesToWalletEndpoint.php

$walletClient = new WalletClient($apiContext);
$addressList = AddressList::fromAddressesArray(array(
    "13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j"
));
$wallet = $walletClient->addAddresses('alice', $addressList);

{
  "token":"c0afcccdde5081d6429de37d16166ead",
  "name":"alice",
  "addresses":[
    "1jr1rHMthQVMNSYswB9ExSvYn339fWMzn",
    "13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j"
  ]
}
```

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/wallets/$NAME/addresses | POST | [Wallet](#wallet) | [Wallet](#wallet)

This endpoint allows you to add public addresses to the $NAME wallet, by POSTing a partially filled out [Wallet](#wallet) object. You only need to include the additional addresses in a new **addresses** array in the object. If successful, it will return the newly modified [Wallet](#wallet), including an up-to-date, complete listing of addresses.

<aside class="notice">
You cannot add custom addresses to HD Wallets, since they are all derived from your extended public key. You can always generate new addresses client-side, or using the <a href="#generate-address-in-wallet-endpoint">Generate Address in Wallet Endpoint below.</a>
</aside>

## Get Wallet Addresses Endpoint

```shell
#normal wallet
curl https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses?token=YOURTOKEN

{"addresses": [
	"13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j",
	"1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
]}

#hd wallet
curl https://api.blockcypher.com/v1/btc/main/wallets/hd/bob/addresses?token=YOURTOKEN

{}
```

```javascript
//normal wallet
$.get('https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses?token=USERTOKEN')
  .then(function(d) {console.log(d)});
> {
> "addresses": [
> 	"13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j",
> 	"1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
> ]}

//hd wallet
$.get('https://api.blockcypher.com/v1/btc/main/wallets/hd/bob/addresses?token=USERTOKEN')
  .then(function(d) {console.log(d)});
> {}
```

```ruby
> block_cypher.wallet_get_addr("alice")
=> {"addresses"=>["13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j", "1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]}

# TODO: ruby hd example
```

```python
//normal wallet
>>> import requests
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.get('https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses', params=params)
>>> r.json()
{'addresses': ['13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j',
  '1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e']}

//hd wallet
>>> r = requests.get('https://api.blockcypher.com/v1/btc/main/wallets/hd/alice/addresses', params=params)
>>> r.json()
{}
```

```php
<?php
// normal wallet
// Run on console:
// php -f .\sample\wallet-api\WalletAddressesEndpoint.php

$walletClient = new WalletClient($apiContext);
$addressList = $walletClient->getWalletAddresses('alice');

{
  "addresses":[
    "13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j",
    "1jr1rHMthQVMNSYswB9ExSvYn339fWMzn"
  ]
}

// hd wallet
// Run on console:
// php -f .\sample\wallet-api\GetHDWalletAddressesEndpoint.php

$walletClient = new HDWalletClient($apiContext);
$addressList = $walletClient->getWalletAddresses('bob');

{
  "addresses":[
    "13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j",
    "1jr1rHMthQVMNSYswB9ExSvYn339fWMzn"
  ]
}
```

Resource | Method | Return Object
-------- | ------ | -------------
/wallets/$NAME/addresses | GET | [Wallet](#wallet)
/wallets/hd/$NAME/addresses | GET | [HDWallet](#hdwallet)

This endpoint returns a list of the addresses associated with the $NAME wallet. It returns the addresses in a partially filled out [Wallet](#wallet) or [HDWallet](#hdwallet) object, which you'll find under the **addresses** attribute.

## Remove Addresses from Wallet Endpoint

```shell
$ curl -X DELETE https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses?token=USERTOKEN&address=1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e

```

```javascript
$.ajax({
  url: "https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses?token=USERTOKEN&address=1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e",
  method: "DELETE"
})
```

```ruby
> block_cypher.wallet_delete_addr("alice",["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"])
=> nil
```

```python
>>> import requests, json
>>> data = {'token': 'YOUR_TOKEN', 'address': '1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e'}
>>> r = requests.delete('https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses', data=json.dumps(data))
# returns nothing, let's just check the status code to be sure
>>> assert r.status_code == 204
```

```php
<?php
// Run on console:
// php -f .\sample\wallet-api\RemoveAddressesFromWalletEndpoint.php

$walletClient = new WalletClient($apiContext);
// List of addresses to be removed from the wallet
$addressList = AddressList::fromAddressesArray(array(
    "13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j"
));
$wallet = $walletClient->removeAddresses('alice', $addressList);

{
  "token":"c0afcccdde5081d6429de37d16166ead",
  "name":"alice",
  "addresses":[
    "1jr1rHMthQVMNSYswB9ExSvYn339fWMzn"
  ]
}
```

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/wallets/$NAME/addresses?address=$ADDRESS | DELETE | *nil* | *nil*

This endpoint allows you to delete an $ADDRESS associated with the $NAME wallet. As a reminder, you [can batch](#http://dev.blockcypher.com/#batching) multiple addresses by appending them with semicolons within the $ADDRESS URL parameter. If successful, it will return an HTTP 204 status code with no return object.

<aside class="notice">
You cannot remove addresses from HD Wallets.
</aside>

## Generate Address in Wallet Endpoint

```shell
# normal wallet
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

#hd wallet
curl -X POST https://api.blockcypher.com/v1/btc/main/wallets/hd/bob/addresses/generate?token=YOURTOKEN

{"token": "YOURTOKEN",
"name": "bob",
"addresses": [
  "1NwEtFZ6Td7cpKaJtYoeryS6avP2TUkSMh"
],
"hd": true,
"address": "1NwEtFZ6Td7cpKaJtYoeryS6avP2TUkSMh",
"public": "029b393153a1ec68c7af3a98e88aecede3a409f27e698c090540098611c79e05b0"}

#hd wallet with subchain_index
curl -X POST 'https://api.blockcypher.com/v1/btc/main/wallets/hd/bob/addresses/generate?token=YOURTOKEN&subchain_index=1'

{"token": "YOURTOKEN",
"name": "bob",
"addresses": [
  "1NwEtFZ6Td7cpKaJtYoeryS6avP2TUkSMh",
  "1NZ97rKhSPy6NLud5Dp89E4yH5a2fUGeyC"
],
"hd": true,
"subchain_index": 1,
"address": "1NZ97rKhSPy6NLud5Dp89E4yH5a2fUGeyC",
"public": "03d18a97975c5f2e11dfa22dd686315f27b35c2db5d32cd7d0c11aea146fdd17c2"}
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

//todo: javascript hd examples
```

```ruby
> block_cypher.wallet_gen_addr("alice")
=> {"token"=>"YOURTOKEN",
 "name"=>"alice",
 "addresses"=>["13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j", "1Bq7QRwfa36p5DGSe6UBy4PYmsEtQhpqC8"],
 "private"=>"f371f0a43f46a09ec2351fb1fc23450167d6031dbadef56f7174dc1e0285fed9",
 "public"=>"037318b43fc83f3af24b5d88c8e4e33ebbdea62c3a3f6eb3a9830e2bb1f6682108",
 "address"=>"1Bq7QRwfa36p5DGSe6UBy4PYmsEtQhpqC8",
 "wif"=>"L5NwFyFdHqfBYhaQDttym6b3tXeaKaH7cepgAJunWrkKY24NUCjb"}

# todo: ruby hd examples
```

```python
# Please use the python library at https://github.com/blockcypher/blockcypher-python to securely generate an address client-side
```

```php
<?php
// normal wallet
// Run on console:
// php -f .\sample\wallet-api\GenerateAddressInWalletEndpoint.php

$walletClient = new WalletClient($apiContext);
$walletGenerateAddressResponse = $walletClient->generateAddress('alice');

{
  "token":"c0afcccdde5081d6429de37d16166ead",
  "name":"alice",
  "addresses":[
    "1jr1rHMthQVMNSYswB9ExSvYn339fWMzn",
    "13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j"
  ],
  "private":"0d42eb9fa80301aa3a5c4e91d2a5c892b4fb99b10bc1f0d636d46ecba32d37e6",
  "public":"030b4780c12784456291191318b231dcce64c93f8eb211fa78eb7e95663009e414",
  "address":"1GuPv187EEUi3BDypdpMS7cmMuaVpWy6PM",
  "wif":"KwfVLLsRSgSC6pkDLkEmMn8fH5VFceaYT9B58Cu8QHZtZzJvnpYZ"
}

// hd wallet
// Run on console:
// php -f .\sample\wallet-api\GenerateAddressInHDWalletEndpoint.php

$walletClient = new HDWalletClient($apiContext);
$hdWalletGenerateAddressResponse = $walletClient->generateAddress('bob');

{
  "token":"c0afcccdde5081d6429de37d16166ead",
  "name":"bob",
  "addresses":[
    "1NwEtFZ6Td7cpKaJtYoeryS6avP2TUkSMh"
  ],
  "hd":true,
  "address":"1NwEtFZ6Td7cpKaJtYoeryS6avP2TUkSMh",
  "public":"029b393153a1ec68c7af3a98e88aecede3a409f27e698c090540098611c79e05b0"
}

// hd wallet with subchain_index
// Run on console:
// php -f .\sample\wallet-api\GenerateAddressInHDWalletWithSubchainIndexEndpoint.php

$walletClient = new HDWalletClient($apiContexts['BTC.main']);
$params = array('subchain_index' => 1);
$hdWalletGenerateAddressResponse = $walletClient->generateAddress('bob', $params);

{
  "token":"c0afcccdde5081d6429de37d16166ead",
  "name":"bob",
  "addresses":[
    "1NwEtFZ6Td7cpKaJtYoeryS6avP2TUkSMh",
    "18FcseQ86zCaXzLbgDsH86292xb2EuKtFW"
  ],
  "hd":true,
  "subchain_index":1,
  "address":"18FcseQ86zCaXzLbgDsH86292xb2EuKtFW",
  "public":"02518873d92d8e9a7720134ef499621eb793ecd85894f5da03ae172a392c69bce8"
}
```

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/wallets/$NAME/addresses/generate | POST | *nil* | [Wallet](#Wallet) + [AddressKeychain](#AddressKeychain)
/wallets/hd/$NAME/addresses/generate | POST | *nil* | [Wallet](#Wallet) + [AddressKeychain](#AddressKeychain)

This endpoint allows you to generate a new address associated with the $NAME wallet, similar to the [Generate Address Endpoint](#generate-address-endpoint). If successful, it will returned the newly modified [Wallet](#wallet) or [HDWallet](#hdwallet) composed with an [AddressKeychain](#AddressKeychain).

For HD Wallets, include a **subchain_index** to generate address on a specific subchain. Otherwise the address is generated on the first chain in the wallet.

## Delete Wallet Endpoint

```shell
# normal wallet
curl -X DELETE https://api.blockcypher.com/v1/btc/main/wallets/alice?token=YOURTOKEN

# hd wallet
curl -X DELETE https://api.blockcypher.com/v1/btc/main/wallets/hd/bob?token=YOURTOKEN
```

```javascript
//normal wallet
$.ajax({
  url: "https://api.blockcypher.com/v1/btc/main/wallets/alice?token=USERTOKEN",
  method: "DELETE"
});

//hd wallet
$.ajax({
  url: "https://api.blockcypher.com/v1/btc/main/wallets/hd/bob?token=USERTOKEN",
  method: "DELETE"
});
```

```ruby
> block_cypher.wallet_delete("alice")
=> nil

# TODO: Ruby hd wallet example
```

```python
//normal wallet
>>> import requests
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.delete('https://api.blockcypher.com/v1/btc/main/wallets/alice', params=params)
# returns nothing, let's just check the status code to be sure
>>> assert r.status_code == 204

//hd wallet
>>> r = requests.delete('https://api.blockcypher.com/v1/btc/main/wallets/hd/bob', params=params)
```

```php
<?php
// normal wallet
// Run on console:
// php -f .\sample\wallet-api\DeleteWalletEndpoint.php

$walletClient = new WalletClient($apiContext);
$result = $walletClient->delete('alice');

// hd wallet
// Run on console:
// php -f .\sample\wallet-api\DeleteHDWalletEndpoint.php

$walletClient = new HDWalletClient($apiContext);
$result = $walletClient->delete('bob');
```

Resource | Method | Return Object
-------- | ------ | -------------
/wallets/$NAME | DELETE | *nil*
/wallets/hd/$NAME | DELETE | *nil*

This endpoint deletes the Wallet or HD Wallet with $NAME. If successful, it will return an HTTP 204 status code with no return object.

