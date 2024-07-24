---
title: Trading APIs Documentation

language_tabs:
  - shell

includes:
  - filter_help

search: true

code_clipboard: true

meta:
  - name: TopArz Trading API dcumentation
    content: Documentation for Trading API
---

# Introduction

Hey Yo!  
How u diong?

Welcome to [TopArz](https://toparz.com) trading API documentation.
Here you can find the required information to work with trading APIs.
First of all, you have to get a token in your dashboard.

# Authentication

> Example of calling a private api

```shell
curl "GET" \
    "http://api-trading.toparz.com/SOME-PRIVATE-ROUTE" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> Make sure to replace `<PUT_YOUR_TOKEN_HERE>` with token from your dashboard

For private api we check `Bearer Token` as authorization method so you have to get a token from your dashboard and put it as a `Authorization` header in your request header:

`Authorization: Bearer <PUT_YOUR_TOKEN_HERE>`

<aside class="notice">
You must replace <code>&lt;PUT_YOUR_TOKEN_HERE&#62;</code> with your personal API key.
</aside>

# Markets

> This a public api

```shell
curl "GET" \
    "https://api-trading.toparz.com/api/v1/markets"
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": [
    {
      "symbol": "USDTTMN",
      "fa_name": "تتر تومان",
      "en_name": "Tether Toman",
      "base_currency": "TMN",
      "fa_base_currency_name": "تومان",
      "en_base_currency_name": "Toman",
      "source_currency": "USDT",
      "fa_source_currency_name": "تتر",
      "en_source_currency_name": "Tether",
      "buy_side_enabled": "OTC",
      "sell_side_enabled": "BOTH",
      "price_digits_count": 0,
      "volume_digits_count": 2
    }
  ]
}
```
to get markets you can you this api

### HTTP Request
`GET /api/v1/markets`

### Response

Parameter | Type | Description
--------- | ---- | -----------
id | string | id
symbol | string | symbol
fa_name | string | name in Farsi
en_name | string | name in English
base_currency | string | base currency
fa_base_currency_name | string | base currency name in Farsi
en_base_currency_name | string | base currency name in English
source_currency | string | source currency
fa_source_currency_name | string | source currency name in Farsi
en_source_currency_name | string | source currency name in English
buy_side_enabled | enum | status of buy side, one of these:<br>`[ NONE, OTC, P2P, BOTH ]`
sell_side_enabled | enum | status of sell side, one of these:<br>`[ NONE, OTC, P2P, BOTH ]`
price_digits_count | number | price digit count
volume_digits_count | number | volumn digit count

# Account

Everything from getting account balance to placing/canceling orders

## Get Balance

```shell
curl "GET" \
    "https://api-trading.toparz.com/api/v1/account/balances" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": [
    {
      "locked_value": 50,
      "currency": "USDT",
      "value": 12000
    }
  ]
}
```

Ever wanted to know how much money do you have? ***\*rolling drums*** so I'm here to bring you out of the dark. Call me and I'll tell you how many of each asset you have, even I can tell you how many of your assets are locked.

### HTTP Request
`GET /api/v1/account/balances`

### Response

Parameter | Type | Description
--------- | ---- | -----------
locked_value | number | value of locked asset
currency | string | currency
value | number | value of whole assets

## Place Order

```shell
curl "POST" \
    "https://api-trading.toparz.com/api/v1/account/orders" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> Body Examples:

> `MARKET` order `side = BUY`:

```json
{  
    "clint_id": "f6921326-cd2a-48e9-9fd1-1c6296deed46",
    "symbol": "BTCTMN",
    "type": "MARKET",
    "side": "BUY",
    "total_price": 1000
}
```

> `MARKET` order `side = SELL`:

```json
{
    "clint_id": "f6921326-cd2a-48e9-9fd1-1c6296deed46",
    "symbol": "BTCTMN",
    "type": "MARKET",
    "side": "SELL",
    "quantity": 100
}
```

> `LIMIT` order `side = BUY`:

```json
{
    "clint_id": "f6921326-cd2a-48e9-9fd1-1c6296deed46",
    "symbol": "BTCTMN",
    "type": "LIMIT",
    "side": "BUY",
    "price": 10,
    "total_price": 1000
}
```

