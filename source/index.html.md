---
title: BlinkTrade API

language_tabs:
  - shell
  - python
  - php

includes:

search: true
---

# Getting Started

### OVERVIEW

BlinkTrade provides a simple and robust WebSocket API to integrate our platform, we strongly recommend you to use over the Rest API

### FIX

Our API is based on the [fix protocol](http://www.onixs.biz/fix-dictionary/4.4/index.html)

## BlinkTrade Endpoints

There are two working environments: `prod` for production purporses and `testnet` for testing purporses:

* `testnet` base URL endpoint is `https://api.testnet.blinktrade.com`
* `prod` base URL endpoint is `https://api.blinktrade.com`

Some features are accessed publicly and others require an authentication API Key. All data messages and responses are in JSON format.


## Brokers

A broker ID is assigned for each exchange powered by BlinkTrade:

Broker       | ID
-------------|---
SurBitcoin   | 1
VBTC         | 3
FoxBit       | 4
**Testnet**  | **5**
UrduBit      | 8
ChileBit     | 9


## Currencies

`<CURRENCY>` | Description
-------------|------------
BRL        | Brazil Reals (FoxBit)
VEF        | Venezuelan Bolivares (SurBitcoin)
CLP        | Chilean Pesos (ChileBit.NET)
VND        | Vietnamise Dongs (VBTC)
PKR        | Pakistani Ruppe (UrduBit)

### Symbols

`<SYMBOL>` | Description
-----------|------------
BTCBRL     | BTC Pair - Brazil Reals (FoxBit)
BTCCLP     | BTC Pair - Chilean Pesos (ChileBit.NET)
BTCPKR     | BTC Pair - Pakistani Ruppe (UrduBit)
BTCVEF     | BTC Pair - Venezuelan Bolivares (SurBitcoin)
BTCVND     | BTC Pair - Vietnamise Dongs (VBTC)

## Messages

List of Messages Based on the Fix Protocol

### USER MESSAGES

Code  | Description
------|---------------------
0   | [Heartbeat]()
1   | [TestRequest]()
C   | [Email]()
V   | [MarketDataRequest]()
W   | [MarketDataFullRefresh]()
X   | [MarketDataIncrementalRefresh]()
Y   | [MarketDataRequestReject]()
BE  | [UserRequest]()
BF  | [UserResponse]()
D   | [NewOrderSingle]()
F   | [OrderCancelRequest]()
8   | [ExecutionReport]()
x   | [SecurityListRequest]()
y   | [SecurityList]()
e   | [SecurityStatusRequest]()
f   | [SecurityStatus]()
AN  | [RequestForPositions]()
AO  | [RequestForPositionsAck]()
AP  | [PositionReport]()
U0  | [Signup]()
U2  | [UserBalanceRequest]()
U3  | [UserBalanceResponse]()
U4  | [OrdersListRequest]()
U5  | [OrdersListResponse]()
U6  | [WithdrawRequest]()
U7  | [WithdrawResponse]()
U9  | [WithdrawRefresh]()
U10 | [ResetPasswordRequest]()
U11 | [ResetPasswordResponse]()
U12 | [ResetPasswordRequest]()
U13 | [ResetPasswordResponse]()
U16 | [EnableDisableTwoFactorAuthenticationRequest]()
U17 | [EnableDisableTwoFactorAuthenticationResponse]()
U18 | [DepositRequest]()
U19 | [DepositResponse]()
U23 | [DepositRefresh]()
U20 | [DepositMethodsRequest]()
U21 | [DepositMethodsResponse]()
U24 | [WithdrawConfirmationRequest]()
U25 | [WithdrawConfirmationResponse]()
U26 | [WithdrawListRequest]()
U27 | [WithdrawListResponse]()
U28 | [BrokerListRequest]()
U29 | [BrokerListResponse]()
U30 | [DepositListRequest]()
U31 | [DepositListResponse]()
U32 | [TradeHistoryRequest]()
U33 | [TradeHistoryResponse]()
U34 | [LedgerListRequest]()
U35 | [LedgerListResponse]()
U36 | [TradersRankRequest]()
U37 | [TradersRankResponse]()
U38 | [UpdateProfile]()
U39 | [UpdateProfileResponse]()
U40 | [ProfileRefresh]()
U42 | [PositionRequest']()
U43 | [PositionResponse']()
U44 | [ConfirmTrustedAddressRequest']()
U45 | [ConfirmTrustedAddressResponse']()
U46 | [SuggestTrustedAddressPublish']()
U48 | [DepositMethodRequest]()
U49 | [DepositMethodResponse]()
U50 | [APIKeyListRequest]()
U51 | [APIKeyListResponse]()
U52 | [APIKeyCreateRequest]()
U53 | [APIKeyCreateResponse]()
U54 | [APIKeyRevokeRequest]()
U55 | [APIKeyRevokeResponse]()
U56 | [GetCreditLineOfCreditRequest]()
U57 | [GetCreditLineOfCreditResponse]()
U58 | [PayCreditLineOfCreditRequest]()
U59 | [PayCreditLineOfCreditResponse]()
U60 | [LineOfCreditListRequest]()
U61 | [LineOfCreditListResponse]()
U62 | [EnableCreditLineOfCreditRequest]()
U63 | [EnableCreditLineOfCreditResponse]()
U65 | [LineOfCreditRefresh]()
U70 | [CancelWithdrawalRequest]()
U71 | [CancelWithdrawalResponse]()
U72 | [CardListRequest]()
U73 | [CardListResponse]()
U74 | [CardCreateRequest]()
U75 | [CardCreateResponse]()
U76 | [CardDisableRequest]()
U77 | [CardDisableResponse]()
U78 | [WithdrawCommentRequest]()
U79 | [WithdrawCommentResponse]()

### BROKER MESSAGES

Code  | Description/Function
------|---------------------
B0  |  [ProcessDeposit]()
B1  |  [ProcessDepositResponse]()
B2  |  [CustomerListRequest]()
B3  |  [CustomerListResponse]()
B4  |  [CustomerRequest]()
B5  |  [CustomerResponse]()
B6  |  [ProcessWithdraw]()
B7  |  [ProcessWithdrawResponse]()
B8  |  [VerifyCustomerRequest]()
B9  |  [VerifyCustomerResponse]()
B1  |: [VerifyCustomerRefresh]()



# Rest API

The public API can be accessed under `/api/v1/<CURRENCY>`, e.g, production is `https://api.blinktrade.com/api/v1/<CURRENCY>` where `<CURRENCY>` value must be one of the following:

An `HTTP GET` request method should be used to fetch data.

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


## Ticker

Ticker is a summary information about the current status of an exchange.

### HTTP Request

`GET /api/v1/<CURRENCY>/ticker?crypto_currency=BTC`

### Parameters

Name            | Description
----------------|------------
crypto_currency | Crypto currency to be used. **Optional**; defaults to BTC.


> __EXAMPLE RESPONSE__ (Assuming `<CURRENCY>` = `BRL` and `crypto_currency` = `BTC`)

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

Name      | Type   | Description
----------|--------|------------
  pair    | string | Currency pair being used.
  last    | number | Value of the last purchase of 1 BTC in the last 24 hours.
  high    | number | Price of the highest purchase in the last 24 hours.
  low     | number | Price of the lowest purchase in the last 24 hours.
  vol     | number | Trading volume in the last 24 hours (BTC).
  vol_brl | number | Trading volume in the last 24 hours (BRL for this example).
  buy     | number | Price for an active buy order of 1 BTC.
  sell    | number | Price for an active sell order of 1 BTC.


### Orderbook

Order book is a list of orders that shows the interest of buyers (bids) and sellers (asks).

### HTTP Request

`GET /api/v1/<CURRENCY>/orderbook?crypto_currency=BTC`

### Parameters

Name            | Description
----------------|------------
crypto_currency | Crypto currency to be used. **Optional**; defaults to BTC.

> __EXAMPLE RESPONSE__ (Assuming `<CURRENCY>` = `BRL` and `crypto_currency` = `BTC`)

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

The response is an object where:

Name    | Type          | Description
------- |---------------|------------
pair    | string        | Currency pair being used.
bids    | array(number) | Array of bids from buyers.
bids[0] | number        | BTC unit price
bids[1] | number        | Amount of BTC to buy
bids[2] | number        | User ID
asks    | array(number) | Array of asks from sellers.
asks[0] | number        | BTC unit price
asks[1] | number        | Amount of BTC to sell
asks[2] | number        | User ID


### Trades

A list of last 100 trades executed in a exchange since a chosen date.

### HTTP Request

`GET /api/v1/<CURRENCY>/trades?crypto_currency=BTC&since=<UNIX_TIMESTAMP>`

### Parameters

Name            | Description
----------------|------------
crypto_currency | Crypto currency to be used. **Optional**; defaults to BTC.
since           | Date which executed trades must be fetched from. `<UNIX_TIMESTAMP>` is in Unix Time date format. **Optional**; defaults to the date of the first executed trade.

### Response

> __EXAMPLE RESPONSE__ (Assuming `<CURRENCY>` = `BRL`, `crypto_currency` = `BTC` and `since` = `1467990302`)

```
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
price  | number | BTC unit price.
amount | number | Amount of BTC bought/sold.
side   | string | "buy" for a buy order or "sell" for sell order.


Trade API
---------

The Trade API can be accessed under `/tapi/v1/message`, e.g, production is `https://api.blinktrade.com/tapi/v1/message`. An API Key is needed in order to authenticate your access.

An `HTTP POST` request method should be used to send a message.


### Creating an API Key

1. Go to some exchange powered by BlinkTrade or `https://tesnet.blinktrade.com` for `testnet` environment
2. Signup
3. Go to API page
4. Click "New API Key"
4. Enter a label and select the permissions for that API Key
5. Get the API Key and API Secret. The API Secret will only be shown once. (The API Password is only used for the WebSocket API.)

### Headers

The following headers must be present in your `POST` message:

Header       | Description
-------------|------------
APIKey       | Your API Key generated from your exchange or testnet environment.
Nonce        | Must be an integer, always greater than the previous one.
Signature    | The HMAC-SHA256 signature of Nonce using your API Secret as key.
Content-Type | `application/json`

### Code Example (Trade API)

The following code snippets can be used to send a message to the Trade API where:

### Parameters

Name         | Description
-------------|------------
API_URL      | either production `https://api.blinktrade.com/tapi/v1/message` or testing `https://api.testnet.blinktrade.com/tapi/v1/message`
API_KEY      | your API Key generated when you created the API
API_SECRET   | your API Secret generated when you created the API
JSON_MESSAGE | the message you want to send

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
  nonce=$(date +%s)

  # Sign the nonce with secret using HMAC-SHA256
  signature=$(echo -n $nonce | openssl dgst -sha256 -hex -hmac $api_secret | cut -d ' ' -f 2)

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


## Request Balance

### Parameters

> __MESSAGE EXAMPLE__

```json
{
	"MsgType": "U2",
	"BalanceReqID": 1
}
```

Name         | Type   | Description/Value
-------------|--------|------------------
MsgType      | string | "U2"
BalanceReqID | number | An ID assigned by you. It can be any number. The response message associated with this request will contain the same ID.

### Response

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
Returns your balance for each BrokerID

Name         | Type    | Description/Value
-------------|---------|------------------
MsgType      | string  | "U3" UserBalanceResponse message.
\<BROKER_ID\>| object  | The BrokerID containing your BTC and FIAT balance, e.g.: "5" stands for your balance with the BrokerID number 5.
ClientID     | number  | Your account ID.
BalanceReqID | number  | This should match the BalanceReqID sent on the message "U2".

Balance model example for BTC and USD

Name         | Type   | Description
-------------|--------|------------
BTC          | number | Amount in satoshis of BTC you have available in your account.
USD          | number | Amount in USD (or your FIAT currency) you have available in your account.
BTC_locked   | number | Amount in satoshis of BTC you have locked (open orders, margin positions, etc).
USD_locked   | number | Amount in USD (or your FIAT currency) you have locked (open orders, margin positions, etc).

## Request Open Orders

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

### Parameters

Name        | Type          | Description/Value
------------|---------------|------------------
MsgType     | string        | "U4"
OrdersReqID | number        | An ID assigned by you.
Page        | number        | **Optional**; defaults to 0.
PageSize    | number        | **Optional**; defaults to 20.
Filter      | array(string) | Set it to "has_leaves_qty eq 1" to get open orders, "has_cum_qty eq 1" to get executed orders, "has_cxl_qty eq 1" to get cancelled orders

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
PageSize    | number        | The page size. If the length of the array `OrdListGrp` is greather or equal to the `PageSize`, you should issue a new request incrementing the `Page`
Columns     | array(string) | Description of all columns of the all orders in `OrdListGrp`.


Index Array (Name) | Type   | Description/Value
-------------------|--------|------------------
0  ("ClOrdID")     | string | Client order ID. Set by you.
1  ("OrderID")     | number | Order ID. Set by BlinkTrade.
2  ("CumQty")      | number | The executed quantity of this order.
3  ("OrdStatus")   | string | "0" = New, "1" = Partially filled, "2" = Filled, "4" = Cancelled, "8" = Rejected, "A" = Pending New
4  ("LeavesQty")   | number | Quantity open for further execution.
5  ("CxlQty")      | number | Total quantity canceled for this order.
6  ("AvgPx")       | number | Calculated average price of all fills on this order.
7  ("Symbol")      | string | "BTCUSD", "BTCBRL", "BTCPKR", "BTCVND", "BTCVEF", "BTCCLP"
8  ("Side")        | string | "1" = Buy, "2" = Sell, "E" = Redem, "F" = Lend, "G" = Borrow
9  ("OrdType")     | string | "1" = Market, "2" = Limited, "3" = Stop, "4" = Stop Limit, "G" = Swap, "P" = Pegged
10 ("OrderQty")    | number | Quantity ordered in satoshis.
11 ("Price")       | number | Price per unit in "satoshis" of your local currency. Example: $ 2,125.89 = 212589000000
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

### Parameters

Name     | Type   | Description/Value
---------|--------|------------------
MsgType  | string | "D" New Order Single message. Check for a full doc here: http://www.onixs.biz/fix-dictionary/4.4/msgType_D_68.html 
ClOrdID  | number | Unique identifier for Order as assigned by you
Symbol   | string | "BTCUSD", "BTCBRL", "BTCPKR", "BTCVND", "BTCVEF", "BTCCLP".
Side     | string | "1" = Buy, "2" = Sell
OrdType  | string | [TODO]
Price    | number | Price in satoshis
OrderQty | number | Qty in satoshis
BrokerID | number | Broker ID as defined in table [Broker ID](#broker-id)

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

The response is an object where:

### Balance Response

field        | Type    | Description/Value
-------------|---------|------------------
\<BROKER_ID\>| object  | The BrokerID containing your BTC and FIAT balance, e.g.: "5" stands for your balance with the BrokerID number 5.
MsgType      | string  | "U3" Balance response. Problably because the request also change your account balance.
ClientID     | number  | Your account ID.
BalanceReqID | number  | This should match the BalanceReqID sent on the message "U2".

### Execution Report Response

field       | Type   | Description/Value
------------|--------|------------------
MsgType     | string | Execution Report. Check for a full fix doc here: http://www.onixs.biz/fix-dictionary/4.4/msgType_8_8.html
OrderID     | number | Unique identifier for Order as assigned by broker.
ExecID      | number | Unique identifier of execution message as assigned by broker.
ExecType    | string | "0" = New, "1" = Partially fill, "2" = Fill, "4" = Cancelled, "8" = Rejected, "A" = Pending New
OrdStatus   | string | "0" = New, "1" = Partially fill, "2 "= Fill, "4" = Cancelled, "8" = Rejected, "A" = Pending New
LeavesQty   | number | Quantity open for further execution
Symbol      | string | Currency pair being used.
OrderQty    | number | Quantity ordered in satoshis.
LastShares  | number | Quantity of shares bought/sold on this fill.
LastPx      | number | Price of the last fill.
CxlQty      | number | Total quantity canceled for this order.
TimeInForce | string | "0" = Day, "1" = Good Till Cancel, "4" = Fill or Kill
CumQty      | number | Total quantity filled.
ClOrdID     | string | Unique identifier for Order as assigned by you
OrdType     | string | "1" = Market, "2" = Limited, "3" = Stop, "4" = Stop Limit, "G" = Swap, "P" = Pegged
Side        | string | "1" = Buy, "2" = Sell, "E" = Redem, "F" = Lend, "G" = Borrow
Price       | number | Price per unit of quantity in satoshis.
ExecSide    | string | Side of this fill.
AvgPx       | number | Calculated average price of all fills on this order.

## Cancel Order Sent

__MESSAGE EXAMPLE__

```json
{
	"MsgType": "F",
	"ClOrdID": 123
}
```

Name    | Type   | Description/Value
--------|--------|------------------
MsgType | string | "F" Order Cancel Request message. Check for a full doc here: http://www.onixs.biz/fix-dictionary/4.4/msgType_F_70.html 
ClOrdID | number | Unique identifier for an Order as assigned by you

### Response

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

The response of Cancel Order Request is almost identical to the New Order Single, but with different paramenters:

Name          | Type   | Description/Value
--------------|--------|------------------
MsgType       | string | "U3" Balance response. Problably because the request also change your account balance.
\<BROKER_ID\> | object | The BrokerID containing your BTC and FIAT balance, e.g.: "5" stands for your balance with the BrokerID number 5.
ClientID      | number | Your account ID.

#### Execution Report Response

Name        | Type   | Description/Value
------------|--------|------------------
MsgType     | string | Execution Report. Check for a full fix doc here: http://www.onixs.biz/fix-dictionary/4.4/msgType_8_8.html
OrderID     | number | Unique identifier for Order as assigned by broker
ExecID      | number | Unique identifier of execution message as assigned by broker
ExecType    | string | "0" = New, "1" = Partially fill, "2" = Fill, "4" = Cancelled, "8" = Rejected, A=Pending New 
OrdStatus   | string | "0" = New, "1" = Partially fill, "2" = Fill, "4" = Cancelled, "8" = Rejected, A=Pending New 
LeavesQty   | number | Quantity open for further execution
Symbol      | string | Currency pair being used.
OrderQty    | number | Quantity ordered in satoshis.
LastShares  | number | Quantity of shares bought/sold on this fill.
LastPx      | number | Price of the last fill.
CxlQty      | number | Total quantity canceled for this order.
TimeInForce | string | "0" = Day, "1" = Good Till Cancel, "4" = Fill or Kill
CumQty      | number | Total quantity filled.
ClOrdID     | string | Unique identifier for Order as assigned by you
OrdType     | string | "1" = Market, "2" = Limited, "3" = Stop, "4" = Stop Limit, "G" = Swap, "P" = Pegged
Side        | string | "1" = Buy, "2" = Sell, "E" = Redem, "F" = Lend, "G" = Borrow 
Price       | number | Price per unit of quantity in satoshis.
ExecSide    | string | Side of this fill.
AvgPx       | number | Calculated average price of all fills on this order.


## Generate Bitcoin Deposit Address

> __MESSAGE EXAMPLE__

```json
{
	"MsgType": "U18",
	"DepositReqID": 1,
	"Currency": "BTC",
	"BrokerID": 5
}
```

### Paramenters

Name         | Type   | Description/Value
-------------|--------|------------------
MsgType      | string | "U18" Deposit Request
DepositReqID | number | An ID chosen by you.
Currency     | string | Currency Code
BrokerID     | number | As listed on [Broker ID](#broker-id)

> __RESPONSE EXAMPLE__

```json
{
	"Status": 200, 
	"Description": "OK", 
	"Responses": [
		{
			"MsgType": "U19",
			"DepositReqID": 1,
			"Currency": "BTC",
			"BrokerID": 5,
			"DepositMethodID": null,
			"DepositMethodName": "deposit_btc",
			"DepositID": "12db13d5c36c436993f8e8156467d2b6",
			"UserID": 90800003,
			"ControlNumber": null,
			"Type": "CRY",
			"Username": "user",
			"AccountID": 90800003,
			"Data": {
				"InputAddress": "mzUfpURjD1hDPNk7QBWQkXN5NbKjpf6e56",
				"Destination": "mtzsTx923NqnFeHugUBsgQKqr8YkEtzQzU"
			}, 
			"ClOrdID": null,
			"Status": "0",
			"Created": "2015-08-31 05:39:31",
			"Value": 0,
			"PaidValue": 0,
			"ReasonID": null,
			"Reason": null,
			"PercentFee": 0.0,
			"FixedFee": 0
		}
	]
}
```

Name              | Type          | Description/Value
------------------|---------------|------------------
MsgType           |               | "U19" Deposit response 
DepositReqID      |               | Deposit Request ID
Currency          |               | Currency
BrokerID          |               | Exchange ID
DepositMethodID   |               | Deposit Method ID
DepositMethodName |               | Deposit method name
DepositID         |               | Deposit ID
UserID            |               | Your account ID
ControlNumber     |               | Control number. Only used for FIAT deposits 
Type              |               | CRY = Crypto Currency 
Username          |               | Your username 
AccountID         |               | Account ID
Data              |               |               
Data.InputAddress |               | The address that you have to deposit               
Data.Destination  |               | This is the exchange wallet. DO NOT DEPOSIT IN THIS ADDRESS.              
ClOrdID           |               | Unique identifier for Order as assigned by you
Status            |               | 0 - New 
Created           |               | Creation date GMT
Value             |               | Amount
PaidValue         |               | Paid amount 
ReasonID          |               | Reason for the rejection - ID
Reason            |               | Reason for the rejection - Description
PercentFee        |               | Percent fee to process this deposit 
FixedFee          |               | Fixed fee in satoshis 


## Request FIAT deposit

__MESSAGE EXAMPLE__

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

Name            | Type   | Description/Value
----------------|--------|------------------
MsgType         | string | "U18" Deposit request.
DepositReqID    | number | Deposit Request ID. 
DepositMethodID | number | Deposit Method ID - Check with your exchange. 
Value000000,    | number | Amount in satoshis
Currency        | string | Currency. 
BrokerID        | number | Exchange ID.

__RESPONSE EXAMPLE__

```json
{
	"Status": 200, 
	"Description": "OK", 
	"Responses": [
		{
			"MsgType": "U19", 
			"DepositMethodName": "bb", 
			"UserID": 90800025, 
			"ControlNumber": 402000057, 
			"State": "UNCONFIRMED", 
			"Type": "DTP", 
			"AccountID": 90800025, 
			"Username": "user", 
			"CreditProvided": 0, 
			"DepositReqID": 6000745, 
			"DepositID": "a637e243382f4b768b5faccb97878ab3", 
			"Reason": None, 
			"PercentFee": 0.0, 
			"Data": {},
			"ClOrdID": None, 
			"Status": "0", 
			"Created": "2016-03-28 20:28:02", 
			"DepositMethodID": 403, 
			"Value": 150000000000, 
			"BrokerID": 4, 
			"PaidValue": 0, 
			"Currency": "BRL", 
			"ReasonID": None, 
			"FixedFee": 0
		}
	]
}
```

Name              | Type   | Description/Value
------------------|--------|------------------
MsgType           | string | "U19"
DepositMethodName | string | 
UserID            | number | 
ControlNumber     | number | 
State             | string | 
Type              | string | 
AccountID         | number | 
Username          | string | 
CreditProvided    | number | 
DepositReqID      | number | 
DepositID         | string | 
Reason            |        | 
PercentFee        | number | 
Data              | object | 
ClOrdID           |        | 
Status            | string | 
Created           | string | 
DepositMethodID   | number | 
Value             | number | 
BrokerID          | number | 
PaidValue         | number | 
Currency          | string | 
ReasonID          |        | 
FixedFee          | number | 


## Request Bitcoin Withdrawal

> __MESSAGE EXAMPLE__

```json
{
	"MsgType": "U6",
	"WithdrawReqID": 617625,
	"Method": "bitcoin",
	"Amount": 3000000,
	"Currency": "BTC",
	"Data": {
		"Wallet": "mwmabpJVisvti3WEP5vhFRtn3yqHRD9KNP"
	}
}
```
Paramenters

Params        | Type   | Description/Value
--------------|--------|------------------
MsgType       | string | "U6" Request ID
WithdrawReqID | number |
Method        | string | bitcoin for BTC. Check with the exchange all available withdrawal methods
Amount        | number | Amount in satoshis
Currency      | string | Currency code
Data          | object | Data object containing your wallet address e.g.: {"Data": { "Wallet": "mwmabpJVisvti3WEP5vhFRtn3yqHRD9KNP" }}

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
			"ClOrdID": None,
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

Name         | Type   | Description/Value
-------------|--------|------------------
\<BROKER_ID\>| object | The BrokerID containing your BTC and FIAT balance, e.g.: "5" stands for your balance with the BrokerID number 5.
MsgType      | string | "U3" Balance response. Problably because the request also change your account balance.
ClientID     | number | Your account ID.
BalanceReqID | number | This should match the BalanceReqID sent on the message "U2".


Name          | Type   | Description/Value
-----------------------|---------------|------------------
MsgType       | string | "U7" Withdrawal Response
WithdrawReqID | number | Withdraw Request ID
WithdrawID    | number | Withdraw ID. You should keep this number in case you want to cancel the Withdraw request.
UserID        | number | Your account ID.
Username      | string | Your username.
Method        | string | Bitcoin method.
Amount        | number | Requested Amount.
BrokerID      | number | Exchange ID.
ClOrdID       |        | Unique identifier for Order as assigned by you.
Created       | string | Creation date GMT.
Currency      | string | Currency code.
Data          | object | Data object containing your wallet address e.g.: {"Data": { "Wallet": "mwmabpJVisvti3WEP5vhFRtn3yqHRD9KNP" }}
FixedFee      | number | Fixed portion of the Fee in satoshis.
PercentFee    | number | Percent portion of the fee.
PaidAmount    | number | The amount + fees that was deducted from the account.
Reason        | string | Reason for the rejection - Description.
ReasonID      | string | Reason for the rejection - ID.
Status        | string | "0" = Unconfirmed (in this case, you must confirm withdrawal using a second factor of authentication), "1" = Pending, "2" = In Progress, "4" = Completed, "8" - Cancelled

## Request a FIAT Withdrawal

[TODO]

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

> __RESPONSE EXAMPLE__

```json
{}
```


## Request a List of Withdrawals

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

Name              | Type          | Description/Value
------------------|---------------|------------------
MsgType           | string        | "U26"
WithdrawListReqID | number        | An ID chosen by you.
Page              | number        | **Optional**; defaults to 0.
PageSize          | number        | **Optional**; defaults to 20.
StatusList        | array(string) | Array of strings where: "1" is Pending, "2" is In Progress, "4" is Completed, "8" is Cancelled. **Optional**; defaults to ["1"]. 

> __RESPONSE EXAMPLE__

```json
{
	"Status": 200,
	"Description": "OK",
	"Responses": [
		{
			"MsgType": "U27",
			"WithdrawListReqID": 1,
			"Page": 0,
			"PageSize": 100,
			"Columns": [
				"WithdrawID",
				"Method",
				"Currency",
				"Amount",
				"Data",
				"Created",
				"Status",
				"ReasonID",
				"Reason",
				"PercentFee",
				"FixedFee",
				"PaidAmount",
				"UserID",
				"Username",
				"BrokerID",
				"ClOrdID"
			],
			"WithdrawListGrp": [
				[
					434,
					"bitcoin",
					"BTC",
					4800000000,
					{
						"Wallet": "mnbsvx4L1ZnvKehUB1mt4tmSHFJg2KUDU9",
						"Fees": "\u0e3f 0.00010000",
						"Instant": "NO",
						"Currency": "BTC"
					},
					"2016-06-08 17:03:02",
					"8",
					-1,
					null,
					0.0,
					10000,
					4800010000,
					90000002,
					"user",
					5,
					null
				]
			]
		}
	]
}
```


# WebSocket API

With the WebSocket API, you have full access to the exchange, you can also check our JavaScript WebSocket implementation [here](https://github.com/blinktrade/frontend/blob/master/jsdev/bitex/api/bitex.js#L760-L1784)

## Connectivity

### Heartbeat

## Authentication

### Login
### Sign Up
### Two factor Authentication
### Change password

## Brokers

## Profile

## Balances

## Market Data

## Security Status

## Orders

### Request Orders List

  Resquests orders List...

### Send Order

  To Send a Order

### Cancel Order

  Cancel a Order


## Ledger

## Deposits

### Instant Deposits

## Withdraws


# References

* [pinhopro/blinktrade_example_restapi.py](https://gist.github.com/pinhopro/60b1fd213b36d576505e)
* [FIX 4.4 - FIX Dictionary - Onix Solutions](http://www.onixs.biz/fix-dictionary/4.4/index.html)
