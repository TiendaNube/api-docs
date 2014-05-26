Coupons
========

A discount coupon is used by our customers to provide discounts in the bills of their own customers.

Properties
----------

| Property             | Explanation                                                                                  |
| -------------------- | -------------------------------------------------------------------------------------------- |
| id                   | The unique numeric identifier for the coupon.                                                                                                                    |
| code                | String that identifies the coupon.                                                                       |
| type                | Type of the coupon. Can take the following values: percentage, absolute or shipping. The percentage type indicates that the value is a percetange discount. The type absolute indicates that the value is an absolute amount of discount. And finally the type shipping indicates that the discount value is on the shipping.               |
| valid       | Flag (true or false) that indicates if the coupon is valid or not.                  |
| start_date                 | Indicates the date from which the coupon is valid.                                                       |
| end_date      | Indicates the date of overdue of the coupon.                                                              |
| deleted_at            | Indicates the date of deletion of the coupon. The value is NULL if the coupon is still valid.                                                           |
| max_uses | Indicates the max number of times the coupon can be used.       |
| value        | Indicates the value of the discount                                                          |
| min_price               | Is a value that indicates the minimun value of the bill for applying the doscount                       |
| categories               | Is a value that indicates the categories ids (separated by commas) of the store where the discount applies.                       |

Endpoints
---------

### GET /coupons

Receive a list of all Coupon.

| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| min_start_date       | The minimum start_date to filter.                                                        |
| min_end_date | The minimum end_date to filter.       |
| max_start_date | The maximum start_date to filter.      |
| max_end_date | The maximum end_date to filter.  |
| valid | Flag (true of false) for filtering the not active coupons.


#### GET /coupons

`HTTP/1.1 200 OK`

```json
[
    {
        "id": 15647,
        "code": "HOJE",
        "type": "percentage",
        "value": "15.00",
        "valid": true,
        "used": 0,
        "max_uses": null,
        "start_date": "2013-07-23",
        "end_date": "2013-07-24",
        "min_price": null,
        "categories_enabled": null
    },
    {
        "id": 32933,
        "code": "PROMODEPRUEBA",
        "type": "absolute",
        "value": "50.00",
        "valid": true,
        "used": 0,
        "max_uses": null,
        "start_date": null,
        "end_date": null,
        "min_price": null,
        "categories_enabled": null
    },
    {
        "id": 15,
        "code": "PROMODICIEMBRE",
        "type": "percentage",
        "value": "12.00",
        "valid": false,
        "used": 0,
        "max_uses": null,
        "start_date": "2014-03-11",
        "end_date": "2014-10-11",
        "min_price": null,
        "categories_enabled": null
    },
    {
        "id": 32937,
        "code": "PROMOF",
        "type": "absolute",
        "value": "30.00",
        "valid": true,
        "used": 0,
        "max_uses": null,
        "start_date": null,
        "end_date": null,
        "min_price": null,
        "categories_enabled": "categories",
        "categories": [
            "30290",
            "56716"
        ],
        "categories_need_all_products": false
    },
    {
        "id": 6,
        "code": "PROMONOVIEMBRE",
        "type": "percentage",
        "value": "20.00",
        "valid": false,
        "used": 1,
        "max_uses": 60,
        "start_date": "2014-02-11",
        "end_date": "2014-10-11",
        "min_price": null,
        "categories_enabled": null
    }
]
```

#### GET /coupons?valid=true

`HTTP/1.1 200 OK`

```json
[
    {
        "id": 15647,
        "code": "HOJE",
        "type": "percentage",
        "value": "15.00",
        "valid": true,
        "used": 0,
        "max_uses": null,
        "start_date": "2013-07-23",
        "end_date": "2013-07-24",
        "min_price": null,
        "categories_enabled": null
    },
    {
        "id": 32933,
        "code": "PROMODEPRUEBA",
        "type": "absolute",
        "value": "50.00",
        "valid": true,
        "used": 0,
        "max_uses": null,
        "start_date": null,
        "end_date": null,
        "min_price": null,
        "categories_enabled": null
    },
    {
        "id": 32937,
        "code": "PROMOF",
        "type": "absolute",
        "value": "30.00",
        "valid": true,
        "used": 0,
        "max_uses": null,
        "start_date": null,
        "end_date": null,
        "min_price": null,
        "categories_enabled": "categories",
        "categories": [
            "30290",
            "56716"
        ],
        "categories_need_all_products": false
    }
]
```

### GET /coupons/#{id}

Receive a single Coupon

#### GET /coupons/32937

`HTTP/1.1 200 OK`

```json
{
    "id": 32937,
    "code": "PROMOF",
    "type": "absolute",
    "value": "30.00",
    "valid": true,
    "used": 0,
    "max_uses": null,
    "start_date": null,
    "end_date": null,
    "min_price": null,
    "categories_enabled": "categories",
    "categories": [
        "30290",
        "56716"
    ],
    "categories_need_all_products": false
}
```

### POST /coupons

Create a new Coupon

#### POST /coupons

| Parameter                | Explanation                                                                             |
| ------------------------ | ----------------------------------------------------------------------------------------|
| code        | The code is mandatory.                              |
| value                 | The value is mandatory if the type is percentage or absolute                                                                         |

```json
{
    "code": "PROMODEPRUEBA", 
    "type": "percentage",
    "value": 20
}
```

`HTTP/1.1 201 Created`

```json
{
    "id": 32950,
    "code": "PROMODEPRUEBA",
    "type": "percentage",
    "value": 20,
    "valid": true,
    "used": 0,
    "max_uses": null,
    "start_date": null,
    "end_date": null,
    "min_price": null,
    "categories_enabled": null
}
```

### PUT /coupons/#{id}

Modify an existing coupon

#### PUT /coupons/32950

```json
{
    "code": "PROMODEPRUEBA", 
    "type": "absolute", 
    "value": 50
}
```

`HTTP/1.1 200 OK`

```json
{
    "id": 32950,
    "code": "PROMODEPRUEBA",
    "type": "absolute",
    "value": 50,
    "valid": true,
    "used": 0,
    "max_uses": null,
    "start_date": null,
    "end_date": null,
    "min_price": null,
    "categories_enabled": null
}
```

### DELETE /coupons/#{id}

Delete an existing coupon

#### DELETE /coupons/32950

`HTTP/1.1 200 OK`

```json
{
  
}
```
