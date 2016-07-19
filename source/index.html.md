---
title: BlinkTrade API

language_tabs:
  - shell
  - python
  - php

includes:
  - restapi
  - websocket

search: true
---

# Getting Started

### OVERVIEW

BlinkTrade provides a simple and robust [WebSocket API](#websocket-api) to integrate our platform, we strongly recommend you to use it over the [Rest API](#rest-api).

### FIX

BlinkTrade's API is based on the [FIX protocol](http://www.onixs.biz/fix-dictionary/4.4/index.html).

## BlinkTrade Endpoints

There are two working environments: `prod` for production purporses and `testnet` for testing purporses:

* `testnet` base URL endpoint is `https://api.testnet.blinktrade.com`
* `prod` base URL endpoint is `https://api.blinktrade.com`

Some features are accessed publicly and others require an API Key based authentication. All data messages and responses are in JSON format.


## Brokers

A broker ID is assigned for each exchange powered by BlinkTrade:

Broker                                         | ID
-----------------------------------------------|------
[SurBitcoin](https://surbitcoin.com)           | 1
[VBTC](https://vbtc.vn)                        | 3
[FoxBit](https://foxbit.com.br)                | 4
[**Testnet**](https://testnet.blinktrade.com/) | **5**
[UrduBit](https://urdubit.com/)                | 8
[ChileBit](https://chilebit.net)               | 9


## Currencies

`<CURRENCY>` | Description
-------------|------------
VEF          | Venezuelan Bolivares (SurBitcoin)
VND          | Vietnamise Dongs (VBTC)
BRL          | Brazil Reals (FoxBit)
PKR          | Pakistani Ruppe (UrduBit)
CLP          | Chilean Pesos (ChileBit.NET)

## Symbols

`<SYMBOL>` | Description
-----------|------------
BTCVEF     | BTC Pair - Venezuelan Bolivares (SurBitcoin)
BTCVND     | BTC Pair - Vietnamise Dongs (VBTC)
BTCBRL     | BTC Pair - Brazil Reals (FoxBit)
BTCPKR     | BTC Pair - Pakistani Ruppe (UrduBit)
BTCCLP     | BTC Pair - Chilean Pesos (ChileBit.NET)

## Messages

List of Messages Based on the FIX Protocol.

### User Messages

Code  | Description
------|---------------------
0     | [Heartbeat](http://www.onixs.biz/fix-dictionary/4.4/msgType_0_0.html)
1     | [TestRequest](http://www.onixs.biz/fix-dictionary/4.4/msgType_1_1.html)
C     | [Email](http://www.onixs.biz/fix-dictionary/4.4/msgType_C_67.html)
V     | [MarketDataRequest](http://www.onixs.biz/fix-dictionary/4.4/msgType_V_86.html)
W     | [MarketDataFullRefresh](http://www.onixs.biz/fix-dictionary/4.4/msgType_W_87.html)
X     | [MarketDataIncrementalRefresh](http://www.onixs.biz/fix-dictionary/4.4/msgType_X_88.html)
Y     | [MarketDataRequestReject](http://www.onixs.biz/fix-dictionary/4.4/msgType_Y_89.html)
BE    | [UserRequest](http://www.onixs.biz/fix-dictionary/4.4/msgType_BE_6669.html)
BF    | [UserResponse](http://www.onixs.biz/fix-dictionary/4.4/msgType_BF_6670.html)
D     | [NewOrderSingle](http://www.onixs.biz/fix-dictionary/4.4/msgType_D_68.html)
F     | [OrderCancelRequest](http://www.onixs.biz/fix-dictionary/4.4/msgType_F_70.html)
8     | [ExecutionReport](http://www.onixs.biz/fix-dictionary/4.4/msgType_8_8.html)
x     | [SecurityListRequest](http://www.onixs.biz/fix-dictionary/4.4/msgType_x_120.html)
y     | [SecurityList](http://www.onixs.biz/fix-dictionary/4.4/msgType_y_121.html)
e     | [SecurityStatusRequest](http://www.onixs.biz/fix-dictionary/4.4/msgType_e_101.html)
f     | [SecurityStatus](http://www.onixs.biz/fix-dictionary/4.4/msgType_f_102.html)
AN    | [RequestForPositions](http://www.onixs.biz/fix-dictionary/4.4/msgType_AN_6578.html)
AO    | [RequestForPositionsAck](http://www.onixs.biz/fix-dictionary/4.4/msgType_AO_6579.html)
AP    | [PositionReport](http://www.onixs.biz/fix-dictionary/4.4/msgType_AP_6580.html)
U0    | [Signup]()
U2    | [UserBalanceRequest]()
U3    | [UserBalanceResponse]()
U4    | [OrdersListRequest]()
U5    | [OrdersListResponse]()
U6    | [WithdrawRequest]()
U7    | [WithdrawResponse]()
U9    | [WithdrawRefresh]()
U10   | [ResetPasswordRequest]()
U11   | [ResetPasswordResponse]()
U12   | [ResetPasswordRequest]()
U13   | [ResetPasswordResponse]()
U16   | [EnableDisableTwoFactorAuthenticationRequest]()
U17   | [EnableDisableTwoFactorAuthenticationResponse]()
U18   | [DepositRequest]()
U19   | [DepositResponse]()
U23   | [DepositRefresh]()
U20   | [DepositMethodsRequest]()
U21   | [DepositMethodsResponse]()
U24   | [WithdrawConfirmationRequest]()
U25   | [WithdrawConfirmationResponse]()
U26   | [WithdrawListRequest]()
U27   | [WithdrawListResponse]()
U28   | [BrokerListRequest]()
U29   | [BrokerListResponse]()
U30   | [DepositListRequest]()
U31   | [DepositListResponse]()
U32   | [TradeHistoryRequest]()
U33   | [TradeHistoryResponse]()
U34   | [LedgerListRequest]()
U35   | [LedgerListResponse]()
U36   | [TradersRankRequest]()
U37   | [TradersRankResponse]()
U38   | [UpdateProfile]()
U39   | [UpdateProfileResponse]()
U40   | [ProfileRefresh]()
U42   | [PositionRequest']()
U43   | [PositionResponse']()
U44   | [ConfirmTrustedAddressRequest']()
U45   | [ConfirmTrustedAddressResponse']()
U46   | [SuggestTrustedAddressPublish']()
U48   | [DepositMethodRequest]()
U49   | [DepositMethodResponse]()
U50   | [APIKeyListRequest]()
U51   | [APIKeyListResponse]()
U52   | [APIKeyCreateRequest]()
U53   | [APIKeyCreateResponse]()
U54   | [APIKeyRevokeRequest]()
U55   | [APIKeyRevokeResponse]()
U56   | [GetCreditLineOfCreditRequest]()
U57   | [GetCreditLineOfCreditResponse]()
U58   | [PayCreditLineOfCreditRequest]()
U59   | [PayCreditLineOfCreditResponse]()
U60   | [LineOfCreditListRequest]()
U61   | [LineOfCreditListResponse]()
U62   | [EnableCreditLineOfCreditRequest]()
U63   | [EnableCreditLineOfCreditResponse]()
U65   | [LineOfCreditRefresh]()
U70   | [CancelWithdrawalRequest]()
U71   | [CancelWithdrawalResponse]()
U72   | [CardListRequest]()
U73   | [CardListResponse]()
U74   | [CardCreateRequest]()
U75   | [CardCreateResponse]()
U76   | [CardDisableRequest]()
U77   | [CardDisableResponse]()
U78   | [WithdrawCommentRequest]()
U79   | [WithdrawCommentResponse]()

### Broker Messages

Code  | Description/Function
------|---------------------
B0    |  [ProcessDeposit]()
B1    |  [ProcessDepositResponse]()
B2    |  [CustomerListRequest]()
B3    |  [CustomerListResponse]()
B4    |  [CustomerRequest]()
B5    |  [CustomerResponse]()
B6    |  [ProcessWithdraw]()
B7    |  [ProcessWithdrawResponse]()
B8    |  [VerifyCustomerRequest]()
B9    |  [VerifyCustomerResponse]()
B1    |  [VerifyCustomerRefresh]()

### REFERENCE

* [FIX 4.4 - FIX Dictionary - Onix Solutions](http://www.onixs.biz/fix-dictionary/4.4/index.html)
