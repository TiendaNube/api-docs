---
layout: default
---

Coupons
========

A discount coupon is a way for a store to provide discounts for its customers. There are three type of coupons. Percentage, absolute and shipping. The percentage type indicates that the value is a percentage discount. The type absolute indicates that the value is an absolute amount of discount. And finally the type shipping indicates that the discount value is on the shipping.

Properties
----------

| Property             | Explanation                                                                                  |
| -------------------- | -------------------------------------------------------------------------------------------- |
| id                   | The unique numeric identifier for the coupon.                                                |
| code                 | String that identifies the coupon.                                                           |
| type                 | Type of the coupon. Can take the following values: percentage, absolute or shipping.         |
| valid                | Flag (true or false) that indicates if the coupon is valid or not.                           |
| start_date           | Date from which the coupon is valid.                                                         |
| end_date             | Date of overdue of the coupon.                                                               |
| deleted_at           | Date when the coupon was deleted. The value is NULL if the coupon is still valid.            |
| max_uses             | Max number of times the coupon can be used.                                                  |
| value                | Value of the discount                                                                        |
| min_price            | Indicates the minimun value of the bill for applying the discount                            |
| categories           | List of [Category](https://github.com/tiendanube/api-docs/blob/master/resources/category.md) objects representing the categories of the store where the discount applies. |

Endpoints
---------

### GET /coupons

Receive a list of all Coupon.

| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| min_start_date | The minimum start_date to filter.                                                                |
| min_end_date   | The minimum end_date to filter.                                                                  |
| max_start_date | The maximum start_date to filter.                                                                |
| max_end_date   | The maximum end_date to filter.                                                                  |
| valid          | Flag (true of false) for filtering valid coupons.                                                |
| created_at_min | Show Products created after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))      |
| created_at_max | Show Products created before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))     |
| updated_at_min | Show Products last updated after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)) |
| updated_at_max | Show Products last updated before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))|
| page           | Page to show                                                                                     |
| per_page       | Amount of results                                                                                |
| fields         | Comma-separated list of fields to include in the response                                        |


#### GET /coupons

`HTTP/1.1 200 OK`

```json
[
    {
        "id": 32965,
        "code": "PR2",
        "type": "percentage",
        "value": "30.00",
        "valid": true,
        "used": 0,
        "max_uses": 100,
        "start_date": "2014-05-08",
        "end_date": "2014-06-08",
        "min_price": 10,
        "categories": [
            {
                "id": 117023,
                "name": {
                    "es": "Oxido N\u00edtrico (NO2)",
                    "en": "Oxido N\u00edtrico (NO2)"
                },
                "description": {
                    "es": "",
                    "en": ""
                },
                "handle": {
                    "es": "oxido-nitrico-no2",
                    "en": "oxido-nitrico-no2"
                },
                "parent": null,
                "subcategories": [],
                "created_at": "2013-03-22T14:40:55+0000",
                "updated_at": "2014-05-28T20:07:05+0000"
            }
        ]
    },
    {
        "id": 32966,
        "code": "PR23",
        "type": "percentage",
        "value": "30.00",
        "valid": true,
        "used": 0,
        "max_uses": 100,
        "start_date": "2014-05-08",
        "end_date": "2014-06-08",
        "min_price": 10,
        "categories": null
    },
    {
        "id": 32964,
        "code": "PR4",
        "type": "percentage",
        "value": "30.00",
        "valid": true,
        "used": 0,
        "max_uses": 100,
        "start_date": "2014-06-07",
        "end_date": "2014-06-08",
        "min_price": 10,
        "categories": [
            {
                "id": 117023,
                "name": {
                    "es": "Oxido N\u00edtrico (NO2)",
                    "en": "Oxido N\u00edtrico (NO2)"
                },
                "description": {
                    "es": "",
                    "en": ""
                },
                "handle": {
                    "es": "oxido-nitrico-no2",
                    "en": "oxido-nitrico-no2"
                },
                "parent": null,
                "subcategories": [],
                "created_at": "2013-03-22T14:40:55+0000",
                "updated_at": "2014-05-28T20:07:05+0000"
            }
        ]
    },
    {
        "id": 32963,
        "code": "PR5",
        "type": "percentage",
        "value": "30.00",
        "valid": false,
        "used": 0,
        "max_uses": null,
        "start_date": null,
        "end_date": null,
        "min_price": 100,
        "categories": [
            {
                "id": 105190,
                "name": {
                    "es": "Pantalones",
                    "en": "Pantalones"
                },
                "description": {
                    "es": "",
                    "en": ""
                },
                "handle": {
                    "es": "pantalones",
                    "en": "pantalones"
                },
                "parent": null,
                "subcategories": [
                    119258
                ],
                "created_at": "2013-02-19T17:34:36+0000",
                "updated_at": "2014-05-28T20:07:06+0000"
            },
            {
                "id": 105191,
                "name": {
                    "es": "Camisas",
                    "en": "Camisas"
                },
                "description": {
                    "es": "",
                    "en": ""
                },
                "handle": {
                    "es": "camisas",
                    "en": "camisas"
                },
                "parent": null,
                "subcategories": [],
                "created_at": "2013-02-19T17:34:37+0000",
                "updated_at": "2014-05-28T20:07:06+0000"
            },
            {
                "id": 117023,
                "name": {
                    "es": "Oxido N\u00edtrico (NO2)",
                    "en": "Oxido N\u00edtrico (NO2)"
                },
                "description": {
                    "es": "",
                    "en": ""
                },
                "handle": {
                    "es": "oxido-nitrico-no2",
                    "en": "oxido-nitrico-no2"
                },
                "parent": null,
                "subcategories": [],
                "created_at": "2013-03-22T14:40:55+0000",
                "updated_at": "2014-05-28T20:07:05+0000"
            }
        ]
    }
]
```

