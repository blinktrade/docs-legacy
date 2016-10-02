# Public Rest API

### Overview

```javascript

var BlinkTradeRest = require("blinktrade").BlinkTradeRest;
var BlinkTrade = new BlinkTradeRest({
  prod: false,
  currency: "BRL",
});

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

$ curl "https://api.blinktrade.com/api/v1/BRL/ticker"

```

### Parameters

Name            | Description
----------------|------------
crypto_currency | Crypto currency to be used. **Optional**; defaults to BTC

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
last             | number | Value of the last price in the last 24 hours
high             | number | Price of the highest price in the last 24 hours
low              | number | Price of the lowest price in the last 24 hours
vol              | number | Trading volume in the last 24 hours
vol_[\<CURRENCY\>](#currencies) | number | Trading volume in the last 24 hours in [\<CURRENCY\>](#currencies) (lowercase)
buy              | number | Price of the most recent buy order
sell             | number | Price of the most recent sell order


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

$ curl "https://api.blinktrade.com/api/v1/BRL/orderbook"

```

### Parameters

Name            | Description
----------------|------------
crypto_currency | Crypto currency to be used. **Optional**; defaults to BTC

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
bids       | array(array)  | Array of bids from buyers
asks       | array(array)  | Array of asks from sellers

For each element of `bids` or `asks` array:

Index Array | Type    | Description
:----------:|---------|------------
0           | number  | Unit price
1           | number  | Amount to buy/sell
2           | number  | User ID

## Trades

A list of the last trades executed on an exchange since a chosen date.

### HTTP Request

`GET /api/v1/<CURRENCY>/trades?crypto_currency=BTC&since=<TIMESTAMP>&limit=<NUMBER>`

```javascript

BlinkTrade.trades({ limit: 2 }).then(function(trades) {
  console.log(trades);
});

```

```shell

$ curl "https://api.blinktrade.com/api/v1/BRL/trades?since=1467990302&limit=2"

```

### Parameters

Name            | Description
----------------|------------
crypto_currency | Crypto currency to be used. **Optional**; defaults to BTC
since           | Date which executed trades must be fetched from. `<TIMESTAMP>` is in Unix Time date format. **Optional**; defaults to the date of the first executed trade
limit           | Limit of trades that will be returned. `<NUMBER>` should be a positive integer. **Optional**; defaults to 100 trades

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
tid    | number | Trade ID
date   | number | Executed date in Unix Time
price  | number | Unit price
amount | number | Amount bought/sold
side   | string | "buy" for a buy order or "sell" for a sell order
