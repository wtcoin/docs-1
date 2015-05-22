# Microtransactions API

In addition to our normal [Transactions API](#transactions-api), we offer a unique, on-chain microtransaction endpoint that makes it easy to propagate smaller, nearly-instantly guaranteed, more frequent transactions that are still publicly auditable and trusted through their existence on the blockchain. How can we do what others claim require off-chain, centralized services?

Simple. We leverage our own infrastructure; in particular, our [Zero Confirmation Confidence factor](#zero-confirmation-confidence). Using this factor, when we reach 99% confidence that a microtransaction will reach the next block (usually ~8 seconds), we guarantee its value and return the completed microtransaction. As of this writing, we haven't had a single mictrotransactions, and we're improving our confidence data all the time.

We also pay for the mining fees on transactions for the first 8,000 microtransactions. After which, we automatically optimize fee structure to achieve a balance between guaranteeing confidence it will be in the next block and minimizing cost. If there isn't enough value for the fee, we'll return an error.

<aside class="warning">
The Microtransaction API will accept values between <b>2,000 satoshis (~$0.005)</b> and <b>4,000,000 satoshis (~$9.50)</b>. Anything below or above these thresholds will return an error. If you need to transfer larger values, use the <a href="#creating-transactions">standard Transaction process.</a> If you need to transfer lower values, you may want to reevaluate your application.
</aside>

## Microtransaction Endpoint

### Via Private Keys

### Via Public Keys and Client-side Signing

## Security vs Convenience
