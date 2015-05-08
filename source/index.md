---
title: API Reference

language_tabs:
  - shell: cURL
  - ruby: Ruby
  - python: Python

toc_footers:
  - <a href='https://accounts.blockcypher.com/'>Register for a Free Token</a>
  - Docs Powered by <a href="https://github.com/tripit/slate">Slate</a>

includes:
  - objects
  - blockchain
  - address
  - tx
  - events
  - payfwd

search: false
---

# Introduction

Welcome to [BlockCypher's](http://www.blockcypher.com/) API documentation! BlockCypher is a simple, mostly RESTful JSON API for interacting with blockchains, accessed over HTTP or HTTPS from the [api.blockcypher.com](https://api.blockcypher.com/v1/btc/main) domain. Currently, BlockCypher supports Bitcoin, Bitcoin TestNet3, Litecoin, Dogecoin, Urocoin, and BlockCypher's Test Chain (more about BlockCypher's Test Chain [below](#testing)).

BlockCypher's API provides a superset of the endpoints you'd find in reference implementations, in addition to some special features that make BlockCypher unique and powerful, such as dependable WebHook or WebSockets-based [Events](#events), [On-Chain Microtransactions](#microtransactions), and [Payment Forwarding](#payment-forwarding).

Consequently, if you're familiar with a blockchain's reference implementation, you'll feel right at home using BlockCypher, but without worrying about scaling or implementation challenges. And if you're not familiar---with the reference implementations or blockchains in general---BlockCypher's API is a great way to dip your toes into blockchain development, without a lengthy setup process. In either case, BlockCypher has 99.99% up-time, and maintains an expressive, logical API that you'll love using.

## Documentation Structure

In these docs you'll find everything you need to leverage BlockCypher for your applications. For all officially supported languages, you'll see code samples on the right pane (or below the description, if you're on mobile), in addition to basic cURL requests/responses, for every single endpoint. You can switch between language context via the selector in the upper right. We're working on supporting more languages, but if you're working on your own language library, definitely let us know: we'd love to add more community supported libraries here.

## API Versioning

All API calls are versioned, and the current BlockCypher API is v1. We will never introduce any breaking changes within v1, but we may add new, non-breaking features from time to time.

## Official Libraries

BlockCypher has client SDKs for the following languages:

- Ruby --- [https://github.com/blockcypher/blockcypher-python](https://github.com/blockcypher/blockcypher-python)
- Python --- [https://github.com/blockcypher/ruby-client](https://github.com/blockcypher/ruby-client)
- Java --- [https://github.com/blockcypher/java-client](https://github.com/blockcypher/java-client)
- Node.js (under development, code samples coming soon) --- [https://github.com/blockcypher/node-client](https://github.com/blockcypher/node-client)

If you're using these languages, we strongly encourage you to use the official SDK. If not, or you'd rather write your own bindings, we always support cURLing endpoints directly, which is the default code sample you'll see to the right.

## Unofficial Libraries

These client SDKs were made by members of the community, and are not officially supported by BlockCypher. As such, BlockCypher cannot guarantee that they're fully up to date, but we hope they will provide a nice jumping-off point for developers using these languages. We'll endeavor to keep this list updated, if any of these prove obsolete. If there's enough support for a particular language, we'll work with the community to turn it into an officially supported SDK.

- .NET --- [https://github.com/bscheiman/BlockCypher](https://github.com/bscheiman/BlockCypher)
- Go --- [https://github.com/acityinohio/blockcy](https://github.com/acityinohio/blockcy)

## Importing Libraries

## RESTful Resources: Coins & Chains

## Rate Limits and Tokens

## Testing 

We offer two different options for testing your blockchain application: Bitcoin TestNet3, and BlockCypher's Test Chain. We offer automated faucets for both Testnet3 and BlockCypher's Test Chain, but we recommend using BlockCypher's Test Chain for a variety of reasons:

- It's nearly identical in characteristics to Bitcoin Main, with a few differences listed below.
- The chain is private (no data is broadcasted, only BlockCypher mines the transactions), making it much more predictable than the Bitcoin's testnet (which is frequently under attack).
- New blocks get built every minute, confirming the transactions that have been created using our transaction API.
- The prefix for standard addreses is 'B' or 'C' (0x1B). The prefix for multisig addresses is 'D' (0x1F).

### Test Faucets
