Shipping Carrier
===============

A shipping carrier (or shipping company) service provides real-time shipping rates for merchants.

Using these endpoints, you can add a shipping carrier to a store and provide shipping rates at checkout.

Shipping Carrier Properties
---------------------------

| Property           | Explanation                                                                                                                   |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| id                 | The unique numeric identifier for the Shipping Carrier.                                                                       |
| name               | The name of the Shipping Carrier as seen by both merchants and buyers.                                                        |
| callback_url       | The URL endpoint that we need to retrieve shipping rates. **Must be HTTPS.**                                                  |
| types              | The supported shipping types, can be one or both _ship_ or _pickup_.                                                          |
| active             | Whether this Shipping Carrier is active.                                                                                      |
| created_at         | Date when the Shipping Carrier was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601).                       | 
| updated_at         | Date when the Shipping Carrier was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601).                  |

Shipping Carrier Options
------------------------

Almost every shipping carrier has multiple options to offer to the consumers, like standard and express shipping options. These options may provide some configurable values to the merchants through the store's admin, like additional costs and days, and also free shipping availability options.

Shipping Carrier Options Properties
-----------------------------------

| Property | Explanation |
|----------|-------------|
| id | The unique numeric identifier for the Shipping Carrier Option. |
| code | A unique code associated with the Shipping Carrier Option. |
| name | The name of the Shipping Carrier Option as seen by both merchants and buyers. |
| additional_days | The additional days configurable value that will be added to the option's estimated delivery time. |
| additional_cost | The additional cost configurable value that will be added to the option's consumer price. |
| allow_free_shipping | The configurable free shipping eligible parameter that specifies that an option allows free shipping. |
| active | Whether this Shipping Carrier Option is active. |
| created_at | Date when the Shipping Carrier Option was created in [ISO 8601 format](https://en.wikipedia.org/wiki/ISO_8601). |
| updated_at | Date when the Shipping Carrier Option was last updated in [ISO 8601 format](https://en.wikipedia.org/wiki/ISO_8601). |


Providing rates to our merchants 
--------------------------------

When adding a Shipping Carrier to a store, you need to provide POST endpoints in the callback properties where we can retrieve the shipping rates.

We provide to different kind of shipments. Shipments that will be shipped to a buyer's address (_ship_) or shipments that will be picked up by the buyer (_pickup_).

The response object rates must be a JSON array of objects with the following fields. We need all required fields for the integration to work properly.

##### Important: you have to post all your available rates, our API will filter by carrier options active, and apply the settings made by users over the rates (additional days and additional cost).


| Property           | Explanation                                                                                                                                |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| name               | __(required)__ The name of the rate which buyers will see at checkout.                                                                     |
| code               | __(required)__ A unique code associated with the carrier option code. It´s used to apply the settings over the rate.                       |
| price              | __(required)__ The rate's price that will be payed by the buyer.                                                                           |
| currency           | __(required)__ The rate's currency. Currency codes must be specified according to [ISO 4217](https://www.currency-iso.org/en/home/tables/table-a1.html)                                                                                                     |
| type               | __(required)__ The rate's type: _ship_ if it will be deliverd to a buyer's address or _pickup_ if it will be picked up by the buyer.       |
| address            | __(required)__ _(pickup only)_ Where the pickup point is located.                                                                          |
| hours              | __(required)__ _(pickup only)_ The location open hours providing: `day` (0: sunday, 6: saturday), `start` and `end` on `HHMM` format.      |
| price_merchant     | The rate's price that will be payed by the merchant. It may be different than `price` due to items with free shipping. Defaults to `price`.|
| min_delivery_date  | The earliest estimated delivery date in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601). Defaults to `null`.                      |
| max_delivery_date  | The latest estimated delivery date in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601). Defaults to `null`.                        | 
| id_required        | Whether the customer must provide an identification number at checkout. Defaults to `false`.                                               |
| phone_required     | Whether the customer must provide a phone number at checkout. Defaults to `false`.                                                         |
| accepts_cod        | Whether the customer is allowed to pay for this rate with a Cash on Delivery (COD) payment method (defaults to `true`).                    |
| reference          | An internal reference yo wish to save for the rate. We will save it and return it on the Order as `shipping_option_reference`.             |
| availability       | _(pickup only)_ Whether there is availability for all items at the location. Defaults to `true`.                                                               |

