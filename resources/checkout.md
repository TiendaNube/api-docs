Checkout
======

A checkout (abandoned cart) is created when a customer doesn't complete the checkout process.

#### Table of Contents
> [Get all checkouts](#GET-checkouts)

Properties
----------

| Property                   | Explanation                                                                                                                                                                 |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                         | The unique numeric identifier for the Checkout. |
| token                      | Specifies the location of the Checkout                                                                                                                                         |
| store_id                   | The unique numeric identifier for the store |
| abandoned_checkout_url     | URL of the abandoned cart (the purchase can be followed with this URL)|
| contact_email              | The email given by the costumer |
| contact_name               | The name given by the costumer |
| contact_phone              | The phone given by the costumer |
| contact_identification     | The identification given by the costumer (In Brazil for example, it would be the CPF/CNPJ. In Argentina DNI/CUIL)|
| shipping_name||
| shipping_phone||
| shipping_address||
| shipping_number||
| shipping_floor||
| shipping_locality||
| shipping_zipcode||
| shipping_city||
| shipping_province||
| shipping_country||
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
| shipping_status            | Order's shipping status. Possible values are "unpacked", "shipped" or "unshipped"                                                                                                       |
| next_action                | Next available operation in the orders flow                                                                                                                                 |
| shipped_at                 | Date when the Order was shipped in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                                 |
| cancel_reason              | Reason why the store owner cancelled an Order. Possible values are "customer", "fraud", "inventory" or "other"                                                              |
| created_at                 | Date when the Order was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                                 | 
| updated_at                 | Date when the Order was last updated in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601)                                                                            |

Endpoints
---------

### GET /categories

Receive a list of all Categories.


| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| since_id       | Restrict results to after the specified ID                                                       |
| language       | Specify search language (required when serching for handle)                                      |
| handle         | Show Categories with a given URL                                                                   |
| parent_id      | Show Categories with a given parent category                                                       |
| created_at_min | Show Categories created after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))      |
| created_at_max | Show Categories created before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))     |
| updated_at_min | Show Categories last updated after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)) |
| updated_at_max | Show Categories last updated before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))|
| page           | Page to show                                                                                     |
| per_page       | Amount of results                                                                                |
| fields         | Comma-separated list of fields to include in the response                                        |


#### GET /checkouts

`HTTP/1.1 200 OK`

```json
[
    {
      "created_at": "2013-01-03T09:11:51-03:00",
      "description": {
          "en": "",
          "es": "",
          "pt": ""
      },
      "handle": {
          "en": "poke-balls",
          "es": "poke-balls",
          "pt": "poke-balls"
      },
      "id": 4567,
      "name": {
          "en": "Poké Balls",
          "es": "Poké Balls",
          "pt": "Poké Balls"
      },
      "parent": null,
      "subcategories": [],
      "updated_at": "2013-03-11T09:14:11-03:00"
    }
]
```

#### GET /categories?fields=id,name,subcategories

`HTTP/1.1 200 OK`

```json
[
    {
      "id": 4567,
      "name": {
          "en": "Poké Balls",
          "es": "Poké Balls",
          "pt": "Poké Balls"
      },
      "subcategories": []
    }
]
```

### GET /categories/#{id}

Receive a single Category

| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| fields         | Comma-separated list of fields to include in the response                                        |

#### GET /categories/4567

`HTTP/1.1 200 OK`

```json
{
  "created_at": "2013-01-03T09:11:51-03:00",
  "description": {
      "en": "",
      "es": "",
      "pt": ""
  },
  "handle": {
      "en": "poke-balls",
      "es": "poke-balls",
      "pt": "poke-balls"
  },
  "id": 4567,
  "name": {
      "en": "Poké Balls",
      "es": "Poké Balls",
      "pt": "Poké Balls"
  },
  "parent": null,
  "subcategories": [],
  "updated_at": "2013-03-11T09:14:11-03:00"
}
```

### POST /categories

Create a new Category

#### POST /categories

```json
{
    "invalid_name": "foobar"
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
    "name": [
      "can't be blank"
    ]
}
```

#### POST /categories

```json
{
    "name": {
      "en": "Gen I",
      "es": "Gen I",
      "pt": "Gen I"
    },
    "parent": 4567    
}
```

`HTTP/1.1 201 Created`

```json
{
  "created_at": "2013-06-01T12:15:11-03:00",
  "description": {
      "en": "",
      "es": "",
      "pt": ""
  },
  "handle": {
      "en": "gen-i",
      "es": "gen-i",
      "pt": "gen-i"
  },
  "id": 5678,
  "name": {
      "en": "Gen I",
      "es": "Gen I",
      "pt": "Gen I"
  },
  "parent": 4567,
  "subcategories": [],
  "updated_at": "2013-06-01T12:15:11-03:00"
}
```

### PUT /categories/#{id}

Modify an existing Category

#### PUT /categories/5678

```json
{
  "id": 5678,
  "parent": null
}
```

`HTTP/1.1 200 OK`

```json
{
  "created_at": "2013-06-01T12:15:11-03:00",
  "description": {
      "en": "",
      "es": "",
      "pt": ""
  },
  "handle": {
      "en": "gen-i",
      "es": "gen-i",
      "pt": "gen-i"
  },
  "id": 5678,
  "name": {
      "en": "Gen I",
      "es": "Gen I",
      "pt": "Gen I"
  },
  "parent": null,
  "subcategories": [],
  "updated_at": "2013-06-01T12:15:11-03:00"
}
```

### DELETE /categories/#{id}

Remove a Category

#### DELETE /categories/4567

`HTTP/1.1 200 OK`

```json
{}
```
