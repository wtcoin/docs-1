# Confidence Factor

One of the things that makes BlockCypher's API uniquely powerful is our ability to accurately predict the likelihood of an attempted double-spend succeeding against a transaction; this percentage probability is automatically included within the **confidence** attribute on any [unconfirmed transaction](#tx) returned by our API, in addition to the explicit [Transaction Confidence Endpoint](#transaction-confidence-endpoint) detailed below.

So what does that **confidence** attribute actually mean? In simpler terms, if an unconfirmed transaction returns a **confidence** factor of 99.9%, then our data says there's a 0.1% chance that an attempted double-spend will succeed. By design, we're conservative. Even when we return 90% confidence, the likelihood of a successful double-spend is significantly lower than 10%.

As you may have gathered from the frequency of links to this feature in these docs, we're exceptionally proud of the work we've done to build this feature and provide it to our users. And that pride extends to our own confidence in our confidence product. We've leveraged this product internally on two major features:

- Our [live block explorer](https://live.blockcypher.com/) visually shows confidence intervals on unconfirmed transactions directly. Check out a [live view of bitcoin](https://live.blockcypher.com/btc/) and click on one of the most recent unconfirmed transactions. Make sure you pick a very recent transaction: our confidence interval rapidly approaches 99% so quickly that you might miss it!
- Our [Microtransaction API](#microtransaction-api) is possible because of our Confidence Factor. Normally, you'd have to wait ~10 minutes for a block confirmation, which makes immediate, frequent, small on-chain transactions impossible. But since we believe in our confidence product---and have done the work necessary to validate this belief---we can guarantee microtransactions up to ~$9 each.

### How does it work?

There are a host of factors that go into our confidence calculation, but you can broadly organize them into two categories. The *shape* of transactions refers to aspects of transactions that *don't* vary with time (time-invariant) in our model. The *behavior* of transactions refers to aspects of transactions that *do* vary with time (time-variant) in our model.

On the *shape* side of our model, we ask a number of questions about a transaction: How is it structured? What are its inputs and outputs? What's its signature type? Broadly speaking, without knowing anything about the network, does it look like this transaction will be confirmed soon? 

The *behavior* part of our model primarily focuses on how a transaction propagates through the network. In order to model this correctly, you need both a great connection to the network and a lot of past data, which we have. As BlockCypher pools resources for many users, we're always connected to a statistically significant number of nodes on the network---we target anywhere between 10 to 20% of the active nodes on any given blockchain---and offer connections in return. By monitoring transaction propagation, the number of nodes that have received it, and how *quickly* they received it, we can calculate its probability to be the "winning" transaction at any given point in time, if a double-spend was attempted. (As an aside, you can always see if a double spend was attempted by checking the **double_spend** and **double_spend_tx** attributes on an [unconfirmed transaction](#tx))

The result is a robust and reliable metric for judging unconfirmed transaction confidence, especially when used in concert with [webhooks and websockets.](#events-and-hooks) Our work here is based on [several public research results](https://eprint.iacr.org/2012/248.pdf) in addition to our own in-depth follow-up research, [which you can read about here](https://medium.com/blockcypher-blog/from-zero-to-hero-bitcoin-transactions-in-8-seconds-7c9edcb3b734). We are constantly improving our confidence model, and we always publish our findings when we do.

<aside class="notice">
While we stand by our work behind our confidence research, please note that these measures are ultimately mitigation strategies if you need fast transaction confirmations. There's always an appreciable risk when accepting unconfirmed transactions, and there's no truly bulletproof way to secure unconfirmed transactions. Double spending attacks, while extremely unlikely, can still occur. Blockchain confirmations ultimately provide the highest level of security, especially when dealing with higher value addresses.
</aside>

## Transaction Confidence Endpoint

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

Our confidence interval is automatically included in any [unconfirmed transaction](#tx), but you can retrieve it through a specific endpoint too.

Resource | Method | Return Object
-------- | ------ | -------------
/txs/$TXHASH/confidence | GET | [TXConfidence](#txconfidence)

TXHASH is a *string* representing the hex-encoded transaction hash you're interested in querying, for example:

`43fa951e1bea87c282f6725cf8bdc08bb48761396c3af8dd5a41a085ab62acc9`

The returned [TXConfidence](#txconfidence) object contains the all-important confidence percentage, receive count and more.