### Example requests for shipping rates

#### POST /your_callback_url

```json
{
    "store_id": 123456,
    "currency": "ARS",
    "language": "es",
    "origin": {
        "name": null,
        "address": "Avenida Cabildo",
        "number": "4781",
        "floor": null,
        "locality": "Nuñez",
        "city": "Capital Federal",
        "province": "Capital Federal",
        "country": "AR",
        "postal_code": "1602",
        "phone": null
    },
    "destination": {
        "name": null,
        "address": "Juan B. Justo",
        "number": "3000",
        "floor": null,
        "locality": "Nuñez",
        "city": "Capital Federal",
        "province": "Capital Federal",
        "country": "AR",
        "postal_code": "1602",
        "phone": null
    },
    "items": [
        {
            "name": "My product",
            "sku": null,
            "quantity": 1,
            "free_shipping": true,
            "grams": 1000,
            "price": 20.00,
            "dimensions": {
                "width": 12.0,
                "height": 10.0,
                "depth": 10.0
            }
        }
    ]
}
```

`HTTP/1.1 200 OK`
```json
{
   "rates": [
       {
           "name": "Standard Shipping",
           "code": "standard",
           "price": 0.00,
           "price_merchant": 14.15,
           "currency": "ARS",
           "type": "ship",
           "min_delivery_date": "2016-07-14T14:48:45-0300",
           "max_delivery_date": "2016-07-17T14:48:45-0300",
           "phone_required": true,
           "reference": "ref123"
       },
       {
           "name": "Express Shipping",
           "code": "express",
           "price": 28.15,
           "currency": "ARS",
           "type": "ship",
           "min_delivery_date": "2016-07-12T14:48:45-0300",
           "max_delivery_date": "2016-07-13T14:48:45-0300",
           "id_required": true,
           "reference": "ref123"
       },
       {
           "name": "Shipping branch #1",
           "code": "pickup_1",
           "price": 14.15,
           "currency": "ARS",
           "type": "pickup",
           "min_delivery_date": "2016-07-14T14:48:45-0300",
           "max_delivery_date": "2016-07-17T14:48:45-0300",
           "phone_required": true,
           "id_required": true,
           "accepts_cod": true,
           "address": {
               "address": "My address",
               "number": "345",
               "floor": null,
               "locality": "Lanús",
               "city": "Lanús",
               "province": "Buenos Aires",
               "country": "AR",
               "phone": "+54 11 5678-1234",
               "zipcode": "1824",
               "latitude": "123456",
               "longitude": "-123456"
           },
           "hours": [
               {
                   "day": 1,
                   "start": "0900",
                   "end": "1800"
               },
               {
                   "day": 2,
                   "start": "0900",
                   "end": "1800"
               },
               {
                   "day": 3,
                   "start": "0900",
                   "end": "1800"
               },
               {
                   "day": 4,
                   "start": "0900",
                   "end": "1800"
               },
               {
                   "day": 5,
                   "start": "0900",
                   "end": "1800"
               }
           ],
           "availability": false,
           "reference": "ref123"
       },
       {
           "name": "Shipping branch #2",
           "code": "pickup_2",
           "price": 14.15,
           "currency": "ARS",
           "type": "pickup",
           "min_delivery_date": "2016-07-12T14:48:45-0300",
           "max_delivery_date": "2016-07-13T14:48:45-0300",
           "accepts_cod": true,
           "phone_required": true,
           "id_required": true,
           "address": {
               "address": "Another address",
               "number": "123",
               "floor": null,
               "locality": "Florida",
               "city": "Vicente Lopez",
               "province": "Buenos Aires",
               "country": "AR",
               "phone": "+54 11 1234-5678",
               "zipcode": "1602",
               "latitude": null,
               "longitude": null
           },
           "hours": [
               {
                   "day": 1,
                   "start": "0900",
                   "end": "1800"
               },
               {
                   "day": 2,
                   "start": "0900",
                   "end": "1800"
               },
               {
                   "day": 3,
                   "start": "0900",
                   "end": "1800"
               },
               {
                   "day": 4,
                   "start": "0900",
                   "end": "1800"
               },
               {
                   "day": 5,
                   "start": "0900",
                   "end": "1800"
               }
           ],
           "reference": "ref123"
       }       
   ]
}
```

