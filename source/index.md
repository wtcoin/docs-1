---
title: BlockCypher Reference 

language_tabs:
  - shell: cURL
  - ruby: Ruby

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
---

# Introduction

```shell
#  ______ _            _    _____             _               
#  | ___ \ |          | |  /  __ \           | |              
#  | |_/ / | ___   ___| | _| /  \/_   _ _ __ | |__   ___ _ __ 
#  | ___ \ |/ _ \ / __| |/ / |   | | | | '_ \| '_ \ / _ \ '__|
#  | |_/ / | (_) | (__|   <| \__/\ |_| | |_) | | | |  __/ |   
#  \____/|_|\___/ \___|_|\_\\____/\__, | .__/|_| |_|\___|_|   
#                                  __/ | |                    
#                                 |___/|_|                    
```

```ruby
puts "BlockCypher!"
```

Welcome to [BlockCypher's](http://www.blockcypher.com/) API documentation! BlockCypher is a simple, mostly RESTful JSON API for interacting with blockchains, accessed over HTTP or HTTPS from the [api.blockcypher.com](https://api.blockcypher.com/v1/btc/main) domain. Currently, BlockCypher supports Bitcoin, Bitcoin Testnet3, Litecoin, Dogecoin, Urocoin, and BlockCypher's Test Chain (more about BlockCypher's Test Chain [below](#testing)).

BlockCypher's API provides a superset of the endpoints you'd find in reference implementations, in addition to some special features that make BlockCypher uniquely powerful, like dependable WebHook or WebSockets-based [Events](#events), [On-Chain Microtransactions](#microtransactions), and [Payment Forwarding](#payment-forwarding).

Consequently, if you're familiar with a blockchain's reference implementation, you'll feel right at home using BlockCypher, but without worrying about scaling or implementation challenges. And if you're not familiar---with the reference implementations or blockchains in general---BlockCypher's API is a great way to dip your toes into blockchain development, without a lengthy setup process. In either case, BlockCypher has 99.99% up-time, and maintains an expressive, logical API that you'll love using.

## Documentation Structure

> You'll see code in this section, like this:

```shell
$ man curl | grep -A 3 "DESCRIPTION"
```
> Alongside expected responses:

```shell
DESCRIPTION
curl is a tool to transfer data from or to a server, using one of the supported protocols (DICT, FILE, FTP, FTPS, GOPHER, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMB, SMBS, SMTP, SMTPS, TELNET and TFTP). The command is designed to work without user interaction.
```

In these docs you'll find everything you need to leverage BlockCypher for your applications. For all officially supported languages, you'll see code samples, in addition to basic cURL requests/responses for every single endpoint. You can switch between cURL/language samples via the selector in the upper right. We're working on supporting more languages, but if you're working on your own language library, definitely let us know: we'd love to add more community supported libraries here.

<aside class="notice">
Currently, we only have cURL code samples, but are working on adding other official SDK samples soon!
</aside>

## API Versions

All API calls are versioned, and the current BlockCypher API is v1. We will never introduce any breaking changes within v1, but we may add new, non-breaking features from time to time.

## Official Libraries

BlockCypher has client SDKs for the following languages:

- **Ruby** --- [https://github.com/blockcypher/blockcypher-python](https://github.com/blockcypher/blockcypher-python)
- **Python** --- [https://github.com/blockcypher/ruby-client](https://github.com/blockcypher/ruby-client)
- **Java** --- [https://github.com/blockcypher/java-client](https://github.com/blockcypher/java-client)
- **Node.js** (under development, code samples coming soon) --- [https://github.com/blockcypher/node-client](https://github.com/blockcypher/node-client)

If you're using these languages, we strongly encourage you to use the official SDK. If not, or you'd rather write your own bindings, we always support cURLing endpoints directly, which is the default code sample you'll see to the right.

## Unofficial Libraries

These client SDKs were made by members of the community, and are not officially supported by BlockCypher. As such, BlockCypher cannot guarantee that they're fully up to date, but we hope they will provide a nice jumping-off point for developers using these languages. We'll endeavor to keep this list updated, if any of these prove obsolete. If there's enough support for a particular language, we'll work with the community to turn it into an officially supported SDK.

- **.NET** --- [https://github.com/bscheiman/BlockCypher](https://github.com/bscheiman/BlockCypher)
- **Go** --- [https://github.com/acityinohio/blockcy](https://github.com/acityinohio/blockcy)

## RESTful Resources: Coins & Chains

```shell
$ curl https://api.blockcypher.com/v1/btc/main

{
  "name": "BTC.main",
  "height": 355578,
  "hash": "00000000000000000a0b253f20709b0c77d8a56aa8db632ecbdc7381816504cd",
  "time": "2015-05-08T23:12:55.243311146Z",
  "latest_url": "https://api.blockcypher.com/v1/btc/main/blocks/00000000000000000a0b253f20709b0c77d8a56aa8db632ecbdc7381816504cd",
  "previous_hash": "00000000000000000acef50ef89494493b4a08a8419588e1e3e20cd73bc85a6b",
  "previous_url": "https://api.blockcypher.com/v1/btc/main/blocks/00000000000000000acef50ef89494493b4a08a8419588e1e3e20cd73bc85a6b",
  "peer_count": 250,
  "unconfirmed_count": 637
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

## Rate Limits and Tokens

## Batching

## Testing 

We offer two different options for testing your blockchain application: Bitcoin Testnet3, and BlockCypher's Test Chain. We offer automated faucets for both Testnet3 and BlockCypher's Test Chain, but we recommend using BlockCypher's Test Chain for a variety of reasons:

- It's nearly identical in characteristics to Bitcoin Main, with a few differences listed below.
- The chain is private (no data is broadcasted, only BlockCypher mines the transactions), making it much more predictable than the Bitcoin's testnet (which is frequently under attack).
- New blocks get built every minute, confirming the transactions that have been created using our transaction API.
- The prefix for standard addreses is 'B' or 'C' (0x1B). The prefix for multisig addresses is 'D' (0x1F).

### Test Faucets
