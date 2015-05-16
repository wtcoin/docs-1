# Address API

BlockCypher's Address API allows you to look up information about public addresses on the blockchain, generate single-use, low-value key pairs with corresponding addresses, help generate multisig addresses, and collect multiple addresses into a single shortcut for address viewing, all [based on the coin/chain resource](#restful-resources:-coins-&-chains) you've selected for your endpoints.

If you're new to blockchains, you can think of public addresses as similar to bank account numbers in a traditional ledger. The biggest differences:

- Anyone can generate a public address themselves (through [ECDSA](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm) in Bitcoin). No single authority is needed to generate new addresses; it's just public-private key cryptography.
- Public addresses are significantly more lightweight. Consequently, and unlike traditional bank accounts, you can (and should!) generate new addresses for every transaction.
- Addresses can also leverage [*pay-to-script-hash*](https://en.bitcoin.it/wiki/Pay_to_script_hash), which means they can represent exotic things beyond a single private-public key pair; the most prominent example being multi-signature addresses that require n-of-m signatures to spend.

## Address Balance Endpoint

## Address Endpoint

## Address Detail Endpoint

## Generate Address Endpoint

## Generate Multisig Address Endpoint

## Wallets
