# Transactions API

BlockCypher's Transactions API allows you to look up information about unconfirmed transactions, query transactions based on hash, and create and propagate your own transactions, including multisignature transactions---all [based on the coin/chain resource](#restful-resources:-coins-&-chains) you've selected for your endpoints.

If you're new to blockchains, the idea of transactions is relatively self-explanatory. Here's what's going on underneath the hood: a transaction takes previous "unspent transaction outputs" (also known as [UTXOs](https://bitcoin.org/en/glossary/unspent-transaction-output)) as "transaction inputs" and creates new "locking scripts" on those inputs such that they are "sent" to new addresses (to become new UTXOs). While most of these public addresses are reference points for single private keys that can "unlock" the newly created UTXOs, occasionally they are sent to more exotic addresses through *pay-to-script-hash*, typically multisignature addresses.

Generally speaking, UTXOs are generated from previous transactions (except for [Coinbase inputs](https://bitcoin.org/en/glossary/coinbase)).

<aside class="success">
Even if you don't use the API to assist in creating transactions, we highly recommend pushing your raw transactions through our API. BlockCypher maintains connections to 20% of all nodes on all blockchains it supports, which means we can propagate transactions faster than almost anyone else. Given the speed and reliability with which we can propagate transactions, this is especially powerful when used in concert with our <a href="#zero-confirmation-confidence">Zero Confirmation Confidence factor.</a>
</aside>

## Unconfirmed Transactions Endpoint

## Transaction Hash Endpoint

## Creating Transactions

### New Transaction Endpoint

### Send Transaction Endpoint

## Push Raw Transaction Endpoint

## Decode Raw Transaction Endpoint

## Multisig Transactions
