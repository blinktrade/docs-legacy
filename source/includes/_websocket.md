# WebSocket API

With the WebSocket API, you have full access to the exchange, you can also check our JavaScript WebSocket implementation [here](https://github.com/blinktrade/frontend/blob/master/jsdev/bitex/api/bitex.js#L760-L1784).

## Connectivity

### Heartbeat

You must send this message each 30 seconds, in order to keep your connection alive.

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
[TestReqID](http://www.onixs.biz/fix-dictionary/4.4/tagNum_112.html) | string | An ID assigned by you. It can be any number. The response message associated with this request will contain the same ID
SendTime  | number | Unix Time Stamp


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
[TestReqID](http://www.onixs.biz/fix-dictionary/4.4/tagNum_112.html) | string | This should match the TestReqID sent on the message
SendTime        | number | This should match your SendTime
ServerTimestamp | number | Server Unix Time Stamp

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

Name                    | Type   | Description
------------------------|--------|------------
MsgType                 | string | [BE](http://www.onixs.biz/fix-dictionary/4.4/msgType_BE_6669.html)
UserReqID               | number | Request Id
BrokerID                | number | [\<BROKER_ID\>](#brokers)
Username                | string | The email address, username or API Key of the user
Password                | string | The password of the user
UserReqTyp              | string | "1"
FingerPrint             | string | Browser [fingerprint](#fingerprint)
UserAgent               | string | **Optional**; Browser user agent navigator
UserAgentLanguage       | string | **Optional**; User agent language
UserAgentTimezoneOffset | number | **Optional**; User agent timezone offset
UserAgentPlatform       | string | **Optional**; User agent platform

### Response

> __EXAMPLE RESPONSE__

```json
{
    "MsgType": "BF",
    "Broker": {
      // Broker Model
    },
    "BrokerID": 5,
    "DepositFixedFee": null,
    "DepositPercentFee": null,
    "EmailLang": "en",
    "HasLineOfCredit": false,
    "IsBroker": false,
    "IsMSB": true,
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
    "TakerTransactionFeeBuy": 500,
    "TakerTransactionFeeSell": 500,
    "TransactionFeeBuy": 500,
    "TransactionFeeSell": 500,
    "TwoFactorEnabled": false,
    "UserID": 90800127,
    "UserStatus": 1,
    "Username": "cesaraugusto",
    "WithdrawFixedFee": null,
    "WithdrawPercentFee": null,
    "ConfirmationOrder": false,
    "UserReqID": 1123107,
    "IsMarketMaker": false,
    "EmailTwoFactorEnabled": false,
    "PermissionList": {
        "*": []
    }
}
```

Parameter               | Type   | Description
------------------------|--------|-------------
MsgType                 | string | [BF](http://www.onixs.biz/fix-dictionary/4.4/msgType_BF_6670.html)
Broker                  | object | Broker model object, see [broker response](#brokerlistgrp-model) for more informations
BrokerID                | number | [\<BROKER_ID\>](#brokers)
DepositFixedFee         | number | Fixed deposit fee
DepositPercentFee       | number | Deposit fixed fee
EmailLang               | string | Email Language
HasLineOfCredit         | boolean| `boolean` indicating whether user has line of credit
IsBroker                | boolean| `boolean` whether user is a broker
IsMSB                   | boolean|
Profile                 | object | Profile model object, see [profile model]() for more informations
TakerTransactionFeeBuy  | number | Taker fee for buy orders
TakerTransactionFeeSell | number | Taker fee for sell orders
TransactionFeeBuy       | number | Maker fee for buy orders
TransactionFeeSell      | number | Maker fee for sell orders
TwoFactorEnabled        | boolean| `boolean` indicating whether user has two factor enabled
UserID                  | number | Integer with the user ID
UserReqTyp              | number | Returns only if it is equal to 3 when password has been changed
UserStatus              | number | "1" = Logged In, "2" = Not Logged In, "3" = User Not Recognised
Username                | string | String with the username of the user
WithdrawFixedFee        | number | Fixed fee for withdraws
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
MsgType                 | string | U0
UserReqID               | number | An ID assigned by you. It can be any number. The response message associated with this request will contain the same ID
Username                | string | Username you want to use
Password                | string | Password you want use
Email                   | string | User email
State                   | string | State
CountryCode             | string | ContryCode, e.g. "BR"
BrokerID                | number | [\<BROKER_ID\>](#brokers)
UserAgent               | string | **Optional**; Browser user agent navigator
UserAgentLanguage       | string | **Optional**; User agent language
UserAgentTimezoneOffset | number | **Optional**; User agent timezone offset
UserAgentPlatform       | string | **Optional**; User agent platform


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
------------------------|--------|------------
MsgType                 | string | "BF"
UserReqID               | number | 123
Username                | string | 
UserStatus              | number | 3,
NeedSecondFactor        | bool   | true
UserStatusText          | string | 
SecondFactorType        | string | 


## Two factor Authentication

### Enable

> __EXAMPLE MESSAGE__

```json
{
    "MsgType": "U16",
    "EnableTwoFactorReqID": 123,
    "Enable": true
}
```

### Parameters

Name                    | Type   | Description 
------------------------|--------|-------------
MsgType                 | string |  
EnableTwoFactorReqID    | string |  
Enable                  | bool   |  
Secret                  |        |  
Code                    |        |  
ClientID                |        |  

### Response




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
SecondFactor | number | **Optional**; Second Factor Authentication

### Response

Returns should be the same as [Login](#login).

## Brokers

### Request Broker List

> __EXAMPLE MESSAGE__

```json
{
    "MsgType": "U28",
    "BrokerListReqID": 876687,
    "StatusList": ["1"]
}
```

### Parameters

Name            | Type   | Description
----------------|--------|------------
MsgType         | string | "U28"
BrokerListReqID | number | Request ID
Page            | number | Page Index
PageSize        | number | Page Size
StatusList      |        | 

### Response

> __EXAMPLE RESPONSE__

```json

{
    "PageSize": 20,
    "BrokerListGrp": [
        [5, "exchange", "BlinkTrade Demo Exchange", "21 Bitcoin Ave", "New York", "NY", "10000", "United States", null, null, "blinktrade", "USD", "https://docs.google.com/a/blinktrade.com/document/d/1HyFRs_2Seh4LGZYjPk8bmbxueUjF7RMz-koAM3rG2Pc/pub?embedded=true", [{
            "Operation": "Wire transfer",
            "Fee": "1%",
            "Terms": "Exchange operator decides its fees"
        }], 60, 60, "1", 5, "demo@blinktrade.com", "US", [{
            "Wallets": [{
                "managed_by": "TESTNET",
                "signatures": [],
                "type": "hot",
                "multisig": false,
                "address": "n19ZAH1WGoUkQhubQw71fH11BenifxpBxf"
            }],
            "CurrencyCode": "BTC",
            "Confirmations": [
                [0, 25000000000, 1],
                [25000000000, 20000000000, 3],
                [20000000000, 2100000000000000, 6]
            ],
            "CurrencyDescription": "Bitcoin"
        }], {
            "USD": [{
                "percent_cost": 0,
                "limits": {
                    "1": {
                        "enabled": false
                    },
                    "0": {
                        "enabled": false
                    },
                    "3": {
                        "enabled": true
                    },
                    "2": {
                        "enabled": false
                    },
                    "5": {
                        "enabled": true
                    },
                    "4": {
                        "enabled": true
                    }
                },
                "fixed_cost": 0,
                "fields": [{
                    "name": "email",
                    "type": "text",
                    "required": "text",
                    "value": "",
                    "label": "email",
                    "validator": "required",
                    "placeholder": "email",
                    "side": "client"
                }],
                "percent_fee": 0,
                "formatted_percent_fee": "0,00\u00a0%",
                "formatted_fixed_fee": "$ 0,00",
                "disclaimer": "",
                "method": "PayPal",
                "fixed_fee": 0,
                "description": "PayPal"
            }, {
                "percent_cost": 0,
                "limits": {
                    "1": {
                        "enabled": false
                    },
                    "0": {
                        "enabled": false
                    },
                    "3": {
                        "enabled": true
                    },
                    "2": {
                        "enabled": false
                    },
                    "5": {
                        "enabled": true
                    },
                    "4": {
                        "enabled": true
                    }
                },
                "fixed_cost": 0,
                "fields": [{
                    "name": "email",
                    "placeholder": "email",
                    "required": "text",
                    "value": "",
                    "label": "email",
                    "validator": "required",
                    "type": "text",
                    "side": "client"
                }],
                "description": "Test2",
                "method": "Test2",
                "fixed_fee": 0,
                "percent_fee": 2,
                "disclaimer": ""
            }, {
                "percent_cost": 0,
                "limits": {
                    "1": {
                        "enabled": true
                    },
                    "0": {
                        "enabled": false
                    },
                    "3": {
                        "enabled": true
                    },
                    "2": {
                        "enabled": false
                    },
                    "5": {
                        "enabled": true
                    },
                    "4": {
                        "enabled": true
                    }
                },
                "fixed_cost": 0,
                "fields": [{
                    "name": "email",
                    "placeholder": "Email",
                    "required": "text",
                    "value": "",
                    "label": "email",
                    "validator": "required",
                    "type": "text",
                    "side": "client"
                }],
                "description": "Test 3",
                "method": "Test3",
                "formatted_percent_fee": "0,00\u00a0%",
                "formatted_fixed_fee": "$ 0,00",
                "percent_fee": 0,
                "fixed_fee": 0,
                "disclaimer": "Test"
            }],
            "BTC": [{
                "percent_cost": 1,
                "limits": {
                    "1": {
                        "max": 1000000000,
                        "enabled": true
                    },
                    "0": {
                        "max": 1000000000,
                        "enabled": true
                    },
                    "3": {
                        "max": 5000000000,
                        "enabled": true
                    },
                    "2": {
                        "max": 1000000000,
                        "enabled": true
                    },
                    "5": {
                        "max": 50000000000,
                        "enabled": true
                    },
                    "4": {
                        "max": 50000000000,
                        "enabled": true
                    }
                },
                "fixed_cost": 10000,
                "fields": [{
                    "name": "Wallet",
                    "placeholder": "",
                    "required": "text",
                    "value": "",
                    "label": "Wallet",
                    "validator": "validateAddress",
                    "type": "text",
                    "side": "client"
                }, {
                    "name": "TransactionID",
                    "placeholder": "",
                    "required": "text",
                    "value": "",
                    "label": "Transaction ID",
                    "validator": "validateAlphaNum",
                    "type": "text",
                    "side": "broker"
                }],
                "description": "Bitcoin withdrawal",
                "method": "bitcoin",
                "fixed_fee": 10000,
                "percent_fee": 1,
                "disclaimer": ""
            }]
        }, "mailto:rodrigo@blinktrade.com", "BlinkTrade Demo Exchange", [
            ["*"],
            ["CU", "SO", "SD", "NG", "IR", "KP"]
        ], false, 100, 100],
    ],
    "Page": 0,
    "MsgType": "U29",
    "BrokerListReqID": 7073382,
    "Columns": ["BrokerID", "ShortName", "BusinessName", "Address", "City", "State", "ZipCode", "Country", "PhoneNumber1", "PhoneNumber2", "Skype", "Currencies", "TosUrl", "FeeStructure", "TransactionFeeBuy", "TransactionFeeSell", "Status", "ranking", "Email", "CountryCode", "CryptoCurrencies", "WithdrawStructure", "SupportURL", "SignupLabel", "AcceptCustomersFrom", "IsBrokerHub", "TakerTransactionFeeBuy", "TakerTransactionFeeSell"]
}
```

| Name            | Type   | Description
|-----------------|--------|-------------------------------------------------|
| MsgType         | string | U29                                             |
| BrokerListReqID | number | Request ID                                      |
| BrokerListGrp   | Array  | Array of Arrays with all brokers informations   |
| Columns         | Array  | Array describing each column from BrokerListGrp |
| Page            | number | **Optional**; defaults to 0                     |
| PageSize        | number | **Optional**; defaults to 20                    |

### BrokerListGrp Model

| Name                         | Type   | Description
|------------------------------|--------|------------
| 0  (BrokerID)                | number | BrokerID
| 1  (ShortName)               | string | Short Name of the broker
| 2  (BusinessName)            | string | Broker's Business Name
| 3  (Address)                 | string | Broker's Address
| 4  (City)                    | string | Broker's City
| 5  (State)                   | string | Broker's State Code
| 6  (ZipCode)                 | string | Broker's ZipCode
| 7  (Country)                 | string | Broker's Country
| 8  (PhoneNumber1)            | string | Broker's first phone number
| 9  (PhoneNumber2)            | string | Broker's second phone number
| 10 (Skype)                   | string | Broker's skype address
| 11 (Currencies)              | string | Default currency of the broker, e.g.: "BRL"
| 12 (TosUrl)                  | string | Terms of Service url
| 13 (FeeStructure)            | array  | Array of objects describing the broker's fees
| 14 (TransactionFeeBuy)       | number | Maker Fee for buy orders
| 15 (TransactionFeeSell)      | number | Maker Fee for sell order
| 16 (Status)                  | string | Broker's status
| 17 (ranking)                 | number | Broker's ranking
| 18 (Email)                   | string | Broker's email
| 19 (CountryCode)             | string | Broker's Country Code
| 20 (CryptoCurrencies)        | array  | Wallet objects describing the broker's wallets
| 21 (WithdrawStructure)       | object | Withdraw methods of the broker, see [Withdraw methods](#Withdraw methods) for more information
| 22 (SupportURL)              | string | Customer support URL
| 23 (SignupLabel)             | string | Signup Label
| 24 (AcceptCustomersFrom)     | array  | Array containing from which country customers can signup
| 25 (IsBrokerHub)             | boolean| Is Broker Hub
| 26 (TakerTransactionFeeBuy)  | number | Taker fee for buy orders
| 27 (TakerTransactionFeeSell) | number | Taker fee for sell orders


## Update Profile

Users can only change a few fields, such as `TwoFactorEnabled`, `EmailLang` and `ConfirmationOrder`

### Parameters

| Name        | Type   | Description                    |
|-------------|--------|--------------------------------|
| MsgType     | string | "U38"                          |
| UpdateReqID | string | Request ID                     |
| Fields      | object | Fields that you want to update |
| UserID      | number | **Optional**; UserID           |

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
| ClientID     | object | **Optional**; ClientID

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
[\<BROKER_ID\>](#brokers)| object  | The [Broker ID](#brokers) containing your BTC and FIAT balance, e.g.: "5" stands for your balance with the [Broker ID](#brokers) number 5.
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
| Page         | number | **Optional**; defaults to 0   |
| PageSize     | number | **Optional**; defaults to 20  |


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

List all your trades activity, each row on the ledger represents either a trade occurred or trade fees.

> __EXAMPLE MESSAGE__

```json
{
    "MsgType": "U34",
    "LedgerListReqID": 123
}
```

### Parameters

Name            | Type          | Description/Value
----------------|---------------|------------------
MsgType         | string        | U34
LedgerListReqID | number        | Request ID
Page            | number        | **Optional**; defaults to 0
PageSize        | number        | **Optional**; defaults to 20


> __EXAMPLE RESPONSE__

```json
{
  "MsgType": "U35",
  "Page": 0,
  "PageSize": 100,
  "LedgerListReqID": 123,
  "Columns": [
    "LedgerID",
    "Currency",
    "Operation",
    "AccountID",
    "BrokerID",
    "PayeeID",
    "PayeeBrokerID",
    "Amount",
    "Balance",
    "Reference",
    "Created",
    "Description",
    "AccountName"
  ],
  "LedgerListGrp": [
    [
      128197,
      "BTC",
      "D",
      90000002,
      5,
      90000001,
      5,
      994273,
      29128175022,
      "1459028474211.1459028474155",
      "2016-07-20 11:23:07",
      "TF",
      "user"
    ]
  ]
}
```

### Response

| Name               | Type   | Description
|--------------------|--------|------------
| 0  (LedgerID)      | number | Ledger row ID
| 1  (Currency)      | string | Currency of the trade, either crypto or fiat
| 2  (Operation)     | string | MsgType of the trade occurred e.g.: "D"
| 3  (AccountID)     | number | Your account id
| 4  (BrokerID)      | number | BrokerID
| 5  (PayeeID)       | number | Payee's account id
| 6  (PayeeBrokerID) | number | Payee's BrokerID
| 7  (Amount)        | number | Amount of the trade depending of the currency occurred
| 8  (Balance)       | number | Balance after trade occurred
| 9  (Reference)     | string | Reference data
| 10 (Created)       | string | Creating date
| 11 (Description)   | string | "TF" = Trade Fees, "T" = Trade
| 12 (AccountName)   | string | Your profile usernam


## Deposit

### Deposit List

> __EXAMPLE MESSAGE__

```json
{
    "MsgType": "U30",
    "DepositListReqID": 123,
    "StatusList": ["1"]
}
```

### Parameters

Name             | Type      | Description/Value
-----------------|-----------|------------------
MsgType          |           | 
DepositListReqID |           | 
Page             |           | 
PageSize         |           | 
StatusList       |           | 
Filter           |           | 
ClientID         |           | 

### Response

> __EXAMPLE RESPONSE__

```json
{
  "PageSize": 20,
  "DepositListReqID": 123,
  "MsgType": "U31",
  "DepositListGrp": [
    
  ],
  "Page": 0,
  "Columns": [
    "DepositID",
    "DepositMethodID",
    "DepositMethodName",
    "Type",
    "Currency",
    "Value",
    "PaidValue",
    "Data",
    "Created",
    "ControlNumber",
    "PercentFee",
    "FixedFee",
    "Status",
    "ReasonID",
    "Reason",
    "Username",
    "UserID",
    "BrokerID",
    "ClOrdID",
    "CreditProvided",
    "State"
  ]
}
```

Name             | Type      | Description/Value
-----------------|-----------|------------------
                 |           | 


### Instant Deposits

## Withdraws

### Requesting a withdraw

### Withdraw methods

There are different methods of withdraws for each currency, they are created dynamically by each broker, you can take a look logging as a broker on
[testnet.blinktrade.com](https://testnet.blinktrade.com), and go to `admin` tab and `Withdraw methods`

## API Key

BlinkTrade provides a flexible way to you handle your APIs, you can choose permission for any message call,
and you will only be allowed to send messages that your API has permission to.

### Permissions

Some permissions requires that you pass some extra parameters, in order to match individual parameters of the message.
You can setup a permission that is only allowed to send a buy order as follows:

`{ "D": [["Side", "eq", "1"]]}`

> __LIST OF PERMISSIONS__

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

Returns all your APIKeys, **note** that your APISecret and APIPassword are only shown when were created

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
