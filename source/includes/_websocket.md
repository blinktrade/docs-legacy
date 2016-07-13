# WebSocket API

With the WebSocket API, you have full access to the exchange, you can also check our JavaScript WebSocket implementation [here](https://github.com/blinktrade/frontend/blob/master/jsdev/bitex/api/bitex.js#L760-L1784)

## Connectivity

### Heartbeat

In 

### Paratemers
| Name      | Type   | Description |
|-----------|--------|-------------|
| MsgType   | string | 1           |
| TestReqID | string | Request ID  |
| SendTime  | object | Time Stamp  |

### Response

| Name            | Type   | Description       |
|-----------------|--------|-------------------|
| MsgType         | string | 0                 |
| TestReqID       | number | Request ID        |
| SendTime        | number | Time Stamp        |
| ServerTimestamp | number | Server Time Stamp |

## Authentication

Most of websocket calls requires authentication, once you login with your username / password on the websocket connection, you will be able to access your account.
You can also logged with your APIKey and APIPassword, you only be allowed to send messages that your APIKey has permission to it

NOTE that when you logged with your APIKey you only be

### FingerPrint

Each request requires that you pass a fingerprint from your browser, there's a few finger prints implementations that you can use.

* [Our google closure implementation](https://github.com/blinktrade/frontend/blob/master/jsdev/bitex/util/util.js#L96-L157)
* [fingerprintjs2](https://github.com/Valve/fingerprintjs2)


### Login

### Parameters

| Name | Type | Description |
|------|------|-------------|
| MsgType | string | [BE](http://www.onixs.biz/fix-dictionary/4.4/msgType_BE_6669.html) |
| UserReqID | number | Request Id |
| BrokerID | number | The ID of the broker |
| Username | string | The email address or username of the user |
| Password | string | The password of the user |
| UserReqTyp | string | 1 |
| UserAgent | string | Brower user agent navigator |
| UserAgentLanguage | string | User agent language |
| UserAgentTimezoneOffset | integer | User agent timezone offset |
| UserAgentPlatform | string | User agent platform |
| FingerPrint | string | Browser finger print |

### Response

| Parameter | Type | Description |
|-----------|------|-------------|
| MsgType   | string | [BF](http://www.onixs.biz/fix-dictionary/4.4/msgType_BF_6670.html) |
| Broker | object | Broker model object |
| BrokerID | number | Broker ID |
| DepositFixedFee | number ||
| DepositPercentFee | number ||
| EmailLang | string | Email Language |
| HasLineOfC3redit | bool | bool indicating whether user has line of credit |
| IsBroker | bool | bool whether user is a broker |
| IsMSB | bool  ||
| Profile | object | Profile model object |
| TakerTransactionFeeBuy | number ||
| TakerTransactionFeeSell | number ||
| TransactionFeeBuy | number ||
| TransactionFeeSell | number ||
| TwoFactorEnabled | bool | bool indicating whether user has two factor enabled |
| UserID | number | Integer with the user ID |
| UserStatus | number | 1 = Logged In, 2 = Not Logged In |
| Username | string | String with the username of the user |
| WithdrawFixedFee | number ||
| WithdrawPercentFee | number ||

### Sign Up
### Two factor Authentication
### Change password

In order to change your password, you do the same as a login request, but passing `UserReqTyp` as 3 and the new password on `Password`

### Parameters

| Name         | Type   | Description             |
|--------------|--------|-------------------------|
| MsgType      | string | BE                      |
| UserReqID    | number | Request ID              |
| UserReqTyp   | number | Should be 3             |
| BrokerID     | number | Current Logged BrokerID |
| Username     | string | Username                |
| Password     | string | New password            |
| SecondFactor | number | Optional Second Factor Authentication  |

### Response

Returns should be the same as login

## Brokers

### Request Broker List

### Parameters

| Name            | Type   | Description |
|-----------------|--------|-------------|
| MsgType         | string | U28         |
| BrokerListReqID | number | Request ID  |
| Page            | number | Page Index  |
| PageSize        | number | Page Size.  |
| StatusList      |        |             |

## Profile

### Parameters

| Name        | Type   | Description                    |
|-------------|--------|--------------------------------|
| MsgType     | string | U38                            |
| UpdateReqID | string | Request ID                     |
| Fields      | object | Fields that you want to update |
| UserID      | number | Optional UserID                |

### Response

## Balances

### Parameters

| Name         | Type   | Description       |
|--------------|--------|-------------------|
| MsgType      | string | U2                |
| BalanceReqID | string | Request ID        |
| ClientID     | object | Optional ClientID |

### Returns

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

## API Key

BlinkTrades provides a flexible way to you handle your APIs, you can choose any permission for an individual message type

#### Permissions

You can choose any individual message type permission to your APIKey, some messages also requires some

> Permissions

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
    ],
}
```



### List APIKeys


### Parameters

| Name            | Type   | Description |
|-----------------|--------|-------------|
| MsgType         | string | U50         |
| APIKeyListReqID | number | Request ID  |
| Page            | number | Page number |
| PageSize        | number | Page Size   |


### Response

| Name            | Type   | Description                                                   |
|-----------------|--------|---------------------------------------------------------------|
| MsgType         | string | U50                                                           |
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
| MsgType           | string | U54                             |
| APIKeyRevokeReqID | number | Request ID                      |
| APIKey            | string | API Key that you want to revoke |


### Response

| Name              | Type   | Description     |
|-------------------|--------|-----------------|
| MsgType           | string | U55             |
| APIKeyRevokeReqID | number | Request ID      |
| APIKey            | string | API Key revoked |
| Status            | number | API Status      |