> `LIMIT` order `side = SELL`:

```json
{
    "clint_id": "f6921326-cd2a-48e9-9fd1-1c6296deed46",
    "symbol": "BTCTMN",
    "type": "LIMIT",
    "side": "SELL",
    "price": 10 ,
    "quantity": 100,
}
```

> <hr>

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": {
    "order": {
      "id": 93,
      "command_id": "f6921326-cd2a-48e9-9fd1-1c6296deed46",
      "symbol": "USDTTMN",
      "quantity": null,
      "price": null,
      "total_price": 232036,
      "filled_quantity": 4,
      "filled_total_price": 232036,
      "type": "MARKET",
      "side": "BUY",
      "created_at": 1721719000615,
      "maker_fee_coefficient": 0.0035,
      "taker_fee_coefficient": 0.0035,
      "status": "FINISHED",
      "updated_at": 1721719000620,
      "stop_price": null,
      "stop_price_triggered": false,
      "stop_price_condition": null
    },
    "trades": [
      {
        "id": 382,
        "order_command_id": "f6921326-cd2a-48e9-9fd1-1c6296deed46",
        "order_side": "BUY",
        "trace_id": 191,
        "symbol": "USDTTMN",
        "fee_coefficient": 0.0035,
        "fee": 0.01,
        "fee_currency": "USDT",
        "side": "TAKER",
        "type": "P2P",
        "price": 58009,
        "total_price": 232036,
        "quantity": 4,
        "created_at": 1721719000615,
        "reverted": false
      }
    ]
  }
}
```

For placing new order call this api.

<aside class="notice">
Using this API is a bit tricky, so read descriptions carefully and look at examples for better understanding.

Asking why? Based on your request, we read specific parameters from your request, here are some examples of each request
</aside>

<hr>

### HTTP Request
`POST /api/v1/account/orders`

### Request

Parameter | Type | Description | Requiered
--------- | ---- | ----------- | ---------
client_id | string | UUID v4 with max 36 characters<br>in case of getting `duplicated command id` send your request with new id | for every APIs
symbol | string | market symbol in uppercase with `SourceCurrency+BaseCurrency` format<br>Ex. `BTCTMN`, `BTCUSDT` | for every APIs
quantity | nbumber | amount of assets you want to sell | when `side == SELL`
side | enum | which side of history are you standing?<br>`[ BUY, SELL ]` | for every APIs
type | enum | what is your type?<br>`[ MARKET, LIMIT ]` | for every APIs
price | number | price of each asset you want to trade | when `type = LIMIT`
total_price | number | how much of base currency you want to give?  | when `side == BUY`

### Response

- order

    Parameter | Type | Description
    --------- | ---- | -----------
    id | number | increamental id of orders
    command_id | string | id of order that you placed
    symbol | string | markte symbol Ex. `TRXTMN`
    quantity | string | amount of asset that requested to sell
    price | string | price of each asset in case of `LIMIT` trade request
    total_price | string | total of currency recieving by trade
    filled_quantity | string | amount of order that filled
    filled_total_price | string | how much did you recieved so far
    type | enum | your order type<br>`[ MARKET, LIMIT ]`
    side | enum | side of history I mentioned earlier<br><sub><sup><sub><sup>if you don't remember go back and read the damn #*@#$%^! document this part is important, you don't bother yourself to read these then come and ask for support for what? for not reading this piece of text that I spent so much time writing instead of palying Dota</sup></sub></sup></sub><br>`[ BUY, SELL]`
    created_at | number | time of placing order
    maker_fee_coefficient | string | as you guessed, coefficient of maker fee
    taker_fee_coefficient | string | coefficient of taker fee, WOW such a supprise
    status | enum | what happed to your order, one of these:<br>`[ ACTIVE, FINISHED, CANCELED, FAILED ]`
    updated_at | number | time of last update on this entity
    stop_price | number | when it had to be stopped
    stop_price_triggered | bool | did it stopped on stopPrice or not

- trades
  
    list of trades that matched with your order

    Parameter | Type | Description
    --------- | ---- | -----------
    order_command_id | string | id of order 
    order_side | emun | side of order that matched<br>`[ BUY, SELL ]`
    trace_id | number | some number that is none of your concern
    fee_coefficient | number | coefficient of fee
    fee | number | amount of fee
    fee_currency | string | currency of fee
    total_price | number | total price of this order
    created_at | number | when did this order placed
    id | number | increamental id of trades
    symbol | string | market symbol
    side | enum | *uhhhh*, again same side of history joke<br>`[ BUY, SELL]`
    type | enum | another order type that matched with you<br>`[ P2P, OTC ]`<sup><sub>(if you're looking for similarity in types, even if you matched with her doesn't mean you share same tastes so use tinder instead of exchange)</sup><sub>
    price | number | price of order
    quantity | number | quantity of order
    reverted | bool | did it reverted?<br>if asking what does it mean, I don't know neither

## Get Orders History

```shell
curl "GET" \
    "https://api-trading.toparz.com/api/v1/account/orders?filter=YOUR_DESIERED_FILTERS&limit=10" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": [
    {
      "id": "string",
      "command_id": "string",
      "symbol": "string",
      "quantity": "string",
      "price": "string",
      "total_price": "string",
      "filled_quantity": "string",
      "filled_total_price": "string",
      "type": "MARKET",
      "side": "string",
      "created_at": "string",
      "maker_fee_coefficient": "string",
      "taker_fee_coefficient": "string",
      "status": "ACTIVE",
      "updated_at": "string",
      "stop_price": "string",
      "stop_price_triggered": "string"
    }
  ]
}
```

Here you can see history of your orders

### HTTP Request
`GET /api/v1/account/orders`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| filter | query | see [Filter Help](#filter-query-param-help) | No | string |
| limit | query | number of records | No | number |

### Response

Parameter | Type | Description
--------- | ---- | -----------
id | number | increamental id of orders
command_id | string | id of order that you placed
symbol | string | markte symbol Ex. `TRXTMN`
quantity | string | amount of asset that requested to sell
price | string | price of each asset in case of `LIMIT` trade request
total_price | string | total of currency recieving by trade
filled_quantity | string | amount of order that filled
filled_total_price | string | how much did you recieved so far
type | enum | your order type<br>`[ MARKET, LIMIT ]`
side | enum | at this point if still you don't know what is side, so I think you have to work on some of your personal skills. just becasuse Hooman will kill me here's a breif explanation:<br>`[ BUY, SELL]`
created_at | number | time of placing order
maker_fee_coefficient | string | yes, coefficient of maker fee
taker_fee_coefficient | string | and obviously coefficient of taker fee
status | enum | what happed to your order, one of these:<br>`[ ACTIVE, FINISHED, CANCELED, FAILED ]`
updated_at | number | time of last update on this entity
stop_price | number | when it had to be stopped
stop_price_triggered | bool | did it stopped on stopPrice or not

## Get Active Orders

```shell
curl "GET" \
    "https://api-trading.toparz.com/api/v1/account/orders/active?filter=YOUR_DESIERED_FILTERS&limit=10" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": [
    {
      "id": "string",
      "commandId": "string",
      "symbol": "string",
      "quantity": "string",
      "price": "string",
      "totalPrice": "string",
      "filledQuantity": "string",
      "filledTotalPrice": "string",
      "type": "MARKET",
      "side": "string",
      "createdAt": "string",
      "makerFeeCoefficient": "string",
      "takerFeeCoefficient": "string",
      "status": "ACTIVE",
      "updatedAt": "string",
      "stopPrice": "string",
      "stopPriceTriggered": "string"
    }
  ]
}
```

As title suggested give list of your active orders

### HTTP Request
`GET /api/v1/account/orders/active`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| filter | query | see [Filter Help](#filter-query-param-help) | No | string |
| limit | query | number of records | No | number |

### Response

Parameter | Type | Description
--------- | ---- | -----------
id | number | increamental id of orders
command_id | string | id of order that you placed
symbol | string | markte symbol Ex. `TRXTMN`
quantity | string | amount of asset that requested to sell
price | string | price of each asset in case of `LIMIT` trade request
total_price | string | total of currency recieving by trade
filled_quantity | string | amount of order that filled
filled_total_price | string | how much did you recieved so far
type | enum | your order type<br>`[ MARKET, LIMIT ]`
side | enum | a palceholder joke with side<br>`[ BUY, SELL]`
created_at | number | time of placing order
maker_fee_coefficient | string | as you guessed, coefficient of maker fee
taker_fee_coefficient | string | coefficient of taker fee, WOW such a supprise
status | enum | what happed to your order, one of these:<br>`[ ACTIVE, FINISHED, CANCELED, FAILED ]`
updated_at | number | time of last update on this entity
stop_price | number | when it had to be stopped
stop_price_triggered | bool | did it stopped on stopPrice or not

## Get Order Detail

```shell
curl "GET" \
    "https://api-trading.toparz.com/api/v1/account/orders/{client_order_id}" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": {
    "order": {
      "id": "string",
      "commandId": "string",
      "userId": "string",
      "symbol": "string",
      "quantity": "string",
      "price": "string",
      "totalPrice": "string",
      "filledQuantity": "string",
      "filledTotalPrice": "string",
      "type": "MARKET",
      "side": "string",
      "createdAt": "string",
      "makerFeeCoefficient": "string",
      "takerFeeCoefficient": "string",
      "status": "ACTIVE",
      "updatedAt": "string",
      "stopPrice": "string",
      "stopPriceTriggered": "string"
    },
    "trades": [
      {
        "order_command_id": "string",
        "order_side": "BUY",
        "user_id": "string",
        "trace_id": 0,
        "fee_coefficient": 0,
        "fee_currency": 0,
        "total_price": 0,
        "created_at": 0,
        "id": 0,
        "symbol": "string",
        "fee": 0,
        "side": "TAKER",
        "type": "P2P",
        "price": 0,
        "quantity": 0,
        "reverted": true
      }
    ]
  },
  "error_code": {
    "code": 0,
    "message": "string"
  }
}
```

Detail of order if it's already completed or canceled or is ongoing

### HTTP Request
`GET /api/v1/account/orders/{client_order_id}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| client_order_id | path | id of order you want | Yes | number |

