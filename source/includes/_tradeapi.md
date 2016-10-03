# Trade API

On our RESTful API, we provide a trade endpoint that you're allowed to send and cancel orders,
request deposits and withdrawals. You need to [create an API Key](#create-api-key) through our platform and set their respective permission that gives you access to it.

The Trade endpoint is internaly a bridge to our WebSocket API, so you can access it both on `REST` and `WebSocket API`.
On `REST` it can be accessed under `/tapi/v1/message`, e.g, production is `https://api.blinktrade.com/tapi/v1/message`,
and API Key is needed in order to authenticate your access.

<aside class="warning">
  <b>NOTE</b>
  <p>Be aware that our Restful trade endpoint can be changed at any time, we strongly recommend using the WebSocket API over the Restful API.</p>
</aside>

<aside class="notice">
  <b>NOTE</b> that when generate the API Key and the API Secret, it will be only shown once, you should save it securely, the API Password is only used in the WebSocket API
</aside>

An `HTTP POST` request method should be used to send a RESTful HTTP message.

### REST HTTP Headers

The following headers must be present in your `POST` message:

Header       | Description
-------------|------------
APIKey       | Your API Key generated from your exchange or testnet environment
Nonce        | Must be an integer, always greater than the previous one
Signature    | The HMAC-SHA256 signature of Nonce using your API Secret as key
Content-Type | `application/json`


```javascript

// REST Transport
var BlinkTradeRest = require("blinktrade").BlinkTradeRest;
var blinktrade = new BlinkTradeRest({
  prod: false,
  key: "YOUR_API_KEY_GENERATED_IN_API_MODULE",
  secret: "YOUR_SECRET_KEY_GENERATED_IN_API_MODULE",
  currency: "BRL",
});

// WebSocket Transport
var BlinkTradeWS = require("blinktrade").BlinkTradeWS;
var blinktrade = new BlinkTradeWS({ prod: false });

```

```shell
# Message to be sent
message='JSON_MESSAGE_TO_BE_SENT'

# REST Trade API URL
api_url='API_URL_REST_ENDPOINT'

# Set API Key and Secret
api_key='YOUR_API_KEY_GENERATED_IN_API_MODULE'
api_secret='YOUR_SECRET_KEY_GENERATED_IN_API_MODULE'

# Generate a nonce
nonce=`date +%s`

# Sign the nonce with secret using HMAC-SHA256
signature=`echo -n "$nonce" | openssl dgst -sha256 -hex -hmac "$api_secret" | cut -d ' ' -f 2`

# Send a POST message to API URL
curl -X POST "$api_url"              \
  -H "APIKey:$api_key"               \
  -H "Nonce:$nonce"                  \
  -H "Signature:$signature"          \
  -H "Content-Type:application/json" \
  -d "$message"
```

## Balance

> __MESSAGE EXAMPLE__

```json
{
	"MsgType": "U2",
	"BalanceReqID": 1
}
```

```javascript

blinktrade.balance().then(function(balance) {
  console.log(balance);
});

```

```shell
message='{ "MsgType": "U2", "BalanceReqID": 1 }'

api_url='API_URL_REST_ENDPOINT'
api_key='YOUR_API_KEY_GENERATED_IN_API_MODULE'
api_secret='YOUR_SECRET_KEY_GENERATED_IN_API_MODULE'

nonce=`date +%s`
signature=`echo -n "$nonce" | openssl dgst -sha256 -hex -hmac "$api_secret" | cut -d ' ' -f 2`

curl -X POST "$api_url"              \
  -H "APIKey:$api_key"               \
  -H "Nonce:$nonce"                  \
  -H "Signature:$signature"          \
  -H "Content-Type:application/json" \
  -d "$message"
```


### Parameters

Name         | Type   | Description/Value
-------------|--------|------------------
MsgType      | string | "U2"
BalanceReqID | number | An ID assigned by you. It can be any number. The response message associated with this request will contain the same ID

> __RESPONSE EXAMPLE__

```json
{
    "MsgType": "U3",
    "ClientID": 90800003,
    "BalanceReqID": 5178228,
    "Available": {
        "USD": 177814907002760,
        "BTC": 1468038442214
    },
    "5": {
        "BTC_locked": 0,
        "USD": 177911657052760,
        "BTC": 1468038442214,
        "USD_locked": 96750050000
    }
}

```

### Response

Returns your balance for each broker.

Name         | Type    | Description/Value
-------------|---------|------------------
MsgType      | string  | "U3" UserBalanceResponse message
[\<BROKER_ID\>](#brokers)| object  | The [Broker ID](#brokers) containing your BTC and FIAT balance, e.g.: "5" stands for your balance with the [Broker ID](#brokers) number 5
ClientID     | number  | Your account ID
BalanceReqID | number  | This should match the BalanceReqID sent on the message "U2"
Available    | object  | Available balance only returned on JavaScript SDK

Balance model example for BTC and USD:

Name         | Type   | Description
-------------|--------|------------
BTC          | number | Amount in satoshis of BTC you have available in your account
USD          | number | Amount in USD (or your FIAT currency) you have available in your account
BTC_locked   | number | Amount in satoshis of BTC you have locked (open orders, margin positions, etc)
USD_locked   | number | Amount in USD (or your FIAT currency) you have locked (open orders, margin positions, etc)


## My Orders

Request a list of your open orders.

> __MESSAGE EXAMPLE__

```json
{
  "MsgType": "U4",
  "OrdersReqID": 930019,
  "Page": 0,
  "PageSize": 1
}
```

```javascript

blinktrade.myOrders().then(function(myOrders) {
  console.log(myOrders);
});

```

```shell
message='{ "MsgType": "U4", "OrdersReqID": 930019, "Page": 0, "PageSize": 1 }'

api_url='API_URL_REST_ENDPOINT'
api_key='YOUR_API_KEY_GENERATED_IN_API_MODULE'
api_secret='YOUR_SECRET_KEY_GENERATED_IN_API_MODULE'

nonce=`date +%s`
signature=`echo -n "$nonce" | openssl dgst -sha256 -hex -hmac "$api_secret" | cut -d ' ' -f 2`

curl -X POST "$api_url"              \
  -H "APIKey:$api_key"               \
  -H "Nonce:$nonce"                  \
  -H "Signature:$signature"          \
  -H "Content-Type:application/json" \
  -d "$message"
```


### Parameters

Name        | Type          | Description/Value
------------|---------------|------------------
MsgType     | string        | "U4"
OrdersReqID | number        | An ID assigned by you. It can be any number. The response message associated with this request will contain the same ID
Page        | number        | **Optional**; defaults to 0
PageSize    | number        | **Optional**; defaults to 20

> __RESPONSE EXAMPLE__

```json
{
    "OrdListGrp": [{
        "ClOrdID": "8475400",
        "OrderID": 1459028830968,
        "CumQty": 0,
        "OrdStatus": "0",
        "LeavesQty": 5000000,
        "CxlQty": 0,
        "AvgPx": 0,
        "Symbol": "BTCUSD",
        "Side": "1",
        "OrdType": "2",
        "OrderQty": 5000000,
        "Price": 50001000000,
        "OrderDate": "2016-09-07 04:35:30",
        "Volume": 0,
        "TimeInForce": "1"
    }],
    "PageSize": 1,
    "OrdersReqID": 930019,
    "MsgType": "U5",
    "Page": 0
}

```

### Response

Returns an array of Orders Model Objects.

Name        | Type   | Description/Value
------------|--------|------------------
ClOrdID     | string | Client order ID set by you
OrderID     | number | Order ID set by blinktrade
CumQty      | number | The executed quantity of this order
OrdStatus   | string | "0" = New, "1" = Partially filled, "2" = Filled, "4" = Cancelled, "8" = Rejected, "A" = Pending New
LeavesQty   | number | Quantity open for further execution
CxlQty      | number | Total quantity canceled for this order
AvgPx       | number | Calculated average price of all fills on this order
Symbol      | string | [\<SYMBOL\>](#symbols)
Side        | string | "1" = Buy, "2" = Sell
OrdType     | string | "2" = Limited
OrderQty    | number | Quantity ordered in satoshis
Price       | number | Price per unit in your local currency
OrderDate   | string | Order date in UTC
Volume      | number | Quantity x Price
TimeInForce | string | "0" = Day, "1" = Good Till Cancel, "4" = Fill or Kill


## Send Order

<aside class="notice">
<b><a href="http://floating-point-gui.de/basic/">Floats are Evil!</a></b>
<p>
  Converting Floats to Integers can be dangerous. Different programming languages can get weird rounding errors and imprecisions, so all API returns prices and bitcoin values as Integers and in "satoshis" format. We also expect Integers as input, make sure that you're formatting the values properly to avoid precision issues.
</p>
</aside>

> __MESSAGE EXAMPLE__

```json
{
	"MsgType": "D",
	"ClOrdID": 8426208,
	"Symbol": "BTCUSD",
	"Side": "1",
	"OrdType": "2",
	"Price": 55000000000,
	"OrderQty": 5000000,
	"BrokerID": 5
}
```

```javascript

blinktrade.sendOrder({
  "side": "1", // Buy
  "price": parseInt((550 * 1e8).toFixed(0)),
  "amount": parseInt((0.05 * 1e8).toFixed(0)),
  "symbol": "BTCUSD",
}).then(function(order) {
  console.log(order);
});

```

```shell
message='{ "MsgType": "D", "ClOrdID": 8426208, "Symbol": "BTCUSD", "Side": "1", "OrdType": "2", "Price": 55000000000, "OrderQty": 5000000, "BrokerID": 5 }'

api_url='API_URL_REST_ENDPOINT'
api_key='YOUR_API_KEY_GENERATED_IN_API_MODULE'
api_secret='YOUR_SECRET_KEY_GENERATED_IN_API_MODULE'

nonce=`date +%s`
signature=`echo -n "$nonce" | openssl dgst -sha256 -hex -hmac "$api_secret" | cut -d ' ' -f 2`

curl -X POST "$api_url"              \
  -H "APIKey:$api_key"               \
  -H "Nonce:$nonce"                  \
  -H "Signature:$signature"          \
  -H "Content-Type:application/json" \
  -d "$message"
```

### Parameters

Name     | Type   | Description/Value
---------|--------|------------------
MsgType  | string | "D"
ClOrdID  | number | Unique identifier for Order as assigned by you
Symbol   | string | [\<SYMBOL\>](#symbols)
Side     | string | "1" = Buy, "2" = Sell
OrdType  | string | "2" = Limited
Price    | number | Price in satoshis
OrderQty | number | Quantity in satoshis
BrokerID | number | [\<BROKER_ID\>](#brokers)

### Response

Returns an Execution Report Model Object, if you're using with `REST Transport`,
it will response as an array together with the balance response.

> __RESPONSE EXAMPLE__


```json

{
  "OrderID": 1459028830972,
  "ExecID": 741322,
  "ExecType": "0",
  "OrdStatus": "0",
  "CumQty": 0,
  "Symbol": "BTCUSD",
  "OrderQty": 5000000,
  "LastShares": 0,
  "LastPx": 0,
  "Price": 55000000000,
  "TimeInForce": "1",
  "LeavesQty": 5000000,
  "MsgType": "8",
  "ExecSide": "1",
  "OrdType": "2",
  "CxlQty": 0,
  "Side": "1",
  "ClOrdID": 8426208,
  "AvgPx": 0
}

```

### Execution Report Response

field       | Type   | Description/Value
------------|--------|------------------
MsgType     | string | "8"
OrderID     | number | Unique identifier for Order as assigned by broker
ExecID      | number | Unique identifier of execution message as assigned by broker
ExecType    | string | "0" = New, "1" = Partially fill, "2" = Fill, "4" = Cancelled, "8" = Rejected, "A" = Pending New
OrdStatus   | string | "0" = New, "1" = Partially fill, "2 "= Fill, "4" = Cancelled, "8" = Rejected, "A" = Pending New
LeavesQty   | number | Quantity open for further execution
Symbol      | string | Currency pair being used
OrderQty    | number | Quantity ordered in satoshis
LastShares  | number | Quantity of shares bought/sold on this fill
LastPx      | number | Price of the last fill
CxlQty      | number | Total quantity canceled for this order
TimeInForce | string | "0" = Day, "1" = Good Till Cancel, "4" = Fill or Kill
CumQty      | number | Total quantity filled
ClOrdID     | string | Unique identifier for Order as assigned by you
OrdType     | string | "2" = Limited
Side        | string | "1" = Buy, "2" = Sell
Price       | number | Price per unit of quantity in satoshis
ExecSide    | string | Side of this fill
AvgPx       | number | Calculated average price of all fills on this order

## Cancel Order

> __MESSAGE EXAMPLE__

```json
{
    "MsgType": "F",
    "OrderID": 1459028830899,
    "ClOrdID": 8426208
}
```

```javascript

blinktrade.cancelOrder({ orderID: order.OrderID, clientId: order.ClOrdID }).then(function(order) {
  console.log("Order Cancelled");
});

```

```shell
message='{ "MsgType": "F", "OrderID": 1459028830899, "ClOrdID": 8426208 }'

api_url='API_URL_REST_ENDPOINT'
api_key='YOUR_API_KEY_GENERATED_IN_API_MODULE'
api_secret='YOUR_SECRET_KEY_GENERATED_IN_API_MODULE'

nonce=`date +%s`
signature=`echo -n "$nonce" | openssl dgst -sha256 -hex -hmac "$api_secret" | cut -d ' ' -f 2`

curl -X POST "$api_url"              \
  -H "APIKey:$api_key"               \
  -H "Nonce:$nonce"                  \
  -H "Signature:$signature"          \
  -H "Content-Type:application/json" \
  -d "$message"
```

### Parameters

Name    | Type   | Description/Value
--------|--------|------------------
MsgType | string | "F" Order Cancel Request message. Check for a full doc here: [http://www.onixs.biz/fix-dictionary/4.4/msgType_F_70.html](http://www.onixs.biz/fix-dictionary/4.4/msgType_F_70.html).
ClOrdID | number | ID for an Order as assigned by you.

### Response

The response will be the same as the `sendOrder` with `ExecType`: "4"

## Deposits

### Request Deposit List

> __MESSAGE EXAMPLE__

```json

{
    "MsgType": "U30",
    "DepositListReqID": 123,
    "Page": 0,
    "PageSize": 1,
    "StatusList": ["1", "2", "4", "8"]
}

```

```javascript

blinktrade.requestDepositList().then(function(deposits) {
  console.log(deposits);
});

```

```shell
message='{ "MsgType": "U30", "DepositListReqID": 7739992, "Page": 0, "PageSize": 1 }'

api_url='API_URL_REST_ENDPOINT'
api_key='YOUR_API_KEY_GENERATED_IN_API_MODULE'
api_secret='YOUR_SECRET_KEY_GENERATED_IN_API_MODULE'

nonce=`date +%s`
signature=`echo -n "$nonce" | openssl dgst -sha256 -hex -hmac "$api_secret" | cut -d ' ' -f 2`

curl -X POST "$api_url"              \
  -H "APIKey:$api_key"               \
  -H "Nonce:$nonce"                  \
  -H "Signature:$signature"          \
  -H "Content-Type:application/json" \
  -d "$message"
```


Name              | Type          | Description/Value
------------------|---------------|------------------
MsgType           | string        | "U30"
DepositListReqID  | number        | Request ID
Page              | number        | **Optional**; defaults to 0.
PageSize          | number        | **Optional**; defaults to 20.
StatusList        | array(string) | "0" = Unconfirmed, "1" = Pending, "2" = In Progress, "4" = Completed, "8" = Cancelled

> __EXAMPLE RESPONSE__

```json

{
    "PageSize": 1,
    "DepositListReqID": 7739992,
    "MsgType": "U31",
    "DepositListGrp": [{
        "DepositID": "8312c0f951ef44a393050dc399fb8698",
        "DepositMethodID": 502,
        "DepositMethodName": "wire_transfer_usa",
        "Type": "DTP",
        "Currency": "USD",
        "Value": 20000000000,
        "PaidValue": 0,
        "Data": {},
        "Created": "2016-09-07 14:33:22",
        "ControlNumber": 502000174,
        "PercentFee": 1,
        "FixedFee": 0,
        "Status": "0",
        "ReasonID": null,
        "Reason": null,
        "Username": "rodrigo",
        "UserID": 90800003,
        "BrokerID": 5,
        "ClOrdID": "6351214",
        "CreditProvided": 0,
        "State": "UNCONFIRMED"
    }],
    "Page": 0
}


```


### Response

Returns an array of Deposits Model Object.

### Request Deposit

> __MESSAGE EXAMPLE__

```json
{
  "MsgType": "U18",
  "DepositReqID": 3115044,
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
message='{ "MsgType": "U18", "DepositReqID": 3115044, "DepositMethodID": 403, "Value": 150000000000, "Currency": "BRL", "BrokerID": 5 }'

api_url='API_URL_REST_ENDPOINT'
api_key='YOUR_API_KEY_GENERATED_IN_API_MODULE'
api_secret='YOUR_SECRET_KEY_GENERATED_IN_API_MODULE'

nonce=`date +%s`
signature=`echo -n "$nonce" | openssl dgst -sha256 -hex -hmac "$api_secret" | cut -d ' ' -f 2`

curl -X POST "$api_url"              \
  -H "APIKey:$api_key"               \
  -H "Nonce:$nonce"                  \
  -H "Signature:$signature"          \
  -H "Content-Type:application/json" \
  -d "$message"
```

### Parameters

Name            | Type   | Description/Value
----------------|--------|------------------
MsgType         | string | "U18"
DepositReqID    | number | Deposit Request ID
DepositMethodID | number | Deposit Method ID - Check with your exchange
Value           | number | Amount in satoshis
Currency        | string | [\<CURRENCY\>](#currencies)
BrokerID        | number | [\<BROKER_ID\>](#brokers)

> __EXAMPLE RESPONSE__

```json

{
    "DepositMethodName": "deposit_btc",
    "UserID": 90800003,
    "ControlNumber": null,
    "State": "UNCONFIRMED",
    "Type": "CRY",
    "PercentFee": 0,
    "Username": "rodrigo",
    "CreditProvided": 0,
    "DepositReqID": 3115044,
    "DepositID": "937a0c5e2133400280a356e5902eb6bd",
    "Reason": null,
    "AccountID": 90800003,
    "Data": {
        "InputAddress": "mvehwFpiMjhGL5wVSoX3QBSQmeVRdGFTSC",
        "Destination": "n19ZAH1WGoUkQhubQw71fH11BenifxpBxf"
    },
    "ClOrdID": "3115044",
    "Status": "0",
    "Created": "2016-09-07 14:33:22",
    "DepositMethodID": null,
    "Value": 0,
    "BrokerID": 5,
    "PaidValue": 0,
    "Currency": "BTC",
    "ReasonID": null,
    "MsgType": "U19",
    "FixedFee": 0
}

```

### Response

Returns a Deposit Model Object.

<aside class="notice">
  <b>NOTE</b>
  <p>The `Data.InputAddress` is the address that you have to deposit, <b>DO NOT DEPOSIT</b> on `Data.Destination` address.</p>
</aside>

## Withdrawals

### Request Withdrawal List

> __MESSAGE EXAMPLE__

```json
{
  "MsgType": "U26",
  "WithdrawListReqID": 1,
  "Page": 0,
  "PageSize": 1,
  "StatusList": ["1", "2"]
}
```

```javascript

blinktrade.requestWithdrawList().then(function(withdraws) {
  console.log(withdraws);
});

```

```shell
message='{ "MsgType": "U26", "WithdrawListReqID": 6695476, "Page": 0, "PageSize": 1 }'

api_url='API_URL_REST_ENDPOINT'
api_key='YOUR_API_KEY_GENERATED_IN_API_MODULE'
api_secret='YOUR_SECRET_KEY_GENERATED_IN_API_MODULE'

nonce=`date +%s`
signature=`echo -n "$nonce" | openssl dgst -sha256 -hex -hmac "$api_secret" | cut -d ' ' -f 2`

curl -X POST "$api_url"              \
  -H "APIKey:$api_key"               \
  -H "Nonce:$nonce"                  \
  -H "Signature:$signature"          \
  -H "Content-Type:application/json" \
  -d "$message"
```

### Parameters

Name              | Type          | Description/Value
------------------|---------------|------------------
MsgType           | string        | "U26"
WithdrawListReqID | number        | An ID chosen by you
Page              | number        | **Optional**; defaults to 0
PageSize          | number        | **Optional**; defaults to 20
StatusList        | array(string) | "0" = Unconfirmed, "1" = Pending, "2" = In Progress, "4" = Completed, "8" = Cancelled

> __RESPONSE EXAMPLE__

```json

{
    "WithdrawListReqID": 6695476,
    "PageSize": 1,
    "WithdrawListGrp": [{
        "WithdrawID": 365,
        "Method": "bitcoin",
        "Currency": "BTC",
        "Amount": 100000,
        "Data": {
            "Wallet": "2Mx3TZycg4XL5sQFfERBgNmg9Ma7uxowK9y",
            "Instant": "NO",
            "Fees": "฿ 0.00010000"
        },
        "Created": "2016-05-03 01:15:45",
        "Status": "8",
        "ReasonID": null,
        "Reason": null,
        "PercentFee": 0,
        "FixedFee": 10000,
        "PaidAmount": 110000,
        "UserID": 90800003,
        "Username": "rodrigo",
        "BrokerID": 5,
        "ClOrdID": null
    }],
    "MsgType": "U27",
    "Page": 0
}

```
### Response

Returns an array of Withdrawals Model Objects.

### Request Withdrawal

To request withdrawals, you need to pass a "data" information, which represents the information to your withdrawal. It's related to bank accounts, numbers, or a bitcoin address, this information is dynamically and is different for each broker.

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

blinktrade.requestWithdraw({
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
Data          | object | Data object containing the withdraws required fields

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
    "Username": "rodrigo",
    "Status": "1",
    "SecondFactorType": "",
    "Created": "2016-09-07 14:52:23",
    "PaidAmount": 20000000000,
    "UserID": 90800003,
    "Reason": null,
    "Currency": "USD",
    "Amount": 20000000000,
    "ReasonID": null,
    "BrokerID": 5,
    "ClOrdID": "3184990",
    "WithdrawID": 545,
    "WithdrawReqID": 3184990,
    "MsgType": "U7",
    "Data": {
        "AccountBranch": "111",
        "AccountNumber": "4444-5",
        "AccountType": "corrente",
        "CPF_CNPJ": "00000000000"
    },
    "Method": "PayPal",
    "FixedFee": 0,
    "PercentFee": 0
}

```

### Response

Returns a Balance and Withdraw Model Object.
