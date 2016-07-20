# WebSocket API

With the WebSocket API, you have full access to the exchange, you can also check our JavaScript WebSocket implementation [here](https://github.com/blinktrade/frontend/blob/master/jsdev/bitex/api/bitex.js#L760-L1784).

## Connectivity

### Heartbeat

> __EXAMPLE MESSAGE__

```json
{
    "MsgType": "1",
    "TestReqID": "123",
    "SendTime": 1469031953
}
```

### Paratemers

Name      | Type   | Description
----------|--------|------------
MsgType   | string | [1](http://www.onixs.biz/fix-dictionary/4.4/msgType_1_1.html)
[TestReqID](http://www.onixs.biz/fix-dictionary/4.4/tagNum_112.html) | string | An ID assigned by you. It can be any number. The response message associated with this request will contain the same ID.
SendTime  | number | Unix Time Stamp.


> __EXAMPLE RESPONSE__

```json
{
    "MsgType": "0",
    "TestReqID": "123",
    "SendTime": 1469031953,
    "ServerTimestamp": 1469032804
}
```

### Response

Name            | Type   | Description
----------------|--------|------------
MsgType         | string | [0](http://www.onixs.biz/fix-dictionary/4.4/msgType_0_0.html)
[TestReqID](http://www.onixs.biz/fix-dictionary/4.4/tagNum_112.html) | string | This should match the TestReqID sent on the message.
SendTime        | number | This should match your SendTime.
ServerTimestamp | number | Server Unix Time Stamp.

## Authentication

Most of WebSocket calls requires authentication, once you login with your username and password on the WebSocket connection, you will be able to access your account. You can also login with your API Key and API Password, you only be allowed to send messages that your API Key has permission to send.

### FingerPrint

Each request requires that you pass a fingerprint from your browser, there's a few finger prints implementations that you can use.

* [Our google closure implementation](https://github.com/blinktrade/frontend/blob/master/jsdev/bitex/util/util.js#L96-L157)
* [fingerprintjs2](https://github.com/Valve/fingerprintjs2)

### Login

> __EXAMPLE MESSAGE__

```json
{
    "MsgType": "BE",
    "UserReqID": 1123107,
    "BrokerID": 5,
    "Username": "TYD3ZilY0FDB1QdkD1iDdlqvcRdnV0sZILNh3D18ZKs",
    "Password": "ySIMrLdTqSDo3vL",
    "UserReqTyp": "1",
    "FingerPrint": "b959a35c7f3f5e9315c99b5a25c2bbda"
}
```

### Parameters

Name                    | Type | Description
------------------------|------|------------
MsgType                 | string | [BE](http://www.onixs.biz/fix-dictionary/4.4/msgType_BE_6669.html)
UserReqID               | number | Request Id
BrokerID                | number | [\<BROKER_ID\>](#brokers)
Username                | string | The email address, username or API Key of the user.
Password                | string | The password of the user.
UserReqTyp              | string | "1"
UserAgent               | string | Browser user agent navigator. **Optional**.
UserAgentLanguage       | string | User agent language. **Optional**.
UserAgentTimezoneOffset | number | User agent timezone offset. **Optional**.
UserAgentPlatform       | string | User agent platform. **Optional**.
FingerPrint             | string | Browser [fingerprint](#fingerprint).

### Response

> __EXAMPLE RESPONSE__

```json
{
    "UserID": 90800127,
    "TwoFactorEnabled": false,
    "EmailLang": "en",
    "Username": "cesaraugusto",
    "IsMSB": true,
    "WithdrawFixedFee": null,
    "Broker": {
      // Broker Model
    },
    "HasLineOfCredit": false,
    "UserStatus": 1,
    "IsBroker": false,
    "Profile": {
        "VerificationData": [{
            "177.97.245.78:1513288343": ["cesaraugusto"]
        }],
        "Verified": 3,
        "UserID": 90800127,
        "TwoFactorEnabled": false,
        "EmailLang": "en",
        "EmailTwoFactorEnabled": false,
        "Type": "USER",
        "Email": "cesardeazevedo@outlook.com",
        "Username": "cesaraugusto",
        "IsMSB": true,
        "CountryCode": "US",
        "WithdrawFixedFee": null,
        "HasLineOfCredit": false,
        "TakerTransactionFeeSell": 500,
        "ConfirmationOrder": false,
        "Country": "US",
        "TakerTransactionFeeBuy": 500,
        "IsMarketMaker": false,
        "ID": 90800127,
        "DepositPercentFee": null,
        "DepositFixedFee": null,
        "WithdrawPercentFee": null,
        "TransactionFeeBuy": 500,
        "State": "NY",
        "TransactionFeeSell": 500,
        "NeedWithdrawEmail": false
    },
    "TakerTransactionFeeSell": 500,
    "ConfirmationOrder": false,
    "TakerTransactionFeeBuy": 500,
    "UserReqID": 1123107,
    "MsgType": "BF",
    "IsMarketMaker": false,
    "DepositPercentFee": null,
    "DepositFixedFee": null,
    "WithdrawPercentFee": null,
    "TransactionFeeBuy": 500,
    "TransactionFeeSell": 500,
    "EmailTwoFactorEnabled": false,
    "BrokerID": 5,
    "PermissionList": {
        "*": []
    }
}
```

Parameter               | Type   | Description 
------------------------|--------|-------------
MsgType                 | string | [BF](http://www.onixs.biz/fix-dictionary/4.4/msgType_BF_6670.html) 
Broker                  | object | Broker model object.
BrokerID                | number | [\<BROKER_ID\>](#brokers)
DepositFixedFee         | number |
DepositPercentFee       | number |
EmailLang               | string | Email Language.
HasLineOfC3redit        | bool   | `bool` indicating whether user has line of credit.
IsBroker                | bool   | `bool` whether user is a broker.
IsMSB                   | bool   |
Profile                 | object | Profile model object.
TakerTransactionFeeBuy  | number |
TakerTransactionFeeSell | number |
TransactionFeeBuy       | number |
TransactionFeeSell      | number |
TwoFactorEnabled        | bool   | `bool` indicating whether user has two factor enabled .
UserID                  | number | Integer with the user ID 
UserStatus              | number | "1" = Logged In, "2" = Not Logged In 
Username                | string | String with the username of the user 
WithdrawFixedFee        | number |
WithdrawPercentFee      | number |


## Sign Up

> __MESSAGE EXAMPLE__

```json
{
    "MsgType": "U0",
    "UserReqID": 123,
    "Username": "mynewusername",
    "Password": "mynewpassword",
    "Email": "myemail@example.com",
    "State": "SP",
    "CountryCode": "BR",
    "BrokerID": 5
}
```

### Parameters

Name                    | Type   | Description 
------------------------|--------|-------------
MsgType                 | string | "U0",
UserReqID               | number | An ID assigned by you. It can be any number. The response message associated with this request will contain the same ID.
Username                | string | Username you want to use
Password                | string | Password you want use
Email                   | string | User email.
State                   | string | State
CountryCode             | string | ContryCode, e.g. "BR"
BrokerID                | number | [\<BROKER_ID\>](#brokers)


> __EXAMPLE RESPONSE__

```json
{
    "MsgType": "BF",
    "UserReqID": 123,
    "Username": "",
    "UserStatus": 3,
    "NeedSecondFactor": true,
    "UserStatusText": "MSG_SIGNUP_CONFIRM_EMAIL",
    "SecondFactorType": "EMAIL"
}
```

### Response

Name                    | Type   | Description 
------------------------|--------|-------------
MsgType                 | string | "BF"
UserReqID               | number | 123
Username                | string | 
UserStatus              | number | 3,
NeedSecondFactor        | bool   | true
UserStatusText          | string | 
SecondFactorType        | string | 


## Two factor Authentication

## Change password

In order to change your password, you do the same as a login request, but passing `UserReqTyp` as 3 and the new password on `Password`

### Parameters

Name         | Type   | Description
-------------|--------|------------
MsgType      | string | [BE](http://www.onixs.biz/fix-dictionary/4.4/msgType_BE_6669.html)
UserReqID    | number | Request ID
UserReqTyp   | number | Should be 3
BrokerID     | number | Current Logged [\<BROKER_ID\>](#brokers)
Username     | string | Username
Password     | string | New password
SecondFactor | number | Optional Second Factor Authentication

### Response

Returns should be the same as [Login](#login).

## Brokers

### Request Broker List

### Parameters

Name            | Type   | Description
----------------|--------|------------
MsgType         | string | "U28"
BrokerListReqID | number | Request ID
Page            | number | Page Index. 
PageSize        | number | Page Size. 
StatusList      |        | 

## Update Profile

Users can only change a few fields, such as `TwoFactorEnabled`, `EmailLang` and `ConfirmationOrder`

### Parameters

| Name        | Type   | Description                    |
|-------------|--------|--------------------------------|
| MsgType     | string | "U38"                          |
| UpdateReqID | string | Request ID                     |
| Fields      | object | Fields that you want to update |
| UserID      | number | Optional UserID                |

> EXAMPLE RESPONSE

```json
{
    "Username": "cesaraugusto",
    "WithdrawPercentFee": 0.0,
    "TakerTransactionFeeSell": 500,
    "IsMSB": true,
    "TransactionFeeBuy": 500,
    "UserID": 90800127,
    "TakerTransactionFeeBuy": 500,
    "TransactionFeeSell": 500,
    "EmailTwoFactorEnabled": false,
    "HasLineOfCredit": false,
    "TwoFactorEnabled": false,
    "Profile": {
        "VerificationData": [{
            "177.18.234.69:2048362583": ["cesaraugusto", "rodrigo"]
        }],
        "Verified": 3,
        "UserID": 90800127,
        "TwoFactorEnabled": false,
        "EmailLang": "en",
        "EmailTwoFactorEnabled": false,
        "Type": "USER",
        "Email": "cesardeazevedo@outlook.com",
        "Username": "cesaraugusto",
        "IsMSB": true,
        "CountryCode": "US",
        "WithdrawFixedFee": null,
        "HasLineOfCredit": false,
        "TakerTransactionFeeSell": 500,
        "ConfirmationOrder": false,
        "Country": "US",
        "TakerTransactionFeeBuy": 500,
        "IsMarketMaker": false,
        "ID": 90800127,
        "DepositPercentFee": null,
        "DepositFixedFee": null,
        "WithdrawPercentFee": null,
        "TransactionFeeBuy": 500,
        "State": "NY",
        "TransactionFeeSell": 500,
        "NeedWithdrawEmail": false
    },
    "MsgType": "U39",
    "IsMarketMaker": false,
    "WithdrawFixedFee": null,
    "BrokerID": 5,
    "IsBroker": false,
    "UpdateReqID": 367266,
    "DepositPercentFee": 0.0,
    "DepositFixedFee": null
}
```

### Response

| Name                    | Type   | Description                            |
|-------------------------|--------|----------------------------------------|
| MsgType                 | string | "U39"                                  |
| UpdateReqID             | number | Request ID                             |
| UserID                  | number | User ID                                |
| Username                | string | Username Profile                       |
| BrokerID                | number | Broker ID                              |
| IsMSB                   | number | Is MSB                                 |
| IsBroker                | bool   | User is broker                         |
| IsMarketMaker           | bool   | Is Market Maker                        |
| DepositFixedFee         | number | Deposit Fixed Fee                      |
| DepositPercentFee       | number | Deposit Percent Fee                    |
| WithdrawFixedFee        | number | Withdraw fixed fee                     |
| WithdrawPercentFee      | number | Withdraw percent fee                   |
| TakerTransactionFeeBuy  | number | Taker fee for buy orders               |
| TakerTransactionFeeSell | number | Taker fee for sell orders              |
| TransactionFeeSell      | number | Maker fee for sell orders              |
| TransactionFeeBuy       | number | Maker fee for buy orders               |
| TwoFactorEnabled        | bool   | Has two factor authentication          |
| EmailTwoFactorEnabled   | bool   | Has two factor authentication on email |
| HasLineOfCredit         | bool   | Has line of credit                     |
| Profile                 | object | Profile model object                   |

### Profile Response Model

| Name                    | Type   | Description                                |
|-------------------------|--------|--------------------------------------------|
| ID                      | number | User ID                                    |
| UserID                  | number | User ID                                    |
| Email                   | string | User Email                                 |
| Username                | string | Username profile                           |
| Type                    | string | User type                                  |
| IsMSB                   | bool   | Is MSB                                     |
| State                   | string | State Code e.g: "NY"                       |
| CountryCode             | string | Country Code                               |
| IsMarketMaker           | bool   | Is Market Maker                            |
| NeedWithdrawEmail       | bool   | Need confirmation on email to do withdraws |
| ConfirmationOrder       | bool   | Need alert confirmation to send orders     |
| HasLineOfCredit         | bool   | Has Line of Credit                         |
| TakerTransactionFeeBuy  | number | Taker fee for buy orders                   |
| TakerTransactionFeeSell | number | Taker fee for sell orders                  |
| DepositFixedFee         | number | Deposit Fixed Fee                          |
| DepositPercentFee       | number | Deposit Percent Fee                        |
| WithdrawFixedFee        | number | Withdraw Fixed Fee                         |
| WithdrawPercentFee      | number | Withdraw Percent Fee                       |
| TransactionFeeBuy       | number | Maker fee for buy orders                   |
| TransactionFeeSell      | number | Maker fee for sell orders                  |

## Balances

### Parameters

| Name         | Type   | Description       |
|--------------|--------|-------------------|
| MsgType      | string | U2                |
| BalanceReqID | string | Request ID        |
| ClientID     | object | Optional ClientID |

### Response

> EXAMPLE RESPONSE

``` json
{
    "MsgType": "U3",
    "5": {
        "BTC_locked": 505000000,
        "USD": 560047769123,
        "BTC": 2430523267,
        "USD_locked": 2472100000
    },
    "ClientID": 90800127,
    "BalanceReqID": 5821551
}
```

Returns your balance for each [BrokerID](#brokers).

Name         | Type    | Description/Value
-------------|---------|------------------
MsgType      | string  | "U3" UserBalanceResponse message.
[\<BROKER_ID\>](#brokers)| object  | The [BrokerID](#brokers) containing your BTC and FIAT balance, e.g.: "5" stands for your balance with the [BrokerID](#brokers) number 5.
ClientID     | number  | Your account ID.
BalanceReqID | number  | This should match the BalanceReqID sent on the message "U2".

Balance model example for BTC and USD

Name         | Type   | Description
-------------|--------|------------
BTC          | number | Amount in satoshis of BTC you have available in your account.
USD          | number | Amount in USD (or your FIAT currency) you have available in your account.
BTC_locked   | number | Amount in satoshis of BTC you have locked (open orders, margin positions, etc).
USD_locked   | number | Amount in USD (or your FIAT currency) you have locked (open orders, margin positions, etc).


## Market Data

### Subscribe Market Data

You can subscribe to one or more Market Data and receive one or more Market Data Entries in realtime,
each Market Data Entry is composed by a bid, offer or a trade occured.

### Parameters

| Name                    | Type   | Description                                                        |
|-------------------------|--------|--------------------------------------------------------------------|
| MsgType                 | string | "V"                                                                |
| MDReqID                 | number | Request ID                                                         |
| SubscriptionRequestType | string | "1" = Subscribe, "2" = Unsubscribe                                 |
| MarketDepth             | string | "0" = Full Book, "1" = Top of Book                                 |
| [MDEntryTypes](http://www.onixs.biz/fix-dictionary/4.4/tagNum_269.html) | array(string) | "0" = Bid, "1" = Offer, "2" = Trade |
| [MDUpdateType](http://www.onixs.biz/fix-dictionary/4.4/tagNum_265.html) | string | "0" = Full Refresh, "1" = Incremental RefreshRefresh                                            |
| Instruments             | array(string) | Array with the symbols that you want to subscribe e.g.: ['BTCBRL'] |

### Response Full Book

| Name        | Type   | Description                    |
|-------------|--------|--------------------------------|
| MsgType     | string | "W"                            |
| MDReqID     | number | Request ID                     |
| MarketDepth | string | "0" = Full Book, "1" = Top of Book |
| Symbol      | string | Instrument symbol subscribed   |
| MDFullGrp   | object | Object containing all orders   |


> __EXAMPLE RESPONSE FULL ORDER BOOK__

```json
{
    "MDReqID": 3140401,
    "Symbol": "BTCUSD",
    "MsgType": "W",
    "MDFullGrp": [{
        "MDEntryPositionNo": 1,
        "MDEntrySize": 113810,
        "MDEntryPx": 62181000000,
        "MDEntryID": 1459028463557,
        "MDEntryTime": "02:21:06",
        "MDEntryDate": "2016-07-13",
        "UserID": 90000002,
        "OrderID": 1459028463557,
        "MDEntryType": "0",
        "Broker": "exchange"
    }],
    "MarketDepth": 0
}
```

While you are subscribed to incremental updates, you will receive bids, asks and trades occured in realtime.

### Incremental Response

| Name     | Type   | Description                         |
|----------|--------|-------------------------------------|
| MsgType  | string | "X"                                 |
| MDReqID  | number | Request ID                          |
| MDBkTyp  | string | "3" = Order Depth                   |
| MDIncGrp | array  | Array containing the new data entry |

### Response MDIncGrp Entry

> __EXAMPLE RESPONSE__

```json
{
    "MDReqID": 3140401,
    "MDBkTyp": "3",
    "MsgType": "X",
    "MDIncGrp": [{
        "OrderID": 1459028463562,
        "MDEntryPx": 61795000000,
        "MDUpdateAction": "0",
        "MDEntryTime": "18:09:09",
        "Symbol": "BTCUSD",
        "UserID": 90800127,
        "Broker": "exchange",
        "MDEntryType": "0",
        "MDEntryPositionNo": 10,
        "MDEntrySize": 1000000,
        "MDEntryID": 1459028463562,
        "MDEntryDate": "2016-07-13"
    }]
}
```

| Name              | Type   | Description                                                        |
|-------------------|--------|--------------------------------------------------------------------|
| OrderID           | number | Order ID                                                           |
| MDEntryPx         | number | Order Price                                                        |
| MDUpdateAction    | string | "0" = New, "1" = Update, "2" = Delete, "3" = Delete Thru           |
| MDEntryTime       | string | Time when order was created                                        |
| Symbol            | string | Order symbol that you have subscribed on market data e.g: "BTCUSD" |
| UserID            | number | User ID                                                            |
| Broker            | string | Broker name that order belongs to                                  |
| MDEntryType       | string | "0" = Bid, "1" = Offer, "2" = Trade                                |
| MDEntryPositionNo | number | Order position on the book                                         |
| MDEntrySize       | number | Order size amount                                                  |
| MDEntryID         | number | Market data entry ID                                               |
| MDEntryDate       | string | Date when market data was received                                 |

## Security Status

## Orders

### Order List Request

> __EXAMPLE MESSAGE__

```json
{
  "MsgType": "U4",
  "OrdersReqID": 123
}
```

| Name         | Type   | Description                   |
|--------------|--------|-------------------------------|
| MsgType      | string | U4                            |
| BalanceReqID | string | Request ID                    |
| Page         | number | **Optional**; defaults to 0.  |
| PageSize     | number | **Optional**; defaults to 20. |


> __EXAMPLE RESPONSE__

```json
{
  "MsgType": "U5",
  "OrdersReqID": 123,
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
      "7891067",
      1459028474127,
      100000000,
      "2",
      0,
      0,
      66563000000,
      "BTCUSD",
      "1",
      "2",
      100000000,
      66563000000,
      "2016-07-17 18:32:21",
      66563000000.0,
      "1"
    ]
  ]
}
```

### Order List Response

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
8  ("Side")        | string | "1" = Buy, "2" = Sell, "E" = Redem, "F" = Lend, "G" = Borrow
9  ("OrdType")     | string | "1" = Market, "2" = Limited, "3" = Stop, "4" = Stop Limit, "G" = Swap, "P" = Pegged
10 ("OrderQty")    | number | Quantity ordered in satoshis.
11 ("Price")       | number | Price per unit in "satoshis" of your local currency. Example: $ 2,125.89 = 212589000000
12 ("OrderDate")   | string | Order date in UTC.
13 ("Volume")      | number | Quantity * Price
14 ("TimeInForce") | string | "0" = Day, "1" = Good Till Cancel, "4" = Fill or Kill


### Send Order

  To Send a Order

### Cancel Order

  Cancel a Order


## Ledger

## Deposits

### Instant Deposits

## Withdraws

## API Key

BlinkTrade provides a flexible way to you handle your APIs, you can choose permission for any message call,
and you will only be allowed to send messages that your API has permission to.

### Permissions

Some permissions requires that you pass some extra parameters, in order to match individual parameters of the message.
You can setup a permission that is only allowed to send a buy order as follows:

`{ "D": [["Side", "eq", "1"]]}`

> List of Permissions

```json
{
    "BF":  [],
    "U4":  [],
    "U34": [],
    "U26": [],
    "U2":  [],
    "U30": [],
    "U24": [],
    "D": [
        ["Side", "eq", "1", "OrdType", "eq", "2"],
        ["Side", "eq", "2", "OrdType", "eq", "2"],
        ["Side", "eq", "1", "OrdType", "eq", "1"],
        ["Side", "eq", "2", "OrdType", "eq", "1"]
    ],
    "F":   [],
    "U6": [
        ["Method", "eq", "PayPal"],
        ["Method", "eq", "Test2"],
        ["Method", "eq", "Test3"],
        ["Method", "eq", "bitcoin"]
    ],
    "U18": [
        ["Currency", "eq", "BTC"],
        ["DepositMethodID", "eq", "501"],
        ["DepositMethodID", "eq", "502"]
    ]
}
```

### List APIKeys

Returns all your APIKeys, **note** that your APISecret and APIPassword are only shown when you created the APIKey

> __EXAMPLE RESPONSE__

```json
{
    "PageSize": 20,
    "ApiKeyListGrp": [
        ["aleYuussya80JDl6f3mNZqPwyJDevSuDwUzmuDBitO4", "Test", [], {
            "BF": [],
            "U4": [],
            "U2": [],
            "U26": [],
            "U34": [],
            "U30": []
        }, "2016-06-17 05:44:45", "2016-06-17 05:44:45"],
    ],
    "MsgType": "U51",
    "APIKeyListReqID": 8603676,
    "Page": 0,
    "Columns": ["APIKey", "Label", "IPWhiteList", "PermissionList", "Created", "LastUsed"]
}
```

### Parameters

| Name            | Type   | Description |
|-----------------|--------|-------------|
| MsgType         | string | "U50"       |
| APIKeyListReqID | number | Request ID  |
| Page            | number | Page number |
| PageSize        | number | Page Size   |


### Response

| Name            | Type   | Description                                                   |
|-----------------|--------|---------------------------------------------------------------|
| MsgType         | string | "U51"                                                         |
| APIKeyListReqID | number | Request ID                                                    |
| Page            | number | Page number                                                   |
| PageSize        | number | Page Size                                                     |
| Columns         | array  | Description of all columns of the all orders in ApiKeyListGrp |

### Response ApiKeyListGrp

| Name                 | Type   | Description                         |
|----------------------|--------|-------------------------------------|
| 0 ("APIKey")         | string | APIKey                              |
| 1 ("Label")          | string | API Label                           |
| 2 ("IPWhiteList")    | array  | API IP white list                   |
| 3 ("PermissionList") | object | API Permissions                     |
| 4 ("Created")        | string | String time when API was created    |
| 5 ("LastUsed)"       | string | The last time when API Key was used |


### Create APIKey

> __EXAMPLE RESPONSE__

```json
{
    "Created": "2016-07-13 21:09:32",
    "APIPassword": "rZI98pYk9Irjg6y",
    "Label": "Test",
    "IPWhiteList": "\"[]\"",
    "MsgType": "U53",
    "Status": 1,
    "APIKey": "ACyWs5WiodoEXcOg6JJbP7VyLQoSTqZrAnHlzaLxcls",
    "APISecret": "dNNqZaU4LI6HQfY5bUtXRMuDWBZ3o62EYTo8DfEgjrY",
    "LastUsed": "2016-07-13 21:09:32",
    "Revocable": true,
    "APIKeyCreateReqID": 9783841,
    "PermissionList": "\"{\\\"BF\\\": []}\""
}
```

### Parameters

| Name              | Type   | Description                                 |
|-------------------|--------|---------------------------------------------|
| MsgType           | string | U52                                         |
| APIKeyCreateReqID | number | Request ID                                  |
| Label             | string | API Label                                   |
| PermissionList    | object | Stringify object containing API Permissions |
| IPWhiteList       | array  | Stringify array containing IP White list    |
| Revocable         | bool   | Revocable API                               |

### Response

| Name              | Type   | Description                                 |
|-------------------|--------|---------------------------------------------|
| MsgType           | string | U53                                         |
| APIKeyCreateReqID | number | Request ID                                  |
| APIKey            | string | Your API Key                                |
| APIPassword       | string | Your API Password                           |
| APISecret         | string | Your API Secret                             |
| Label             | string | API Label                                   |
| PermissionList    | object | Stringify object containing API Permissions |
| IPWhiteList       | array  | Stringify array containing IP White list    |
| Revocable         | bool   | Revocable API                               |
| Created           | string | String time when API was created            |
| LastUsed          | string | The last time when API Key was used         |
| Status            | number | API Status                                  |


### Revoke APIKey

### Parameters

| Name              | Type   | Description                     |
|-------------------|--------|---------------------------------|
| MsgType           | string | "U54"                           |
| APIKeyRevokeReqID | number | Request ID                      |
| APIKey            | string | API Key that you want to revoke |



> __EXAMPLE RESPONSE__

```json
{
    "MsgType": "U55",
    "APIKeyRevokeReqID": 8851626,
    "APIKey": "7Y1sKQvbIqRz117aDy3zWHdgv5cswdYZ8D2BFyC01DE",
    "Status": 0
}
```

### Response

| Name              | Type   | Description     |
|-------------------|--------|-----------------|
| MsgType           | string | "U55"           |
| APIKeyRevokeReqID | number | Request ID      |
| APIKey            | string | API Key revoked |
| Status            | number | API Status      |