### Response

- order

    Parameter | Type | Description
    --------- | ---- | -----------
    id | number | increamental id of orders
    command_id | string | id of order that you placed
    symbol | string | markte symbol Ex. `TRXTMN`
    quantity | string | amount of asset that requested to sell
    price | string | price of each asset in case of `LIMIT` trade request
    total_price | string | total of currency recieving by trade
    filled_quantity | string | amount of order that filled
    filled_total_price | string | how much did you recieved so far
    type | enum | your order type<br>`[ MARKET, LIMIT ]`
    side | enum | SIDE`[ BUY, SELL]`
    created_at | number | time of placing order
    maker_fee_coefficient | string | as you guessed, coefficient of maker fee
    taker_fee_coefficient | string | coefficient of taker fee, WOW such a supprise
    status | enum | what happed to your order, one of these:<br>`[ ACTIVE, FINISHED, CANCELED, FAILED ]`
    updated_at | number | time of last update on this entity
    stop_price | number | when it had to be stopped
    stop_price_triggered | bool | did it stopped on stopPrice or not

- trades

    list of trades that matched with your order

    Parameter | Type | Description
    --------- | ---- | -----------
    order_command_id | string | id of order 
    order_side | emun | side of order that matched<br>`[ BUY, SELL ]`
    trace_id | number | some number :)
    fee_coefficient | number | coefficient of fee
    fee | number | amount of fee
    fee_currency | string | currency of fee (do I need to explain all of these?)
    total_price | number | total price of this order
    created_at | number | when did this order placed
    id | number | increamental id of trades
    symbol | string | market symbol
    side | enum | **SIDE**<br>`[ BUY, SELL]`
    type | enum | order type that matched<br>`[ P2P, OTC ]`
    price | number | price of order
    quantity | number | quantity of order
    reverted | bool | did it reverted or not


