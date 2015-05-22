# Microtransactions API

In addition to our normal [Transactions API](#transactions-api), we offer a unique, on-chain microtransaction endpoint that makes it easy to propagate smaller, nearly-instantly guaranteed, more frequent transactions that are still publicly auditable and trusted through their existence on the blockchain. How can we do what others claim require off-chain, centralized services?

Simple. We leverage our own infrastructure; in particular, our [Zero Confirmation Confidence factor](#zero-confirmation-confidence). Using this factor, when we reach 99% confidence that a microtransaction will reach the next block (usually ~8 seconds), we guarantee its value and return the completed microtransaction. As of this writing, we haven't had a single failed mictrotransaction, and we're improving our confidence data all the time.

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

The endpoint is simple to employ, and uses a slightly pared down transaction object, appropriately called a [Microtransaction](#microtransaction). For low value source addresses and testing, we support directly sending private keys---we'll construct, sign, and send the transaction for you. **We never store the private key, and remove it from server memory as soon as its used.** That said, where your application model supports it and/or when dealing with higher-value source addresses, we encourage you to send public keys and sign the data yourself---a two-step process similar to using `/tx/new` and `/tx/send` with [normal Transactions](#creating-transactions).

In either case, you use the same endpoint.

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/txs/micro | POST | [Microtransaction](#microtransaction) | [Microtransaction](#microtransaction)

The request object is a partially filled out [Microtransaction](#microtransaction), whose format differs depending on whether you're sending a private key or public key for the source address. In either case, there are two options you can set:

- If not set, **change_address** defaults to the original source address computed by the private or public key sent. You can set it manually in the request object, useful if your source address is high-value, or you want to mitigate security risk after sending private keys.
- If not set, **wait_guarantee** defaults to *true*, which means the API will wait for BlockCypher to guarantee the transaction, using our [Zero Confirmation Confidence](#zero-confirmation-confidence) factor. The guarantee usually takes around 8 seconds. If manually set to *false*, the Microtransaction endpoint will return as soon as the transaction is broadcast.

### Via Private Keys

The simplest way to send a Microtransaction is by using a private key. Within the request object you must include **from_private** (or the equivalent**from_wif**), **to_address**, **token**, and **value_satoshis**. You can read more descriptions about these fields within [Microtransaction object description](#microtransaction), although they should be self-explanatory.

The call will hold until the confidence factor reaches 99% (usually about 8 seconds). If successful thereafter, a completed [Microtransaction object](#microtransaction) will be returned (which will include the transaction's **hash** for future queries), along with an HTTP Status Code 201.

### Via Public Keys and Client-side Signing

If your security/application model allows it, we strongly recommend using public keys instead. Much like [creating normal transactions](#creating-transactions) the process requires two endpoint calls; the first is similar to the private key method, but with public keys. E.g., your required object must contain **from_pubkey** (instead of private keys), **to_address**, **token**, and **value_satoshis**.

After POSTing this to `/txs/micro` you'll receive another partially filled out [Microtransaction](#microtransaction) object, but one containing a **tosign** array. You must then sign the data in this array with your locally-stored private key; signing can be a tricky process, but you can use our [signer tool](https://github.com/blockcypher/btcutils/tree/master/signer) as a baseline. Once that data is signed, it must be inserted to a **signatures** within the previously returned [Microtransaction](#microtransaction) object. Then, you send this completed [Microtransaction](#microtransaction) to `/txs/micro`.

As with the private key method, the call will hold until the confidence factor reaches 99% (usually about 8 seconds). If successful thereafter, a completed [Microtransaction object](#microtransaction) will be returned (which will include the transaction's **hash** for future queries), along with an HTTP Status Code 201.

## Security vs Convenience

In general, sending any private key to an outside service is bad security practice. However, security and convenience are often in opposition. In terms of deciding whether passing private keys for microtransactions is a reasonable tradeoff, consider the following:

- The private key data is sent in a JSON document over SSL, protected against eavesdroppers.
- We never log or store any private key. They're only present on our systems during the time of signing.
- The use of a different change addresses is highly encouraged; in this way, leftover value from the source address gets transferred to another private key.
- Regardless of the above, never pass a private key for addresses with high value. The Microtransaction API is meant for low-value transactions and low-value addresses.

Finally, we only offer the option of passing private keys because we understand that signing on the client side can be difficult, depending on the environment. We hope that by fully understanding the tradeoffs and risks, you'll make the right choice for your situation.
