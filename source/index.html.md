---
title: BlinkTrade API

language_tabs:
  - javascript: JavaScript
  - shell: Shell

includes:
  - restapi
  - tradeapi
  - websocket

search: true
---

# Getting Started

### Overview

Welcome to [BlinkTrade](https://blinktrade.com) API documentation!

BlinkTrade provides a simple and robust [WebSocket API](#websocket-api) to integrate our platform, we strongly recommend you to use it over the [REST API](#public-rest-api).

## BlinkTrade SDK

We provide a simple WebSocket and REST JavaScript SDK that enables you runs in either Node.js or in a browser. You can easily send and cancel orders, request deposits and withdrawals, and get real time market data through our WebSocket API.

All SDK supports either promises and callbacks. If a callback is provided as the last argument, it will be called as `callback(error, result)`, otherwise it will just return the original promise. We also provide event emitters that you can use to get realtime updates through our WebSocket API.

### Install

`$ npm install blinktrade`

```javascript
var BlinkTrade = require('blinktrade');

var BlinkTradeRest = BlinkTrade.BlinkTradeRest;
var BlinkTradeWS = BlinkTrade.BlinkTradeWS;

// REST Transport
var blinktrade = new BlinkTradeRest();

// WebSocket Transport
var blinktrade = new BlinkTradeWS();

blinktrade.heartbeat().then(function(heartbeat) {
  console.log(heartbeat.Latency);
});

```

[Check our JavaScript SDK documentation](https://github.com/blinktrade/BlinkTradeJS)

<aside class="notice">
  <b>NOTE</b>
  <p>
    We aimed to design a concise API by returning a JSON format that can be slightly different from the original WebSocket call. These returns aims to increase readability and avoid complexity of the JSON, some returns such arrays of arrays and a `Columns` field that describes each array position, are already formatted for you.
  </p>
</aside>

## BlinkTrade Endpoints

There are two working environments: `prod` for production purporses and `testnet` for testing purporses.

Some features are accessed publicly and others require an API Key based authentication. All data messages and responses are in JSON format.

### REST

* `testnet` base URL endpoint is `https://api.testnet.blinktrade.com`
* `prod` base URL endpoint is `https://api.blinktrade.com`

### WebSocket

* `testnet` URL endpoint is `wss://api.testnet.blinktrade.com/trade/`
* `prod` URL endpoint is `wss://ws.blinktrade.com/trade/`

## Create API Key

1. Go to some exchange powered by BlinkTrade or `https://testnet.blinktrade.com` for `testnet` environment
2. Signup
3. Go to API page
4. Click "New API Key"
4. Enter a label and select the permissions for that API Key
5. Get the API Key and API Secret. The API Secret will only be shown once. (The API Password is only used for the WebSocket API.)

## Brokers

A broker ID is assigned for each exchange powered by BlinkTrade:

| `<BROKER_ID>` |  Description
|:-------------:|-----------------------------------------------
|       1       | [SurBitcoin](https://surbitcoin.com)
|       3       | [VBTC](https://vbtc.vn)
|       4       | [FoxBit](https://foxbit.com.br)
|     **5**     | [**Testnet**](https://testnet.blinktrade.com/)
|       8       | [UrduBit](https://urdubit.com/)
|       9       | [ChileBit](https://chilebit.net)


## Currencies

Currency codes and related brokers:

`<CURRENCY>` | Description
-------------|------------
VEF          | Venezuelan Bolivares (SurBitcoin)
VND          | Vietnamise Dongs (VBTC)
BRL          | Brazil Reals (FoxBit)
PKR          | Pakistani Ruppe (UrduBit)
CLP          | Chilean Pesos (ChileBit.NET)

## Symbols

Currency pair symbols and related brokers:

`<SYMBOL>` | Description
-----------|------------
BTCVEF     | BTC Pair - Venezuelan Bolivares (SurBitcoin)
BTCVND     | BTC Pair - Vietnamise Dongs (VBTC)
BTCBRL     | BTC Pair - Brazil Reals (FoxBit)
BTCPKR     | BTC Pair - Pakistani Ruppe (UrduBit)
BTCCLP     | BTC Pair - Chilean Pesos (ChileBit.NET)