## Cancel an Order by ID

```shell
curl "DELETE" \
    "https://api-trading.toparz.com/api/v1/account/orders/{client_order_id}" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": {
    "order": {
      "id": "string",
      "commandId": "string",
      "userId": "string",
      "symbol": "string",
      "quantity": "string",
      "price": "string",
      "totalPrice": "string",
      "filledQuantity": "string",
      "filledTotalPrice": "string",
      "type": "MARKET",
      "side": "string",
      "createdAt": "string",
      "makerFeeCoefficient": "string",
      "takerFeeCoefficient": "string",
      "status": "ACTIVE",
      "updatedAt": "string",
      "stopPrice": "string",
      "stopPriceTriggered": "string"
    }
  }
}
```

Have ever did something and immediately said [**OH! $#!+**](https://www.reddit.com/r/instant_regret/). don't worry I'm here just for you if you put a stupid order or a very super optimistic one or even if you are bipolar and don't know what you are doing with your life (who am I to judge) and want to cancel your previous orders, you can use me to cancel remaining orders.

### HTTP Request
`DELETE /api/v1/account/orders/{client_order_id}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| client_order_id | path | id of order you want to cancel | Yes | number |

### Response

data of *RIP* ed order *(F in the chat)*

Parameter | Type | Description
--------- | ---- | -----------
id | number | increamental id of orders
command_id | string | id of order that you placed
symbol | string | markte symbol Ex. `TRXTMN`
quantity | string | amount of asset that requested to sell
price | string | price of each asset in case of `LIMIT` trade request
total_price | string | total of currency recieving by trade
filled_quantity | string | amount of order that filled
filled_total_price | string | how much did you recieved so far
type | enum | your order type<br>`[ MARKET, LIMIT ]`
side | enum | I copy/past all of table and have to change this row, just tierd of generating new joke, side of order:<br>`[ BUY, SELL]`
created_at | number | time of placing order
maker_fee_coefficient | string | coefficient of maker fee
taker_fee_coefficient | string | coefficient of taker **checks note*  fee
status | enum | what happed to your order, one of these:<br>`[ ACTIVE, FINISHED, CANCELED, FAILED ]`
updated_at | number | time of last update on this entity
stop_price | number | when it had to be stopped
stop_price_triggered | bool | did it stopped on stopPrice before you kill him, how dare you?


## Cancel Order by Symbol

```shell
curl "DELTE" \
    "https://api-trading.toparz.com/api/v1/account/orders?symbol=BTCTMN" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": {
    "orders": [
      {
        "id": "string",
        "command_id": "string",
        "symbol": "string",
        "quantity": "string",
        "price": "string",
        "total_price": "string",
        "filled_quantity": "string",
        "filled_total_price": "string",
        "type": "MARKET",
        "side": "string",
        "created_at": "string",
        "maker_fee_coefficient": "string",
        "taker_fee_coefficient": "string",
        "status": "ACTIVE",
        "updated_at": "string",
        "stop_price": "string",
        "stop_price_triggered": "string"
      }
    ]
  }
}
```

You can undo your mistakes by its symbol too, but pay attention that it is possible only in our system.

### HTTP Request
`DELETE /api/v1/account/orders`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | query | market symbol of orders to cancel<br>not sending it will cancel all of your orders<br>`TRXUSDT` | No | string |

### Response

data of canceled order

Parameter | Type | Description
--------- | ---- | -----------
id | number | increamental id of orders
command_id | string | id of order that you placed
symbol | string | markte symbol Ex. `TRXTMN`
quantity | string | amount of asset that requested to sell
price | string | price of each asset in case of `LIMIT` trade request
total_price | string | total of currency recieving by trade
filled_quantity | string | amount of order that filled
filled_total_price | string | how much did you recieved so far
type | enum | your order type<br>`[ MARKET, LIMIT ]`
side | enum | side DIGE!<br>`[ BUY, SELL]`
created_at | number | time of placing order
maker_fee_coefficient | string | coefficient of maker fee
taker_fee_coefficient | string | coefficient of taker fee
status | enum | what happed to your order, one of these:<br>`[ ACTIVE, FINISHED, CANCELED, FAILED ]`
updated_at | number | time of last update on this entity
stop_price | number | when it had to be stopped
stop_price_triggered | bool | did it stopped on stopPrice before massacre

## Get Trades History

```shell
curl "GET" \
    "https://api-trading.toparz.com/api/v1/account/trades" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": [
    {
      "order_command_id": "string",
      "order_side": "BUY",
      "user_id": "string",
      "trace_id": 0,
      "fee_coefficient": 0,
      "fee_currency": 0,
      "total_price": 0,
      "created_at": 0,
      "id": 0,
      "symbol": "string",
      "fee": 0,
      "side": "TAKER",
      "type": "P2P",
      "price": 0,
      "quantity": 0,
      "reverted": true
    }
  ]
}
```

Show history of your trades

### HTTP Request
`GET /api/v1/account/trades`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| filter | query | see [Filter Help](#filter-query-param-help) | No | string |
| limit | query | number of records | No | number |

### Response

Parameter | Type | Description
--------- | ---- | -----------
order_command_id | string | id of order 
order_side | emun | side of order that matched<br>`[ BUY, SELL ]`
trace_id | number | some number that is none of your business
fee_coefficient | number | coefficient of fee
fee | number | amount of fee
fee_currency | string | currency of fee
total_price | number | total price of this order
created_at | number | when did this order placed
id | number | increamental id of trades
symbol | string | market symbol
side | enum | SIDE SIDE SIDE SIDE<br>`[ BUY, SELL]`
type | enum | trade type<br>`[ P2P, OTC ]`
price | number | price of order
quantity | number | quantity of order
reverted | bool | did it reverted or not

## Placing a Crypto Withdraw Request

```shell
curl "POST" \
    "https://api-trading.toparz.com/api/v1/account/crypto-withdraw" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```
> Body:

```json
{
  "client_id": "string",
  "symbol": "BTC",
  "amount": 0,
  "destination": "string",
  "network_id": "BITCOIN"
}
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true
}
```

In case of want to withdraw your assets from us you can use this API ([but please don't](https://youtu.be/XJFFgc9jZz0?si=7RKMxA_inmxX0MdN))

### HTTP Request
`POST /api/v1/account/crypto-withdraw`

### Body

Parameter | Type | Description
--------- | ---- | -----------
client_id | string | UUID v4 (max 36 characters)
symbol | string | symbol of currency you want Ex. `BTC`
amount | number | amount of your asset you want
destination | string | destination wallet
network_id | string | network id Ex. `BITCOIN`, `TRON`

## Get Crypto Withdraw History

```shell
curl "GET" \
    "https://api-trading.toparz.com/api/v1/account/crypto-withdraw" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": [
    {
      "id": "string",
      "client_id": "string",
      "date": "2024-07-23T12:27:34.235Z",
      "destination": "string",
      "amount": "string",
      "currency": "BTC",
      "network_name": "string",
      "network_id": "BITCOIN",
      "transaction_id": "string",
      "status": "DRAFT"
    }
  ],
  "meta": {
    "page": 1,
    "page_size": 10,
    "page_count": 56
  }
}
```

You can see how much money you withdraw <sub><sup>*\*cough* steal *\*cough*</sup></sub> from us.

### HTTP Request
`GET /api/v1/account/crypto-withdraw`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| order | query | sorting order<br>Ex. `created_at;ASC` | No | string |
| page | query | index of page you want | No | number |
| page_size | query | size of each page | No | number |
| currency_filter | query | one of<br>`[ BTC, ETH, TRX, USDT ]` | No | enum |
| network_filter | query | one of<br>`[ BITCOIN, ETHEREUM, TRON, DODGE ]` | No | enum |
| status_filter | query | one of<br>`[ DRAFT, IN_PROGRESS, COMPLETE, FAIL ]` | No | enum |

### Response

Parameter | Type | Description
--------- | ---- | -----------
id | string | id of your request
client_id | string | unique id that you sent with your request
date | Date | date of placing request
destination | string | destination wallet
amount | number | amount of asset for withdrawing
currency | string | currency of asset that you withdrawed
network_name | string | name of network
network_id | string | on which network it was
transaction_id | string | unique id for tracking
status | enum | state of your request

## Get a Withdraw Request Detail

```shell
curl "GET" \
    "https://api-trading.toparz.com/api/v1/account/crypto-withdraw/{client_id}" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": {
    "id": 0,
    "commandId": "string",
    "userId": "string",
    "currency": "string",
    "value": "string",
    "status": "IN_PROGRESS",
    "createdAt": 0,
    "updatedAt": 0
  }
}
```

Send client id and get detail of your withdraw request

### HTTP Request
`GET /api/v1/account/crypto-withdraw/{client_id}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| client_id | path | client id that you are searching for | Yes | string |

