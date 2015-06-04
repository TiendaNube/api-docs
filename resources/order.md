Order
=====

An order is created when a customer completes the checkout process. Orders can't be created through the API.

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
| shipping_status            | Order's shipping status. Possible values are "shipped" or "unshipped"                                                                                                       |
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


Endpoints
---------

### GET /orders

Receive a list of all Orders.


| Parameter      | Explanation                                                                                                 |
| -------------- | ----------------------------------------------------------------------------------------------------------- |
| since_id       | Restrict results to after the specified ID                                                                  |
| status         | Show Orders with a given state. "any" is the default                                                        |
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

### PUT /orders/#{id}

Change the Order's owner_note (the only value you can modify through the API)

#### PUT /orders/450789469

```json
{
    "owner_note": "Need to gift wrap this order"
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
    "payment_status": "pending",
    "shipping": "ups",
    "shipping_status": "unshipped",
    "shipping_tracking_number": null,
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
