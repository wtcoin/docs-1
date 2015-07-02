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

### Hierarchical Deterministic (HD) Wallets

We also offer support for HD Wallets, which make it easy to manage multiple addresses under a single name. All HD wallet addresses are derived from a single seed.

HD wallets can be created, deleted and have new addresses generated. However, unlike normal addresses cannot be removed.

When creating a wallet, one can optionally include one or more "subchain" indexes. These subchains can later be referenced when generating new addresses or sending txs. If none are provided in wallet creation, the wallet will derive & use addresses straight from the given extended pubkey. If no index is given when using the wallet with other APIs, it defaults to using the wallet's first (sub) chain.

Please see [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) for the background on HD wallets. For subchains, refer to [BIP32 wallet layout.](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#the-default-wallet-layout)

HD Wallets can be used with the [Address API](#address-api), the [Events](#events-and-hooks) and with the [Transaction API](#transaction-api).

If an address ahead of current addresses in a wallet receives a transaction, it will be added to the wallet, along with any addresses between the new address and the last used one.

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

```ruby
> block_cypher.wallet_create("alice",["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"])
=> {"token"=>"YOURTOKEN", "name"=>"alice", "addresses"=>["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]}
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

```php
<?php
// Run on console:
// php -f .\sample\wallet-api\CreateWalletEndpoint.php

// Create a new instance of Wallet object
$wallet = new Wallet();
$wallet->setName('alice');
$wallet->setAddresses(array(
    "1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
));
$wallet->create();

{
  "token":"c0afcccdde5081d6429de37d16166ead",
  "name":"alice",
  "addresses":[
    "1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
  ]
}
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

```ruby
> block_cypher.wallet_get("alice")
=> {"token"=>"YOURTOKEN", "name"=>"alice", "addresses"=>["1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]}
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

$wallet = Wallet::get('alice');
$addressesList = AddressList::fromAddressesArray(array(
    "13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j"
));
$wallet->addAddresses($addressesList);

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

```ruby
> block_cypher.wallet_get_addr("alice")
=> {"addresses"=>["13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j", "1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"]}
```

```python
>>> import requests
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.get('https://api.blockcypher.com/v1/btc/main/wallets/alice/addresses', params=params)
>>> r.json()
{'addresses': ['13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j',
  '1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e']}
```

```php
<?php

// Run on console:
// php -f .\sample\wallet-api\WalletAddressesEndpoint.php

$wallet = Wallet::getOnlyAddresses('alice');

{
  "token":"c0afcccdde5081d6429de37d16166ead",
  "name":"alice",
  "addresses":[
    "13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j",
    "1jr1rHMthQVMNSYswB9ExSvYn339fWMzn"
  ]
}
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

// List of addresses to be removed from the wallet
$addressesList = AddressesList::fromAddressesArray(array(
    "13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j"
));

$wallet = Wallet::get('alice');
$wallet->removeAddresses($addressesList);

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

```ruby
> block_cypher.wallet_gen_addr("alice")
=> {"token"=>"YOURTOKEN",
 "name"=>"alice",
 "addresses"=>["13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j", "1Bq7QRwfa36p5DGSe6UBy4PYmsEtQhpqC8"],
 "private"=>"f371f0a43f46a09ec2351fb1fc23450167d6031dbadef56f7174dc1e0285fed9",
 "public"=>"037318b43fc83f3af24b5d88c8e4e33ebbdea62c3a3f6eb3a9830e2bb1f6682108",
 "address"=>"1Bq7QRwfa36p5DGSe6UBy4PYmsEtQhpqC8",
 "wif"=>"L5NwFyFdHqfBYhaQDttym6b3tXeaKaH7cepgAJunWrkKY24NUCjb"}
```

```python
# Please use the python library at https://github.com/blockcypher/blockcypher-python to securely generate an address client-side
```

```php
<?php
// Run on console:
// php -f .\sample\wallet-api\GenerateAddressInWalletEndpoint.php

$wallet = Wallet::get('alice');
$walletGenerateAddressResponse = $wallet->generateAddress();

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

```ruby
> block_cypher.wallet_delete("alice")
=> nil
```

```python
>>> import requests
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.delete('https://api.blockcypher.com/v1/btc/main/wallets/alice', params=params)
# returns nothing, let's just check the status code to be sure
>>> assert r.status_code == 204
```

```php
<?php
// Run on console:
// php -f .\sample\wallet-api\DeleteWalletEndpoint.php

$wallet = Wallet::get('alice');
$result = $wallet->delete();
```

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/wallets/$NAME | DELETE | [Wallet](#wallet) | *nil*

This endpoint deletes the wallet with $NAME. If successful, it will return an HTTP 204 status code with no return object.

<br />

### Create HD Wallet Endpoint

```shell
curl -d '{"name": "alice", "extended_public_key": "xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8"}' https://api.blockcypher.com/v1/btc/main/wallets/hd?token=YOURTOKEN

{"token": "YOURTOKEN",
"name": "alice",
"hd" : true,
"extended_public_key": "xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8"
}
```

```javascript
var data = {"name": "alice", "extended_public_key": "xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoCu1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8"};
$.post('https://api.blockcypher.com/v1/btc/main/wallets?token=USERTOKEN', data)
  .then(function(d) {console.log(d)});
> {"token": "YOURTOKEN",
> "name": "alice",
> "hd" : true,
> "extended_public_key": "xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoC> u1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8"
> }
```

```ruby
> TODO: Ruby objets & functions for HD
=>
```

```python
>>> import requests, json
>>> data = {'name': 'alice', 'extended_public_key': 'xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoC> u1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8'}
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.post('https://api.blockcypher.com/v1/btc/main/wallets/hd', data=json.dumps(data), params=params)
>>> r.json()
{'name': 'alice',
 'token': 'YOUR_TOKEN',
 'extended_public_key': 'xpub661MyMwAqRbcFtXgS5sYJABqqG9YLmC4Q1Rdap9gSE8NqtwybGhePY2gZ29ESFjqJoC> u1Rupje8YtGqsefD265TMg7usUDFdp6W1EGMcet8'}
```

```php
<?php

// TODO: PHP object & functions for HD
```

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/wallets/hd | POST | [Wallet](#wallet) | [Wallet](#wallet)

This endpoint allows you to create a new HD wallet, by POSTing a partially filled out [Wallet](#wallet) object.

At minimum, you must include the **name** and the **extended_public_key** attributes. The encoding of the key is <a href="https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#serialization-format">documented here</a>

 If successful, it will return the same [Wallet](#wallet) object you requested, appended with your user token.

Pass **subchain_indexes** to initialise the wallet with one or more subchains. If not given, the wallet will derive address straight from the given extended pubkey. See <a href="https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki#The_default_wallet_layout">BIP32</a> for more info.

<aside class="warning">
If the named wallet already exists under your token, attempting to create a new wallet will return an error.
</aside>

<br />

### Generate Address in HD Wallet Endpoint

```shell
curl -X POST https://api.blockcypher.com/v1/btc/main/wallets/hd/alice/addresses/new?token=YOURTOKEN

{"new_address": "13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j"}
```

```javascript
$.post('https://api.blockcypher.com/v1/btc/main/wallets/hd/alice/addresses/new?token=USERTOKEN')
  .then(function(d) {console.log(d)});
> {
>   "new_address": "13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j"
> }
```

```ruby
> TODO: Ruby objets & functions for HD
=>
```

```python
# Please use the python library at https://github.com/blockcypher/blockcypher-python to securely generate an address client-side
```

```php
<?php

// TODO: PHP object & functions for HD

```

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/wallets/hd/$NAME/addresses/new | POST | *nil* | [Wallet](#Wallet) + [AddressKeychain](#AddressKeychain)

This endpoint allows you to generate a new address associated with the $NAME wallet, similar to the [Generate Address Endpoint](#generate-address-endpoint). Pass **subchain_index** to generate address on a specific subchain. Otherwise the address is generated on the first chain in the wallet.

<br />

### Get HD Wallet Addresses Endpoint

```shell
curl https://api.blockcypher.com/v1/btc/main/wallets/hd/alice/addresses?token=YOURTOKEN

{"addresses": [
	"13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j",
	"1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
]}
```

```javascript
$.get('https://api.blockcypher.com/v1/btc/main/wallets/hd/alice/addresses?token=USERTOKEN')
  .then(function(d) {console.log(d)});
> {
> "addresses": [
> 	"13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j",
> 	"1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e"
> ]}
```

```ruby
> TODO: Ruby objets & functions for HD
=>
```

```python
>>> import requests
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.get('https://api.blockcypher.com/v1/btc/main/wallets/hd/alice/addresses', params=params)
>>> r.json()
{'addresses': ['13cj1QtfW61kQHoqXm3khVRYPJrgQiRM6j',
  '1JcX75oraJEmzXXHpDjRctw3BX6qDmFM8e']}
```

```php
<?php

// TODO: PHP object & functions for HD

```

Resource | Method | Return Object
-------- | ------ | -------------
/wallets/$NAME/addresses | GET | [Wallet](#wallet)

This endpoint returns a list of the addresses associated with the $NAME wallet. It returns the addresses in a partially filled out [Wallet](#wallet) object, which you'll find under the **addresses** attribute.

<br />

### Delete Wallet Endpoint

```shell
curl -X DELETE https://api.blockcypher.com/v1/btc/main/wallets/hd/alice?token=YOURTOKEN

```

```javascript
$.ajax({
  url: "https://api.blockcypher.com/v1/btc/main/wallets/hd/alice?token=USERTOKEN",
  method: "DELETE"
});
```

```ruby
> TODO: Ruby objets & functions for HD
=>
```

```python
>>> import requests
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.delete('https://api.blockcypher.com/v1/btc/main/wallets/hd/alice', params=params)
# returns nothing, let's just check the status code to be sure
>>> assert r.status_code == 204
```

```php
<?php
// TODO: PHP object & functions for HD

```

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/wallets/hd/$NAME | DELETE | [Wallet](#wallet) | *nil*

This endpoint deletes the wallet with $NAME. If successful, it will return an HTTP 204 status code with no return object.

<br />