Server-side caching of requests
-------------------------------

We will provide server-side caching to reduce the number of requests done to your callback URLs. Any shipping rate request that identically matches the following fields will be retrieved from our cache of the initial response:

- variant ids
- variant quantities
- default shipping box weight and dimensions
- shipping carrier id
- origin address
- destination address
- item weights & dimensions

If any of these fields differ, or if the cache has expired since the original request, then new shipping rates are requested.

About caching expire time (TTL)

- Success responses, with status code *200* from callback URLs, expire after 15 minutes.
- Error responses, with status code *422* from callback URLs, expire after 1 minute.
- Otherwise, the responses won't be cached.

Endpoints
---------

### POST /shipping_carriers

Create a new Shipping Carrier

| Parameter          | Explanation                                                                                                    |
| ------------------ | -------------------------------------------------------------------------------------------------------------- |
| name               | __(required)__ The name of the Shipping Carrier as seen by both merchants and buyers.                                         |
| callback_url       | __(required)__ The URL endpoint that we need to retrieve shipping rates when the buyer wants the items ship to an address.    |
| types              | __(required)__ Comma separated values indicating supported methods: can be any of `ship` or `pickup`.                         |
| active             | The shipping carrier availability status, used to disable or enable it for the store. Defaults to __true__.             |

#### POST /shipping_carriers

```json
{
    "name": "My Shipping Company",
    "callback_url": "https://example.com/rates",
    "types": "ship,pickup"
}
```

`HTTP/1.1 201 Created`

```json
{
    "id": 123,
    "name": "My Shipping Company",
    "active": true,
    "callback_url": "https://example.com/rates",
    "types": "ship,pickup",
    "created_at": "2013-06-11T11:12:10-03:00",
    "updated_at": "2013-06-11T11:12:10-03:00"    
}
```

### GET /shipping_carriers

Receive a list of all Shipping Carriers.


#### GET /shipping_carriers

`HTTP/1.1 200 OK`

```json
[
    {
        "id": 1,
        "name": "Example Company",
        "active": true,
        "callback_url": "https://example.com/rates",
        "created_at": "2013-04-12T10:15:10-03:00",
        "updated_at": "2013-04-12T10:15:10-03:00"
    },
    {
        "id": 2,
        "name": "Another example Company",
        "active": true,
        "callback_url": "https://example.com/rates",
        "created_at": "2013-04-12T10:20:13-03:00",
        "updated_at": "2013-04-12T10:20:13-03:00"
    }
]
```

### GET /shipping_carriers/{id}

Receive a single Shipping Carrier

#### GET /shipping_carrier/1

`HTTP/1.1 200 OK`

```json
{
    "id": 1,
    "name": "Example Company",
    "active": true,
    "callback_url": "https://example.com/rates",
    "created_at": "2013-04-12T10:15:10-03:00",
    "updated_at": "2013-04-12T10:15:10-03:00"
}
```

### PUT /shipping_carriers/{id}

Modify an existing Shipping Carrier

#### PUT /shipping_carriers/123

```json
{
    "name": "My Super Shipping Company",
    "types": "ship",
    "active": false  
}
```

`HTTP/1.1 200 OK`

```json
{
    "id": 123,
    "name": "My Super Shipping Company",
    "active": false,
    "callback_url": "https://example.com/rates",
    "types": "ship",
    "created_at": "2013-06-11T11:12:10-03:00",
    "updated_at": "2013-06-13T23:09:43-03:00"    
    
}
```

### DELETE /shipping_carriers/{id}

Remove a Shipping Carrier

#### DELETE /shipping_carriers/123

