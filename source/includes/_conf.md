# Confidence Factor

One of the things that makes BlockCypher's API uniquely powerful is our ability to accurately predict confidence intervals for the likelihood of unconfirmed transactions to make it into the next block. As you may have gathered from the frequency of links to this feature in these docs, we're exceptionally proud of the work we've done to build this feature and provide it to our users.

And that pride extends to our own confidence in our confidence product. We've leveraged this product internally on two major features:

- Our [live block explorer](https://live.blockcypher.com/) visually shows confidence intervals on unconfirmed transactions directly. Check out a [live view of bitcoin](https://live.blockcypher.com/btc/) and click on one of the most recent unconfirmed transactions. Make sure you pick a very recent transaction: our confidence interval rapidly approaches 99% so quickly that you might miss it!
- Our [Microtransactions API](#microtransactions-api) is possible because of our Confidence Factor. Normally, you'd have to wait ~10 minutes for a block confirmation, which makes immediate, frequent, small on-chain transactions impossible. But since we believe in our confidence product---and have done the work necessary to validate this belief---we can guarantee microtransactions up to ~$9 each.

### How does it work?

The confidence is calculated based on both elapsed time, and the number of nodes that have received the transaction (the **receive_count** attribute in a [Transaction](#transaction) object) and is based on [several public research results](https://eprint.iacr.org/2012/248.pdf). We are constantly improving our confidence model, and generally publish these findings on [our blog](http://blog.blockcypher.com/) as well.

As BlockCypher pools resources for many users, we're always connected to a statistically significant number of nodes on the network---we target around 20% of the active nodes on any given blockchain---and offer connections in return. While maintaining those connections, we can also assess whether a given node has received a particular transaction. By monitoring the propagation of transactions across the network, we can calculate its probability to be the "winning" transaction at any given point in time, if a double-spend was attempted. You can also see if a double spend was attempted by checking the **double_spend** and **double_spend_tx** attributes on an unconfirmed [Transaction](#transaction).

The result is a robust and reliable metric for judging unconfirmed transaction confidence, especially when used in concert with [webhooks and websockets.](#events-and-hooks)

<aside class="notice">
While we stand by our work behind our confidence research, please note that these measures are ultimately mitigation strategies if you need fast transaction confirmations. There's always an appreciable risk when accepting unconfirmed transactions, and there's no truly bulletproof way to secure unconfirmed transactions. Double spending attacks, while extremely unlikely, can still occur. Block chain confirmations ultimately provide the highest level of security, especially when dealing with higher value addresses.
</aside>

## Transaction Confidence Endpoint

```shell
$ curl https://api.blockcypher.com/v1/btc/main/txs/43fa951e1bea87c282f6725cf8bdc08bb48761396c3af8dd5a41a085ab62acc9/confidence

{
"age_millis": 12725,
"receive_count": 666,
"confidence": 0.9901509730004237,
"txhash": "43fa951e1bea87c282f6725cf8bdc08bb48761396c3af8dd5a41a085ab62acc9",
"txurl": "https://api.blockcypher.com/v1/btc/main/txs/43fa951e1bea87c282f6725cf8bdc08bb48761396c3af8dd5a41a085ab62acc9"
}
```

```python
# Fund existing address with faucet
>>> import requests
>>> r = requests.get('https://api.blockcypher.com/v1/btc/main/txs/43fa951e1bea87c282f6725cf8bdc08bb48761396c3af8dd5a41a085ab62acc9/confidence')
>>> r.json()
{'age_millis': 12725,
 'txurl': 'https://api.blockcypher.com/v1/btc/main/txs/43fa951e1bea87c282f6725cf8bdc08bb48761396c3af8dd5a41a085ab62acc9',
 'confidence': 0.9901509730004237,
 'receive_count': 666,
 'txhash': '43fa951e1bea87c282f6725cf8bdc08bb48761396c3af8dd5a41a085ab62acc9'}
```

Our confidence interval is automatically included in any unconfirmed [Transaction](#transaction) object, but you can retrieve it through a specific endpoint too.

Resource | Method | Return Object
-------- | ------ | -------------
/txs/$TXHASH/confidence | GET | [TXConfidence](#txconfidence)

$TXHASH is a *string* representing the hex-encoded transaction hash you're interested in querying, for example:

`43fa951e1bea87c282f6725cf8bdc08bb48761396c3af8dd5a41a085ab62acc9`

The returned [TXConfidence](#txconfidence) object contains the all-important confidence percentage, receive count and more.
