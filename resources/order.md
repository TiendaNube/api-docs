Order
=====

An order is created when a customer completes the checkout process. Orders also can be created through the API.

#### Table of Contents
> [Get all orders](#GET-orders)
> 
> [Get an order](#GET-ordersid)
> 
> [Create an order](#POST-orders)
> 
> [Update an order](#PUT-ordersid)
> 
> [Close an order](#POST-ordersidclose)
> 
> [Reopen an order](#POST-ordersidopen)
> 
> [Pack an order](#POST-ordersidpack)
> 
> [Fulfill an order](#POST-ordersidpack)
> 
> [Cancel an order](#POST-ordersidcancel)
> 
> [Create an invoice](#create-an-invoice)
> 
> [Read an invoice](#read-an-invoice)
> 

Properties
----------

| Property                   | Explanation                                                                                                                                                                 |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                         | The unique numeric identifier for the Order. It's different from `number`                                                                                                   |
| token                      | Specifies the location of the Order                                                                                                                                         |
| number                     | Unique numberc identifier for an Order used by the shop owner and customers. It's sequential and starts at 100                                                              |
| customer                   | [Customer](https://github.com/tiendanube/api-docs/blob/master/resources/customer.md) that purchased this Order. Only given if the 'read_customers' scope is set for the app |
| products                   | List of the products purchased by the `customer`. Contents are explained below and values hold are the ones corresponding to the time the products were purchased           |
| note                       | Customer's note about the order                                                                                                                                             |
| owner_note                 | Store owner's note about the order                                                                                                                                          |
| coupon                     | List of coupons applied to the order                                                                                                                                        |
| discount                   | Total value of the discount applied to the price of the order                                                                                                               |
| subtotal                   | Price of the order before shipping                                                                                                                                          |
| total                      | Total price of the order including shipping and discounts                                                                                                                   |
| total_usd                  | Total price of the order in US dollars                                                                                                                                      |
| currency                   | The total spent's currency in [ISO 4217 format](http://en.wikipedia.org/wiki/ISO_4217)                                                                                      |
| language                   | Order's language used by the customer during the checkout process                                                                                                           |
| gateway                    | The payment gateway used                                                                                                                                                    |
| shipping                   | The shipping method used                                                                                                                                                    |
| shipping_pickup_type       | "ship" if the order is going to be shipped; "pickup" if it's going to be picked up from a store branch                                                                      |
| shipping_store_branch_name | If order is going to be picked up, shows the store branch name                                                                                                              |
| shipping_address           | The customer's shipping address where the order will be shipped                                                                                                             |
| shipping_tracking_number   | The shipping tracking number for the order. This may be null if not available                                                                                               |
| shipping_min_days          | The minimum number of weekdays needed for the order to be delivered                                                                                                         |
| shipping_max_days          | The maximum number of weekdays needed for the order to be delivered                                                                                                         |
| shipping_cost_owner        | The shipping cost the store owner has to pay to the shipping company.                                                                                                       |
| shipping_cost_customer     | The shipping cost the customer has to pay to the store owner.                                                                                                               |
| shipping_option            | The shipping option chosen by the customer during the checkout process. |
| shipping_option_code       | The shipping option code selected by the consumers. |
| shipping_option_reference  | The shipping option reference provided by a custom shipping carrier during the checkout process. |
| shipping_pickup_details    | The shipping pickup details (address and/or business hours) of the selected pickup point. |
| shipping_tracking_url      | The shipping tracking URL where the customer can check for the shipment status. |
| billing_address            | Billing address for the order                                                                                                                                               |
| billing_number             | Billing number for the order                                                                                                                                                |
| billing_floor              | Billing floor for the order                                                                                                                                                 |
| billing_locality           | Billing locality for the order                                                                                                                                              |
| billing_zipcode            | Billing zipcode for the order                                                                                                                                               |
| billing_city               | Billing city for the order                                                                                                                                                  |
| billing_province           | Billing province for the order                                                                                                                                              |
| billing_country            | Billing country code for the order                                                                                                                                          |
| extra                      | A JSON object containing custom information. Can be set via the API or through custom form fields of name "extra[key]" on the cart's checkout form in the storefront.       |
| weight                     | Order's total weight, in kilograms                                                                                                                                          |
| status                     | Order's status. Possible values are "open", "closed" or "cancelled"                                                                                                         |
| payment_status             | Order's payment status. Possible values are "authorized", "pending", "paid", "abandoned", "refunded" or "voided"                                                            |
| shipping_status            | Order's shipping status. Possible values are "unpacked", "fulfilled" (means "shipped") or "unshipped"                                                                                                       |
| next_action                | Next available operation in the orders flow                                                                                                                                 |
| shipped_at                 | Date when the Order was shipped in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                                 |
| cancel_reason              | Reason why the store owner cancelled an Order. Possible values are "customer", "fraud", "inventory" or "other"                                                              |
| created_at                 | Date when the Order was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                                 | 
| updated_at                 | Date when the Order was last updated in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                            |

Property `next_action` can take one of the following values:
- __noop__: no action to take
- __close__: order should be closed
- __waiting_ipn__: we are waiting for the gateway to update us on the order status, the seller just needs to wait
- __waiting_manual_confirmation__: we are waiting for the seller to confirm the transaction
- __waiting_packing__: we are waiting for the seller to pack the order items
- __waiting_pickup__: we are waiting for the fulfillment provider to pick up the order
- __waiting_client_pickup__: we are waiting for the buyer to pick up the order (he shipped it to a seller's [B&M store](http://en.wikipedia.org/wiki/Brick_and_mortar))
- __waiting_shipment__: we are waiting for the seller to ship the order


The `products` field has the following contents:

| Property         | Explanation                                                                                                  |
| ---------------- | ------------------------------------------------------------------------------------------------------------ |
| product_id       | [Product](https://github.com/tiendanube/api-docs/blob/master/resources/product.md) purchased                 |
| variant_id       | [Product Variant](https://github.com/tiendanube/api-docs/blob/master/resources/product_variant.md) purchased |
| name             | Product's name at the time of purchase                                                                       |
| price            | Product's price at the time of purchase                                                                      |
| quantity         | Quantity purchased                                                                                           |
| weight           | Product's weight at the time of purchase                                                                     |
| width            | Product's width at the time of purchase                                                                      |
| height           | Product's height at the time of purchase                                                                     |
| depth            | Product's depth at the time of purchase                                                                      |
| free_shipping    | Indicates if the product has free shipping or not.                                                           |
| issues           | Possibles issues that can happen to an order. Contents are explained below.

The `issues` field has the following content:

| Property | Value Explanation | Issue explanation |
| :---: | :---: | :---: |
| unclaimed_stock | Number of items claimed by the user with insufficient stock | Can happens due to a race condition while the user is trying to pay the order and another user buys the same item. |

Endpoints
---------

### GET /orders

Receive a list of all Orders.


| Parameter      | Explanation                                                                                                 |
| -------------- | ----------------------------------------------------------------------------------------------------------- |
| since_id       | Restrict results to after the specified ID                                                                  |
| status         | Show Orders with a given state. "any" is the default                                                        |
| channels       | Restrict results to the specified sales channel. "any" is the default (it means orders from pos, api, store, etc) |
| payment_status | Show Orders with a given payment state. "any" is the default (it means authorized, pending and paid orders) |
| shipping_status| Show Orders with a given shipping state. "any" is the default                                               |
| created_at_min | Show Orders created after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))                   |
| created_at_max | Show Orders created before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))                  |
| updated_at_min | Show Orders last updated after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))              |
| updated_at_max | Show Orders last updated before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))             |
| total_min      | Show Orders with total value bigger or equals than the specified value                                      |
| total_max      | Show Orders with total value lower or equals than the specified value                                       |
| customer_ids   | Restrict results to the specified customer IDs (comma-separated)                                            |
| page           | Page to show                                                                                                |
| per_page       | Amount of results                                                                                           |
| fields         | Comma-separated list of fields to include in the response                                                   |
| q              | Search Orders by the given number; or containing the given text in the customer name or email               |
| app_id         | Show Orders created by a given app                                                                           |


#### GET /orders

`HTTP/1.1 200 OK`

```json
[
    {
      "cancel_reason": null,
      "created_at": "2008-01-10T11:00:00-05:00",
      "currency": "USD",
      "gateway": "paypal",
      "id": 450789469,
      "landing_site": "http://www.example.com?source=abc",
      "language": "en",
      "location_id": null,
      "name": "#1001",
      "note": null,
      "number": 1,
      "owner_note": null,
      "payment_status": "pending",
      "shipping": "ups",
      "shipping_status": "unshipped",
      "shipping_tracking_number": null,
      "shipping_tracking_url": null,
      "shipping_min_days": 2,
      "shipping_max_days": 4,
      "shipping_cost_owner": "20.00",
      "shipping_cost_customer": "20.00",
      "shipping_option": "Synthetics",
      "shipping_option_code": "table_131313",
      "shipping_option_reference": null,
      "status": "open",
      "subtotal": "38.00",
      "token": "898544a54283414238f74cd08f0efd3916f74b75",
      "discount": "0.00",
      "price": "58.00",
      "price_usd": "58.00",
      "weight": "2.00",
      "updated_at": "2008-01-10T11:00:00-05:00",
      "shipped_at": "2008-01-10T11:00:00-05:00",
      "number": 101,
      "coupon": [
        {
          "code": "SUPERDISCOUNT"
        }
      ],
      "products": [
        {
          "depth": null,
          "height": null,
          "price": "19.00",
          "product_id": 1234,
          "quantity": 2,
          "free_shipping": false,
          "variant_id": 101,
          "weight": "2.00",
          "width": null
        }
      ],
      "billing_address": "Evergreen Terrace",
      "billing_city": "Springfield",
      "billing_country": "US",
      "billing_default": true,
      "billing_floor": null,
      "billing_locality": null,
      "billing_number": "742",
      "billing_phone": "555-123-0413",
      "billing_province": "Oregon",
      "billing_zipcode": "97475",
      "extra": {
        "gift-wrap": "deluxe"
      },
      "shipping_pickup_type": "ship",
      "shipping_store_branch_name": null,
      "shipping_address": {
        "address": "Evergreen Terrace",
        "city": "Springfield",
        "country": "US",
        "created_at": "2013-01-03T09:11:51-03:00",
        "default": true,
        "floor": null,
        "id": 1234,
        "locality": null,
        "number": "742",
        "phone": "555-123-0413",
        "province": "Oregon",
        "updated_at": "2013-03-10T11:13:01-03:00",
        "zipcode": "97475"
      },
      "customer": {
        "created_at": "2013-01-03T09:11:51-03:00",
        "email": "john.doe@example.com",
        "id": 101,
        "last_order_id": 9001,
        "name": "John Doe",
        "total_spent": "89.00",
        "total_spent_currency": "USD",
        "updated_at": "2013-03-11T09:14:11-03:00",
        "default_address": {
          "address": "Evergreen Terrace",
          "city": "Springfield",
          "country": "US",
          "created_at": "2013-01-03T09:11:51-03:00",
          "default": true,
          "floor": null,
          "id": 1234,
          "locality": null,
          "number": "742",
          "phone": "555-123-0413",
          "province": "Oregon",
          "updated_at": "2013-03-10T11:13:01-03:00",
          "zipcode": "97475"
        }
      }
    }
]
```

#### GET /orders?fields=id,number,price_usd

`HTTP/1.1 200 OK`

```json
[
    {
      "id": 450789469,
      "number": "101",
      "price_usd": "58.00"
    }
]
```

### GET /orders/#{id}

Receive a single Order

| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| fields         | Comma-separated list of fields to include in the response                                        |

#### GET /orders/450789469

`HTTP/1.1 200 OK`

```json
{
    "cancel_reason": null,
    "created_at": "2008-01-10T11:00:00-05:00",
    "currency": "USD",
    "gateway": "paypal",
    "id": 450789469,
    "landing_site": "http://www.example.com?source=abc",
    "language": "en",
    "location_id": null,
    "name": "#1001",
    "note": null,
    "number": 1,
    "owner_note": null,
    "payment_status": "pending",
    "shipping": "ups",
    "shipping_status": "unshipped",
    "shipping_tracking_number": null,
    "shipping_tracking_url": null,
    "shipping_min_days": 2,
    "shipping_max_days": 4,
    "shipping_cost_owner": "20.00",
    "shipping_cost_customer": "20.00",
    "shipping_option": "Synthetics",
    "shipping_option_code": "table_131313",
    "shipping_option_reference": null,
    "status": "open",
    "subtotal": "38.00",
    "token": "898544a54283414238f74cd08f0efd3916f74b75",
    "discount": "0.00",
    "price": "58.00",
    "price_usd": "58.00",
    "weight": "2.00",
    "updated_at": "2008-01-10T11:00:00-05:00",
    "shipped_at": "2008-01-10T11:00:00-05:00",
    "number": 101,
    "coupon": [
      {
        "code": "SUPERDISCOUNT"
      }
    ],
    "products": [
      {
        "depth": null,
        "height": null,
        "price": "19.00",
        "product_id": 1234,
        "quantity": 2,
        "free_shipping": false,
        "variant_id": 101,
        "weight": "2.00",
        "width": null
      }
    ],
    "billing_address": "Evergreen Terrace",
    "billing_city": "Springfield",
    "billing_country": "US",
    "billing_default": true,
    "billing_floor": null,
    "billing_locality": null,
    "billing_number": "742",
    "billing_phone": "555-123-0413",
    "billing_province": "Oregon",
    "billing_zipcode": "97475",
    "extra": {
      "gift-wrap": "deluxe"
    },
    "shipping_pickup_type": "ship",
    "shipping_store_branch_name": null,
    "shipping_address": {
      "address": "Evergreen Terrace",
      "city": "Springfield",
      "country": "US",
      "created_at": "2013-01-03T09:11:51-03:00",
      "default": true,
      "floor": null,
      "id": 1234,
      "locality": null,
      "number": "742",
      "phone": "555-123-0413",
      "province": "Oregon",
      "updated_at": "2013-03-10T11:13:01-03:00",
      "zipcode": "97475"
    },
    "customer": {
      "created_at": "2013-01-03T09:11:51-03:00",
      "email": "john.doe@example.com",
      "id": 101,
      "last_order_id": 9001,
      "name": "John Doe",
      "total_spent": "89.00",
      "total_spent_currency": "USD",
      "updated_at": "2013-03-11T09:14:11-03:00",
      "default_address": {
        "address": "Evergreen Terrace",
        "city": "Springfield",
        "country": "US",
        "created_at": "2013-01-03T09:11:51-03:00",
        "default": true,
        "floor": null,
        "id": 1234,
        "locality": null,
        "number": "742",
        "phone": "555-123-0413",
        "province": "Oregon",
        "updated_at": "2013-03-10T11:13:01-03:00",
        "zipcode": "97475"
      }
    }
}
```

### POST /orders/

Create an Order.

| Parameter          | Description                                                                                                                           |Required|
|--------------------| --------------------------------------------------------------------------------------------------------------------------------------|--------|
| currency           | The order currency code ([ISO 4217 format](https://en.wikipedia.org/wiki/ISO_4217)). The default is the store currency.               | No     |
| language           | The language code ([ISO 639-1 format](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)). The default is the store main language.| No     | 
| gateway            | The order's payment gateway ([Payment Gateway](#Payment-Gateway)). The default is `not-provided`.                                     | Yes    |
| payment_status     | The order's payment status ([Payment Status](#Payment-Status)). The default is `paid`.                                                                       | No     | 
| status             | The order's status ([Order Status](#Order-Status)). The default is `open`.                                                                                     | No     |
| fulfillment_status | The order's status ([Order Status](#Order-Status)). The default is `unpacked`.                                                                                     | No     |
| products           | Order's products list ([Product](#Product)).                                                                                          | Yes    |
| total              | The sum of all products prices, shipping costs and discounts. Must be positive. If not specified, it's calculated considering the provided costs and discounts. | No    |
| inventory_behaviour| The inventory behaviour that the order must perform ([Inventory Behaviour](#Inventory-Behaviour)).                                    | No     |
| customer           | The customer object ([Customer](#Customer)).                                                                                          | Yes    |
| note               | An additional customer note for the order.                                                                                            | No     |
| billing_address    | The customer's billing address object ([Address](#Address)).                                                                          | Yes    |
| shipping_address   | The customer's shipping address object ([Address](#Address)).                                                                         | Yes    |
| shipping_pickup_type | The shipping pickup type ([Shipping Type](#Shipping-Type)). The default is `pickup`.                                                | Yes    |
| shipping             | The shipping method ([Shipping Method](#Shipping-Method)).  The defaults are `Não informado` for pt_BR and `No informado` in every other cases (es_AR, es_MX, es_CO).                                          | Yes    |
| shipping_option      | The order's shipping option nice name. The defaults are `Não informado` for pt_BR and `No informado` in every other cases (es_AR, es_MX, es_CO).                                                               | Yes    |
| shipping_tracking_number | The order's shipping tracking number                                                                                            | No     |
| shipping_cost_customer   | The customer's shipping cost double value. The value 0 means free shipping. The default is 0.                                   | Yes    |
| shipping_cost_owner      | The owner's shipping cost double value.                                                                                         | No     |
| send_confirmation_email  | Send the order confirmation email to the customer . The default is false.                                                       | No     |
| send_fulfillment_email   | Send the order fulfillment email to the customer . The default is false.                                                        | No     |


### Objects

#### Customer

| Value       | Description                    | Type    | Required |
|-------------|--------------------------------|---------|----------|
| name        | The customer's name. The defaults are `Não informado` for pt_BR and `No informado` in every other cases (es_AR, es_MX, es_CO).            | String  | Yes      |
| email       | The customer's email address. The defaults are `email@naoinformado.com` for pt_BR and `email@noinformado.com` in every other cases (es_AR, es_MX, es_CO).   | E-mail  | Yes      |
| phone       | The customer's phone number    | String  | No       |
| document    | The customer's document number | String  | No       |

#### Address

| Value       | Description                                                                         | Type   | Required |
|-------------|-------------------------------------------------------------------------------------|--------|----------|
| first_name  | The customer's first name. The defaults are `Não informado` for pt_BR and `No informado` in every other cases (es_AR, es_MX, es_CO).                                                          | String | Yes      |
| last_name   | The customer's last name. The defaults are `Não informado` for pt_BR and `No informado` in every other cases (es_AR, es_MX, es_CO).                                                           | String | Yes      |
| address     | The customer's street. The defaults are `Não informado` for pt_BR and `No informado` in every other cases (es_AR, es_MX, es_CO).                                                               | String | Yes      |
| number      | The address's number. The defaults are `Não informado` for pt_BR and `No informado` in every other cases (es_AR, es_MX, es_CO).                                                                | String | Yes      |
| floor       | The address's complement. The defaults are `Não informado` for pt_BR and `No informado` in every other cases (es_AR, es_MX, es_CO).                                                            | String | No       |
| locality    | The address's locality. The defaults are `Não informado` for pt_BR and `No informado` in every other cases (es_AR, es_MX, es_CO).                                                              | String | No       | 
| city        | The address's city. The defaults are `Não informado` for pt_BR and `No informado` in every other cases (es_AR, es_MX, es_CO).                                                                  | String | Yes      |
| province    | The address's province. The defaults are `Não informado` for pt_BR and `No informado` in every other cases (es_AR, es_MX, es_CO).                                                              | String | Yes      |
| zipcode     | The address's postal code. The default is `0000`.                                                          | String | Yes      |
| country     | The address's country ([ISO 3166-1 Format](http://en.wikipedia.org/wiki/ISO_3166-1)). The default is the store country. | String | Yes      |
| phone       | The address's phone number.                                                          | String | No       |


#### Product
| Value       | Description                                                       | Type    | Required |
|-------------|-------------------------------------------------------------------|---------|----------|
| variant_id  | The product variant ID                                            | Integer | Yes      |
| quantity    | The product quantity                                              | Integer | Yes      |
| price       | The item price. Defaults to tiendanube's product variant price.   | Double  | No       |

#### Order Status

| Value        | Description                                         |
|--------------| ----------------------------------------------------|
| open         | The order is open                                   |
| closed       | The order is closed                                 |
| cancelled    | The order is cancelled                              |


#### Payment Status

| Value        | Description                                         |
|--------------|-----------------------------------------------------|
| pending      | The payment confirmation is pending                 |
| authorized   | The payment was authorized but not captured yet     |
| paid         | The payment was successfully confirmed and captured |
| voided       | The payment confirmation is voided                  |
| refunded     | The payment was refunded to the customer            |
| abandoned    | The payment confirmation is abandoned               |


#### Payment Gateway 

| Value            | Description                         |
|------------------|-------------------------------------|
| offline          | Offline payment gateway             |
| mercadopago      | Mercado Pago                        |
| pagseguro        | PagSeguro                           |
| cielo            | Cielo                               |
| moip             | Wirecard                            |
| boleto_paghiper  | PagHiper                            |
| payu             | Payu                                |
| todopago         | Todo Pago                           |
| not-provided     | The payment gateway is not provided |


#### Shipping Type

| Value           | Description                           |
|-----------------|---------------------------------------|
| ship            | Home delivery                         |
| pickup          | Pickup in a physical location         |

#### Shipping Method

| Value             | Description                           |
|-------------------|---------------------------------------|
| branch            | Store branches                        |
| correios          | Brazilian Correios                    |
| correo-argentino  | Correo Argentino                      |
| oca-partner-ar    | OCA                                   |
| table             | Custom                                |
| not-provided      | The shipping method was not provided  |

#### Inventory Behaviour

| Value    | Description                                                  |
|----------|--------------------------------------------------------------|
| bypass   | Do not claim inventory (default)                             |
| claim    | Attempt to claim inventory, it could prevent order creation  |


#### POST /orders/

`HTTP/1.1 201 Created`

```json
{
   "currency": "ARS",
   "language": "es",
   "status": "open",
   "gateway": "mercadopago",
   "payment_status": "pending",
   "products": [
       {
           "variant_id": 101,
           "quantity": 2
       }
   ],
   "inventory_behaviour" : "bypass",
   "customer": {
       "email": "john.doe@example.com",
       "name": "John Doe",
       "phone": "+55 11 99999-9999",
       "document": "12345678901"
   },
   "note": null,
   "billing_address": {
       "first_name": "John",
       "last_name": "Doe",
       "address": "Evergreen Terrace",
       "number": "742",
       "floor": null,
       "locality": null,
       "city": "Springfield",
       "province": "Oregon",
       "zipcode": "97475",
       "country": "US",
       "phone": "5551230413"
   },
   "shipping_address": {
       "first_name": "John",
       "last_name": "Doe",
       "address": "Evergreen Terrace",
       "number": "742",
       "floor": null,
       "locality": null,
       "city": "Springfield",
       "province": "Oregon",
       "zipcode": "97475",
       "country": "US",
       "phone": "5551230413"
   },
   "shipping_pickup_type": "ship",
   "shipping": "correios",
   "shipping_option": "Correios - PAC",
   "shipping_tracking_number": null,
   "shipping_cost_customer": 20.00,
   "shipping_cost_owner": 20.00,
   "send_confirmation_email" : false,
   "send_fulfillment_email" : false
}
```

### PUT /orders/#{id}

Change an Order's attributes (just `owner_note` for now) and/or update an Order's status

#### PUT /orders/450789469

```json
{
    "owner_note": "Need to gift wrap this order",
    "status": "paid"
}
```

`HTTP/1.1 200 OK`

```json
{
    "cancel_reason": null,
    "created_at": "2008-01-10T11:00:00-05:00",
    "currency": "USD",
    "gateway": "paypal",
    "id": 450789469,
    "landing_site": "http://www.example.com?source=abc",
    "language": "en",
    "location_id": null,
    "name": "#1001",
    "note": null,
    "number": 1,
    "owner_note": "Need to gift wrap this order",
    "payment_status": "paid",
    "shipping": "ups",
    "shipping_status": "unshipped",
    "shipping_tracking_number": null,
    "shipping_tracking_url": null,
    "shipping_min_days": 2,
    "shipping_max_days": 4,
    "shipping_cost_owner": "20.00",
    "shipping_cost_customer": "20.00",
    "status": "open",
    "subtotal": "38.00",
    "token": "898544a54283414238f74cd08f0efd3916f74b75",
    "discount": "0.00",
    "price": "58.00",
    "price_usd": "58.00",
    "weight": "2.00",
    "updated_at": "2008-01-10T11:00:00-05:00",
    "shipped_at": "2008-01-10T11:00:00-05:00",
    "number": 101,
    "coupon": [
      {
        "code": "SUPERDISCOUNT"
      }
    ],
    "products": [
      {
        "depth": null,
        "height": null,
        "price": "19.00",
        "product_id": 1234,
        "quantity": 2,
        "free_shipping": false,
        "variant_id": 101,
        "weight": "2.00",
        "width": null
      }
    ],
    "billing_address": "Evergreen Terrace",
    "billing_city": "Springfield",
    "billing_country": "US",
    "billing_default": true,
    "billing_floor": null,
    "billing_locality": null,
    "billing_number": "742",
    "billing_phone": "555-123-0413",
    "billing_province": "Oregon",
    "billing_zipcode": "97475",
    "extra": {
      "gift-wrap": "deluxe"
    },
    "shipping_pickup_type": "ship",
    "shipping_store_branch_name": null,
    "shipping_address": {
      "address": "Evergreen Terrace",
      "city": "Springfield",
      "country": "US",
      "created_at": "2013-01-03T09:11:51-03:00",
      "default": true,
      "floor": null,
      "id": 1234,
      "locality": null,
      "number": "742",
      "phone": "555-123-0413",
      "province": "Oregon",
      "updated_at": "2013-03-10T11:13:01-03:00",
      "zipcode": "97475"
    },
    "customer": {
      "created_at": "2013-01-03T09:11:51-03:00",
      "email": "john.doe@example.com",
      "id": 101,
      "last_order_id": 9001,
      "name": "John Doe",
      "total_spent": "89.00",
      "total_spent_currency": "USD",
      "updated_at": "2013-03-11T09:14:11-03:00",
      "default_address": {
        "address": "Evergreen Terrace",
        "city": "Springfield",
        "country": "US",
        "created_at": "2013-01-03T09:11:51-03:00",
        "default": true,
        "floor": null,
        "id": 1234,
        "locality": null,
        "number": "742",
        "phone": "555-123-0413",
        "province": "Oregon",
        "updated_at": "2013-03-10T11:13:01-03:00",
        "zipcode": "97475"
      }
    }
}
```


### POST /orders/#{id}/close

Close an Order

#### POST /orders/450789469/close

`HTTP/1.1 200 OK`

```json
{
    "cancel_reason": null,
    "created_at": "2008-01-10T11:00:00-05:00",
    "currency": "USD",
    "gateway": "paypal",
    "id": 450789469,
    "landing_site": "http://www.example.com?source=abc",
    "language": "en",
    "location_id": null,
    "name": "#1001",
    "note": null,
    "number": 1,
    "owner_note": null,
    "payment_status": "pending",
    "shipping": "ups",
    "shipping_status": "unshipped",
    "shipping_tracking_number": null,
    "shipping_tracking_url": null,
    "shipping_min_days": 2,
    "shipping_max_days": 4,
    "shipping_cost_owner": "20.00",
    "shipping_cost_customer": "20.00",
    "status": "closed",
    "subtotal": "38.00",
    "token": "898544a54283414238f74cd08f0efd3916f74b75",
    "discount": "0.00",
    "price": "58.00",
    "price_usd": "58.00",
    "weight": "2.00",
    "updated_at": "2008-01-10T11:00:00-05:00",
    "shipped_at": "2008-01-10T11:00:00-05:00",
    "number": 101,
    "coupon": [
      {
        "code": "SUPERDISCOUNT"
      }
    ],
    "products": [
      {
        "depth": null,
        "height": null,
        "price": "19.00",
        "product_id": 1234,
        "quantity": 2,
        "free_shipping": false,
        "variant_id": 101,
        "weight": "2.00",
        "width": null
      }
    ],
    "billing_address": "Evergreen Terrace",
    "billing_city": "Springfield",
    "billing_country": "US",
    "billing_default": true,
    "billing_floor": null,
    "billing_locality": null,
    "billing_number": "742",
    "billing_phone": "555-123-0413",
    "billing_province": "Oregon",
    "billing_zipcode": "97475",
    "extra": {
      "gift-wrap": "deluxe"
    },
    "shipping_pickup_type": "ship",
    "shipping_store_branch_name": null,
    "shipping_address": {
      "address": "Evergreen Terrace",
      "city": "Springfield",
      "country": "US",
      "created_at": "2013-01-03T09:11:51-03:00",
      "default": true,
      "floor": null,
      "id": 1234,
      "locality": null,
      "number": "742",
      "phone": "555-123-0413",
      "province": "Oregon",
      "updated_at": "2013-03-10T11:13:01-03:00",
      "zipcode": "97475"
    },
    "customer": {
      "created_at": "2013-01-03T09:11:51-03:00",
      "email": "john.doe@example.com",
      "id": 101,
      "last_order_id": 9001,
      "name": "John Doe",
      "total_spent": "89.00",
      "total_spent_currency": "USD",
      "updated_at": "2013-03-11T09:14:11-03:00",
      "default_address": {
        "address": "Evergreen Terrace",
        "city": "Springfield",
        "country": "US",
        "created_at": "2013-01-03T09:11:51-03:00",
        "default": true,
        "floor": null,
        "id": 1234,
        "locality": null,
        "number": "742",
        "phone": "555-123-0413",
        "province": "Oregon",
        "updated_at": "2013-03-10T11:13:01-03:00",
        "zipcode": "97475"
      }
    }
}
```

### POST /orders/#{id}/open

Re-open a closed Order

#### POST /orders/450789469/open

`HTTP/1.1 200 OK`

```json
{
    "cancel_reason": null,
    "created_at": "2008-01-10T11:00:00-05:00",
    "currency": "USD",
    "gateway": "paypal",
    "id": 450789469,
    "landing_site": "http://www.example.com?source=abc",
    "language": "en",
    "location_id": null,
    "name": "#1001",
    "note": null,
    "number": 1,
    "owner_note": null,
    "payment_status": "pending",
    "shipping": "ups",
    "shipping_status": "unshipped",
    "shipping_tracking_number": null,
    "shipping_tracking_url": null,
    "shipping_min_days": 2,
    "shipping_max_days": 4,
    "status": "open",
    "subtotal": "38.00",
    "token": "898544a54283414238f74cd08f0efd3916f74b75",
    "discount": "0.00",
    "price": "58.00",
    "price_usd": "58.00",
    "weight": "2.00",
    "updated_at": "2008-01-10T11:00:00-05:00",
    "shipped_at": "2008-01-10T11:00:00-05:00",
    "number": 101,
    "coupon": [
      {
        "code": "SUPERDISCOUNT"
      }
    ],
    "products": [
      {
        "depth": null,
        "height": null,
        "price": "19.00",
        "product_id": 1234,
        "quantity": 2,
        "variant_id": 101,
        "weight": "2.00",
        "width": null
      }
    ],
    "billing_address": "Evergreen Terrace",
    "billing_city": "Springfield",
    "billing_country": "US",
    "billing_default": true,
    "billing_floor": null,
    "billing_locality": null,
    "billing_number": "742",
    "billing_phone": "555-123-0413",
    "billing_province": "Oregon",
    "billing_zipcode": "97475",
    "extra": {
      "gift-wrap": "deluxe"
    },
    "shipping_pickup_type": "ship",
    "shipping_store_branch_name": null,
    "shipping_address": {
      "address": "Evergreen Terrace",
      "city": "Springfield",
      "country": "US",
      "created_at": "2013-01-03T09:11:51-03:00",
      "default": true,
      "floor": null,
      "id": 1234,
      "locality": null,
      "number": "742",
      "phone": "555-123-0413",
      "province": "Oregon",
      "updated_at": "2013-03-10T11:13:01-03:00",
      "zipcode": "97475"
    },
    "customer": {
      "created_at": "2013-01-03T09:11:51-03:00",
      "email": "john.doe@example.com",
      "id": 101,
      "last_order_id": 9001,
      "name": "John Doe",
      "total_spent": "89.00",
      "total_spent_currency": "USD",
      "updated_at": "2013-03-11T09:14:11-03:00",
      "default_address": {
        "address": "Evergreen Terrace",
        "city": "Springfield",
        "country": "US",
        "created_at": "2013-01-03T09:11:51-03:00",
        "default": true,
        "floor": null,
        "id": 1234,
        "locality": null,
        "number": "742",
        "phone": "555-123-0413",
        "province": "Oregon",
        "updated_at": "2013-03-10T11:13:01-03:00",
        "zipcode": "97475"
      }
    }
}
```

### POST /orders/#{id}/pack

Pack an Order

#### POST /orders/450789469/pack

`HTTP/1.1 200 OK`

```json
{
    "cancel_reason": null,
    "created_at": "2008-01-10T11:00:00-05:00",
    "currency": "USD",
    "gateway": "paypal",
    "id": 450789469,
    "landing_site": "http://www.example.com?source=abc",
    "language": "en",
    "location_id": null,
    "name": "#1001",
    "note": null,
    "number": 1,
    "owner_note": null,
    "payment_status": "pending",
    "shipping": "ups",
    "shipping_status": "unshipped",
    "shipping_tracking_number": null,
    "shipping_tracking_url": null,
    "shipping_min_days": 2,
    "shipping_max_days": 4,
    "shipping_cost_owner": "20.00",
    "shipping_cost_customer": "20.00",
    "status": "open",
    "subtotal": "38.00",
    "token": "898544a54283414238f74cd08f0efd3916f74b75",
    "discount": "0.00",
    "price": "58.00",
    "price_usd": "58.00",
    "weight": "2.00",
    "updated_at": "2008-01-10T11:00:00-05:00",
    "shipped_at": "2008-01-10T11:00:00-05:00",
    "number": 101,
    "coupon": [
      {
        "code": "SUPERDISCOUNT"
      }
    ],
    "products": [
      {
        "depth": null,
        "height": null,
        "price": "19.00",
        "product_id": 1234,
        "quantity": 2,
        "free_shipping": false,
        "variant_id": 101,
        "weight": "2.00",
        "width": null
      }
    ],
    "billing_address": "Evergreen Terrace",
    "billing_city": "Springfield",
    "billing_country": "US",
    "billing_default": true,
    "billing_floor": null,
    "billing_locality": null,
    "billing_number": "742",
    "billing_phone": "555-123-0413",
    "billing_province": "Oregon",
    "billing_zipcode": "97475",
    "extra": {
      "gift-wrap": "deluxe"
    },
    "shipping_pickup_type": "ship",
    "shipping_store_branch_name": null,
    "shipping_address": {
      "address": "Evergreen Terrace",
      "city": "Springfield",
      "country": "US",
      "created_at": "2013-01-03T09:11:51-03:00",
      "default": true,
      "floor": null,
      "id": 1234,
      "locality": null,
      "number": "742",
      "phone": "555-123-0413",
      "province": "Oregon",
      "updated_at": "2013-03-10T11:13:01-03:00",
      "zipcode": "97475"
    },
    "customer": {
      "created_at": "2013-01-03T09:11:51-03:00",
      "email": "john.doe@example.com",
      "id": 101,
      "last_order_id": 9001,
      "name": "John Doe",
      "total_spent": "89.00",
      "total_spent_currency": "USD",
      "updated_at": "2013-03-11T09:14:11-03:00",
      "default_address": {
        "address": "Evergreen Terrace",
        "city": "Springfield",
        "country": "US",
        "created_at": "2013-01-03T09:11:51-03:00",
        "default": true,
        "floor": null,
        "id": 1234,
        "locality": null,
        "number": "742",
        "phone": "555-123-0413",
        "province": "Oregon",
        "updated_at": "2013-03-10T11:13:01-03:00",
        "zipcode": "97475"
      }
    }
}
```

### POST /orders/#{id}/fulfill

Fulfill an Order

| Parameter                    | Explanation                                                                        |
| ---------------------------- | ---------------------------------------------------------------------------------- |
| shipping_tracking_number     | Shipment's tracking number provided by the shipping company                        |
| shipping_tracking_url        | Shipment's tracking URL provided by the shipping company                           |
| notify_customer              | Notify the customer about the fulfillment (the default value is true)              |

#### POST /orders/450789469/fulfill

```json
{
    "shipping_tracking_number": "ABC1234",
    "shipping_tracking_url": "https://shipping.com/tracking/ABC1234",
    "notify_customer": true
}
```

`HTTP/1.1 200 OK`

```json
{
    "cancel_reason": null,
    "created_at": "2008-01-10T11:00:00-05:00",
    "currency": "USD",
    "gateway": "paypal",
    "id": 450789469,
    "landing_site": "http://www.example.com?source=abc",
    "language": "en",
    "location_id": null,
    "name": "#1001",
    "note": null,
    "number": 1,
    "owner_note": null,
    "payment_status": "pending",
    "shipping": "ups",
    "shipping_status": "shipped",
    "shipping_tracking_number": "ABC1234",
    "shipping_tracking_url": "https://shipping.com/tracking/ABC1234",
    "shipping_min_days": 2,
    "shipping_max_days": 4,
    "shipping_cost_owner": "20.00",
    "shipping_cost_customer": "20.00",
    "status": "open",
    "subtotal": "38.00",
    "token": "898544a54283414238f74cd08f0efd3916f74b75",
    "discount": "0.00",
    "price": "58.00",
    "price_usd": "58.00",
    "weight": "2.00",
    "updated_at": "2008-01-10T11:00:00-05:00",
    "shipped_at": "2008-01-10T11:00:00-05:00",
    "number": 101,
    "coupon": [
      {
        "code": "SUPERDISCOUNT"
      }
    ],
    "products": [
      {
        "depth": null,
        "height": null,
        "price": "19.00",
        "product_id": 1234,
        "quantity": 2,
        "free_shipping": false,
        "variant_id": 101,
        "weight": "2.00",
        "width": null
      }
    ],
    "billing_address": "Evergreen Terrace",
    "billing_city": "Springfield",
    "billing_country": "US",
    "billing_default": true,
    "billing_floor": null,
    "billing_locality": null,
    "billing_number": "742",
    "billing_phone": "555-123-0413",
    "billing_province": "Oregon",
    "billing_zipcode": "97475",
    "extra": {
      "gift-wrap": "deluxe"
    },
    "shipping_pickup_type": "ship",
    "shipping_store_branch_name": null,
    "shipping_address": {
      "address": "Evergreen Terrace",
      "city": "Springfield",
      "country": "US",
      "created_at": "2013-01-03T09:11:51-03:00",
      "default": true,
      "floor": null,
      "id": 1234,
      "locality": null,
      "number": "742",
      "phone": "555-123-0413",
      "province": "Oregon",
      "updated_at": "2013-03-10T11:13:01-03:00",
      "zipcode": "97475"
    },
    "customer": {
      "created_at": "2013-01-03T09:11:51-03:00",
      "email": "john.doe@example.com",
      "id": 101,
      "last_order_id": 9001,
      "name": "John Doe",
      "total_spent": "89.00",
      "total_spent_currency": "USD",
      "updated_at": "2013-03-11T09:14:11-03:00",
      "default_address": {
        "address": "Evergreen Terrace",
        "city": "Springfield",
        "country": "US",
        "created_at": "2013-01-03T09:11:51-03:00",
        "default": true,
        "floor": null,
        "id": 1234,
        "locality": null,
        "number": "742",
        "phone": "555-123-0413",
        "province": "Oregon",
        "updated_at": "2013-03-10T11:13:01-03:00",
        "zipcode": "97475"
      }
    }
}
```


### POST /orders/#{id}/cancel

Cancel an Order

| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| reason         | The reason for the order cancellation. Possible values are "customer", "inventory", "fraud" or "other" |
| email          | Notify the customer of the cancellation (the default value is true)                              |
| restock        | Restock the store's products (the default value is true)                                         |

#### POST /orders/450789469/cancel

`HTTP/1.1 200 OK`

```json
{
    "cancel_reason": null,
    "created_at": "2008-01-10T11:00:00-05:00",
    "currency": "USD",
    "gateway": "paypal",
    "id": 450789469,
    "landing_site": "http://www.example.com?source=abc",
    "language": "en",
    "location_id": null,
    "name": "#1001",
    "note": null,
    "number": 1,
    "owner_note": null,
    "payment_status": "pending",
    "shipping": "ups",
    "shipping_status": "unshipped",
    "shipping_tracking_number": null,
    "shipping_tracking_url": null,
    "shipping_min_days": 2,
    "shipping_max_days": 4,
    "shipping_cost_owner": "20.00",
    "shipping_cost_customer": "20.00",
    "status": "cancelled",
    "subtotal": "38.00",
    "token": "898544a54283414238f74cd08f0efd3916f74b75",
    "discount": "0.00",
    "price": "58.00",
    "price_usd": "58.00",
    "weight": "2.00",
    "updated_at": "2008-01-10T11:00:00-05:00",
    "shipped_at": "2008-01-10T11:00:00-05:00",
    "number": 101,
    "coupon": [
      {
        "code": "SUPERDISCOUNT"
      }
    ],
    "products": [
      {
        "depth": null,
        "height": null,
        "price": "19.00",
        "product_id": 1234,
        "quantity": 2,
        "free_shipping": false,
        "variant_id": 101,
        "weight": "2.00",
        "width": null
      }
    ],
    "billing_address": "Evergreen Terrace",
    "billing_city": "Springfield",
    "billing_country": "US",
    "billing_default": true,
    "billing_floor": null,
    "billing_locality": null,
    "billing_number": "742",
    "billing_phone": "555-123-0413",
    "billing_province": "Oregon",
    "billing_zipcode": "97475",
    "extra": {
      "gift-wrap": "deluxe"
    },
    "shipping_pickup_type": "ship",
    "shipping_store_branch_name": null,
    "shipping_address": {
      "address": "Evergreen Terrace",
      "city": "Springfield",
      "country": "US",
      "created_at": "2013-01-03T09:11:51-03:00",
      "default": true,
      "floor": null,
      "id": 1234,
      "locality": null,
      "number": "742",
      "phone": "555-123-0413",
      "province": "Oregon",
      "updated_at": "2013-03-10T11:13:01-03:00",
      "zipcode": "97475"
    },
    "customer": {
      "created_at": "2013-01-03T09:11:51-03:00",
      "email": "john.doe@example.com",
      "id": 101,
      "last_order_id": 9001,
      "name": "John Doe",
      "total_spent": "89.00",
      "total_spent_currency": "USD",
      "updated_at": "2013-03-11T09:14:11-03:00",
      "default_address": {
        "address": "Evergreen Terrace",
        "city": "Springfield",
        "country": "US",
        "created_at": "2013-01-03T09:11:51-03:00",
        "default": true,
        "floor": null,
        "id": 1234,
        "locality": null,
        "number": "742",
        "phone": "555-123-0413",
        "province": "Oregon",
        "updated_at": "2013-03-10T11:13:01-03:00",
        "zipcode": "97475"
      }
    }
}
```

### Invoices (e.g. NFe in Brazil)

We currently do not offer an `Invoice` API, but there are many apps which need to read and/or write
invoice information. The way to achieve this is using [`Metafields`](https://github.com/TiendaNube/api-docs/blob/master/resources/metafields.md).

The advantage of using `Metafields` is that a certain app could generate the invoice and another app can read it.
Let's take a real Brazilian example: an ERP can generate the *nota fiscal* (NFe) and a Shipping Carrier can use that
NFe to fulfill a shipment. 

#### Create an invoice

To make sure other apps can read the invoice you create, please use this example as-is.

##### POST /metafields

`value` should have the invoice number and `owner_id` should hold the `Order` id.

```json
{
    "namespace": "nfe",
    "key": "number",
    "value": "NFE_NUMBER",
    "description": "Número da NFe",
    "owner_resource": "Order",
    "owner_id": 12345678
}
```

`HTTP/1.1 201 Created`

```json
{
    "id": 1234,
    "namespace": "nfe",
    "key": "number",
    "value": "NFE_NUMBER",
    "description": "Número da NFe",
    "owner_resource": "Order",
    "owner_id": 12345678,
    "created_at":"2015-01-02 20:27:51",
    "updated_at":"2015-01-02 20:27:51",
    "deleted_at":null
}
```

#### Read an invoice

To read an invoice we need to search for the previously created metafield holding the invoice.

##### GET /metafields/orders?owner_id=ORDER_ID&namespace=nfe&key=number&fields=owner_id,key,value

`HTTP/1.1 200 OK`

```json
{
    "key": "number",
    "value": "NFE_NUMBER",
    "owner_id": 12345678
}
```