#### GET /coupons?valid=true

`HTTP/1.1 200 OK`

```json
[
    {
        "id": 32965,
        "code": "PR2",
        "type": "percentage",
        "value": "30.00",
        "valid": true,
        "used": 0,
        "max_uses": 100,
        "start_date": "2014-05-08",
        "end_date": "2014-06-08",
        "min_price": 10,
        "categories": [
            {
                "id": 117023,
                "name": {
                    "es": "Oxido N\u00edtrico (NO2)",
                    "en": "Oxido N\u00edtrico (NO2)"
                },
                "description": {
                    "es": "",
                    "en": ""
                },
                "handle": {
                    "es": "oxido-nitrico-no2",
                    "en": "oxido-nitrico-no2"
                },
                "parent": null,
                "subcategories": [],
                "created_at": "2013-03-22T14:40:55+0000",
                "updated_at": "2014-05-28T20:07:05+0000"
            }
        ]
    },
    {
        "id": 32966,
        "code": "PR23",
        "type": "percentage",
        "value": "30.00",
        "valid": true,
        "used": 0,
        "max_uses": 100,
        "start_date": "2014-05-08",
        "end_date": "2014-06-08",
        "min_price": 10,
        "categories": null
    },
    {
        "id": 32964,
        "code": "PR4",
        "type": "percentage",
        "value": "30.00",
        "valid": true,
        "used": 0,
        "max_uses": 100,
        "start_date": "2014-06-07",
        "end_date": "2014-06-08",
        "min_price": 10,
        "categories": [
            {
                "id": 117023,
                "name": {
                    "es": "Oxido N\u00edtrico (NO2)",
                    "en": "Oxido N\u00edtrico (NO2)"
                },
                "description": {
                    "es": "",
                    "en": ""
                },
                "handle": {
                    "es": "oxido-nitrico-no2",
                    "en": "oxido-nitrico-no2"
                },
                "parent": null,
                "subcategories": [],
                "created_at": "2013-03-22T14:40:55+0000",
                "updated_at": "2014-05-28T20:07:05+0000"
            }
        ]
    }
]
```

### GET /coupons/{id}

Receive a single Coupon

#### GET /coupons/32964

`HTTP/1.1 200 OK`

```json
{
    "id": 32964,
    "code": "PR4",
    "type": "percentage",
    "value": "30.00",
    "valid": true,
    "used": 0,
    "max_uses": 100,
    "start_date": "2014-06-07",
    "end_date": "2014-06-08",
    "min_price": 10,
    "categories": [
        {
            "id": 117023,
            "name": {
                "es": "Oxido N\u00edtrico (NO2)",
                "en": "Oxido N\u00edtrico (NO2)"
            },
            "description": {
                "es": "",
                "en": ""
            },
            "handle": {
                "es": "oxido-nitrico-no2",
                "en": "oxido-nitrico-no2"
            },
            "parent": null,
            "subcategories": [],
            "created_at": "2013-03-22T14:40:55+0000",
            "updated_at": "2014-05-28T20:07:05+0000"
        }
    ]
}
```

### POST /coupons

Create a new Coupon

#### POST /coupons

| Parameter                | Explanation                                                                             |
| ------------------------ | ----------------------------------------------------------------------------------------|
| code                     | The code is required. Must be unique and can contain only alfanumeric characters        |
| value                    | The value is mandatory if the type is percentage or absolute                            |

```json
{
    "code": "PRUEBA",
    "type": "percentage",
    "value": "30.00",
    "max_uses": 100,
    "min_price": 10,
    "categories": null,
    "start_date": "2014-05-08",
    "end_date": "2014-06-08"
}
```

`HTTP/1.1 201 Created`

```json
{
    "id": 32967,
    "code": "PRUEBA",
    "type": "percentage",
    "value": "30.00",
    "valid": true,
    "used": null,
    "max_uses": 100,
    "start_date": "2014-05-08",
    "end_date": "2014-06-08",
    "min_price": 10,
    "categories": null
}
```

### PUT /coupons/{id}

Modify an existing coupon

#### PUT /coupons/32967

```json
{
    "code": "OTRAPRUEBA", 
    "type": "absolute", 
    "value": 50
}
```

`HTTP/1.1 200 OK`

```json
{
    "id": 32967,
    "code": "OTRAPRUEBA",
    "type": "absolute",
    "value": 50,
    "valid": true,
    "used": 0,
    "max_uses": 100,
    "start_date": "2014-05-08",
    "end_date": "2014-06-08",
    "min_price": 10,
    "categories": null
}
```

### DELETE /coupons/{id}

Delete an existing coupon

#### DELETE /coupons/32967

`HTTP/1.1 200 OK`

```json
{
  
}
```
