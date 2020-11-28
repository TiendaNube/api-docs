Draft Order
=====

A draft order enables create orders through outside channels.

#### Table of Contents
>[List all draft orders](#GET-draft-orders)
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

| Property                   | Explanation                                                                                                                                                                        |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                         | The unique numeric identifier for the draft order.                                                                                                                                 |
| token                      | Specifies the location of the draft order.                                                                                                                                         |
| store_id                   | The unique numeric identifier for the Store.                                                                                                                                       |
| abandoned_checkout_url     | Checkout url for the abandoned cart. Only when creating a draft order.                                                                                                             |
| shipping_min_days          | The minimum number of weekdays needed for the order to be delivered.                                                                                                               |
| shipping_max_days          | The maximum number of weekdays needed for the order to be delivered.                                                                                                               |
| billing_name               | Billing customer name for the draft order.                                                                                                                                         |
| billing_phone              | Billing phone for the draft order.                                                                                                                                                 |
| billing_address            | Billing address for the draft order.                                                                                                                                               |
| billing_number             | Billing number for the draft order.                                                                                                                                                |
| billing_floor              | Billing floor for the draft order.                                                                                                                                                 |
| billing_locality           | Billing locality for the draft order.                                                                                                                                              |
| billing_city               | Billing city for the draft order.                                                                                                                                                  |
| billing_province           | Billing province for the draft order.                                                                                                                                              |
| billing_country            | Billing country code for the draft order.                                                                                                                                          |
| shipping_cost_owner        | The shipping cost the store owner has to pay to the shipping company.                                                                                                              |
| shipping_cost_customer     | The shipping cost the customer has to pay to the store owner.                                                                                                                      |
| coupon                     | List of coupons applied to the draft order.                                                                                                                                        |
| promotional_discount       | Promotional discount applied to the draft order.                                                                                                                                   |
| subtotal                   | Price of the order before shipping.                                                                                                                                                |
| discount                   | Total value of the discount applied to the price of the draft order.                                                                                                               |
| discount_coupon            | Total value of the coupon discount applied to the price of the draft order.                                                                                                        |
| discount_gateway           | Total value of the gateway discount applied to the price of the draft order.                                                                                                       |
| total                      | Total price of the draft order including shipping and discounts.                                                                                                                   |
| total_usd                  | Total price of the order in US dollars.                                                                                                                                            |
| checkout_enabled           | Indicates if checkout is enabled for draft order.                                                                                                                                  |
| weight                     | Draft order's total weight, in kilograms.                                                                                                                                          |
| currency                   | The total spent's currency in [ISO 4217 format](http://en.wikipedia.org/wiki/ISO_4217).                                                                                            |
| language                   | Draft Order's language used by the customer.                                                                                                                                       |
| gateway                    | Payment gateway code. It is set as not provided.                                                                                                                                   |
| gateway_id                 | The unique numeric identifier for the gateway.                                                                                                                                     |
| shipping                   | The shipping method used.                                                                                                                                                          |
| shipping_option            | The shipping option chosen by the customer.                                                                                                                                        |
| shipping_option_code       | The shipping option code selected by the consumers.                                                                                                                                |
| shipping_option_reference  | The shipping option reference provided by a custom shipping carrier during the checkout process.                                                                                   |
| shipping_pickup_details    | The shipping pickup details (address and/or business hours) of the selected pickup point.                                                                                          |
| shipping_tracking_number   | Shipment's tracking number provided by the shipping company.                                                                                                                       |
| shipping_tracking_url      | Shipment's tracking URL provided by the shipping company.                                                                                                                          |
| shipping_store_branch_name | If order is going to be picked up, shows the store branch name.                                                                                                                    |
| shipping_pickup_type       | "ship" if the order is going to be shipped; "pickup" if it's going to be picked up from a store branch.                                                                            |
| shipping_suboption         | List of suboptions chosen for the shipping method.                                                                                                                                 |
| extra                      | A JSON object containing custom information. Can be set via the API or through custom form fields of name "extra[key]" on the cart's checkout form in the storefront.              |
| storefront                 | Origin of the order. Possible values are "store", "meli", "api" or "form". A draft order is always "form".                                                                         |
| note                       | Customer's note about the draft order.                                                                                                                                             |
| created_at                 | Date when the draft order was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601).                                                                                 |
| updated_at                 | Date when the draft order was last updated in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601).                                                                            |
| completed_at               | A JSON object containing the date when the draft order was confirm.                                                                                                                |
| next_action                | Next available operation in the draft orders flow. A draft order is always "noop".                                                                                                 |
| payment_details            | A JSON object containing payment details.                                                                                                                                          |
| attributes                 | Line item properties.                                                                                                                                                              |
| customer                   | [Customer](https://github.com/tiendanube/api-docs/blob/master/resources/customer.md) that purchased this draft order. Only given if the 'read_customers' scope is set for the app. |
| products                   | List of the products purchased by the `customer`. Contents are explained below and values hold are the ones corresponding to the time the products were purchased.                 |
| clearsale                  | Information on risk management. Only applies to stores in Brazil.                                                                                                                  |
| number                     | Unique number identifier for an order used by the shop owner and customers.                                                                                                        |
| cancel_reason              | Reason why the store owner cancelled an Order. Possible values are "customer", "fraud", "inventory" or "other".                                                                    |
| owner_note                 | Merchant owner's note about the draft order.                                                                                                                                       |
| cancelled_at               | Date when the draft order was cancelled in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601).                                                                               |
| closed_at                  | Date when the draft order was closed in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601).                                                                                  |
| read_at                    | Date when the merchant view the draft order for first time in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601).                                                            |
| status                     | Draft order's status. Possible values are "open", "closed" or "cancelled".                                                                                                         |
| payment_status             | Draft order's payment status. Possible values are "pending", "pending_confirmation" or "paid".                                                                                     |
| shipping_address           | Shipping address for the draft order.                                                                                                                                              |
| shipping_status            | Draft order's shipping status. A draft order is always "unpackaged".                                                                                                               |
| shipped_at                 | Date when the draft order was shipped in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601).                                                                                 |
| paid_at                    | Date when the draft order was paid in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601).                                                                                    |
| landing_url                | Store url.                                                                                                                                                                         |
| client_details             | Customer details for analytics.                                                                                                                                                    |
| app_id                     | The unique numeric identifier for the app.                                                                                                                                         |
| checkout_url               | Url of the checkout corresponding to the draft order.                                                                                                                              |

