# REST API

### Overview

This documentation provides information on available requests and how to interact via [Public](#public-api) and [Trade](#trade-api) APIs.

### Usage Example

* [pinhopro/blinktrade_example_restapi.py](https://gist.github.com/pinhopro/60b1fd213b36d576505e)

## Public API

> `<API_URL>` is a complete Public API URL pointing to a resource.

```javascript

var BlinkTradeRest = require("blinktrade").BlinkTradeRest;
var BlinkTrade = new BlinkTradeRest({
  prod: false,
  currency: "BRL",
});

```

```shell
curl '<API_URL>'
```

```php
<?php

$api_url = '<API_URL>';

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $api_url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$response = curl_exec($ch);
curl_close($ch);

echo $response;
```

```python
import requests

api_url = '<API_URL>'
response = requests.get(api_url)

print response.text
```

The Public API can be accessed under `/api/v1/<CURRENCY>`, e.g, production is `https://api.blinktrade.com/api/v1/<CURRENCY>`

An `HTTP GET` request method should be used to fetch data.

## Ticker

Ticker is a summary information about the current status of an exchange.

### HTTP Request

`GET /api/v1/<CURRENCY>/ticker?crypto_currency=BTC`

```javascript

BlinkTrade.ticker().then(function(ticker) {
  console.log(ticker);
});

```

```shell

$ curl -XGET https://api.blinktrade.com/api/v1/BRL/ticker

```

### Parameters

Name            | Description
----------------|------------
crypto_currency | Crypto currency to be used. **Optional**; defaults to BTC.

> __EXAMPLE RESPONSE__

```json
{
	"pair": "BTCBRL",
	"last": 2280.0,
	"high": 2306.0,
	"low": 2205.0,
	"vol": 113.17267938,
	"vol_brl": 255658.20705113,
	"buy": 2263.0,
	"sell": 2279.77
}
```

### Response

Name             | Type   | Description
-----------------|--------|------------
pair             | string | [\<SYMBOL\>](#symbols)
last             | number | Value of the last purchase in the last 24 hours.
high             | number | Price of the highest purchase in the last 24 hours.
low              | number | Price of the lowest purchase in the last 24 hours.
vol              | number | Trading volume in the last 24 hours.
vol_[\<CURRENCY\>](#currencies) | number | Trading volume in the last 24 hours in [\<CURRENCY\>](#currencies) (lowercase).
buy              | number | Price of the most recent buy order.
sell             | number | Price of the most recent sell order.


## Orderbook

Order book is a list of orders that shows the interest of buyers (bids) and sellers (asks).

### HTTP Request

`GET /api/v1/<CURRENCY>/orderbook?crypto_currency=BTC`

```javascript

BlinkTrade.orderbook().then(function(orderbook) {
  console.log(orderbook);
});

```

```shell

$ curl -XGET https://api.blinktrade.com/api/v1/BRL/orderbook

```

### Parameters

Name            | Description
----------------|------------
crypto_currency | Crypto currency to be used. **Optional**; defaults to BTC.

> __EXAMPLE RESPONSE__

```json
{
	"pair": "BTCBRL",
	"bids": [
		[2257.89, 0.20752212, 90852987],
		[2257.88, 1.01201126, 90800395],
		[2249.98, 0.05052466, 90806289]
	],
	"asks": [
		[2272.14, 2.3648572, 90803493],
		[2279.63, 0.08722052, 90840584],
		[2279.75, 0.04118941, 90823262]
	]
}
```

### Response

Name       | Type          | Description
-----------|---------------|------------
pair       | string        | [\<SYMBOL\>](#symbols)
bids       | array(array)  | Array of bids from buyers.
asks       | array(array)  | Array of asks from sellers.

For each element of `bids` or `asks` array:

Index Array | Type    | Description
:----------:|---------|------------
0           | number  | Unit price.
1           | number  | Amount to buy/sell.
2           | number  | User ID.

## Trades

A list of the last trades executed on an exchange since a chosen date.

### HTTP Request

`GET /api/v1/<CURRENCY>/trades?crypto_currency=BTC&since=<TIMESTAMP>&limit=<NUMBER>`

```javascript

BlinkTrade.trades({ limit: 2, since: 1467990302}).then(function(trades) {
  console.log(trades);
});

```

```shell

$ curl -XGET https://api.blinktrade.com/api/v1/BRL/trades?since=1467990302&limit=2

```

### Parameters

Name            | Description
----------------|------------
crypto_currency | Crypto currency to be used. **Optional**; defaults to BTC.
since           | Date which executed trades must be fetched from. `<TIMESTAMP>` is in Unix Time date format. **Optional**; defaults to the date of the first executed trade.
limit           | Limit of trades that will be returned. `<NUMBER>` should be a positive integer. **Optional**; defaults to 100 trades.

### Response

> __EXAMPLE RESPONSE__

```json
[
	{
		"tid": 404681,
		"date": 1467990302,
		"price": 2280.89,
		"amount": 0.53487892,
		"side": "buy"
	},
	{
		"tid": 404682,
		"date": 1467990492,
		"price": 2261.01,
		"amount": 0.0373,
		"side": "sell"
	}
]
```

The response is an array of objects where for each object:

Name   | Type   | Description
-------|--------|------------
tid    | number | Trade ID.
date   | number | Executed date in Unix Time.
price  | number | Unit price.
amount | number | Amount bought/sold.
side   | string | "buy" for a buy order or "sell" for a sell order.


## Trade API

The Trade API can be accessed under `/tapi/v1/message`, e.g, production is `https://api.blinktrade.com/tapi/v1/message`. An API Key is needed in order to authenticate your access.

An `HTTP POST` request method should be used to send a message.

### Headers

The following headers must be present in your `POST` message:

Header       | Description
-------------|------------
APIKey       | Your API Key generated from your exchange or testnet environment.
Nonce        | Must be an integer, always greater than the previous one.
Signature    | The HMAC-SHA256 signature of Nonce using your API Secret as key.
Content-Type | `application/json`

```javascript

var BlinkTradeRest = require("blinktrade").BlinkTradeRest;
var BlinkTrade = new BlinkTradeRest({
  prod: false,
  key: "YOUR_API_KEY_GENERATED_IN_API_MODULE",
  secret: "YOUR_SECRET_KEY_GENERATED_IN_API_MODULE",
  currency: "BRL",
});

```

```shell
#!/bin/bash
# usage: ./this-script '<JSON_MESSAGE>'

# Get message from first command line argument
[ "$1" ] || {
	echo "error: missing JSON message to be sent"
	echo "usage: ./this-script '<JSON_MESSAGE>'"
	exit 1
}
message="$1"

# Trade API URL
api_url='<API_URL>'

# Set API Key and Secret
api_key='<API_KEY>'
api_secret='<API_SECRET>'

# Generate a nonce
nonce=`date +%s`

# Sign the nonce with secret using HMAC-SHA256
signature=`echo -n $nonce | openssl dgst -sha256 -hex -hmac $api_secret`

# Send a POST message to API URL
curl -X POST "$api_url" \
	-H "APIKey:$api_key" \
	-H "Nonce:$nonce" \
	-H "Signature:$signature" \
	-H "Content-Type:application/json" \
	-d "$message"
```

```php
<?php

// @param {array} $message a JSON_MESSAGE to be sent
function send($message)
{
	$message = json_encode($message);

	// Trade API URL
	$api_url = '<API_URL>';

	// Set API Key and Secret
	$api_key    = '<API_KEY>';
	$api_secret = '<API_SECRET>';

	// Generate a nonce
	$nonce = strval(time());

	// Sign the nonce with secret using HMAC-SHA256
	$signature = hash_hmac('sha256', $nonce, $api_secret);

	// Define headers
	$headers = array(
		'APIKey:'. $api_key,
		'Nonce:'. $nonce,
		'Signature:'. $signature,
		'Content-Type:application/json'
	);

	// Send a POST message to API URL
	$ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, $api_url);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
	curl_setopt($ch, CURLOPT_CUSTOMREQUEST, 'POST');
	curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
	curl_setopt($ch, CURLOPT_POSTFIELDS, $message);
	$response = curl_exec($ch);
	curl_close($ch);

	return $response;
}
```

```python
#!/usr/bin/env python
import hashlib
import hmac
import time
import requests
import datetime

# @param {dict} message the JSON_MESSAGE to be sent
def send(message):
	# Trade API URL
	api_url = '<API_URL>'

	# Set API Key and Secret
	api_key    = '<API_KEY>'
	api_secret = '<API_SECRET>'

	# Generate a nonce
	dt = datetime.datetime.now()
	nonce = str(int((time.mktime( dt.timetuple() )  + dt.microsecond/1000000.0) * 1000000))

	# Sign the nonce with secret using HMAC-SHA256
	signature = hmac.new(api_secret, nonce, digestmod=hashlib.sha256).hexdigest()

	# Set headers
	headers = {
		'APIKey': api_key,
		'Nonce': nonce,
		'Signature': signature,
		'Content-Type': 'application/json'
	}

	# Send a POST message to API URL
	return requests.post(api_url, json=message, verify=True, headers=headers).json()
```

### Code Example (Trade API)

The following parameters are used in the code snippets:

Name             | Description
-----------------|------------
\<API_URL\>      | Either production `https://api.blinktrade.com/tapi/v1/message` or testing `https://api.testnet.blinktrade.com/tapi/v1/message`
\<API_KEY\>      | Your API Key generated when you created the API.
\<API_SECRET\>   | Your API Secret generated when you created the API.
\<JSON_MESSAGE\> | The message you want to send.


## Request Balance

Request your balances for each broker you use.

> __MESSAGE EXAMPLE__

```json
{
	"MsgType": "U2",
	"BalanceReqID": 1
}
```

```javascript

var BlinkTradeRest = require("blinktrade").BlinkTradeRest;
var BlinkTrade = new BlinkTradeRest({
  prod: false,
  key: "YOUR_API_KEY_GENERATED_IN_API_MODULE",
  secret: "YOUR_SECRET_KEY_GENERATED_IN_API_MODULE",
});

BlinkTrade.balance().then(function(balance) {
  console.log(balance);
});

```

```shell

api_key=YOUR_API_KEY_GENERATED_IN_API_MODULE
api_secret=YOUR_SECRET_KEY_GENERATED_IN_API_MODULE

nonce=`date +%s`
signature=`echo -n $nonce | openssl dgst -sha256 -hex -hmac $api_secret`

curl -XPOST https://api.testnet.blinktrade.com/tapi/v1/message \
    -H "Nonce:$nonce" \
    -H "APIKey:$api_key" \
    -H "Content-Type:application/json" \
    -H "Signature:$signature" \
    -d '{"MsgType":"U2","BalanceReqID":1}'

```


### Parameters

Name         | Type   | Description/Value
-------------|--------|------------------
MsgType      | string | "U2"
BalanceReqID | number | An ID assigned by you. It can be any number. The response message associated with this request will contain the same ID.

> __RESPONSE EXAMPLE__

```json
{
	"Status": 200,
	"Description": "OK",
	"Responses": [
		{
			"MsgType": "U3",
			"5": {
				"BTC": 37941788734,
				"USD": 5563564460190,
				"BTC_locked": 1200000000,
				"USD_locked": 0
			},
			"ClientID": 90000002,
			"BalanceReqID": 1
		}
	]
}
```

### Response

Returns your balance for each broker.

Name         | Type    | Description/Value
-------------|---------|------------------
MsgType      | string  | "U3" UserBalanceResponse message.
[\<BROKER_ID\>](#brokers)| object  | The [Broker ID](#brokers) containing your BTC and FIAT balance, e.g.: "5" stands for your balance with the [Broker ID](#brokers) number 5.
ClientID     | number  | Your account ID.
BalanceReqID | number  | This should match the BalanceReqID sent on the message "U2".

Balance model example for BTC and USD:

Name         | Type   | Description
-------------|--------|------------
BTC          | number | Amount in satoshis of BTC you have available in your account.
USD          | number | Amount in USD (or your FIAT currency) you have available in your account.
BTC_locked   | number | Amount in satoshis of BTC you have locked (open orders, margin positions, etc).
USD_locked   | number | Amount in USD (or your FIAT currency) you have locked (open orders, margin positions, etc).

## Request Open Orders

Request a list of your open orders.

> __MESSAGE EXAMPLE__

```json
{
	"MsgType": "U4",
	"OrdersReqID": 1,
	"Page": 0,
	"PageSize": 20,
	"Filter": ["has_leaves_qty eq 1"]
}
```

```javascript

BlinkTrade.myOrders().then(function(myOrders) {
  console.log(myOrders);
});

```

```shell

api_key=YOUR_API_KEY_GENERATED_IN_API_MODULE
api_secret=YOUR_SECRET_KEY_GENERATED_IN_API_MODULE

nonce=`date +%s`
signature=`echo -n $nonce | openssl dgst -sha256 -hex -hmac $api_secret`

curl -XPOST https://api.testnet.blinktrade.com/tapi/v1/message \
    -H "Nonce:$nonce" \
    -H "APIKey:$api_key" \
    -H "Content-Type:application/json" \
    -H "Signature:$signature" \
    -d '{"MsgType":"U4","OrdersReqID":1,"Page":0,"PageSize":20}'

```


### Parameters

Name        | Type          | Description/Value
------------|---------------|------------------
MsgType     | string        | "U4"
OrdersReqID | number        | An ID assigned by you. It can be any number. The response message associated with this request will contain the same ID.
Page        | number        | **Optional**; defaults to 0.
PageSize    | number        | **Optional**; defaults to 20.
Filter      | array(string) | Set it to "has_leaves_qty eq 1" to get open orders, "has_cum_qty eq 1" to get executed orders, "has_cxl_qty eq 1" to get cancelled orders.

> __RESPONSE EXAMPLE__

```json
{
	"Status": 200,
	"Description": "OK",
	"Responses": [
		{
			"MsgType": "U5",
			"OrdersReqID": 1,
			"Page": 0,
			"PageSize": 20,
			"Columns": [
				"ClOrdID",
				"OrderID",
				"CumQty",
				"OrdStatus",
				"LeavesQty",
				"CxlQty",
				"AvgPx",
				"Symbol",
				"Side",
				"OrdType",
				"OrderQty",
				"Price",
				"OrderDate",
				"Volume",
				"TimeInForce"
			],
			"OrdListGrp": [
				[
					"7510926",
					1459028463348,
					0,
					"0",
					1000000000,
					0,
					0,
					"BTCUSD",
					"2",
					"2",
					1000000000,
					64500000000,
					"2016-07-10 18:22:06",
					0,
					"1"
				]
			]
		}
	]
}
```

### Response

Name        | Type          | Description/Value
------------|---------------|------------------
MsgType     | string        | "U5" OrdersListResponse message.
OrdersReqID | number        | Match the request OrdersReqID field
Page        | number        | Starts at 0.
PageSize    | number        | The page size. If the length of the array `OrdListGrp` is greather or equal to the `PageSize`, you should issue a new request incrementing the `Page`.
Columns     | array(string) | Description of all columns of all orders in `OrdListGrp`.


Index Array (Name) | Type   | Description/Value
-------------------|--------|------------------
0  ("ClOrdID")     | string | Client order ID set by you.
1  ("OrderID")     | number | Order ID set by BlinkTrade.
2  ("CumQty")      | number | The executed quantity of this order.
3  ("OrdStatus")   | string | "0" = New, "1" = Partially filled, "2" = Filled, "4" = Cancelled, "8" = Rejected, "A" = Pending New
4  ("LeavesQty")   | number | Quantity open for further execution.
5  ("CxlQty")      | number | Total quantity canceled for this order.
6  ("AvgPx")       | number | Calculated average price of all fills on this order.
7  ("Symbol")      | string | [\<SYMBOL\>](#symbols)
8  ("Side")        | string | "1" = Buy, "2" = Sell
9  ("OrdType")     | string | "2" = Limited
10 ("OrderQty")    | number | Quantity ordered in satoshis.
11 ("Price")       | number | Price per unit in "satoshis" of your local currency.
12 ("OrderDate")   | string | Order date in UTC.
13 ("Volume")      | number | Quantity * Price
14 ("TimeInForce") | string | "0" = Day, "1" = Good Till Cancel, "4" = Fill or Kill


## Send New Order

> __MESSAGE EXAMPLE__

```json
{
	"MsgType": "D",
	"ClOrdID": 123,
	"Symbol": "BTCUSD",
	"Side": "1",
	"OrdType": "2",
	"Price": 26381000000,
	"OrderQty": 2723810,
	"BrokerID": 5
}
```

```javascript

BlinkTrade.sendOrder({
  "side": "1", // Buy
  "price": parseInt((550 * 1e8).toFixed(0)),
  "amount": parseInt((0.05 * 1e8).toFixed(0)),
  "symbol": "BTCUSD",
}).then(function(order) {
  console.log(order);
});

```

```shell

api_key=YOUR_API_KEY_GENERATED_IN_API_MODULE
api_secret=YOUR_SECRET_KEY_GENERATED_IN_API_MODULE

nonce=`date +%s`
signature=`echo -n $nonce | openssl dgst -sha256 -hex -hmac $api_secret`

curl -XPOST https://api.testnet.blinktrade.com/tapi/v1/message \
    -H "Nonce:$nonce" \
    -H "APIKey:$api_key" \
    -H "Content-Type:application/json" \
    -H "Signature:$signature" \
    -d '{"MsgType":"D","ClOrdID":123,"Side":"1","Symbol":"BTCUSD","OrdType":"2","Price":48000000000,"OrderQty":50000000,"BrokerID":5}'

```

### Parameters

Name     | Type   | Description/Value
---------|--------|------------------
MsgType  | string | "D"
ClOrdID  | number | Unique identifier for Order as assigned by you.
Symbol   | string | [\<SYMBOL\>](#symbols)
Side     | string | "1" = Buy, "2" = Sell
OrdType  | string | "2" = Limited
Price    | number | Price in satoshis.
OrderQty | number | Quantity in satoshis.
BrokerID | number | [\<BROKER_ID\>](#brokers)

> __RESPONSE EXAMPLE__


```json
{
	"Status": 200,
	"Description": "OK",
	"Responses": [
		{
			"MsgType": "U3",
			"5": {
				"USD_locked": 718568316
			},
			"ClientID": 90800003
		}, 
		{
			"MsgType": "8",
			"OrderID": 5669865,
			"ExecID": 35,
			"ExecType": "0",
			"OrdStatus": "0",
			"LeavesQty": 2723810,
			"Symbol": "BTCUSD",
			"OrderQty": 2723810,
			"LastShares": 0,
			"LastPx": 0,
			"CxlQty": 0,
			"TimeInForce": "1",
			"CumQty": 0,
			"ClOrdID": "123",
			"OrdType": "2",
			"Side": "1",
			"Price": 26381000000,
			"ExecSide": "1",
			"AvgPx": 0
		}
	]
}
```

__NOTE__: In this example, the request returned 2 messages.

### Balance Response

field        | Type    | Description/Value
-------------|---------|------------------
[\<BROKER_ID\>](#brokers)| object  | The [Broker ID](#brokers) containing your BTC and FIAT balance, e.g.: "5" stands for your balance with the [Broker ID](#brokers) number 5.
MsgType      | string  | "U3" Balance response. Problably because the request also change your account balance.
ClientID     | number  | Your account ID.

### Execution Report Response

field       | Type   | Description/Value
------------|--------|------------------
MsgType     | string | "8" Execution Report. Check for a full fix doc here: [http://www.onixs.biz/fix-dictionary/4.4/msgType_8_8.html](http://www.onixs.biz/fix-dictionary/4.4/msgType_8_8.html).
OrderID     | number | Unique identifier for Order as assigned by broker.
ExecID      | number | Unique identifier of execution message as assigned by broker.
ExecType    | string | "0" = New, "1" = Partially fill, "2" = Fill, "4" = Cancelled, "8" = Rejected, "A" = Pending New
OrdStatus   | string | "0" = New, "1" = Partially fill, "2 "= Fill, "4" = Cancelled, "8" = Rejected, "A" = Pending New
LeavesQty   | number | Quantity open for further execution.
Symbol      | string | Currency pair being used.
OrderQty    | number | Quantity ordered in satoshis.
LastShares  | number | Quantity of shares bought/sold on this fill.
LastPx      | number | Price of the last fill.
CxlQty      | number | Total quantity canceled for this order.
TimeInForce | string | "0" = Day, "1" = Good Till Cancel, "4" = Fill or Kill
CumQty      | number | Total quantity filled.
ClOrdID     | string | Unique identifier for Order as assigned by you
OrdType     | string | "2" = Limited
Side        | string | "1" = Buy, "2" = Sell
Price       | number | Price per unit of quantity in satoshis.
ExecSide    | string | Side of this fill.
AvgPx       | number | Calculated average price of all fills on this order.

## Cancel Order Sent

> __MESSAGE EXAMPLE__

```json
{
    "MsgType": "F",
    "OrderID": 1459028830899,
    "ClOrdID": 123
}
```

```javascript

BlinkTrade.cancelOrder({ orderID: order.OrderID, clientId: order.ClOrdID }).then(function(order) {
  console.log("Order Cancelled");
});

```

```shell

api_key=YOUR_API_KEY_GENERATED_IN_API_MODULE
api_secret=YOUR_SECRET_KEY_GENERATED_IN_API_MODULE

nonce=`date +%s`
signature=`echo -n $nonce | openssl dgst -sha256 -hex -hmac $api_secret`

curl -XPOST https://api.testnet.blinktrade.com/tapi/v1/message \
    -H "Nonce:$nonce" \
    -H "APIKey:$api_key" \
    -H "Content-Type:application/json" \
    -H "Signature:$signature" \
    -d '{"MsgType":"D","OrderID":1459028830899,"ClOrdID":123}'

```

### Parameters

Name    | Type   | Description/Value
--------|--------|------------------
MsgType | string | "F" Order Cancel Request message. Check for a full doc here: [http://www.onixs.biz/fix-dictionary/4.4/msgType_F_70.html](http://www.onixs.biz/fix-dictionary/4.4/msgType_F_70.html).
ClOrdID | number | ID for an Order as assigned by you.

### Response

> __MESSAGE RESPONSE__

```json
{
	"Status": 200,
	"Description": "OK",
	"Responses": [
		{
			"MsgType": "U3",
			"5": {
				"USD_locked": 0
			},
			"ClientID": 90800003
		},
		{
			"MsgType": "8",
			"OrderID": 5669865,
			"ExecID": 36,
			"ExecType": "4",
			"OrdStatus": "4",
			"LeavesQty": 0,
			"Symbol": "BTCUSD",
			"OrderQty": 2723810,
			"LastShares": 0,
			"LastPx": 0,
			"CxlQty": 2723810,
			"TimeInForce": "1",
			"CumQty": 0,
			"ClOrdID": "123",
			"OrdType": "2",
			"Side": "1",
			"Price": 26381000000,
			"ExecSide": "1",
			"AvgPx": 0
		}
	]
 }
```

The response is the same as the send order, together with the balance response.

## Request Deposit

> __MESSAGE EXAMPLE__

```json
{
	"MsgType": "U18",
	"DepositReqID": 1,
	"DepositMethodID": 403,
	"Value": 150000000000,
	"Currency": "BRL",
	"BrokerID": 5
}
```

```javascript


// Bitcoin Deposit
blinktrade.requestDeposit().then(function(deposit) {
  console.log(deposit);
});

// Fiat Deposit

blinktrade.requestDeposit({
  value: parseInt(200 * 1e8),
  currency: "BRL",
  depositMethodId: 403,
}).then(function(deposit) {
  console.log(deposit);
});

```

```shell

api_key=YOUR_API_KEY_GENERATED_IN_API_MODULE
api_secret=YOUR_SECRET_KEY_GENERATED_IN_API_MODULE

nonce=`date +%s`
signature=`echo -n $nonce | openssl dgst -sha256 -hex -hmac $api_secret`

curl -XPOST https://api.testnet.blinktrade.com/tapi/v1/message \
    -H "Nonce:$nonce" \
    -H "APIKey:$api_key" \
    -H "Content-Type:application/json" \
    -H "Signature:$signature" \
    -d '{"MsgType":"U18","DepositReqID":1,"DepositMethodID":403,"Value":150000000000,"Currency":"BRL","BrokerID":5}'

```

### Parameters

Name            | Type   | Description/Value
----------------|--------|------------------
MsgType         | string | "U18"
DepositReqID    | number | Deposit Request ID.
DepositMethodID | number | Deposit Method ID - Check with your exchange.
Value           | number | Amount in satoshis
Currency        | string | [\<CURRENCY\>](#currencies)
BrokerID        | number | [\<BROKER_ID\>](#brokers)

> __FIAT RESPONSE EXAMPLE__

```json
{
    "Status": 200,
    "Description": "OK",
    "Responses": [
        {
            "DepositMethodName": "deposit_btc",
            "UserID": 90800003,
            "ControlNumber": null,
            "State": "UNCONFIRMED",
            "Type": "CRY",
            "PercentFee": 0,
            "Username": "user",
            "CreditProvided": 0,
            "DepositReqID": 7302188,
            "DepositID": "2a6b5e322fd24574a4d9f988681a542f",
            "Reason": null,
            "AccountID": 90800003,
            "Data": {
                "InputAddress": "mjjVMr8WcYQwVGzYc8HpaRyAZc89ngTdKV",
                "Destination": "n19ZAH1WGoUkQhubQw71fH11BenifxpBxf"
            },
            "ClOrdID": "7302188",
            "Status": "0",
            "Created": "2016-09-03 23:08:26",
            "DepositMethodID": null,
            "Value": 0,
            "BrokerID": 5,
            "PaidValue": 0,
            "Currency": "BTC",
            "ReasonID": null,
            "MsgType": "U23",
            "FixedFee": 0
        }
    ]
}
```

### Response

Returns a Deposit Model Object.

**NOTE** The `Data.InputAddress` is the address that you have to deposit, **DO NOT DEPOSIT** on `Data.Destination` address.

## Request List of Deposits

> __MESSAGE EXAMPLE__

```json

{
    "MsgType": "U30",
    "DepositListReqID": 123,
    "Page": 0,
    "PageSize": 20,
    "StatusList": ["1", "2", "4", "8"]
}

```

```javascript

blinktrade.requestDepositList().then(function(deposits) {
  console.log(deposits);
});

```

```shell

api_key=YOUR_API_KEY_GENERATED_IN_API_MODULE
api_secret=YOUR_SECRET_KEY_GENERATED_IN_API_MODULE

nonce=`date +%s`
signature=`echo -n $nonce | openssl dgst -sha256 -hex -hmac $api_secret`

curl -XPOST https://api.testnet.blinktrade.com/tapi/v1/message \
    -H "Nonce:$nonce" \
    -H "APIKey:$api_key" \
    -H "Content-Type:application/json" \
    -H "Signature:$signature" \
    -d '{"MsgType":"U30","DepositListReqID":1,"Page":0,"PageSize":100}'

```


Name              | Type          | Description/Value
------------------|---------------|------------------
MsgType           | string        | "U26"
DepositListReqID  | number        | Request ID
Page              | number        | **Optional**; defaults to 0.
PageSize          | number        | **Optional**; defaults to 20.
StatusList        | array(string) | "0" = Unconfirmed, "1" = Pending, "2" = In Progress, "4" = Completed, "8" = Cancelled

> EXAMPLE RESPONSE

```json

{
    "Status": 200,
    "Description": "OK",
    "Responses": [{
        "PageSize": 1,
        "DepositListReqID": 1,
        "MsgType": "U31",
        "DepositListGrp": [
            ["6f0e1819a30b485591145c7c0d045012", 502, "wire_transfer_usa", "DTP", "USD", 20000000000, 0, {}, "2016-09-06 23:44:05", 502000172, 1.0, 0, "0", null, null, "rodrigo", 90800003, 5, "166399", 0, "UNCONFIRMED"]
        ],
        "Page": 0,
        "Columns": ["DepositID", "DepositMethodID", "DepositMethodName", "Type", "Currency", "Value", "PaidValue", "Data", "Created", "ControlNumber", "PercentFee", "FixedFee", "Status", "ReasonID", "Reason", "Username", "UserID", "BrokerID", "ClOrdID", "CreditProvided", "State"]
    }]
}

```


### Response

Returns a Withdraw Model Object.

## Request Withdrawal

> __MESSAGE EXAMPLE__

```json
{
	"MsgType": "U6",
	"WithdrawReqID": 382616,
	"Method": "bradesco",
	"Currency": "BRL",
	"Amount": 1500000000,
	"Data": {
		"AccountBranch": "111",
		"AccountNumber": "4444-5",
		"AccountType": "corrente", 
		"CPF_CNPJ": "00000000000"
	}
}
```

```javascript

BlinkTrade.requestWithdraw({
  "amount": parseInt(400 * 1e8),
  "currency": "BRL",
  "method": "bradesco",
  "data": {
    "AccountBranch": "111",
    "AccountNumber": "4444-5",
    "AccountType": "corrente",
    "CPF_CNPJ": "00000000000"
  }
}).then(function(withdraw) {
  console.log(withdraw);
});

```

```shell

api_key=YOUR_API_KEY_GENERATED_IN_API_MODULE
api_secret=YOUR_SECRET_KEY_GENERATED_IN_API_MODULE

nonce=`date +%s`
signature=`echo -n $nonce | openssl dgst -sha256 -hex -hmac $api_secret`

curl -XPOST https://api.testnet.blinktrade.com/tapi/v1/message \
    -H "Nonce:$nonce" \
    -H "APIKey:$api_key" \
    -H "Content-Type:application/json" \
    -H "Signature:$signature" \
    -d '{"MsgType":"U6","DepositReqID":1,"WithdrawReqID":617625,"Method":"PayPal","Amount":3000000,"Currency":"USD","BrokerID":5,"Data":{"Email":"user@blinktrade.com"}}'

```

### Paramenters

Params        | Type   | Description/Value
--------------|--------|------------------
MsgType       | string | "U6"
WithdrawReqID | number | Request ID
Method        | string | bitcoin for BTC. Check with the exchange all available withdrawal methods
Amount        | number | Amount in satoshis
Currency      | string | Currency code
Data          | object | Data object containing the withdraws required fields.

**FOXBIT**

| Methods               | Required Data fields                                                      |
|-----------------------|---------------------------------------------------------------------------|
| bradesco              | AccountBranch, AccountNumber, AccountType, CPF_CNPJ                       |
| bb                    | AccountBranch, AccountNumber, AccountType, CPF_CNPJ                       |
| Caixa                 | AccountBranch, AccountNumber, AccountType, CPF_CNPJ                       |
| ted                   | BankName, BankNumber, AccountBranch, AccountNumber, AccountType, CPF_CNPJ |

**VBTC**

| Methods                | Required Data fields                                                                                             |
|------------------------|------------------------------------------------------------------------------------------------------------------|
| banktransfer           | BankName, AccountBranch, BankCity, AccountName, AccountNumber, BankSwift                                         |
| VPBankinternaltransfer | VPbankbranch, BankCity, AccountName, AccountNumber, BankSwift                                                    |
| cashtoID               | BankName, BankBranch, BankCity, Clientname, ClientIDNr, Issue Date ID, Place of Issue, Phone Number of Recipient |

> __RESPONSE EXAMPLE__

```json
{
	"Status": 200,
	"Description": "OK",
	"Responses": [
		{
			"MsgType": "U3",
			"4": {
				"BTC": 2087629545
			},
			"ClientID": 90800025
		},
		{
			"MsgType": "U7",
			"WithdrawReqID": 617625,
			"WithdrawID": 339,
			"UserID": 90800025,
			"Username": "user",
			"Method": "bitcoin",
			"Amount": 3000000,
			"BrokerID": 4,
			"ClOrdID": null,
			"Created": "2016-03-21 08:27:45",
			"Currency": "BTC",
			"Data": {
				"Wallet": "mwmabpJVisvti3WEP5vhFRtn3yqHRD9KNP"
			},
			"FixedFee": 10000,
			"PercentFee": 0.0,
			"PaidAmount": 3010000,
			"Reason": null,
			"ReasonID": null,
			"Status": "1"
		}
	]
}

```

### Response

Returns a Balance and Withdraw Model Object.

## Request List of Withdrawals

> __MESSAGE EXAMPLE__

```json
{
	"MsgType": "U26",
	"WithdrawListReqID": 1,
	"Page": 0,
	"PageSize": 100,
	"StatusList": ["1", "2"]
}
```

```javascript

blinktrade.requestWithdrawList().then(function(withdraws) {
  console.log(withdraws);
});

```

```shell

api_key=YOUR_API_KEY_GENERATED_IN_API_MODULE
api_secret=YOUR_SECRET_KEY_GENERATED_IN_API_MODULE

nonce=`date +%s`
signature=`echo -n $nonce | openssl dgst -sha256 -hex -hmac $api_secret`

curl -XPOST https://api.testnet.blinktrade.com/tapi/v1/message \
    -H "Nonce:$nonce" \
    -H "APIKey:$api_key" \
    -H "Content-Type:application/json" \
    -H "Signature:$signature" \
    -d '{"MsgType":"U26","WithdrawListReqID":1,"Page":0,"PageSize":1}'

```

### Parameters

Name              | Type          | Description/Value
------------------|---------------|------------------
MsgType           | string        | "U26"
WithdrawListReqID | number        | An ID chosen by you.
Page              | number        | **Optional**; defaults to 0.
PageSize          | number        | **Optional**; defaults to 20.
StatusList        | array(string) | "0" = Unconfirmed, "1" = Pending, "2" = In Progress, "4" = Completed, "8" = Cancelled

> __RESPONSE EXAMPLE__

```json
{
    "Status": 200,
    "Description": "OK",
    "Responses": [{
        "WithdrawListReqID": 1,
        "PageSize": 1,
        "WithdrawListGrp": [
            [543, "PayPal", "USD", 20000000000, {
                "Instant": "NO",
                "Email": "user@blinktrade.com"
            }, "2016-09-07 00:30:49", "1", null, null, 0.0, 0, 20000000000, 90800003, "rodrigo", 5, "4660612"]
        ],
        "MsgType": "U27",
        "Page": 0,
        "Columns": ["WithdrawID", "Method", "Currency", "Amount", "Data", "Created", "Status", "ReasonID", "Reason", "PercentFee", "FixedFee", "PaidAmount", "UserID", "Username", "BrokerID", "ClOrdID"]
    }]
}

```
### Response

Returns an array of Withdraws Model Objects.
