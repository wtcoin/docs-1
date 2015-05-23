# Events & Hooks

Blockchains are highly transactional systems. Many usage patterns require knowing when an event occurs: i.e., when a transaction is included into a block, or when an unconfirmed transaction is relayed through the network. Instead of requiring you to continuously poll resources, we provide push APIs to facilitate those use cases, and support both WebSockets and WebHooks.

### Which Should I Use?

**[WebSockets](https://developer.mozilla.org/en-US/docs/WebSockets)** are typically used in client applications when a server is not already running: e.g., a web page displaying the most recent transactions or a wallet app updating its UI when a transaction has been confirmed. Websockets are less reliable in longer interactions (> 1 hour) because they require a connection to stay open.

**[WebHooks](https://webhooks.pbworks.com/w/page/13385124/FrontPage)** are the most reliable way to get event notifications but requires running a server to receive the callbacks. We automatically retry HTTP requests 3 times.

## Types of Events

We support a number of different event types, and you can filter your notification requests depending on how you structure your [Event](#event) request object. 

Event | Description
----- | -----------
*unconfirmed-tx* | Triggered for every new transaction BlockCypher receives before it's confirmed in a block; basically, for every unconfirmed transaction. The payload is an unconfirmed [Transaction](#transaction).
*new-block* | Triggered for every new block added to the blockchain you've selected as your base resource. The payload is a [Block](#block).
*confirmed-tx* | Triggered for every new transaction making it into a new block; in other words, for every first transaction confirmation. This is equivalent to listening to the *new-block* event and fetching each transaction in the new [Block](#block). The payload is a confirmed [Transaction](#transaction).
*tx-confirmation* | Simplifies listening to confirmations on all transactions for a given address up to a provided threshold. Sends first the unconfirmed transaction and then the transaction for each confirmation. Use the **confirmations** property within the [Event](#event) to manually specify the number of confirmations desired (maximum 10, defaults to 6). The payload is a [Transaction](#transaction).
*double-spend-tx* | Triggered any time a double spend is detected by BlockCypher. The payload is the [Transaction](#transaction) that triggered the event; the hash of the transaction that it's trying to double spend is included in its **double_spend_tx** property.

## Using WebSockets

## Using WebHooks

### Create WebHook Endpoint

### List WebHooks Endpoint

### WebHook ID Endpoint

### Delete WebHook Endpoint