The `promotional_discount` field has the following contents:

| Property              | Explanation                                                                                        |
| ----------------------| ---------------------------------------------------------------------------------------------------|
| id                    | The unique numeric identifier for the promotional discount.                                        |
| store_id              | The unique numeric identifier for the store.                                                       |
| order_id              | The unique numeric identifier for the draft order.                                                 |
| created_at            | Date when the draft order was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601). |
| total_discount_amount | Total value of the promotions applied.                                                             |
| contents              | A JSON object containing products discounts information.                                           |
| promotions_applied    | List of promotions applied to the draft order's products.                                          |


The `products` field has the following contents:

| Property      | Explanation                                                                                                   |
| ------------- | ------------------------------------------------------------------------------------------------------------- |
| id            | The unique numeric identifier for the checkout cart.                                                          |
| depth         | Product's depth at the time of purchase.                                                                      |
| height        | Product's height at the time of purchase.                                                                     |
| name          | Product's name at the time of purchase.                                                                       |
| price         | Product's price at the time of purchase.                                                                      |
| product_id    | [Product](https://github.com/tiendanube/api-docs/blob/master/resources/product.md) purchased.                 |
| image         | Information about the product image.                                                                          |
| quantity      | Quantity purchased.                                                                                           |
| free_shipping | Indicates if the product has free shipping or not.                                                            |
| weight        | Product's weight at the time of purchase.                                                                     |
| width         | Product's width at the time of purchase.                                                                      |
| variant_id    | [Product Variant](https://github.com/tiendanube/api-docs/blob/master/resources/product_variant.md) purchased. |
| properties    | Line item properties.                                                                                         |
| sku           | Product Variant sku number.                                                                                   |
| barcode       | Product Variant barcode number.                                                                               |

Endpoints
---------

### GET /draft-orders

List all draft orders.

#### GET /draft-orders

`HTTP/1.1 200 OK`

```json
[
    {
        "id": 39138281,
        "token": "73137f3ddd99f3483e89315ed7645f3ebde78867",
        "store_id": "97902",
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
            "store_id": 97902,
            "order_id": "39138281",
            "created_at": "2020-11-28T00:53:17+0000",
            "total_discount_amount": "0.00",
            "contents": [],
            "promotions_applied": []
        },
        "subtotal": "712.00",
        "discount": "0.00",
        "discount_coupon": "0.00",
        "discount_gateway": "0.00",
        "total": "712.00",
        "total_usd": "0.00",
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
        "created_at": "2020-11-27T19:51:11+0000",
        "updated_at": "2020-11-27T19:51:11+0000",
        "completed_at": {
            "date": "2020-11-27T19:51:11+0000",
            "timezone_type": 3,
            "timezone": "UTC"
        },
        "next_action": "noop",
        "payment_details": {
            "method": null,
            "credit_card_company": null,
            "installments": "1"
        },
        "attributes": [],
        "customer": null,
        "products": [
            {
                "id": 32720573,
                "depth": "0.00",
                "height": "0.00",
                "name": "Oleo Esencial Barba",
                "price": "712.00",
                "product_id": 17310749,
                "image": {
                    "id": 0,
                    "product_id": 0,
                    "src": "www.example.com/photo-example",
                    "position": 0,
                    "alt": [],
                    "created_at": "2020-11-28T00:53:19+0000",
                    "updated_at": "2020-11-28T00:53:19+0000"
                },
                "quantity": 1,
                "free_shipping": false,
                "weight": "0.03",
                "width": "0.00",
                "variant_id": 33739143,
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
        "number": 0,
        "cancel_reason": null,
        "owner_note": "My nota",
        "cancelled_at": null,
        "closed_at": null,
        "read_at": null,
        "status": "open",
        "payment_status": "pending",
        "shipping_address": null,
        "shipping_status": "unpacked",
        "shipped_at": null,
        "paid_at": null,
        "landing_url": null,
        "client_details": {
            "browser_ip": null,
            "user_agent": null
        },
        "app_id": null,
        "checkout_url": "www.example.com/checkout-example"
    },
    {
        "id": 39138280,
        "token": "22b4e8f860bdc31b16ca3938f742faf8b1d83356",
        "store_id": "97902",
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
            "store_id": 97902,
            "order_id": "39138280",
            "created_at": "2020-11-28T00:53:17+0000",
            "total_discount_amount": "0.00",
            "contents": [],
            "promotions_applied": []
        },
        "subtotal": "712.00",
        "discount": "0.00",
        "discount_coupon": "0.00",
        "discount_gateway": "0.00",
        "total": "712.00",
        "total_usd": "0.00",
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
        "created_at": "-0001-11-30T00:00:00+0000",
        "updated_at": "2020-11-26T19:51:52+0000",
        "completed_at": {
            "date": "-0001-11-30 00:00:00.000000",
            "timezone_type": 3,
            "timezone": "UTC"
        },
        "next_action": "noop",
        "payment_details": {
            "method": null,
            "credit_card_company": null,
            "installments": "1"
        },
        "attributes": [],
        "customer": null,
        "products": [
            {
                "id": 32720572,
                "depth": "0.00",
                "height": "0.00",
                "name": "Pomada para cabello",
                "price": "712.00",
                "product_id": 17310748,
                "image": {
                    "id": 0,
                    "product_id": 0,
                    "src": "www.example.com/photo-example",
                    "position": 0,
                    "alt": [],
                    "created_at": "2020-11-28T00:53:19+0000",
                    "updated_at": "2020-11-28T00:53:19+0000"
                },
                "quantity": 1,
                "free_shipping": false,
                "weight": "0.03",
                "width": "0.00",
                "variant_id": 33739146,
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
        "number": 0,
        "cancel_reason": null,
        "owner_note": "My nota",
        "cancelled_at": null,
        "closed_at": null,
        "read_at": null,
        "status": "open",
        "payment_status": "pending",
        "shipping_address": null,
        "shipping_status": "unpacked",
        "shipped_at": null,
        "paid_at": null,
        "landing_url": null,
        "client_details": {
            "browser_ip": null,
            "user_agent": null
        },
        "app_id": null,
        "checkout_url": "www.example.com/checkout-example"
    }
]
```

`HTTP/1.1 500 Internal Server Error`

```json
{
    "code": 500,
    "message": "Internal Server Error",
    "description": "Example error."
}
```

### GET /draft-orders/{id}

Get a draft order.

#### GET /draft-orders/39138263

`HTTP/1.1 200 OK`

```json
{
    "id": 39138263,
    "token": "462ff0b053bf906e02c79f997a4872df28652e26",
    "store_id": "97902",
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
        "store_id": 97902,
        "order_id": "39138263",
        "created_at": "2020-11-28T02:37:32+0000",
        "total_discount_amount": "0.00",
        "contents": [],
        "promotions_applied": []
    },
    "subtotal": "712.00",
    "discount": "0.00",
    "discount_coupon": "0.00",
    "discount_gateway": "0.00",
    "total": "712.00",
    "total_usd": "0.00",
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
    "created_at": "2020-11-17T14:53:30+0000",
    "updated_at": "2020-11-17T14:53:30+0000",
    "completed_at": {
        "date": "2020-11-17 14:53:30.000000",
        "timezone_type": 3,
        "timezone": "UTC"
    },
    "next_action": "noop",
    "payment_details": {
        "method": null,
        "credit_card_company": null,
        "installments": "1"
    },
    "attributes": [],
    "customer": null,
    "products": [
        {
            "id": 32720555,
            "depth": "0.00",
            "height": "0.00",
            "name": "Oleo Esencial Barba",
            "price": "712.00",
            "product_id": 17310749,
            "image": {
                "id": 0,
                "product_id": 0,
                "src": "www.example.com/photo-example",
                "position": 0,
                "alt": [],
                "created_at": "2020-11-28T02:37:33+0000",
                "updated_at": "2020-11-28T02:37:33+0000"
            },
            "quantity": "1",
            "free_shipping": false,
            "weight": "0.03",
            "width": "0.00",
            "variant_id": "33739143",
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
    "number": 0,
    "cancel_reason": null,
    "owner_note": "",
    "cancelled_at": "2020-11-17T14:53:30+0000",
    "closed_at": null,
    "read_at": null,
    "status": "cancelled",
    "payment_status": "pending",
    "shipping_address": null,
    "shipping_status": "unpacked",
    "shipped_at": null,
    "paid_at": null,
    "landing_url": null,
    "client_details": {
        "browser_ip": null,
        "user_agent": null
    },
    "app_id": null,
    "checkout_url": "www.example.com/checkout-example"
}
```

`HTTP/1.1 404 Not Found`

```json
{
    "code": 404,
    "message": "Not Found",
    "description": "Invalid draft order 39138263"
}
```

`HTTP/1.1 500 Internal Server Error`

```json
{
    "code": 500,
    "message": "Internal Server Error",
    "description": "Example error."
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
    "id": 39138283,
    "token": "baadc175bb1fa32aed0ff355d2c54073dc2ea6ee",
    "store_id": 97902,
    "abandoned_checkout_url": "www.example.com/abandoned-checkout-url-example",
    "contact_email": "pedro.alfonso@gmail.com",
    "contact_name": "Pedro Alfonso",
    "contact_phone": "2930495",
    "contact_identification": "37894333",
    "shipping_name": "Pedro",
    "shipping_phone": "2930495",
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
        "store_id": 97902,
        "order_id": 39138283,
        "created_at": "2020-11-28T02:45:31+0000",
        "total_discount_amount": "0.00",
        "contents": [],
        "promotions_applied": []
    },
    "subtotal": "712.00",
    "discount": "0.00",
    "discount_coupon": "0.00",
    "discount_gateway": "0.00",
    "total": "712.00",
    "total_usd": "0.00",
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
    "created_at": "2020-11-28T02:45:31+0000",
    "updated_at": "2020-11-28T02:45:31+0000",
    "completed_at": null,
    "next_action": "noop",
    "payment_details": {
        "method": null,
        "credit_card_company": null,
        "installments": 1
    },
    "attributes": [],
    "customer": null,
    "products": [
        {
            "id": 32720575,
            "depth": "0.00",
            "height": "0.00",
            "name": "Oleo Esencial Barba",
            "price": "712.00",
            "product_id": 17310749,
            "image": {
                "id": 0,
                "product_id": 0,
                "src": "www.example.com/photo-example",
                "position": 0,
                "alt": [],
                "created_at": "2020-11-28T02:45:32+0000",
                "updated_at": "2020-11-28T02:45:32+0000"
            },
            "quantity": 1,
            "free_shipping": false,
            "weight": "0.03",
            "width": "0.00",
            "variant_id": 33739143,
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
    "checkout_url": "www.example.com/checkout-example"
}
```

`HTTP/1.1 500 Internal Server Error`

```json
{
    "code": 500,
    "message": "Internal Server Error",
    "description": "Example error."
}
```

### POST /draft-orders/{id}/confirm

Confirm a draft order and converts it to an order. Return an [Order](https://github.com/tiendanube/api-docs/blob/master/resources/order.md).

#### POST /draft-orders/39138283/confirm

`HTTP/1.1 200 OK`

```json
{
    "id": 39138283,
    "token": "baadc175bb1fa32aed0ff355d2c54073dc2ea6ee",
    "store_id": 97902,
    "abandoned_checkout_url": "www.example.com/abandoned-checkout-url-example",
    "contact_email": "pedro.alfonso@gmail.com",
    "contact_name": "Pedro Alfonso",
    "contact_phone": "2930495",
    "contact_identification": "37894333",
    "shipping_name": "Pedro",
    "shipping_phone": "2930495",
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
        "store_id": 97902,
        "order_id": 39138283,
        "created_at": "2020-11-28T02:49:11+0000",
        "total_discount_amount": "0.00",
        "contents": [],
        "promotions_applied": []
    },
    "subtotal": "712.00",
    "discount": "0.00",
    "discount_coupon": "0.00",
    "discount_gateway": "0.00",
    "total": "712.00",
    "total_usd": "44.64",
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
    "created_at": "2020-11-28T02:45:31+0000",
    "updated_at": "2020-11-28T02:49:11+0000",
    "completed_at": {
        "date": "2020-11-28 02:49:09.000000",
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
        "id": 8539063,
        "name": "Pedro Alfonso",
        "email": "pedro.alfonso@gmail.com",
        "identification": "37894333",
        "phone": "2930495",
        "note": null,
        "default_address": {
            "address": "",
            "city": "",
            "country": "AR",
            "created_at": "2020-10-21T13:51:20+0000",
            "default": true,
            "floor": "",
            "id": 5115443,
            "locality": "",
            "name": "Pedro Alfonso",
            "number": "",
            "phone": "2930495",
            "province": "",
            "updated_at": "2020-10-21T13:51:20+0000",
            "zipcode": ""
        },
        "addresses": [
            {
                "address": "",
                "city": "",
                "country": "AR",
                "created_at": "2020-10-21T13:51:20+0000",
                "default": true,
                "floor": "",
                "id": 5115443,
                "locality": "",
                "name": "Pedro Alfonso",
                "number": "",
                "phone": "2930495",
                "province": "",
                "updated_at": "2020-10-21T13:51:20+0000",
                "zipcode": ""
            },
            {
                "address": "",
                "city": "",
                "country": "AR",
                "created_at": "2020-08-24T01:33:12+0000",
                "default": false,
                "floor": "",
                "id": 5115437,
                "locality": "",
                "name": "Pedro Alfonso",
                "number": "",
                "phone": "",
                "province": "",
                "updated_at": "2020-08-24T01:33:12+0000",
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
        "total_spent": "12816.00",
        "total_spent_currency": "ARS",
        "last_order_id": 39138283,
        "active": false,
        "created_at": "2020-08-24T01:33:12+0000",
        "updated_at": "2020-11-28T02:49:09+0000"
    },
    "products": [
        {
            "id": 32720575,
            "depth": "0.00",
            "height": "0.00",
            "name": "Oleo Esencial Barba",
            "price": "712.00",
            "product_id": 17310749,
            "image": {
                "id": 0,
                "product_id": 0,
                "src": "www.example.com/photo-example",
                "position": 0,
                "alt": [],
                "created_at": "2020-11-28T02:49:12+0000",
                "updated_at": "2020-11-28T02:49:12+0000"
            },
            "quantity": 1,
            "free_shipping": false,
            "weight": "0.03",
            "width": "0.00",
            "variant_id": 33739143,
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
    "checkout_url": "www.example.com/checkout-example"
}
```

`HTTP/1.1 404 Not Found`

```json
{
    "code": 404,
    "message": "Not Found",
    "description": "Invalid draft order 39138283"
}
```

`HTTP/1.1 500 Internal Server Error`

```json
{
    "code": 500,
    "message": "Internal Server Error",
    "description": "Example error."
}
```

### DELETE /draft-orders/{id}

Delete a draft order.

#### DELETE /draft-orders/39138263

`HTTP/1.1 200 OK`

```json
{}
```

`HTTP/1.1 404 Not Found`

```json
{
    "code": 404,
    "message": "Not Found",
    "description": "Invalid draft order 39138263"
}
```

`HTTP/1.1 500 Internal Server Error`

```json
{
    "code": 500,
    "message": "Internal Server Error",
    "description": "Example error."
}
```