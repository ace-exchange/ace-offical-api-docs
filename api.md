
# General API Information
* The openapi endpoint is: https://ace.io/polarisex/oapi
* The base endpoint is: https://ace.io/polarisex/open/v1
* All endpoints return either a JSON object or array.
* All time and timestamp related fields are in milliseconds.
* For GET endpoints, parameters must be sent as a query string.
* For POST, PUT, and DELETE endpoints, the parameters may be sent as a query string or in the request body with content type application/x-www-form-urlencoded.
* You may mix parameters between both the query string and request body if you wish to do so.
* Parameters may be sent in any order.

# Signing_API_Requests
Important Note: Do not reveal your 'apiKey' and 'securityKey' to anyone. They are as important as your password.

To prevent the request(s) from being tempered in the process of network transmission, signature authentication is required for your API Key for the private interface, which guarantees that you are the source of the request(s). A legal ACE signature consists of parameters connected by “&” in alphabetical order, and your api_secret, through `MD5` method. The signature needed to be placed in the parameter sign.

* Three parameters are required to be uploaded, including ACE_SIGN, timestamp and phone number, all are involved in signature expect forsign.
* Follow the rule to combine the parameters : `ACE_SIGN + timestamp + phone number`.
* If your `phone number` is `0886123456789`, and the current thirteen-digit `timestamp` is `1234567890000`, then your will get `ACE_SIGN12345678900000886123456789`.
* Signing the obtained string with the MD5 digest algorithm results: `ca2aa2ff46d309ee9abeee2951ae8d27`. Place this result into `signKey` parameter.


# ENUM definitions
### Kline/Candlestick chart intervals:
m -> minutes; h -> hours; d -> days; w -> weeks; M -> months
* 1m
* 5m
* 10m
* 30m
* 1h
* 2h
* 4h
* 8h
* 12h
* 1d
* 1w
* 1M
### Order side:
* BUY
* SELL
### Currency ID
* TWD＝1
* BTC＝2
* ETH＝4
* LTC＝7
* XRP＝10
* EOS＝11
* XLM＝12
* TRX＝13
* USDT＝14
* BNB＝17
* BTT＝19
* HWGC＝22
* GTO＝54
* USDC＝57
* MOT＝58
* UNI＝59
* MOS＝65
* MOCT＝66
* PT＝67
* SOX＝68
* DET＝70
* SOLO＝71
* QQQ＝72
* APT＝73
* HT＝74
* UNI＝75
* QTC＝76
* STC＝78
* MCO＝79
* FTT＝81
* BAAS＝83
* OKB＝84
* DAI＝85
* MCC＝86
* TACEX＝87
* ACEX＝88
* LINK＝89
* DEC＝90
* FANSI＝91
* PANDA＝92
* HWGC＝93
* KNC＝94
* COMP＝95
* DS＝96
* CRO＝97

# OPEN API - Trade Price
    GET /list/tradePrice
### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| base_volume | Float |  |
| last_price | Float |  |
| quote_volume | Float |  |

    {
        "BTC/USDT":{
            "base_volume":229196.34035399999,
            "last_price":11881.06,
            "quote_volume":19.2909
        }
     }


# OPEN API - Market Pair
    GET /list/marketPair
### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| market_pair | String Array |  |

    {
        "market_pair":["BTC/TWD","ETH/TWD","USDT/TWD","USDC/TWD"]
    }

# OPEN API - Order Books
    GET /list/orderBooks/<currency_name>/<basecurrency_name>
### Response:
    {
        "market_pair":"BTC/TWD",
        "orderbook": {
        "asks": [
            [0.0042,359370.9],[0.0005,360882.3]
            ]
        }
    }

# OPEN API - 24Hours Trade History
    GET /list/trades/<currency_name>/<basecurrency_name>
### Response:

    {
    "attachment": {
        "total": 3,
        "list": [{
            "tradeId": 10140160,
            "price": 30.3547,
            "baseVolume": 242.8376,
            "quoteVolume": 8.0,
            "timestamp": 1606717020522,
            "type": "buy"
        }, {
            "tradeId": 10140159,
            "price": 30.3547,
            "baseVolume": 151.7735,
            "quoteVolume": 5.0,
            "timestamp": 1606717014489,
            "type": "buy"
        }, {
            "tradeId": 10140158,
            "price": 30.3547,
            "baseVolume": 151.7735,
            "quoteVolume": 5.0,
            "timestamp": 1606717009256,
            "type": "buy"
        }] },
        "message": null,
        "parameters": null,
        "status": 200
    }

# Account Balance
    POST /coin/customerAccount
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| uid | Long | YES |
| timeStamp | Long | YES |
| signKey | String | YES |
| apiKey | String | YES |
| securityKey | String | YES |

### Response:

    {
        "currencyId": 4,
        "amount": 6.896,
        "cashAmount": 6.3855,
        "uid": 000,
        "currencyNameEn": "BTC"
    }

# Kline/Candlestick data
    POST /kline/getKlineMin
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| uid | Long | YES |
| timeStamp | Long | YES |
| signKey | String | YES |
| apiKey | String | YES |
| securityKey | String | YES |
| tradeCurrencyId | INT | YES |
| baseCurrencyId | INT | YES |
| limit | INT | NO | 1~2000