### Response

Parameter | Type | Description
--------- | ---- | -----------

## Get Crypto Deposit History

```shell
curl "GET" \
    "https://api-trading.toparz.com/api/v1/account/crypto-deposit" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": [
    {
      "date": "2024-07-23T13:06:41.778Z",
      "amount": "string",
      "currency": "BTC",
      "network_id": "BITCOIN",
      "network_name": "string",
      "transaction_id": "string",
      "status": "IN_PROGRESS"
    }
  ],
  "meta": {
    "page": 0,
    "page_size": 0,
    "page_count": 0
  }
}
```

History of deposits in your dedicated wallet

### HTTP Request
`GET /api/v1/account/crypto-deposit`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| order | query | sorting order<br>Ex. `created_at;ASC` | No | string |
| page | query | index of page you want | No | number |
| page_size | query | size of each page | No | number |

### Response

Parameter | Type | Description
--------- | ---- | -----------
date | Date | time of deposit happening
amount | number | amount of asset deposited
currency | string | currency of deposit
network_id | string | in which network it was
network_name | string | name of that network
transaction_id | string | id to track trasaction
status | enum | state of transaction<br>`[ IN_PROGRESS, DONE ]`


# Stat API

Everythings that are related to our system statistics that may be useful to you :wink: :heart:

