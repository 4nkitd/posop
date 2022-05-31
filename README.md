# Standard

The motive of this is to enforce a way to communicate with all available POS machine in a way that is common for all pos machine and make payment with online mediums possible with the least amount of work possible.  

# Security

having worked in fin-tech for quite some time it is clear that the security is a must have keeping in mind no matter what security we implement it's going to be broken if not today then in a hundred years or later. keeping that in mind our implementation doesn't share any payment related information that can be used to spoof payment success or failure. it just not possible. `Don't believe me give it a try.`

## POS - Online Payment Standard

> This will allow users to pay at pos machines with their phones using all available payment options of the given payment gateway.

---

### Flow

- Step 1 :  Open the app with sdk implementation
- Step 2 : TaP Mobile on POS machine which is already asking payment
- Step 3 : SDK will be emitting a json object which will be - application name and a network_id
- Step 4 : POS machine will receive the json and response back with order id and a url.
- Step 5 : Sdk will receive the url and order id
- Step 6 : Sdk will allow to integrate any payment gateway and use the given url as callback

# [SDK2POS]

# Request object from SDK to POS using NFC

```json
{
 "network_id": "pay_app",
 "customer_names": "paytring",
 "customer_phone": "9898998989",
 "customer_email": "user@example.com"
}
```

NOTE : all listed below fields are at a
MAX_L_255 = max length of 255
ALPHA_NUM = [a-zA-Z0-9]
COMMON_SPEC =  [@+.]

## Request Params

| Param | Description | Example Value | Validation |
|--------|------------|---------------|------------|
| network_id | This is supposes to be an identifier for the app | pay_app_123 | MAX_L_255, ALPHA_NUM, COMMON_SPEC |
| customer_name | This is a self explainers field | Jhon Doe | MAX_L_255, ALPHA_NUM |
| customer_phone | This is a self explainers field | 9898989898 | MAX_L_255, NUMERIC |
| customer_email | This is a self explainers field | user[at]example.com | MAX_L_255, ALPHA_NUM, COMMON_SPEC |

# [POS2SDK]

# Response object from POS to SDK using NFC

NOTE : all listed below fields are compliant with these validation

- max length 255 validation
- alpha numeric values
- included special chars [ /:.-_ ]

```json
{
 "callback": "https://machine.pay.in/order/o1o2o3o4",
 "order_id": "o1o2o3o4",
 "payment": "https://online.pay.in/order/o1o2o3o4"
}
```

## Callback

once the payment is complete the sdk is supposed to hit callback url without any data and intern the POS Network is supposed to check the payment status.

## Order Id

this is the identifier which the app can use to ping pos apis to get more info about order.

## Payment

this is supposed to be the url for payment gateway which comes pre integrated with the POS Machine the application just need to open it in a webview for this.

# [ POSAOSC ]

# POS Auto Order Status Check

as soon as the pos receive the customer info it create an order at the POS backend server and return the sdk a repose. after that the pos machine can implement a poling system or a websocket to check the order payment status.
