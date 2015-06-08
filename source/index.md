---
title: BlockCypher Reference

language_tabs:
  - shell: cURL

toc_footers:
  - <a href='https://accounts.blockcypher.com/'>Register for a Free Token</a>
  - Docs Powered by <a href="https://github.com/tripit/slate">Slate</a>

includes:
  - objects
  - blockchain
  - address
  - tx
  - microtx
  - conf
  - payfwd
  - events
---

# Introduction

```shell
#  _               _                  
# |_) |  _   _ |  /     ._  |_   _  ._
# |_) | (_) (_ |< \_ \/ |_) | | (/_ | 
#                    /  |             
```

Welcome to [BlockCypher's](http://www.blockcypher.com/) API documentation! BlockCypher is a simple, mostly RESTful JSON API for interacting with blockchains, accessed over HTTP or HTTPS from the [api.blockcypher.com](https://api.blockcypher.com/v1/btc/main) domain. Currently, BlockCypher supports Bitcoin, Bitcoin Testnet3, Litecoin, Dogecoin, Urocoin, and BlockCypher's Test Chain (more about BlockCypher's Test Chain [below](#testing)).

BlockCypher's API provides a superset of the endpoints you'd find in reference implementations, in addition to some special features that make BlockCypher uniquely powerful, like our unconfirmed transaction [Confidence Factor](#confidence-factor), dependable WebHook or WebSockets-based [Events](#events-and-hooks), [On-Chain Microtransactions](#microtransaction-api), and [Payment Forwarding](#payment-forwarding).

Consequently, if you're familiar with a blockchain's reference implementation, you'll feel at home using BlockCypher, but without worrying about scaling or implementation challenges. And if you're not familiar---with the reference implementations or blockchains in general---BlockCypher's API is a great way to dip your toes into blockchain development, without a lengthy setup process. In either case, BlockCypher has 99.99% up-time, and maintains an expressive, logical API that you'll love.

## Documentation Structure

```shell
$ man curl | grep -A 3 "DESCRIPTION"

DESCRIPTION
curl is a tool to transfer data from or to a server, using one of the supported protocols (DICT, FILE, FTP, FTPS, GOPHER, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMB, SMBS, SMTP, SMTPS, TELNET and TFTP). The command is designed to work without user interaction.
```

In these docs you'll find everything you need to leverage BlockCypher for your applications. For all officially supported languages, you'll see code samples, in addition to basic cURL requests/responses for every endpoint. You can switch between cURL/language samples via the selector in the upper right. We're working on supporting more languages, but if you're working on your own language library, definitely let us know: we'd love to add more community supported libraries here.

<aside class="notice">
Currently, we only have cURL code samples, but are working on adding other official SDK samples soon!
</aside>

### In-Browser Code Examples

We know many learn more from code examples than specific reference documentation, which is why we have code samples in our official languages. In addition, we have a number of in-browser code examples for particular use cases, which you can see here:

- [Browse the Blockchain](https://blockcypher.github.io/documentation/samples/browse-chain.html)
- [Using WebHooks](https://blockcypher.github.io/documentation/samples/nodejs-webhook.html)
- [Sending a Simple Transaction](https://blockcypher.github.io/documentation/samples/create-tx.html)
- [Create Multisig Transactions](https://blockcypher.github.io/documentation/samples/multisig-tx.html)

### Changelog and Errors

Our documentation is powered by [GitHub Pages](https://pages.github.com/) and [Slate](https://github.com/tripit/slate), which makes viewing changes as simple as checking the [git commit history.](https://github.com/blockcypher/documentation/commits/master). If there's an error or you'd like to suggest a change, please consider submitting a pull request to benefit the broader BlockCypher community.

## API Versions

All API calls are versioned, and the current BlockCypher API is v1. We will never introduce any breaking changes within v1, but we may add new, non-breaking features from time to time.

## BlockCypher Supported Language SDKs

BlockCypher has client SDKs for the following languages:

- **Ruby** [https://github.com/blockcypher/ruby-client](https://github.com/blockcypher/ruby-client)
- **Python** [https://github.com/blockcypher/blockcypher-python](https://github.com/blockcypher/blockcypher-python)
- **Java** [https://github.com/blockcypher/java-client](https://github.com/blockcypher/java-client)

These client SDKs are under development, but are still officially supported:

- **Node.js** [https://github.com/blockcypher/node-client](https://github.com/blockcypher/node-client)
- **PHP** [https://github.com/blockcypher/php-client](https://github.com/blockcypher/php-client)

If you're using these languages, we strongly encourage you to use an official SDK. Of course, all our API calls are standard HTTP endpoints using JSON formatted responses, so any language (or cURL from the command-line) will work just fine.

### Unofficial Libraries

These client SDKs were made by members of the community, and are not officially supported by BlockCypher. As such, BlockCypher cannot guarantee that they're fully up to date, but we hope they will provide a nice jumping-off point for developers using these languages. We'll endeavor to keep this list updated, if any of these prove obsolete. If there's enough support for a particular language, we'll work with the community to turn it into an officially supported SDK.

- **.NET**: [https://github.com/bscheiman/BlockCypher](https://github.com/bscheiman/BlockCypher)
- **Go**: [https://github.com/acityinohio/blockcy](https://github.com/acityinohio/blockcy)

## RESTful Resources

```shell
$ curl https://api.blockcypher.com/v1/btc/main

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

Almost all resources exist under a given blockchain, and follow this pattern:

`https://api.blockcypher.com/$API_VERSION/$COIN/$CHAIN/`

Currently, there's only one version of the API (v1). Thus, here's an exhaustive list of blockchains and their corresponding resources:

Coin | Chain | Resource
---- | ----- | --------
Bitcoin | Main | `api.blockcypher.com/v1/btc/main`
Bitcoin | Testnet3 | `api.blockcypher.com/v1/btc/test3`
Dogecoin | Main | `api.blockcypher.com/v1/doge/main`
Litecoin | Main | `api.blockcypher.com/v1/ltc/main`
Urocoin | Main | `api.blockcypher.com/v1/uro/main`
BlockCypher | Test | `api.blockcypher.com/v1/bcy/test`

<aside class="notice">
Unless otherwise noted, all descriptions of direct HTTP requests will assume one of these base resources prepends it. But you can always see the full call in the cURL code sample.
</aside>

<aside class="success">
Our API <b>always</b> returns values in satoshis, or the lowest non-divisible unit in non-Bitcoin blockchains. As a friendly reminder, there are 10^8 satoshis in a single bitcoin (100,000,000s = 1BTC), 10^8 base units per litecoin, and 10^8 koinus per dogecoin (100,000,000k = 1DOGE).
</aside>

## Rate Limits and Tokens

We want everyone to try BlockCypher with as little friction as possible, which is why you don't need a token to get started. However, we do rate-limit non-registered users to maintain fidelity for registered users:

- Classic requests, up to 5 requests/sec and 600 requests/hr
- WebHooks and WebSockets, up to 600 requests/hr

<aside class="warning">
If you exceed these limits as a non-registered user, your requests will return an HTTP Status Code 429!
</aside>

Please [register for a user token](http://acccounts.blockcypher.com/) if your usage exceeds those limits, or if you want to preventively avoid the rate limits. Our future pricing plan will be tiered based on usage and volume. We will have a free tier and an extended free plan for our early users. To request higher limits or SLAs, please [email us.](mailto:contact@blockcypher.com)

```shell
# Adding your token as URL parameter
$ curl https://api.blockcypher.com/v1/btc/main?token=$YOUR_TOKEN
```

Once you have your token, you can append it to all your requests like any other URL parameter if you're using cURL, or through the appropriate method in the language SDK you're using.

## Batching

```shell
# Batching blocks 5, 6, and 7
$ curl https://api.blockcypher.com/v1/btc/main/blocks/5;6;7

[{
"hash": "000000003031a0e73735690c5a1ff2a4be82553b2a12b776fbd3a215dc8f778d",
"height": 6,
"chain": "BTC.main",
"total": 0,
"fees": 0,
"ver": 1,
"time": "2009-01-09T03:29:49Z",
...,
},
{
"hash": "000000009b7262315dbf071787ad3656097b892abffd1f95a1a022f896f533fc",
"height": 5,
"chain": "BTC.main",
"total": 0,
"fees": 0,
"ver": 1,
"time": "2009-01-09T03:23:48Z",
...,
},
{
"hash": "0000000071966c2b1d065fd446b1e485b2c9d9594acd2007ccbd5441cfc89444",
"height": 7,
"chain": "BTC.main",
"total": 0,
"fees": 0,
"ver": 1,
"time": "2009-01-09T03:39:29Z",
...,
}]
```

All endpoints that can retrieve a single [Object](#objects) can be batched to return multiple objects. If you're cURLing the API directly, batching simply requires appending each identifier to the previous one using a semicolon (check the code pane for an example). The results are aggregated in a JSON array and may not be ordered, especially for bigger batches. But this shouldn't matter, as the requested identifiers are always present in the returned objects. The other supported client SDKs batch differently, but each idiomatic to their respective language (check the code pane examples in each library).

<aside class="notice">
The maximum number of elements that can be batched in a single call is 100.
</aside>

When cURLing BlockCypher, batching also works when the identifiers aren't the last part of the URL; e.g., this URL will return the balances of three separate addresses:

`http://api.blockcypher.com/v1/btc/main/addrs/1J38WorKngZLJvA7qMin9g5jqUfTQUBZNE;1JP8FqoXtCMrR1sZc2McLWmHxENox1Y1PV;1ENn7XmqXNnReiQEFHhBGzfiv5gAyBj7r1/balance`

<aside class="warning">
Since the default, non-registered <a href="#rate-limits-and-tokens">rate limit</a> per second is 5, larger batches require an API token. To use larger batches <a href="https://accounts.blockcypher.com/">please register.</a>
</aside>

## Testing

We offer two different options for testing your blockchain application: Bitcoin Testnet3, and BlockCypher's Test Chain. We offer automated faucets for both Testnet3 and BlockCypher's Test Chain, but we recommend using BlockCypher's Test Chain for a variety of reasons:

- It's nearly identical in characteristics to Bitcoin Main, with a few differences listed below.
- The chain is private (no data is broadcasted, only BlockCypher mines the transactions), making it much more predictable than the Bitcoin's testnet (which is frequently under attack).
- New blocks get built every minute, confirming the transactions that have been created using our transaction API.
- The prefix for standard addreses is 'B' or 'C' (0x1B). The prefix for multisig addresses is 'D' (0x1F).

In case you missed the [Resources section](#restful-resources), the BlockCypher Test Chain is accessible from this resource:

`https://api.blockcypher.com/v1/bcy/test`

### Test Faucets

To help facilitate automated testing in your applications, a faucet endpoint is available on both BlockCypher's Test Chain and Bitcoin Testnet3. Calling the faucet endpoint, along with passing a valid address, will automatically create---and propagate---a new transaction funding the address with the amount you provide.

The faucets can be used from your browser if you want to play with them before automating:

- [Bitcoin Testnet Faucet](https://accounts.blockcypher.com/testnet-faucet)
- [BlockCypher Test Chain Faucet](https://accounts.blockcypher.com/blockcypher-faucet)

```shell
# Make new address; returns private key/public key/address
$ curl -X POST http://api.blockcypher.com/v1/bcy/test/addrs?token=$YOUR_TOKEN

{
"private": "26415016a2fb49f51aef161cb35bd537be07b75a6ac1e297d3b7a370cc85433b",
"public": "02c572d062fefcc8c3e1bf5016450addcedb89cd7e4507d8a323f327b4ad1018e0",
"address": "CFqoZmZ3ePwK5wnkhxJjJAQKJ82C7RJdmd"
}

# Fund prior address with faucet
$ curl -d '{"address": "CFqoZmZ3ePwK5wnkhxJjJAQKJ82C7RJdmd", "amount": 100000}' http://api.blockcypher.com/v1/bcy/test/faucet?token=$YOUR_TOKEN
{
"tx_ref": "02dbf5585d438a1cba82a9041dd815635a6b0df684225cb5271e11397a759479"
}
```

This example shows how to leverage the faucet to programmatically fund addresses, to test your applications. While the example used BlockCypher's Test Chain, the same example could have used Bitcoin Testnet3 and worked the exact same way.

<aside class="notice">
You need <a href="https://accounts.blockcypher.com/">a token</a> to use test faucets.
</aside>

<aside class="warning">
On the BlockCypher Test Chain, the faucet will refuse to fund an address with more than 50 billion BlockCypher satoshis and will not fund more than 10 million BlockCypher satoshis at a time. On Bitcoin Testnet3 those numbers are adjusted to 10 million and 500,000 testnet satoshis respectively.
</aside>
