Draft Order
=====

A draft order is created when the merchant manages a pre-sale made outside the platform. Draft orders also can be created through the API.

#### Table of Contents
>[Get a draft order collection](#GET-draft-orders)
>
>[Get a draft order](#GET-draft-ordersid)
> 
>[Create a draft order](#POST-draft-orders)
>
>[Confirm a draft order](#POST-draft-ordersidconfirm)
>
>[Delete a draft order](#DELETE-draft-ordersid)
>

Properties
----------

| Property                   | Explanation                                                                                              |
| -------------------------- | -------------------------------------------------------------------------------------------------------- |
| id                         | The unique numeric identifier for the Draft Order.                                                       |
| store_id                   | The unique numeric identifier for the Store.                                                             |
| storefront                 | Name that refers to the means by which the draft order was managed.                                      |
| hash                       | Specifies the location of the Draft Order.                                                               |
| currency                   | The total spent's currency in [ISO 4217 format](http://en.wikipedia.org/wiki/ISO_4217).                  |
| lang                       | Draft Order's language used by the customer during the checkout process.                                 |
| contact_name               | Customer's full name.                                                                                    |
| contact_email              | Customer contact email.                                                                                  |
| contact_phone              | Customer contact phone.                                                                                  |
| cpf_cnpj                   | Customer identity number.                                                                                |
| owner_note                 | Store owner's note about the draft order.                                                                |
| shipping_method            | Shipping method chosen by the consumer during the checkout process.                                      |
| shipping_option            | The shipping option chosen by the customer during the checkout process.                                  |
| shipping_option_code       | The shipping option code selected by the consumers.                                                      |
| shipping_pickup_type       | "ship" if the order is going to be shipped; "pickup" if it's going to be picked up from a store branch.  |
| shipping_name              | Customer's name who will receive the order.                                                              |
| shipping_last_name         | Customer's lastname who will receive the order.                                                          |
| shipping_phone             | Customer's phone who will receive the order.                                                             |
| shipping_address           | Customer's shipping address where the order will be shipped.                                             |
| shipping_number            | Customer's shipping address number where the order will be shipped.                                      |
| shipping_floor             | Customer's shipping floor where the order will be shipped.                                               |
| shipping_locality          | Customer's shipping locality where the order will be shipped.                                            |
| shipping_city              | Customer's shipping city where the order will be shipped.                                                |
| shipping_zipcode           | Customer's shipping zipcode where the order will be shipped.                                             |
| shipping_province          | Customer's shipping province where the order will be shipped.                                            |
| shipping_country           | Customer's shipping country where the order will be shipped.                                             |
| shipping_extra             | Additional shipping information in JSON format.                                                          |
| shipping_cost              | The shipping cost the customer has to pay to the store owner.                                            |
| shipping_cost_owner        | The shipping cost the store owner has to pay to the shipping company.                                    |
| shipping_additional_days   | Additional days that are added to those of the shipment. It is set as 0.                                 |          
| gateway                    | Payment gateway code. It is set as not provided.                                                         |
| total                      | Total price of the draft order including shipping and discounts.                                         |
| initiated_by               | Unique identifier number that indicates the user who started the Draft Order.                            |
| internal_extra             | Additional draft order information. Includes the sales channel.                                          |
| checkout_url               | Url of the checkout corresponding to the draft order.                                                    |
| started_checkout           | Date the checkout process started in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601).           |
| updated_at                 | Date when the Draft Order was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601).       |
| created_at                 | Date when the Draft Order was last updated in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601).  |

Endpoints
---------

### GET /draft-orders

Get a draft order collection.

#### GET /draft-orders

`HTTP/1.1 200 OK`

```json
{
   "body":[
      {
         "id":1,
         "token":"234ae927c2248a18e9ae3b5a6e9a19e38ae26e96",
         "store_id":"1",
         "shipping_min_days":null,
         "shipping_max_days":null,
         "billing_name":null,
         "billing_phone":null,
         "billing_address":null,
         "billing_number":null,
         "billing_floor":null,
         "billing_locality":null,
         "billing_zipcode":null,
         "billing_city":null,
         "billing_province":null,
         "billing_country":null,
         "shipping_cost_owner":"0.00",
         "shipping_cost_customer":"0.00",
         "coupon":[],
         "promotional_discount":{
            "id":null,
            "store_id":1,
            "order_id":"1",
            "created_at":"2020-11-19T22:42:38+0000",
            "total_discount_amount":"0.00",
            "contents":[],
            "promotions_applied":[]
         },
         "subtotal":"1000.00",
         "discount":"0.00",
         "discount_coupon":"0.00",
         "discount_gateway":"0.00",
         "total":"1000.00",
         "total_usd":"0.00",
         "checkout_enabled":true,
         "weight":"0.200",
         "currency":"ARS",
         "language":"es",
         "gateway":"not-provided",
         "gateway_id":null,
         "shipping":"draft",
         "shipping_option":"¡Te vamos a contactar para coordinar la entrega!",
         "shipping_option_code":"draft_99999",
         "shipping_option_reference":null,
         "shipping_pickup_details":null,
         "shipping_tracking_number":null,
         "shipping_tracking_url":null,
         "shipping_store_branch_name":null,
         "shipping_pickup_type":"ship",
         "shipping_suboption":[],
         "extra":{},
         "storefront":"form",
         "note":null,
         "created_at":"-0001-11-30T00:00:00+0000",
         "updated_at":"2020-11-19T22:42:19+0000",
         "completed_at":{
            "date":"-0001-11-30 00:00:00.000000",
            "timezone_type":3,
            "timezone":"UTC"
         },
         "next_action":"noop",
         "payment_details":{
            "method":null,
            "credit_card_company":null,
            "installments":"1"
         },
         "attributes":[],
         "customer":null,
         "products":[
            {
               "id":1,
               "depth":"0.00",
               "height":"0.00",
               "name":"My first product",
               "price":"1000.00",
               "product_id":1,
               "image":{
                  "id":1,
                  "product_id":1,
                  "src":"https://d26lpennugtm8s.cloudfront.net/stores/001/330/104/products/my-first-product.jpg",
                  "position":1,
                  "alt":[],
                  "created_at":"2020-08-31T17:41:45+0000",
                  "updated_at":"2020-08-31T17:42:13+0000"
               },
               "quantity":1,
               "free_shipping":false,
               "weight":"0.00",
               "width":"0.00",
               "variant_id":1,
               "variant_values":[
                  "Variant"
               ],
               "properties":[   ],
               "sku":null,
               "barcode":null
            }
         ],
         "clearsale":{
            "CodigoIntegracao":false,
            "IP":null,
            "Estado":"C"
         },
         "number":0,
         "cancel_reason":null,
         "owner_note":"",
         "cancelled_at":null,
         "closed_at":null,
         "read_at":null,
         "status":"open",
         "payment_status":"pending",
         "shipping_address":null,
         "shipping_status":"unpacked",
         "shipped_at":null,
         "paid_at":null,
         "landing_url":null,
         "client_details":{
            "browser_ip":null,
            "user_agent":null
         },
         "app_id":null,
         "checkout_url":"https://mystore.mitiendanube.com/checkout/v3/start/325505360/234ae927c2248a18e9ae3b5a6e9a19e38ae26e96?from_store=1"
      }
   ],
   "page":{
      "total_elements":1,
      "size":50,
      "total_pages":1,
      "number":0
   },
   "total_elements":1,
   "paginator":{
      "results":[
         {
            "id":1,
            "token":"234ae927c2248a18e9ae3b5a6e9a19e38ae26e96",
            "store_id":"1",
            "shipping_min_days":null,
            "shipping_max_days":null,
            "billing_name":null,
            "billing_phone":null,
            "billing_address":null,
            "billing_number":null,
            "billing_floor":null,
            "billing_locality":null,
            "billing_zipcode":null,
            "billing_city":null,
            "billing_province":null,
            "billing_country":null,
            "shipping_cost_owner":"0.00",
            "shipping_cost_customer":"0.00",
            "coupon":[],
            "promotional_discount":{
               "id":null,
               "store_id":1,
               "order_id":"1",
               "created_at":"2020-11-19T22:42:38+0000",
               "total_discount_amount":"0.00",
               "contents":[],
               "promotions_applied":[]
            },
            "subtotal":"1000.00",
            "discount":"0.00",
            "discount_coupon":"0.00",
            "discount_gateway":"0.00",
            "total":"1000.00",
            "total_usd":"0.00",
            "checkout_enabled":true,
            "weight":"0.200",
            "currency":"ARS",
            "language":"es",
            "gateway":"not-provided",
            "gateway_id":null,
            "shipping":"draft",
            "shipping_option":"¡Te vamos a contactar para coordinar la entrega!",
            "shipping_option_code":"draft_99999",
            "shipping_option_reference":null,
            "shipping_pickup_details":null,
            "shipping_tracking_number":null,
            "shipping_tracking_url":null,
            "shipping_store_branch_name":null,
            "shipping_pickup_type":"ship",
            "shipping_suboption":[],
            "extra":{},
            "storefront":"form",
            "note":null,
            "created_at":"-0001-11-30T00:00:00+0000",
            "updated_at":"2020-11-19T22:42:19+0000",
            "completed_at":{
               "date":"-0001-11-30 00:00:00.000000",
               "timezone_type":3,
               "timezone":"UTC"
            },
            "next_action":"noop",
            "payment_details":{
               "method":null,
               "credit_card_company":null,
               "installments":"1"
            },
            "attributes":[],
            "customer":null,
            "products":[
               {
                  "id":1,
                  "depth":"0.00",
                  "height":"0.00",
                  "name":"My first product",
                  "price":"1000.00",
                  "product_id":1,
                  "image":{
                     "id":1,
                     "product_id":1,
                     "src":"https://d26lpennugtm8s.cloudfront.net/stores/001/330/104/products/my-first-product.jpg",
                     "position":1,
                     "alt":[],
                     "created_at":"2020-08-31T17:41:45+0000",
                     "updated_at":"2020-08-31T17:42:13+0000"
                  },
                  "quantity":1,
                  "free_shipping":false,
                  "weight":"0.00",
                  "width":"0.00",
                  "variant_id":1,
                  "variant_values":[
                     "Variant"
                  ],
                  "properties":[],
                  "sku":null,
                  "barcode":null
               }
            ],
            "clearsale":{
               "CodigoIntegracao":false,
               "IP":null,
               "Estado":"C"
            },
            "number":0,
            "cancel_reason":null,
            "owner_note":"",
            "cancelled_at":null,
            "closed_at":null,
            "read_at":null,
            "status":"open",
            "payment_status":"pending",
            "shipping_address":null,
            "shipping_status":"unpacked",
            "shipped_at":null,
            "paid_at":null,
            "landing_url":null,
            "client_details":{
               "browser_ip":null,
               "user_agent":null
            },
            "app_id":null,
            "checkout_url":"https://mystore.mitiendanube.com/checkout/v3/start/325505360/234ae927c2248a18e9ae3b5a6e9a19e38ae26e96?from_store=1"
         }
      ],
      "page":1,
      "last":1,
      "total":1,
      "per_page":50
   }
}
```

### GET /draft-orders/{id}

Get a draft order.

#### GET /draft-orders/1

`HTTP/1.1 200 OK`

```json
{
   "id":1,
   "token":"234ae927c2248a18e9ae3b5a6e9a19e38ae26e96",
   "store_id":"1",
   "shipping_min_days":null,
   "shipping_max_days":null,
   "billing_name":null,
   "billing_phone":null,
   "billing_address":null,
   "billing_number":null,
   "billing_floor":null,
   "billing_locality":null,
   "billing_zipcode":null,
   "billing_city":null,
   "billing_province":null,
   "billing_country":null,
   "shipping_cost_owner":"0.00",
   "shipping_cost_customer":"0.00",
   "coupon":[],
   "promotional_discount":{
      "id":null,
      "store_id":1,
      "order_id":"1",
      "created_at":"2020-11-19T22:42:38+0000",
      "total_discount_amount":"0.00",
      "contents":[],
      "promotions_applied":[]
   },
   "subtotal":"100.00",
   "discount":"0.00",
   "discount_coupon":"0.00",
   "discount_gateway":"0.00",
   "total":"1000.00",
   "total_usd":"0.00",
   "checkout_enabled":true,
   "weight":"0.200",
   "currency":"ARS",
   "language":"es",
   "gateway":"not-provided",
   "gateway_id":null,
   "shipping":"draft",
   "shipping_option":"¡Te vamos a contactar para coordinar la entrega!",
   "shipping_option_code":"draft_99999",
   "shipping_option_reference":null,
   "shipping_pickup_details":null,
   "shipping_tracking_number":null,
   "shipping_tracking_url":null,
   "shipping_store_branch_name":null,
   "shipping_pickup_type":"ship",
   "shipping_suboption":[
      
   ],
   "extra":{},
   "storefront":"form",
   "note":null,
   "created_at":"-0001-11-30T00:00:00+0000",
   "updated_at":"2020-11-19T22:42:19+0000",
   "completed_at":{
      "date":"-0001-11-30 00:00:00.000000",
      "timezone_type":3,
      "timezone":"UTC"
   },
   "next_action":"noop",
   "payment_details":{
      "method":null,
      "credit_card_company":null,
      "installments":"1"
   },
   "attributes":[],
   "customer":null,
   "products":[
      {
         "id":1,
         "depth":"0.00",
         "height":"0.00",
         "name":"My first product",
         "price":"1000.00",
         "product_id":1,
         "image":{
            "id":1,
            "product_id":1,
            "src":"https://d26lpennugtm8s.cloudfront.net/stores/001/330/104/products/my-first-product.jpg",
            "position":1,
            "alt":[
               
            ],
            "created_at":"2020-08-31T17:41:45+0000",
            "updated_at":"2020-08-31T17:42:13+0000"
         },
         "quantity":1,
         "free_shipping":false,
         "weight":"0.00",
         "width":"0.00",
         "variant_id":1,
         "variant_values":[
            "Variant"
         ],
         "properties":[],
         "sku":null,
         "barcode":null
      }
   ],
   "clearsale":{
      "CodigoIntegracao":false,
      "IP":null,
      "Estado":"C"
   },
   "number":0,
   "cancel_reason":null,
   "owner_note":"",
   "cancelled_at":null,
   "closed_at":null,
   "read_at":null,
   "status":"open",
   "payment_status":"pending",
   "shipping_address":null,
   "shipping_status":"unpacked",
   "shipped_at":null,
   "paid_at":null,
   "landing_url":null,
   "client_details":{
      "browser_ip":null,
      "user_agent":null
   },
   "app_id":null,
   "checkout_url":"https://mystore.mitiendanube.com/checkout/v3/start/325505360/234ae927c2248a18e9ae3b5a6e9a19e38ae26e96?from_store=1"
}
```

### POST /draft-orders

Create a draft order.

| Parameter          | Description                                                                                     |  Type    | Required |
|--------------------| ------------------------------------------------------------------------------------------------|----------|----------|
| contact_name       | Customer's name.                                                                                |  string  | Yes      |
| contact_lastname   | Customer's lastname.                                                                            |  string  | Yes      |
| contact_email      | Customer contact email.                                                                         |  E-mail  | Yes      |
| contact_phone      | Customer contact phone.                                                                         |  string  | No       |
| cpf_cnpj           | Customer identity number.                                                                       |  string  | No       |
| payment_status     | Draft Order's payment status. Possible values are "unpaid", "pending_confirmation" and "paid".  |  string  | Yes      |
| sale_channel       | Sales channel name.                                                                             |  string  | No       |
| note               | Store owner's note about the draft order.                                                       |  string  | No       |
| products           | Draft Order's products list ([Product](#Product)).                                              |  array   | Yes      |
| discount           | Discount amount applied to the draft order.                                                     |  string  | No       |
| shipping           | Shipping information ([Shipping](#Shipping)).                                                   |  array   | No       |
                                              
                                              
#### Objects

##### Product
| Value              | Description                                                       | Type    | Required |
|--------------------|-------------------------------------------------------------------|---------|----------|
| variant_id         | The product variant ID.                                           | Integer | Yes      |
| quantity           | The product quantity.                                             | Integer | Yes      |


##### Shipping

| Value              | Description                                                      | Type   | Required |
|--------------------|------------------------------------------------------------------|--------|----------|                                     
| cost               | The shipping cost the customer has to pay to the store owner.    | string | No       |
| shipping_address   | Shipping address information ([Shipping Address](#Address)).     | array  | No       |


##### Address

| Value              | Description                                                      | Type   | Required |
|--------------------|------------------------------------------------------------------|--------|----------|                                                   
| address            | The customer's street.                                           | String | No       |
| number             | The address's number.                                            | String | No       |
| floor              | The address's complement.                                        | String | No       |
| locality           | The address's locality.                                          | String | No       | 
| city               | The address's city.                                              | String | No       |
| province           | The address's province.                                          | String | No       |
| zipcode            | The address's postal code.                                       | String | No       |


##### Payment Status

| Value                 | Description                                         |
|-----------------------|-----------------------------------------------------|
| unpaid                | The payment hasn't yet been made                    |
| pending_confirmation  | The payment confirmation is pending                 |
| paid                  | The payment was successfully confirmed and captured |

#### POST /draft-orders

`HTTP/1.1 201 Created`

```json
{
   "id": "39138174",
   "store_id": 1,
   "storefront": "api",
   "hash": "5be919b12255a1823d92869d40c9c3bda4503e37",
   "currency": "ARS",
   "lang": "es_AR",
   "contact_name": "My Example",
   "contact_email": "my.example@gmail.com",
   "contact_phone":"4678-1232",
   "cpf_cnpj": "12345678",
   "owner_note": "My first draft order created",
   "shipping_method": "draft",
   "shipping_option": "Te vamos a contactar para coordinar la entrega!",
   "shipping_option_code": "99999",
   "shipping_pickup_type": "ship",
   "shipping_name": "My",
   "shipping_last_name": "Example",
   "shipping_phone": "4678-1232",
   "shipping_address": "Sta Rosa",
   "shipping_number": "1535",
   "shipping_floor": "",
   "shipping_city": "Moron",
   "shipping_locality": "Castelar",
   "shipping_zipcode": "1712",
   "shipping_province": "Gran Buenos Aires",
   "shipping_country": 10,
   "shipping_extra": {
      "show_time": false,
      "show_price": true
   },
   "shipping_cost": 0,
   "shipping_cost_owner": 0,
   "shipping_additional_days": 0,
   "gateway": "not-provided",
   "total": 712,
   "initiated_by": 372661,
   "internal_extra": {
      "sale_channel": ""
   },
   "checkout_url": "https://mystore.mitiendanube.com/checkout/v3/order/proxy/39138177/945f71f08398e25a7ae6fcf961e9cc10417278c8",
   "started_checkout": "2020-10-01 19:52:04",
   "updated_at": "2020-10-01 19:52:05",
   "created_at": "2020-10-01 19:52:05"
}

```

### POST /draft-orders/{id}/confirm

Confirm a draft order and converts it to an order.

#### POST /draft-orders/1/confirm

`HTTP/1.1 200 OK`

```json
{
    "id": 1,
    "token": "dcf448d80994fb50712785ac433fc06ebe78092b",
    "store_id": 1,
    "abandoned_checkout_url": "https://mystore.mitiendanube.com/checkout/v3/order/proxy/325520041/dcf448d80994fb50712785ac433fc06ebe78092b",
    "contact_email": "carman.j@outlook.com",
    "contact_name": "Jorge Carman",
    "contact_phone": "",
    "contact_identification": "",
    "shipping_name": "Jorge",
    "shipping_phone": "",
    "shipping_address": "",
    "shipping_number": "",
    "shipping_floor": "",
    "shipping_locality": "",
    "shipping_zipcode": "",
    "shipping_city": "",
    "shipping_province": "",
    "shipping_country": "AR",
    "shipping_min_days": null,
    "shipping_max_days": null,
    "billing_name": null,
    "billing_phone": null,
    "billing_address": null,
    "billing_number": null,
    "billing_floor": null,
    "billing_locality": null,
    "billing_zipcode": null,
    "billing_city": null,
    "billing_province": null,
    "billing_country": null,
    "shipping_cost_owner": "0.00",
    "shipping_cost_customer": "0.00",
    "coupon": [],
    "promotional_discount": {
        "id": null,
        "store_id": 1,
        "order_id": 1,
        "created_at": "2020-11-19T23:46:44+0000",
        "total_discount_amount": "0.00",
        "contents": [],
        "promotions_applied": []
    },
    "subtotal": "1000.00",
    "discount": "0.00",
    "discount_coupon": "0.00",
    "discount_gateway": "0.00",
    "total": "1000.00",
    "total_usd": "8.88",
    "checkout_enabled": true,
    "weight": "0.030",
    "currency": "ARS",
    "language": "es",
    "gateway": "not-provided",
    "gateway_id": null,
    "shipping": "draft",
    "shipping_option": "¡Te vamos a contactar para coordinar la entrega!",
    "shipping_option_code": "draft_99999",
    "shipping_option_reference": null,
    "shipping_pickup_details": null,
    "shipping_tracking_number": null,
    "shipping_tracking_url": null,
    "shipping_store_branch_name": null,
    "shipping_pickup_type": "ship",
    "shipping_suboption": [],
    "extra": {},
    "storefront": "form",
    "note": null,
    "created_at": "2020-11-19T23:14:56+0000",
    "updated_at": "2020-11-19T23:46:44+0000",
    "completed_at": {
        "date": "2020-11-19 23:46:44.000000",
        "timezone_type": 3,
        "timezone": "UTC"
    },
    "next_action": "waiting_packing",
    "payment_details": {
        "method": null,
        "credit_card_company": null,
        "installments": 1
    },
    "attributes": [],
    "customer": {
        "id": 1,
        "name": "Jorge Carman",
        "email": "carman.j@outlook.com",
        "identification": "",
        "phone": "",
        "note": null,
        "default_address": {
            "address": "",
            "city": "",
            "country": "AR",
            "created_at": "2020-09-07T11:22:11+0000",
            "default": true,
            "floor": "",
            "id": 1,
            "locality": "",
            "name": "Jorge Carman",
            "number": "",
            "phone": "",
            "province": "",
            "updated_at": "2020-09-07T11:22:11+0000",
            "zipcode": ""
        },
        "addresses": [
            {
                "address": "Honduras",
                "city": "Capital Federal",
                "country": "AR",
                "created_at": "2020-11-17T10:55:21+0000",
                "default": false,
                "floor": "",
                "id": 1,
                "locality": "",
                "name": "Jorge Carman",
                "number": "1010",
                "phone": "",
                "province": "Capital Federal",
                "updated_at": "2020-11-17T10:55:21+0000",
                "zipcode": "1414"
            },
            {
                "address": "Prueba",
                "city": "Capital Federal",
                "country": "AR",
                "created_at": "2020-10-15T10:51:05+0000",
                "default": false,
                "floor": "",
                "id": 1,
                "locality": "",
                "name": "Jorge Carman",
                "number": "245",
                "phone": "",
                "province": "Capital Federal",
                "updated_at": "2020-10-15T10:51:05+0000",
                "zipcode": "1414"
            },
            {
                "address": "",
                "city": "",
                "country": "AR",
                "created_at": "2020-09-07T11:22:11+0000",
                "default": true,
                "floor": "",
                "id": 1,
                "locality": "",
                "name": "Jorge Carman",
                "number": "",
                "phone": "",
                "province": "",
                "updated_at": "2020-09-07T11:22:11+0000",
                "zipcode": ""
            },
            {
                "address": "",
                "city": "",
                "country": "AR",
                "created_at": "2020-09-07T11:17:52+0000",
                "default": false,
                "floor": "",
                "id": 1,
                "locality": "",
                "name": "Pedro Alfonso",
                "number": "",
                "phone": "",
                "province": "",
                "updated_at": "2020-09-07T11:17:52+0000",
                "zipcode": ""
            }
        ],
        "billing_name": null,
        "billing_phone": null,
        "billing_address": null,
        "billing_number": null,
        "billing_floor": null,
        "billing_locality": null,
        "billing_zipcode": null,
        "billing_city": null,
        "billing_province": null,
        "billing_country": null,
        "extra": {},
        "total_spent": "7966.00",
        "total_spent_currency": "ARS",
        "last_order_id": 1,
        "active": false,
        "created_at": "2020-09-07T11:17:52+0000",
        "updated_at": "2020-11-19T23:46:44+0000"
    },
    "products": [
        {
            "id": 1,
            "depth": "0.00",
            "height": "0.00",
            "name": "Oleo Esencial Barba 30Ml",
            "price": "712.00",
            "product_id": 1,
            "image": {
                "id": 1,
                "product_id": 1,
                "src": "https://d26lpennugtm8s.cloudfront.net/stores/001/330/104/products/my-first-product.jpg",
                "position": 1,
                "alt": [],
                "created_at": "2020-08-31T17:56:26+0000",
                "updated_at": "2020-08-31T17:57:30+0000"
            },
            "quantity": 1,
            "free_shipping": false,
            "weight": "0.03",
            "width": "0.00",
            "variant_id": 1,
            "variant_values": [],
            "properties": [],
            "sku": null,
            "barcode": null
        }
    ],
    "clearsale": {
        "CodigoIntegracao": false,
        "IP": null,
        "Estado": ""
    },
    "checkout_url": "https://mystore.mitiendanube.com/checkout/v3/start/325520041/dcf448d80994fb50712785ac433fc06ebe78092b?from_store=1"
}
```

### DELETE /draft-orders/{id}

Delete a draft order.

#### DELETE /draft-orders/1

`HTTP/1.1 200 OK`

```json
{}
```