# Events and Hooks

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

<aside class="notice">
Events like <i>unconfirmed-tx</i> can produce a lot of requests. To avoid rate-limiting, please <a href="http://accounts.blockcypher.com/">register for a token.</a>
</aside>

## Using WebSockets

```shell
# no websockets via cURL :-( but here's a Javascript example
# Get latest unconfirmed transactions live
var ws = new WebSocket("wss://socket.blockcypher.com/v1/btc/main");
var count = 0;
ws.onmessage = function (event) {
  var tx = JSON.parse(event.data);
  var shortHash = tx.hash.substring(0, 6) + "...";
  var total = tx.total / 100000000;
  var addrs = tx.addresses.join(", ");
  $('#browser-websocket').before("<div>Unconfirmed transaction " + shortHash + " totalling " + total + "BTC involving addresses " + addrs + "</div>");
  count++;
  if (count > 10) ws.close();
}
ws.onopen = function(event) {
  ws.send(JSON.stringify({filter: "event=unconfirmed-tx"}));
}
```

Opening a WebSocket to listen to our feeds is easy, like so in Javascript:

`new WebSocket("wss://socket.blockcypher.com/v1/btc/main");`

The code may differ if you're not programming in Javascript (check relevant code examples for our standard libraries) but the URL will be identical. Once the socket is opened, the JSON document representing the [Event](#event) of interest should be sent.

In addition to standard events, WebSockets accept a "ping" event. If you send the following, you will receive the same message back with "ping" replaced by "pong":

`{ "event": "ping" }`

A regular ping (i.e. every 20 seconds) allows the WebSocket to stay connected for a longer period of time.

### A WebSockets Live Example

Using the example above, we can demonstrate a client-side WebSocket event stream directly from your browser. In that example, we will subscribe to all pooled transactions (new, unconfirmed transactions) by opening a new WebSocket and sending the filter (**unconfirmed-tx**). Upon notification of new transactions, we format them and add them into the page.

<button class="tryme" type="button" id="browser-websocket">Try me!</button>

## Using WebHooks

WebHooks leverage similar objects and interactions but with two key differences:

- The JSON [Event](#event) should be sent using a POST request to the "create webhook endpoint" and include a **url** property to denote where payloads should be delivered.
- The [Transaction](#transaction) or [Block](#block) associated with the [Event](#event) will be POSTed to the provided **url**. The POSTed payload will also include *X-EventType* and *X-EventId* metadata in the HTTP header specifying the **event** type and **id** of the WebHook which triggered the payload.

<aside class="warning">
To prevent eavesdropping, we recommend securing your callback <b>url</b> by using SSL and providing a <i>secret</i> parameter appended to the <a href="#event">Event</a> request. We POST the payload to the unaltered <b>url</b>, which allows you to check on your server that the parameter was not modified.
</aside>

<aside class="notice">
Testing WebHooks can be tricky; we recommend using <a href="http://requestb.in/">reqeustb.in</a> to set up a temporary server to test your events.
</aside>

### Create WebHook Endpoint

```shell
$ curl -d {"event": "unconfirmed-tx", "address": "15qx9ug952GWGTNn7Uiv6vode4RcGrRemh", "token": "YOURTOKEN" "url": "https://my.domain.com/callbacks/new-tx"} https://api.blockcypher.com/v1/btc/main/hooks

{
"id": "399d0923-e920-48ee-8928-2051cbfbc369"
"event": "unconfirmed-tx",
"address": "15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
"token": "YOURTOKEN"
"url": "https://my.domain.com/callbacks/new-tx"
}
```

```python
>>> import requests, json
>>> data = {'event': 'unconfirmed-tx', 'address': '15qx9ug952GWGTNn7Uiv6vode4RcGrRemh', 'url': 'https://my.domain.com/callbacks/new-tx'}
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.post('https://api.blockcypher.com/v1/btc/main/hooks', data=json.dumps(data), params=params)
>>> r.json()
{'filter': 'addr=15qx9ug952GWGTNn7Uiv6vode4RcGrRemh&event=unconfirmed-tx',
 'id': '50d1fb13-2bd4-47d0-8e1b-0695e0322581',
 'token': 'YOUR_TOKEN',
 'address': '15qx9ug952GWGTNn7Uiv6vode4RcGrRemh',
 'url': 'https://my.domain.com/callbacks/new-tx',
 'event': 'unconfirmed-tx',
 'callback_errors': 0}
```

Using a partially filled out [Event](#event), you can create a WebHook using this resource. Check the [Event object description](#event) and [types of events](#types-of-events) to understand the options available for your events.

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/hooks | POST | [Event](#event) | [Event](#event)

If successful, it will return the [Event](#event) with a newly generated **id**.

### List WebHooks Endpoint

```shell
$ curl https://api.blockcypher.com/v1/btc/main/hooks?token=YOURTOKEN

[
	{
	"id": "399d0923-e920-48ee-8928-2051cbfbc369"
	"event": "unconfirmed-tx",
	"address": "15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
	"token": "YOURTOKEN"
	"url": "https://my.domain.com/callbacks/new-tx"
	}
]
```

```python
>>> import requests
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.get('https://api.blockcypher.com/v1/btc/main/hooks', params=params)
>>> r.json()
[{'filter': 'addr=15qx9ug952GWGTNn7Uiv6vode4RcGrRemh&event=unconfirmed-tx',
  'id': '50d1fb13-2bd4-47d0-8e1b-0695e0322581',
  'token': 'YOUR_TOKEN',
  'address': '15qx9ug952GWGTNn7Uiv6vode4RcGrRemh',
  'url': 'https://my.domain.com/callbacks/new-tx',
  'event': 'unconfirmed-tx',
  'callback_errors': 0}]
```

This resource lists your currently active events, according the base resource and $YOURTOKEN.

Resource | Method | Return Object
-------- | ------ | -------------
/hooks?token=$YOURTOKEN | GET | Array[[Event](#event)]

### WebHook ID Endpoint

```shell
$ curl https://api.blockcypher.com/v1/btc/main/hooks/399d0923-e920-48ee-8928-2051cbfbc369

{
"id": "399d0923-e920-48ee-8928-2051cbfbc369"
"event": "unconfirmed-tx",
"address": "15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
"token": "YOURTOKEN"
"url": "https://my.domain.com/callbacks/new-tx"
}
```

```python
# Fund existing address with faucet
>>> import requests
>>> r = requests.get('https://api.blockcypher.com/v1/btc/main/hooks/50d1fb13-2bd4-47d0-8e1b-0695e0322581')
>>> r.json()
{'filter': 'addr=15qx9ug952GWGTNn7Uiv6vode4RcGrRemh&event=unconfirmed-tx',
 'id': '50d1fb13-2bd4-47d0-8e1b-0695e0322581',
 'token': 'YOUR_TOKEN',
 'address': '15qx9ug952GWGTNn7Uiv6vode4RcGrRemh',
 'url': 'https://my.domain.com/callbacks/new-tx',
 'event': 'unconfirmed-tx',
 'callback_errors': 0}
```

This resource returns an [Event](#event) based on its generated *id*.

Resource | Method | Return Object
-------- | ------ | -------------
/hooks/$WEBHOOKID | GET | [Event](#event)

$WEBHOOKID is a string representing the event's generated *id*, for example:

`399d0923-e920-48ee-8928-2051cbfbc369`

### Delete WebHook Endpoint

```shell
# Piping into grep to get status code
$ curl -X DELETE -IsL https://api.blockcypher.com/v1/btc/main/hooks/399d0923-e920-48ee-8928-2051cbfbc369 | grep "HTTP/1.1"

HTTP/1.1 200 OK
```

```python
# Fund existing address with faucet
>>> import requests
>>> r = requests.delete('https://api.blockcypher.com/v1/btc/main/hooks/50d1fb13-2bd4-47d0-8e1b-0695e0322581')
# Will return nothing, but we can confirm the status code to be sure
>>> assert r.status_code == 204
```

This resource deletes an active [Event](#event) based on its *id*.

Resource | Method | Return Object
-------- | ------ | -------------
/hooks/$WEBHOOKID | DELETE | *nil*

$WEBHOOKID is a string representing the event's generated *id*, for example:

`399d0923-e920-48ee-8928-2051cbfbc369`

If successful, it won't return any objects, but will respond with an HTTP Status Code 200.