### Response:

    {
        "changeRate": 0,
        "closePrice": 101000.0,
        "lowPrice": 101000.0,
        "highPrice": 101000.0,
        "highPrice": 1573195740000L,
        "openPrice": 101000.0,
        "current": 101000.0,
        "createTime": "2019-11-08 14:49:00"
    }

# New Order
    POST /order/order

### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| baseCurrencyId | INT | YES |
| currencyId | INT | YES |
| buyOrSell | INT | YES | 1 Buy;2 Sell |
| price | INT | YES |
| num | INT | YES |
| type | INT | YES | 1 limit order; 2 market order |
| apiKey | STRING | YES |
| securityKey | STRING | YES |
| uid | STRING | YES |
| timeStamp | Long | YES |
| signKey | String | YES |
| fdPassword | String | NO | Depends on user's security setting of fund password is required. |

### Response:

    {
        "attachment": "15697850529570392100421100482693",
        "message": null,
        "parameters": null,
        "status": 200
    }

# Cancel Order
    POST /order/cancel
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| orderNo | STRING | YES |
| apiKey | STRING | YES |
| securityKey | STRING | YES |
| uid | STRING | YES |
| timeStamp | Long | YES |
| signKey | String | YES |

### Response:

    {
        "attachment": 200,
        "message": null,
        "parameters": null,
        "status": 200
    }

OR

    {
        "attachment": null,
        "message": "委託單已成交或已撤銷",
        "parameters": null,
        "status": 2061
    }

# Order Status
    POST /order/getOrderList
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| uid | Long | YES |
| timeStamp | Long | YES |
| signKey | String | YES |
| apiKey | String | YES |
| securityKey | String | YES |
| baseCurrencyId | INT | NO |
| tradeCurrencyId | INT | NO |
| start | INT | NO | Default 1
| size | INT | NO | 1-100 Default 10

### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| status | INT | 0 unsettled; 1 partial; 2 fill |
| buyOrSell | INT | 1 Buy; 2 Sell |

    {
      "attachment": [
        {
          "uid": 0,
          "orderNo": "16113081376560890227301101413941",
          "orderTime": "2021-01-22 17:35:37",
          "orderTimeStamp": 1611308137656,
          "baseCurrencyId": 1,
          "baseCurrencyNameEn": "TWD",
          "currencyId": 14,
          "currencyNameEn": "USDT",
          "buyOrSell": "1",
          "num": "6.0000000000000000",
          "price": "32.5880000000000000",
          "remainNum": "2.0000000000000000",
          "tradeNum": "4.0000000000000000",
          "tradePrice": "31.19800000000000000000",
          "tradeAmount": "124.7920000000000000",
          "tradeRate": "0.66666666666666666667",
          "status": 1,
          "type": 1
        }
      ],
      "message": null,
      "parameters": null,
      "status": 200
    }    

# Order OrderStatus
    POST /order/showOrderStatus
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| orderId | STRING | YES |
| apiKey | STRING | YES |
| securityKey | STRING | YES |
| uid | STRING | YES |
| timeStamp | Long | YES |
| signKey | String | YES |

### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| status | INT | 0 unsettled; 1 partial; 2 fill |
| buyOrSell | INT | 1 Buy; 2 Sell |

    {
        "attachment": {
            "order": {
                "remainNum": "0.00000000",
                "orderNo": "15681910422154042100431100441305",
                "num": "0.85000000",
                "tradeNum": "0.85000000",
                "baseCurrencyId": 2,
                "baseCurrencyName": "Bitcoin",
                "buyOrSell": 1,
                "orderTime": "2019-09-11 16:37:22.216",
                "currencyName": "Ethereum",
                "price": "0.03096500",
                "averagePrice": "0.03096500",
                "currencyId": 4,
                "status": 2
            }
        },
        "message": null,
        "parameters": null,
        "status": 200
    }

# Order History
    POST /order/showOrderHistory
### Parameters:
| Name | Type | Mandatory | Description |
| ---- | ---- | ---- | ---- |
| orderId | STRING | YES |
| apiKey | STRING | YES |
| securityKey | STRING | YES |
| uid | STRING | YES |
| timeStamp | Long | YES |
| signKey | String | YES |

### Response:
| Name | Type | Description |
| ---- | ---- | ---- |
| status | INT | 0 unsettled; 1 partial; 2 fill |
| buyOrSell | INT | 1 Buy; 2 Sell |

    {
        "attachment": {
            "trades": [
                {
                    "amount": 0.0030965,
                    "tradeNo": "15681920522485652100751000417788",
                    "price": "0.03096500",
                    "num": "0.10000000",
                    "bi": 1,
                    "time": "2019-09-11 16:54:12.248"
                },
                {
                    "amount": 0.02322375,
                    "tradeNo": "15682679767395912100751000467937",
                    "price": "0.03096500",
                    "num": "0.75000000",
                    "bi": 1,
                    "time": "2019-09-12 13:59:36.739"
                }
            ],
            "order": {
                "remainNum": "0.00000000",
                "orderNo": "15681910422154042100431100441305",
                "num": "0.85000000",
                "tradeNum": "0.85000000",
                "baseCurrencyId": 2,
                "baseCurrencyName": "Bitcoin",
                "buyOrSell": 1,
                "orderTime": "2019-09-11 16:37:22.216",
                "currencyName": "Ethereum",
                "price": "0.03096500",
                "averagePrice": "0.03096500",
                "currencyId": 4,
                "status": 2
            }
        },
        "message": null,
        "parameters": null,
        "status": 200
    }
