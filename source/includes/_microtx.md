# Microtransaction API

In addition to our normal [Transaction API](#transaction-api), we offer a unique, on-chain microtransaction endpoint that makes it easy to propagate smaller, nearly-instantly guaranteed, more frequent transactions that are still publicly auditable and trusted through their existence on the blockchain. How can we do what others claim require off-chain, centralized services?

Simple. We leverage our own infrastructure; in particular, our [Confidence Factor](#confidence-factor). When we reach 98% confidence that a microtransaction will reach the next block (usually ~8 seconds), we guarantee its value and return the completed microtransaction. As of this writing, we haven't had a single failed mictrotransaction, and we're improving our confidence data all the time.

We also pay for the mining fees on transactions for the first 8,000 microtransactions. After which, we automatically optimize fee structure to achieve a balance between guaranteeing confidence it will be in the next block and minimizing cost. If there isn't enough value for the fee, we'll return an error.

<aside class="notice">
In order for us to keep track of mining fees, we require that all calls to the Microtransactions API use a token. You can <a href="http://accounts.blockcypher.com/">register for one here.</a>
</aside>

<aside class="warning">
The Microtransaction API will accept values between <b>2,000 satoshis (~$0.005)</b> and <b>4,000,000 satoshis (~$9.50)</b>. Anything below or above these thresholds will return an error. If you need to transfer larger values, use the <a href="#creating-transactions">standard Transaction process.</a> If you need to transfer lower values, you may want to reevaluate your application.
</aside>

<aside class="warning">
As with creating <a href="#creating-transactions">normal Transactions</a>, always use HTTPS with the Microtransaction API.
</aside>

## Microtransaction Endpoint

The endpoint is simple to employ, and uses a slightly pared down transaction object, appropriately called a [MicroTX](#microtx). For low value source addresses and testing, we support directly sending private keys---we'll construct, sign, and send the transaction for you. **We never store the private key, and remove it from server memory as soon as its used.** That said, where your application model supports it and/or when dealing with higher-value source addresses, we encourage you to send public keys and sign the data yourself---a two-step process similar to using `/tx/new` and `/tx/send` with [normal Transactions](#creating-transactions).

In either case, you use the same endpoint.

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/txs/micro | POST | [MicroTX](#microtx) | [MicroTX](#microtx)

The request object is a partially filled out [MicroTX](#microtx), whose format differs depending on whether you're sending a private key or public key for the source address. In either case, there are two options you can set:

- If not set, **change_address** defaults to the original source address computed by the private or public key sent. You can set it manually in the request object, useful if your source address is high-value, or you want to mitigate security risk after sending private keys.
- If not set, **wait_guarantee** defaults to *true*, which means the API will wait for BlockCypher to guarantee the transaction, using our [Confidence Factor](#confidence-factor). The guarantee usually takes around 8 seconds. If manually set to *false*, the Microtransaction endpoint will return as soon as the transaction is broadcast.

```shell
curl -H "Content-Type: application/json" -d '{ "from_private": "97838249d77bf...", "to_address": "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn", "value_satoshis": 10000, "token": "YOURTOKEN" }' https://api.blockcypher.com/v1/bcy/test/txs/micro

{
"from_private": "97838249d77bfa65f97be02b63fd1b7bb6a58474c7c22784a0da63993d1c2f90",
"to_address": "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn",
"value_satoshis": 10000,
"hash": "892fd7b36c1c3a2e5edb9b4a5d4ffd9ba74d78d3acf3b249991bd8d10a287dbd"
}
```

```javascript
var microtx = {
  from_private: "97838249d77bfa65f97be02b63fd1b7bb6a58474c7c22784a0da63993d1c2f90",
  to_address: "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn",
  value_satoshis: 10000,
  token: TOKEN
}
var url = 'https://api.blockcypher.com/v1/bcy/test/txs/micro?token='+TOKEN;
$.post(url, JSON.stringify(microtx))
  .then(function(d) {console.log(d)});
> {
> "from_private": "97838249d77bfa65f97be02b63fd1b7bb6a58474c7c22784a0da63993d1c2f90",
> "to_address": "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn",
> "value_satoshis": 10000,
> "hash": "892fd7b36c1c3a2e5edb9b4a5d4ffd9ba74d78d3acf3b249991bd8d10a287dbd"
> }
```

```python
>>> import requests, json
>>> data = {'from_private': '97838249d77bfa65f97be02b63fd1b7bb6a58474c7c22784a0da63993d1c2f90', 'to_address': 'C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn', 'value_satoshis': 10000, 'token': 'YOUR_TOKEN'}
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.post('https://api.blockcypher.com/v1/bcy/test/txs/micro', data=json.dumps(data), params=params)
>>> r.json()
```

```php
<?php
// Run on console:
// php -f .\sample\microtransaction-api\MicroTransactionViaPrivateKeyEndpoint.php

$microTX = new MicroTX();
$microTX->setFromPrivate("2c2cc015519b79782bd9c5af66f442e808f573714e3c4dc6df7d79c183963cff");
$microTX->setToAddress("C4MYFr4EAdqEeUKxTnPUF3d3whWcPMz1Fi");
$microTX->setValueSatoshis(10000);

$microTX->create($apiContext);

{
  "from_private":"2c2cc015519b79782bd9c5af66f442e808f573714e3c4dc6df7d79c183963cff",
  "to_address":"C4MYFr4EAdqEeUKxTnPUF3d3whWcPMz1Fi",
  "value_satoshis":10000,
  "hash":"6886c3b2f48e410d83eb0ea4ef18396a699aba453a3210bc39bc7af99f061854"
}
```

### Via Private Keys

The simplest way to send a Microtransaction is by using a private key. Within the request object you must include **from_private** (or the equivalent **from_wif**), **to_address**, **token**, and **value_satoshis**. You can read more descriptions about these fields within [MicroTX object description](#microtx), although they should be self-explanatory.

The call will hold until the Confidence Factor reaches 98% (usually about 8 seconds). If successful thereafter, a completed [MicroTX object](#microtx) will be returned (which will include the transaction's **hash** for future queries), along with an HTTP Status Code 201.

<aside class="warning">
The private key method doesn't work with <b>uncompressed public addresses</b> as sources. You can still use uncompressed public addresses via the public key method listed below, but if you want to use the private key method, please use compressed public addresses (which has been the predominant standard since Bitcoin 0.6.0). You can read more about the distinction between <a href="https://bitcoin.org/en/developer-guide#public-key-formats">uncompressed and compressed keys here.</a>
</aside>

```shell
curl -H "Content-Type: application/json" -d '{ "from_pubkey": "02152e2bb5b273561ece7bbe8b1df...", "to_address": "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn", "value_satoshis": 20000, "token": "YOURTOKEN" }' https://api.blockcypher.com/v1/bcy/test/txs/micro

{
  "from_pubkey": "02152e2bb5b273561ece7bbe...",
  "to_address": "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn",
  "value_satoshis": 20000,
  "tosign": [
    "0f804bef755e623ba3f2c89795814ffa3a0dc53a376ad3deb3c7d3e91957cf12"
  ],
  "token": "YOURTOKEN",
  "inputs": [
		...
  ],
  "outputs": [
	...
  ],
  "fees": 735
}

# Using our CLI signer tool...
./signer 0f804bef755e623ba3f2c89795814ffa3a0dc53a376ad3deb3c7d3e91957cf12 $YOURPRIVATEKEY

30450221009050c3c966d1f895b6d6f6ab544113b88b3ce6908083a58170ef1e32da415e2f022037cb5dae353165f505d18a12c50b318c826b2c3552e41f70f7ade186e0434388

#request object is same as return object above, but + signature
curl -H "Content-Type: application/json" -d '{ ..., "signatures": ["30450221009050c3c966d1f895b6d6f6ab544113b88b3ce6908083a58170ef1e32da415e2f022037cb5dae353165f505d18a12c50b318c826b2c3552e41f70f7ade186e0434388"],...}' https://api.blockcypher.com/v1/bcy/test/txs/micro

{
  "from_pubkey": "02152e2bb5b273561ece7bbe8b1...",
  "to_address": "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn",
  "value_satoshis": 20000,
  "hash": "ce8b3a2d5bb1f552c9d526559dd892b4ee...",
  "inputs": [
	...
  ],
  "outputs": [
	...
  ],
  "fees": 735
}
```

```javascript
// here we're using bitcoinjs built to expose bigi, buffer, and require
// you can use this library here:
//   http://dev.blockcypher.com/data/samples/bitcoinjs-min.js

var bitcoin = require("bitcoinjs-lib");
var bigi    = require("bigi");
var buffer  = require('buffer');
var key     = new bitcoin.ECKey(bigi.fromHex(my_hex_private_key), true);

var microtx = {
  from_pubkey: "02152e2bb5b273561ece7bbe...",
  to_address: "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn",
  value_satoshis: 20000,
  token: TOKEN
}
var url = 'https://api.blockcypher.com/v1/bcy/test/txs/micro?token='+TOKEN;

$.post(url, JSON.stringify(microtx)).then(function(tmptx) {
  // signing each of the hex-encoded string required to finalize the transaction
  tmptx.signatures = tmptx.tosign.map(function(tosign) {
    return key.sign(new buffer.Buffer(tosign, "hex")).toDER().toString("hex");
  });
  // sending back the transaction with all the signatures to broadcast
  $.post(url, tmptx).then(function(finaltx) {
    console.log(finaltx);
  })
});

> {
>   "from_pubkey": "02152e2bb5b273561ece7bbe8b1...",
>   "to_address": "C1rGdt7QEPGiwPMFhNKNhHmyoWpa5X92pn",
>   "value_satoshis": 20000,
>   "hash": "ce8b3a2d5bb1f552c9d526559dd892b4ee...",
>   "inputs": [
> 	...
>   ],
>   "outputs": [
> 	...
>   ],
>   "fees": 735
> }
```


```php
<?php
// Run on console:
// php -f .\sample\microtransaction-api\CreateSignAndSendMicrotransactionEndpoint.php

$microTX = new MicroTX();
$microTX->setFromPubkey("02d4e3404e175923adf89c932fab96758716f6a0a896890f2494c5d9141eb3f543")
    ->setToAddress("C4MYFr4EAdqEeUKxTnPUF3d3whWcPMz1Fi")
    ->setValueSatoshis(10000)
    ->create($apiContext)
    ->sign("2c2cc015519b79782bd9c5af66f442e808f573714e3c4dc6df7d79c183963cff")
    ->send($apiContext);

{
  "from_pubkey":"02d4e3404e175923adf89c932fab96758716f6a0a896890f2494c5d9141eb3f543",
  "to_address":"C4MYFr4EAdqEeUKxTnPUF3d3whWcPMz1Fi",
  "value_satoshis":10000,
  "tosign":[
    "488e6defc3f3d26a20505730a1c64b8a3499e225f1bbd8a0e1b51b38251eed0a"
  ],
  "token":"c0afcccdde5081d6429de37d16166ead",
  "inputs":[
    ...
  ],
  "outputs":[
    ...
  ],
  "fees":735,
  "signatures":[
    "3045022100f99bef2add7d1fd9c...d6d64d8ae19ec090373"
  ],
  "hash":"7fc9e41e6ef29486bc4478ff063934ea9a1ecd08bbfb0a207b39ad92fe55bebf"
}
```

### Via Public Keys and Client-side Signing

If your security/application model allows it, we strongly recommend using public keys instead. Much like [creating normal transactions](#creating-transactions) the process requires two endpoint calls; the first is similar to the private key method, but with public keys. E.g., your required object must contain **from_pubkey** (instead of private keys), **to_address**, **token**, and **value_satoshis**.

After POSTing this to `/txs/micro` you'll receive another partially filled out [MicroTX](#microtx) object, but one containing a **tosign** array. You must then sign the data in this array with your locally-stored private key; signing can be a tricky process, but you can use our [signer tool](https://github.com/blockcypher/btcutils/tree/master/signer) as a baseline. Once that data is signed, it must be inserted to a **signatures** within the previously returned [MicroTX](#microtx) object. Then, you send this completed [MicroTX](#microtx) to `/txs/micro`.

As with the private key method, the call will hold until the Confidence Factor reaches 98% (usually about 8 seconds). If successful thereafter, a completed [MicroTX object](#microtx) will be returned (which will include the transaction's **hash** for future queries), along with an HTTP Status Code 201.

## Security vs Convenience

In general, sending any private key to an outside service is bad security practice. However, security and convenience are often in opposition. In terms of deciding whether passing private keys for microtransactions is a reasonable tradeoff, consider the following:

- The private key data is sent in a JSON document over SSL, protected against eavesdroppers.
- We never log or store any private key. They're only present on our systems during the time of signing.
- The use of a different change addresses is highly encouraged; in this way, leftover value from the source address gets transferred to another private key.
- Regardless of the above, never pass a private key for addresses with high value. The Microtransaction API is meant for low-value transactions and low-value addresses.

Finally, we only offer the option of passing private keys because we understand that signing on the client side can be difficult, depending on the environment. We hope that by fully understanding the tradeoffs and risks, you'll make the right choice for your situation.
