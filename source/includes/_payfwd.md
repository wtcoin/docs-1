# Payment Forwarding

One of the well-known benefits of cryptocurrency is the ability to allow users to partake in online commerce without necessarily requiring extensive setup barriers, like registering new accounts. In that spirit, our Payment Forwarding API is the easiest way to accept---and consolidate---payments securely without forcing your users to create accounts and jump through unnecessary loops. It's also a generic way to automatically transfer value from one address to another. While there are many possible use cases, the two we hear most about are:

- A way to generate payment-specific addresses for which funds will automatically transfer to a main merchant address. Great for automatic merchandise (whether physical or virtual) processing.
- A method to easily fund a multisignature address from any wallet by providing a classic address that will automatically transfer to the multisignature/*pay-to-script-hash* address.

We do not take a fee on payment forwarding, other than the required 10,000 satoshi miner fee; payments are free. However, as part of your own services, you can include a fee (either fixed or a percentage) that will also be automatically transfered to your own address in the same transaction. Fee-based business models are thus easily achieved, and moreover, easily auditable via the blockchain.

<aside class="notice">
Payment forwarding is tied to your usage token, you can't create a payment forwarding request without one. You can <a href="http://account.blockcypher.com">register for a token here.</a>
</aside>

<aside class="warning">
By default, all payments will be debited with a 10,000 satoshis mining fee. The amount of the fee is configurable. But for very small payments, if the amount sent is even lower than the mining and user-set processing fees, the forward will fail.
</aside>

## Create Payment Endpoint

```shell
curl -d '{"destination":"15qx9ug952GWGTNn7Uiv6vode4RcGrRemh","callback_url": "https://my.domain.com/callbacks/new-pay","token":"YOURTOKEN"}' http://api.blockcypher.com/v1/btc/main/payments

{
"input_address": "16uKw7GsQSzfMaVTcT7tpFQkd7Rh9qcXWX",
"destination": "15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
"callback_url": "https://my.domain.com/callbacks/payments",
"id": "399d0923-e920-48ee-8928-2051cbfbc369",
"token": "YOURTOKEN"
}
```

```javascript
var payment = {
  "destination":"15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
  "callback_url": "https://my.domain.com/callbacks/new-pay"
}
var url = 'https://api.blockcypher.com/v1/btc/main/payments?token='+TOKEN;
$.post(url, JSON.stringify(payment))
  .then(function(d) {console.log(d)});
{
"input_address": "16uKw7GsQSzfMaVTcT7tpFQkd7Rh9qcXWX",
"destination": "15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
"callback_url": "https://my.domain.com/callbacks/payments",
"id": "399d0923-e920-48ee-8928-2051cbfbc369",
"token": "YOURTOKEN"
}
```

```ruby
> block_cypher.create_forwarding_address("15qx9ug952GWGTNn7Uiv6vode4RcGrRemh", callback_url: "https://my.domain.com/callbacks/new-pay")
=> {"id"=>"72811fff-7826-4b04-8f99-4f492e84aeaa",
 "token"=>"YOURTOKEN",
 "destination"=>"15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
 "input_address"=>"1Php2ZGrEtm5gUaEZNmTtUabmBei6zj9nP",
 "callback_url"=>"https://my.domain.com/callbacks/new-pay"}
```

```python
>>> import requests, json
>>> data = {"destination":"15qx9ug952GWGTNn7Uiv6vode4RcGrRemh","callback_url": "https://my.domain.com/callbacks/new-pay"}
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.post('http://api.blockcypher.com/v1/btc/main/payments', data=json.dumps(data), params=params)
>>> r.json()
{'destination': '15qx9ug952GWGTNn7Uiv6vode4RcGrRemh',
 'callback_url': 'https://my.domain.com/callbacks/new-pay',
 'input_address': '1CUYiFY3LzEd9dXgR6ubRaYPTq2SMxFFCJ',
 'id': 'f35c80c2-3347-410d-b4ac-d049910289ec',
 'token': 'YOUR_TOKEN'}
```

```php
<?php
// Run on console:
// php -f .\sample\payment-api\CreatePaymentEndpoint.php

$paymentForward = new PaymentForward();
$paymentForward->setDestination('15qx9ug952GWGTNn7Uiv6vode4RcGrRemh');
$paymentForward->setCallbackUrl("http://requestb.in/rwp6jirw?uniqid=" . uniqid());

$paymentForward->create($apiContext);

{
  "destination":"15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
  "callback_url":"http://requestb.in/rwp6jirw?uniqid=5583de7d87288",
  "id":"ec2b4b4f-eeb2-4824-b528-7d78a6f52492",
  "token":"c0afcccdde5081d6429de37d16166ead",
  "input_address":"17h2S1KtX7AqS9DJexqqSTNFCgwCoqqxhU"
}
```

First, to create an payment forwarding address, you need to POST a partially filled [PaymentForward](#paymentforward) object to the payment creation endpoint. You need to include at least a **destination** address and your **token**; optionally, you can add a **callback_url**, processing fees (either percent or fixed) and a **process_fee_address**, and a few other options. You can see more details about these options in the [PaymentForward](#paymentforward) object details.

Resource | Method | Request Object | Return Object
-------- | ------ | -------------- | -------------
/payments | POST | [PaymentForward](#paymentforward) | [PaymentForward](#paymentforward)

In return, you'll get a more complete [PaymentForward](#PaymentForward) object, including an **input_address** and **id**.

<aside class="notice">
If you decide to have a <b>callback_url</b>, you'll receive a payload at that url whenever a payment is made to the <b>input_address</b>. The payload will come in the form of a <a href="#paymentforwardcallback">PaymentForwardCallback</a> object.
</aside>

## List Payments Endpoint

```shell
curl http://api.blockcypher.com/v1/btc/main/payments?token=YOURTOKEN

[
	{
	"input_address": "16uKw7GsQSzfMaVTcT7tpFQkd7Rh9qcXWX",
	"destination": "15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
	"callback_url": "https://my.domain.com/callbacks/payments",
	"id": "399d0923-e920-48ee-8928-2051cbfbc369",
	"token": "YOURTOKEN"
	}
]
```

```javascript
$.get('http://api.blockcypher.com/v1/btc/main/payments?token='+TOKEN)
  .then(function(d) {console.log(d)});
[
	{
	"input_address": "16uKw7GsQSzfMaVTcT7tpFQkd7Rh9qcXWX",
	"destination": "15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
	"callback_url": "https://my.domain.com/callbacks/payments",
	"id": "399d0923-e920-48ee-8928-2051cbfbc369",
	"token": "YOURTOKEN"
	}
]
```

```ruby
> block_cypher.list_forwarding_addresses
=> [{"id"=>"72811fff-7826-4b04-8f99-4f492e84aeaa",
  "token"=>"YOURTOKEN",
  "destination"=>"15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
  "input_address"=>"1Php2ZGrEtm5gUaEZNmTtUabmBei6zj9nP",
  "callback_url"=>"https://my.domain.com/callbacks/new-pay"}]
```

```python
>>> import requests
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.post('http://api.blockcypher.com/v1/btc/main/payments', params=params)
>>> r.json()
[{'destination': '15qx9ug952GWGTNn7Uiv6vode4RcGrRemh',
  'callback_url': 'https://my.domain.com/callbacks/payments',
  'input_address': '1CUYiFY3LzEd9dXgR6ubRaYPTq2SMxFFCJ',
  'id': 'f35c80c2-3347-410d-b4ac-d049910289ec',
  'token': 'YOUR_TOKEN'}]
```

```php
<?php
// Run on console:
// php -f .\sample\payment-api\ListPaymentsEndpoint.php

$paymentForwardArray = PaymentForward::getAll(array(), $apiContext);

[
  {
    "id":"ec2b4b4f-eeb2-4824-b528-7d78a6f52492",
    "token":"c0afcccdde5081d6429de37d16166ead",
    "destination":"15qx9ug952GWGTNn7Uiv6vode4RcGrRemh",
    "input_address":"17h2S1KtX7AqS9DJexqqSTNFCgwCoqqxhU",
    "callback_url":"http://requestb.in/rwp6jirw?uniqid=5583de7d87288"
  }
]
```

To list your currently active payment forwarding addresses, you can use this endpoint.

Resource | Method | Return Object
-------- | ------ | -------------
/payments | GET | Array[[PaymentForward](#paymentforward)]

You'll get a full array of your currently active payment forwarding addresses, based on your token.

## Delete Payment Endpoint

```shell
# Piping to grep to just show status code
curl -X DELETE -IsL http://api.blockcypher.com/v1/btc/main/payments/399d0923-e920-48ee-8928-2051cbfbc369?token=YOURTOKEN | grep "HTTP/1.1"

HTTP/1.1 204 No Content
```

```javascript
var url = "https://api.blockcypher.com/v1/btc/main/payments/399d0923-e920-48ee-8928-2051cbfbc369?token="+TOKEN
$.ajax({
  url: url,
  method: "DELETE"
});
```

```ruby
> block_cypher.delete_forwarding_address("72811fff-7826-4b04-8f99-4f492e84aeaa")
=> nil
```

```python
>>> import requests
>>> params = {'token': 'YOUR_TOKEN'}
>>> r = requests.delete('http://api.blockcypher.com/v1/btc/main/payments/f35c80c2-3347-410d-b4ac-d049910289ec', params=params)
# will return nothing, but we can confirm that delete was succesful via http code
>>> assert r.status_code == 204
```

```php
<?php
// Run on console:
// php -f .\sample\payment-api\DeletePaymentEndpoint.php

$webHook = new PaymentForward();
$webHook->setId('ec2b4b4f-eeb2-4824-b528-7d78a6f52492');
$webHook->delete($apiContext);

HTTP/1.1 204 No Content
```

When you're done with a payment forwarding address, you can delete it via its id.

Resource | Method | Return Object
-------- | ------ | -------------
/payments/$PAYID | DELETE |  *nil*

PAYID is a string representing the payment forwarding request you want to delete, for example:

`399d0923-e920-48ee-8928-2051cbfbc369`

This will return no object, but will return an HTTP Status Code 204 if the request was successfully deleted.
