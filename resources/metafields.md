Metafields
========

The metafields are Namespaced Key - Value store for Apps.

The metafields can only be associated with the following entities: 

Product
Product_Variant
Category
Page

To do that you need to set the owner_resource to one of the above, an example would be owner_resource='Product'


A discount metafield is a way for a store to provide discounts for its customers. There are three type of metafields. Percentage, absolute and shipping. The percentage type indicates that the value is a percentage discount. The type absolute indicates that the value is an absolute amount of discount. And finally the type shipping indicates that the discount value is on the shipping.




Entidad Metafield

id
namespace
key
description
value
owner_id
owner_resource
created_at
updated_at
API

Agregar los endpoints
GET /metafields (chusmear filtros de Product)
POST /metafields para crear
PUT /metafields/{id} para editar
GET /metafields/{id} para obtener un metafield particular
DELETE /metafields/{id} para eliminarlo
Agregar la documentación para los Metafields
Scopes
Ojo que si sólo tengo el scope read_products no debería poder escribir metafields
Tiendas

Agregar una product.metafields en el Product_Interface que es un hash.
Ejemplo: Si tenemos una metafield facu en el namespace tiendanube se podría acceder haciendo product.metafields.tiendanube.facu desde la plantilla
Check Shopify's Metafields

This allows for Reviews apps for example to add SEO features combined with HTML/CSS editing by our customers


Properties
----------

| Property             | Explanation                                                                                  |
| -------------------- | -------------------------------------------------------------------------------------------- |
| id                   | The unique numeric identifier for the metafield.                                                |
| namespace                 | The namespace where the metafield makes sense. It can be any string that starts with a letter followed only by: a-Z A-B 0-9 or _.         |
| key                 | String that identifies the metafield in some namespace. It can be any string that starts with a letter followed only by: a-Z A-B 0-9 or _.                                                           |
| description                | String with some description explaining the metafield meaning.                           |
| value           | Metafield's value (String).                                                         |
| owner_resource             | Type of entity to which is associated the metaField.                                                               |
| owner_id             | Entity id to which is associated the metaField.                                                               |
| created_at     | Date when the Product was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)     | 
| updated_at     | Date when the Product was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)|

Endpoints
---------

### GET /metafields

Receive a list of all metafield.

| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| min_start_date | The minimum start_date to filter.                                                                |
| min_end_date   | The minimum end_date to filter.                                                                  |
| max_start_date | The maximum start_date to filter.                                                                |
| max_end_date   | The maximum end_date to filter.                                                                  |
| valid          | Flag (true of false) for filtering valid metafields.                                                |
| created_at_min | Show Products created after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))      |
| created_at_max | Show Products created before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))     |
| updated_at_min | Show Products last updated after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)) |
| updated_at_max | Show Products last updated before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))|
| page           | Page to show                                                                                     |
| per_page       | Amount of results                                                                                |
| fields         | Comma-separated list of fields to include in the response                                        |


#### GET /metafields

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

#### GET /metafields?valid=true

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

### GET /metafields/#{id}

Receive a single metafield

#### GET /metafields/32964

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

### POST /metafields

Create a new metafield

#### POST /metafields

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

### PUT /metafields/#{id}

Modify an existing metafield

#### PUT /metafields/32967

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

### DELETE /metafields/#{id}

Delete an existing metafield

#### DELETE /metafields/32967

`HTTP/1.1 200 OK`

```json
{
  
}
```
