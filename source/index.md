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

BlockCypher's API provides a superset of the endpoints you'd find in the above blockchain's reference implementations, in addition to some special features that make our BlockCypher unique and powerful, such as dependable WebHook or WebSockets-based [Events](#events), [On-Chain Microtransactions](#microtransactions), and [Payment Forwarding](#payment-forwarding).

Consequently, if you're familiar with a blockchain's reference implementation, you'll feel right at home using BlockCypher, but without worrying about scaling or implementation challenges. And if you're not familiar---with the reference implementations or blockchains in general---BlockCypher's API is a great way to dip your toes into blockchain development, without a lengthy setup process. In either case, BlockCypher has a 99.99% up-time, and an expressive, logical API that you'll love using.

## Official Libraries

- cURL
- Ruby
- Python
- Java
- Node.js (under development)

## Unofficial Libraries

- .NET (unofficial)

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