`HTTP/1.1 200 OK`

```json
{}
```

### POST /shipping_carriers/{carrier_id}/options

Create a new Shipping Carrier Option

| Parameter | Explanation | Required |
|-----------|-------------|----------|
| code | A unique code associated with the Shipping Carrier Option. The carrier option `code` is used for matching against the shipping rates. This allows merchant to add additional day and cost settings to the retrieved carrier options. | Yes |
| name | The name of the Shipping Carrier Option as seen by both merchants and buyers. | Yes |
| additional_days | The additional days configurable value that will be added to the option's estimated delivery time. | No |
| additional_cost | The additional cost configurable value that will be added to the option's consumer price. | No |
| allow_free_shipping | The configurable free shipping eligible parameter that specifies that an option allows free shipping. | No |
| active | The avaiability status of the Shipping Carrier Option. Defaults to __true__ | No |

#### POST /shipping_carriers/1234/options

```json
{
    "code": "pac",
    "name": "Correios - PAC"
}
```

`HTTP/1.1 201 Created`

```json
{
    "id": 123,
    "code": "pac",
    "name": "Correios - PAC",
    "additional_days": 2,
    "additional_cost": 10.0,
    "allow_free_shipping": true,
    "active": true,
    "created_at": "2013-04-12T10:15:10-03:00",
    "updated_at": "2013-04-12T10:15:10-03:00"
}
```

### GET /shipping_carriers/{carrier_id}/options

Receive a list of all Shipping Carriers Options.


#### GET /shipping_carriers/1234/options

`HTTP/1.1 200 OK`

```json
[
    {
        "id": 123,
        "code": "pac",
        "name": "Correios - PAC",
        "additional_days": 0,
        "additional_cost": 0.0,
        "allow_free_shipping": false,
        "active": true,
        "created_at": "2013-04-12T10:15:10-03:00",
        "updated_at": "2013-04-12T10:15:10-03:00"
    },
    {
        "id": 456,
        "code": "sedex",
        "name": "Correios - SEDEX",
        "additional_days": 0,
        "additional_cost": 0.0,
        "allow_free_shipping": false,
        "active": true
    }
]
```

### GET /shipping_carriers/{carrier_id}/options/{option_id}

Receive a single Shipping Carrier Option.

#### GET /shipping_carrier/1234/options/123

`HTTP/1.1 200 OK`

```json
{
    "id": 123,
    "code": "pac",
    "name": "Correios - PAC",
    "additional_days": 0,
    "additional_cost": 0.0,
    "allow_free_shipping": false,
    "active": true,
    "created_at": "2013-04-12T10:15:10-03:00",
    "updated_at": "2013-04-12T10:15:10-03:00"
}
```

### PUT /shipping_carriers/{carrier_id}/options/{option_id}

Modify an existing Shipping Carrier Option.

#### PUT /shipping_carriers/1234/options/123

```json
{
    "additional_days": 2,
    "additional_cost": 10.0,
    "allow_free_shipping": true
}
```

`HTTP/1.1 200 OK`

```json
{
    "id": 123,
    "code": "pac",
    "name": "Correios - PAC",
    "additional_days": 2,
    "additional_cost": 10.0,
    "allow_free_shipping": true,
    "active": true,
    "created_at": "2013-04-12T10:15:10-03:00",
    "updated_at": "2013-04-12T10:15:10-03:00"
}
```

### DELETE /shipping_carriers/{carrier_id}/options/{option_id}

Remove a Shipping Carrier Option

#### DELETE /shipping_carriers/1234/options/123

`HTTP/1.1 200 OK`

```json
{}
```

Fulfillment Events 
------------------

A Fulfillment Event represents a tracking event belonging to the order's fulfillment. Fulfillment events are displayed on the order tracking page to give buyers visibility on their shipment status.

### GET /orders/{order_id}/fulfillments

Receive a list of all Fulfillment Events for an order.

`HTTP/1.1 200 OK`

