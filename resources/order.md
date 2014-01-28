Order
=====

An order is created when a customer completes the checkout process. Orders can't be created through the API.

Properties
----------

| Property         | Explanation                                                                                      |
| ---------------- | ------------------------------------------------------------------------------------------------ |
| id               | The unique numeric identifier for the Order. It's different from `number`                        |
| token            | Specifies the location of the Order                                                              |
| number           | Unique numberc identifier for an Order used by the shop owner and customers. It's sequential and starts at 100 |
| customer         | Customer that purchase this Order                                                                |
| note             | Customer's note about the order                                                                  |
| owner_note       | Store owner's note about the order                                                               |
| coupon           | List of coupons applied to the order                                                             |
| discount         | Total value of the discount applied to the price of the order                                    |
| subtotal         | Price of the order before shipping                                                               |
| total            | Total price of the order including shipping and discounts                                        |
| total_usd        | Total price of the order in US dollars                                                           |
| currency         | The total spent's currency in [ISO 4217 format](http://en.wikipedia.org/wiki/ISO_4217)           |
| language         | Order's language used by the customer during the checkout process                                |
| gateway          | The payment gateway used                                                                         |
| shipping         | The shipping method used                                                                         |
| shipping_address | The customer's shipping address where the order will be shipped                                  |
| weight           | Order's total weight, in kilograms                                                               |
| status           | Order's status. Possible values are "open", "closed" or "cancelled"                              |
| payment_status   | Order's payment status. Possible values are "authorized", "pending", "paid", "abandoned", "refunded" or "voided" |
| shipping_status  | Order's shipping status. Possible values are "shipped" or "unshipped"                            |
| cancel_reason    | Reason why the store owner cancelled an Order. Possible values are "customer", "fraud", "inventory" or "other" |
| created_at       | Date when the Order was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)      | 
| updated_at       | Date when the Order was last updated in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601) |

Endpoints
---------

### GET /orders

Receive a list of all Orders.


| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| since_id       | Restrict results to after the specified ID                                                       |
| status         | Show Orders with a given state. "any" is the default                                             |
| payment_status | Show Orders with a given payment state. "any" is the default (it means authorized, pending and paid orders) |
| shipping_status| Show Orders with a given shipping state. "any" is the default                                     |
| created_at_min | Show Orders created after date ([ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601))        |
| created_at_max | Show Orders created before date ([ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601))       |
| updated_at_min | Show Orders last updated after date ([ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601))   |
| updated_at_max | Show Orders last updated before date ([ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601))  |
| page           | Page to show                                                                                     |
| per_page       | Amount of results                                                                                |
| fields         | Comma-separated list of fields to include in the response                                        |


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
      "status": "open",
      "subtotal": "38.00",
      "token": "898544a54283414238f74cd08f0efd3916f74b75",
      "discount": "0.00",
      "price": "58.00",
      "price_usd": "58.00",
      "weight": "2.00",
      "updated_at": "2008-01-10T11:00:00-05:00",
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
          "id": 9912,
          "price": "19.00",
          "product_id": 1234,
          "quantity": 2,
          "variant_id": 101,
          "weight": "2.00",
          "width": null
        }
      ],
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
      "price_usd": "58.00",
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
    "status": "open",
    "subtotal": "38.00",
    "token": "898544a54283414238f74cd08f0efd3916f74b75",
    "discount": "0.00",
    "price": "58.00",
    "price_usd": "58.00",
    "weight": "2.00",
    "updated_at": "2008-01-10T11:00:00-05:00",
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
        "id": 9912,
        "price": "19.00",
        "product_id": 1234,
        "quantity": 2,
        "variant_id": 101,
        "weight": "2.00",
        "width": null
      }
    ],
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
    "status": "closed",
    "subtotal": "38.00",
    "token": "898544a54283414238f74cd08f0efd3916f74b75",
    "discount": "0.00",
    "price": "58.00",
    "price_usd": "58.00",
    "weight": "2.00",
    "updated_at": "2008-01-10T11:00:00-05:00",
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
        "id": 9912,
        "price": "19.00",
        "product_id": 1234,
        "quantity": 2,
        "variant_id": 101,
        "weight": "2.00",
        "width": null
      }
    ],
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
    "status": "open",
    "subtotal": "38.00",
    "token": "898544a54283414238f74cd08f0efd3916f74b75",
    "discount": "0.00",
    "price": "58.00",
    "price_usd": "58.00",
    "weight": "2.00",
    "updated_at": "2008-01-10T11:00:00-05:00",
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
        "id": 9912,
        "price": "19.00",
        "product_id": 1234,
        "quantity": 2,
        "variant_id": 101,
        "weight": "2.00",
        "width": null
      }
    ],
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
    "status": "cancelled",
    "subtotal": "38.00",
    "token": "898544a54283414238f74cd08f0efd3916f74b75",
    "discount": "0.00",
    "price": "58.00",
    "price_usd": "58.00",
    "weight": "2.00",
    "updated_at": "2008-01-10T11:00:00-05:00",
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
        "id": 9912,
        "price": "19.00",
        "product_id": 1234,
        "quantity": 2,
        "variant_id": 101,
        "weight": "2.00",
        "width": null
      }
    ],
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
    "status": "open",
    "subtotal": "38.00",
    "token": "898544a54283414238f74cd08f0efd3916f74b75",
    "discount": "0.00",
    "price": "58.00",
    "price_usd": "58.00",
    "weight": "2.00",
    "updated_at": "2008-01-10T11:00:00-05:00",
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
        "id": 9912,
        "price": "19.00",
        "product_id": 1234,
        "quantity": 2,
        "variant_id": 101,
        "weight": "2.00",
        "width": null
      }
    ],
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