## Get UDF <sub><sup><sub><sup><sub><sup>what the hell does it mean?</sup></sub></sup></sub></sup></sub>

```shell
curl "GET" \
    "https://api-trading.toparz.com/api/v1/udf" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": [
    {
      "start_time": "string",
      "open_price": "string",
      "highest_price": "string",
      "lowest_price": "string",
      "close_price": "string",
      "trade_volume": "string",
      "trade_total_price": "string",
      "symbol": "string",
      "resolution": "MINUTELY"
    }
  ]
}
```

Don't know what is it doing!

### HTTP Request
`GET /api/v1/udf`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | query | symbol of market | Yes | string
| resolution | query | `[ MINUTELY, HOURLY, DAILY ]` | Yes | enum
| start_time | query | start time obviously | Yes | number
| end_time | query | end of period | Yes | number
| limit | query | number of record limit | No | number


### Response

Parameter | Type | Description
--------- | ---- | -----------
start_time | ? | ?
open_price | ? | ?
highest_price | ? | ?
lowest_price | ? | ?
close_price | ? | ?
trade_volume | ? | ?
trade_total_price | ? | ?
symbol | string | this data is for that symbol
resolution | enum | `[ MINUTELY, HOURLY, DAILY ]`

## Get Market Depth

```shell
curl "GET" \
    "https://api-trading.toparz.com/api/v1/depth/{symbol}" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": [
    {
      "asks": [
        {
          "price": 0,
          "quantity": 0
        }
      ],
      "bids": [
        {
          "price": 0,
          "quantity": 0
        }
      ]
    }
  ]
}
```

Get metket depth base on market

### HTTP Request
`GET /api/v1/depth/{symbol}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | market symbol you're looking for | Yes | string

### Response

Parameter | Type | Description
--------- | ---- | -----------
asks | array | sell orders
bids | array | buy orders

## Get Last Trades Summery

```shell
curl "GET" \
    "https://api-trading.toparz.com/api/v1/latest-trade-summaries/{symbol}" \
    -H "Authorization: Bearer <PUT_YOUR_TOKEN_HERE>"
```

> The above command returns JSON structured like this:

```json
{
  "succeed": true,
  "data": [
    {
      "created_at": 0,
      "taker_order_side": "BUY",
      "price": 0,
      "quantity": 0,
      "symbol": "string"
    }
  ]
}
```

lasest trades based on a market

### HTTP Request
`GET /api/v1/latest-trade-summaries/{symbol}`

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| symbol | path | market symbol you're looking for | Yes | string

### Response

Parameter | Type | Description
--------- | ---- | -----------
created_at | number | when it placed
taker_order_side | enum | finally it's last side I'm writing (for now)<br>`[ BUY, SELL ]`
price | number | price of it
quantity | number | amount of it
symbol | string | symbol of it