```json
[
    {
        "id": 1,
        "order_id": 123,
        "status": "dispatched",
        "description": "Objeto postado",
        "city": "São Paulo",
        "province": "São Paulo",
        "country": "BR",
        "created_at": "2013-04-20T08:23:42-03:00",
        "updated_at": "2013-04-20T08:23:42-03:00",
        "happened_at": "2013-04-20T05:17:02-03:00",
        "estimated_delivery_at": "2013-04-22T12:00:00-03:00"
    },
    {
        "id": 2,
        "order_id": 123,
        "status": "in_transit",
        "description": "Objeto encaminhado",
        "city": "São Paulo",
        "province": "São Paulo",
        "country": "BR",
        "created_at": "2013-04-21T17:15:00-03:00",
        "updated_at": "2013-04-21T17:15:00-03:00",
        "happened_at": "2013-04-21T17:14:25-03:00",
        "estimated_delivery_at": "2013-04-22T12:00:00-03:00"
    }
]
```

### GET /orders/{order_id}/fulfillments/{id}

Receive a single Fulfillment Event

#### GET /orders/123/fulfillments/1

`HTTP/1.1 200 OK`

```json
{
    "id": 1,
    "order_id": 123,
    "status": "dispatched",
    "description": "Objeto postado",
    "city": "São Paulo",
    "province": "São Paulo",
    "country": "BR",
    "created_at": "2013-04-20T08:23:42-03:00",
    "updated_at": "2013-04-20T08:23:42-03:00",
    "happened_at": "2013-04-20T05:17:02-03:00",
    "estimated_delivery_at": "2013-04-22T12:00:00-03:00"
}
```

### POST /orders/{order_id}/fulfillments

Create a new Fulfillment Event

| Parameter            | Explanation                                                                                                     |
| -------------------- | --------------------------------------------------------------------------------------------------------------- |
| status               | Fulfillment Event's status (see below for accepted values).                                                     |
| description          | Message describing the status.                                                                                   |
| city                 | Fulfillment Event's city.                                                                                       |
| province             | Fulfillment Event's province.                                                                                   |
| country              | Fulfillment Event's country in [ISO 3166-1 format](http://en.wikipedia.org/wiki/ISO_3166-1).                    |
| happened_at          | Date when the Fulfillment Event occured was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601).|
| estimated_delivery_at| Estimated date when the package will be delivered in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601).  |

Valid status are:


| Status                 | Explanation                                                    |
| ---------------------- | -------------------------------------------------------------- |
| dispatched             | Package has been posted by the merchant.                       |
| received_by_post_office| Package has been received by the Shipping Carrier.             |
| in_transit             | Package is in transit.                                         |
| out_for_delivery       | Package is out for delivery.                                   |
| delivery_attempt_failed| Package could not be delivered.                                |
| delayed                | Package delayed.                                               |
| ready_for_pickup       | Package is ready for pickup.                                   |
| delivered              | Package was delivered.                                         |
| returned_to_sender     | Package was returned to sender.                                |
| lost                   | Package lost.                                                  |
| failure                | Package delivery failed.                                       |



#### POST /orders/123/fulfillments

```json
{
    "status": "delivered",
    "description": "Objeto entregue ao destinatário",
    "city": "São Paulo",
    "province": "São Paulo",
    "country": "BR",
    "happened_at": "2013-04-22T11:39:12-03:00",
    "estimated_delivery_at": "2013-04-22T11:39:12-03:00"
}
```

`HTTP/1.1 201 Created`

```json
{
    "id": 3,
    "order_id": 123,
    "status": "delivered",
    "description": "Objeto entregue ao destinatário",
    "city": "São Paulo",
    "province": "São Paulo",
    "country": "BR",
    "created_at": "2013-04-20T08:23:42-03:00",
    "updated_at": "2013-04-20T08:23:42-03:00",
    "happened_at": "2013-04-20T05:17:02-03:00",
    "estimated_delivery_at": "2013-04-22T12:00:00-03:00"
}
```

### DELETE /orders/${order_id}/fulfillments/{id}

Remove a Fulfillment Event

#### DELETE /orders/123/fulfillments/1

`HTTP/1.1 200 OK`

```json
{}
```


