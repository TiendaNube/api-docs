# Product Variant

Product variants allow you to group a shoe with different sizes and colors in the same product.

## Properties

| Property          | Explanation                                                                                                                                                                                             |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                | The unique numeric identifier for the Product Variant                                                                                                                                                   |
| image_id          | The id of the product's image associated with the variant                                                                                                                                               |
| product_id        | The id of the product associated with the variant                                                                                                                                                       |
| price             | Price of the Product Variant. _null_ indicates the product will initiate a contact instead of a checkout process                                                                                        |
| promotional_price | Lower price to display as a sale. The value of price will be displayed crossed out for comparison                                                                                                       |
| stock_management  | Specifies whether or not Tiendanube/Nuvemshop tracks the number of items in stock for this product variant. Valid values are _true_ if Tiendanube/Nuvemshop tracks the stock, _false_ if it doesn't.    |
| stock             | Stock of the Product Variant. If `stock_management` is false the stock will be null                                                                                                                     |
| weight            | Weight of the Product Variant in kilograms                                                                                                                                                              |
| width             | Width of the Product Variant in centimetres                                                                                                                                                             |
| height            | Height of the Product Variant in centimetres                                                                                                                                                            |
| depth             | Depth of the Product Variant in centimetres                                                                                                                                                             |
| sku               | Unique identifier of the Product Variant in your store                                                                                                                                                  |
| values            | List of the values of the attributes whose values define the variant. E.g.: Large, Medium, etc. It is important that the number of `values` is equal to the number of `attributes` within the products. |
| barcode           | The value associated with an identifier of the product (GTIN, EAN, ISBN, etc.)                                                                                                                          |
| mpn               | The Manufacturer Part Number (MPN) of the product, or the product to which the offer refers.                                                                                                            |
| age_group         | Attribute to set the demographic that the product is designed for. It is optional and only supports the values: "newborn", "infant", "toddler", "kids" and "adult"                                      |
| gender            | Attribute to specify the gender your product is designed for. It is optional and only supports the values: "female", "male" and "unisex"                                                                |
| created_at        | Date when the Product Variant was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)                                                                                                   |
| updated_at        | Date when the Product Variant was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)                                                                                              |

## Endpoints

### GET /products/{product_id}/variants

Receive a list of all Product Variants for a given product.

| Parameter      | Explanation                                                                                               |
| -------------- | --------------------------------------------------------------------------------------------------------- |
| since_id       | Restrict results to after the specified ID                                                                |
| created_at_min | Show Product Variants created after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))       |
| created_at_max | Show Product Variants created before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))      |
| updated_at_min | Show Product Variants last updated after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))  |
| updated_at_max | Show Product Variants last updated before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)) |
| page           | Page to show                                                                                              |
| per_page       | Amount of results                                                                                         |
| fields         | Comma-separated list of fields to include in the response                                                 |

#### GET /products/1234/variants

`HTTP/1.1 200 OK`

```json
[
  {
    "id": 101,
    "image_id": null,
    "promotional_price": "19.00",
    "created_at": "2013-01-03T09:11:51-03:00",
    "depth": null,
    "height": null,
    "values": [
      {
        "en": "Small"
      }
    ],
    "price": "25.00",
    "product_id": 1234,
    "stock_management": true,
    "stock": 5,
    "sku": "BSG1234A",
    "mpn": null,
    "age_group": null,
    "gender": null,
    "updated_at": "2013-03-11T09:14:11-03:00",
    "weight": "2.00",
    "width": null
  },
  {
    "id": 112,
    "image_id": null,
    "promotional_price": "19.00",
    "created_at": "2013-01-03T09:11:51-03:00",
    "depth": null,
    "height": null,
    "values": [
      {
        "en": "Medium"
      }
    ],
    "price": "25.00",
    "product_id": 1234,
    "stock_management": true,
    "stock": 5,
    "sku": "BSG1234B",
    "mpn": null,
    "age_group": null,
    "gender": null,
    "updated_at": "2013-03-11T09:14:11-03:00",
    "weight": "2.25",
    "width": null
  },
  {
    "id": 133,
    "image_id": null,
    "promotional_price": "19.00",
    "created_at": "2013-01-03T09:11:51-03:00",
    "depth": null,
    "height": null,
    "values": [
      {
        "en": "Small"
      }
    ],
    "price": "25.00",
    "product_id": 1234,
    "stock_management": true,
    "stock": 5,
    "sku": "BSG1234C",
    "mpn": null,
    "age_group": null,
    "gender": null,
    "updated_at": "2013-03-11T09:14:11-03:00",
    "weight": "2.50",
    "width": null
  }
]
```

#### GET /product/1234/variants?since_id=105

`HTTP/1.1 200 OK`

```json
[
  {
    "id": 112,
    "image_id": null,
    "promotional_price": "19.00",
    "created_at": "2013-01-03T09:11:51-03:00",
    "depth": null,
    "height": null,
    "values": [
      {
        "en": "Small"
      }
    ],
    "price": "25.00",
    "product_id": 1234,
    "stock_management": true,
    "stock": 5,
    "sku": "BSG1234B",
    "mpn": "00638HAY",
    "age_group": "adult",
    "gender": "unisex",
    "updated_at": "2013-03-11T09:14:11-03:00",
    "weight": "2.25",
    "width": null
  },
  {
    "id": 133,
    "image_id": null,
    "promotional_price": "19.00",
    "created_at": "2013-01-03T09:11:51-03:00",
    "depth": null,
    "height": null,
    "values": [
      {
        "en": "Small"
      }
    ],
    "price": "25.00",
    "product_id": 1234,
    "stock_management": true,
    "stock": 5,
    "sku": "BSG1234C",
    "mpn": "00638ANG",
    "age_group": "adult",
    "gender": "unisex",
    "updated_at": "2013-03-11T09:14:11-03:00",
    "weight": "2.50",
    "width": null
  }
]
```

### GET /products/{product_id}/variants/{id}

Receive a single Product Variant

| Parameter | Explanation                                               |
| --------- | --------------------------------------------------------- |
| fields    | Comma-separated list of fields to include in the response |

#### GET /products/1234/variants/112

`HTTP/1.1 200 OK`

```json
{
  "id": 112,
  "image_id": null,
  "promotional_price": "19.00",
  "created_at": "2013-01-03T09:11:51-03:00",
  "depth": null,
  "height": null,
  "values": [
    {
      "en": "Small"
    }
  ],
  "price": "25.00",
  "product_id": 1234,
  "stock_management": true,
  "stock": 5,
  "sku": "BSG1234B",
  "mpn": "LO2302GIU",
  "age_group": "adult",
  "gender": "female",
  "updated_at": "2013-03-11T09:14:11-03:00",
  "weight": "2.25",
  "width": null
}
```

### POST /products/{product_id}/variants

Create a new Product Variant

#### POST /products/1234/variants

```json
{
  "values": [
    {
      "en": "X-Large"
    }
  ],
  "price": "19.00"
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
  "code": 422,
  "message": "Unprocessable Entity",
  "description": "Variants cannot be repeated"
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
  "code": 422,
  "message": "Unprocessable Entity",
  "description": "Product is not allowed to have more than 1000 variants"
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
  "code": 422,
  "message": "Unprocessable Entity",
  "description": "The selected age group is invalid"
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
  "code": 422,
  "message": "Unprocessable Entity",
  "description": "The selected gender is invalid"
}
```

`HTTP/1.1 201 Created`

```json
{
  "id": 144,
  "image_id": null,
  "promotional_price": null,
  "created_at": "2013-01-03T09:11:51-03:00",
  "depth": null,
  "height": null,
  "values": [
    {
      "en": "X-Large"
    }
  ],
  "price": "19.00",
  "product_id": 1234,
  "stock_management": false,
  "stock": null,
  "sku": null,
  "mpn": null,
  "age_group": null,
  "gender": null,
  "updated_at": "2013-06-01T09:15:11-03:00",
  "weight": null,
  "width": null
}
```

### PUT /products/{product_id}/variants/{id}

Modify an existing Product Variant

#### PUT /products/1234/variants/144

```json
{
  "id": 144,
  "image_id": null,
  "promotional_price": "19.00",
  "created_at": "2013-01-03T09:11:51-03:00",
  "depth": null,
  "height": null,
  "values": [
    {
      "en": "X-Large"
    }
  ],
  "price": "25.00",
  "product_id": 1234,
  "stock_management": true,
  "stock": 5,
  "sku": "BSG1234D",
  "mpn": null,
  "age_group": null,
  "gender": null,
  "updated_at": "2013-06-01T09:15:11-03:00",
  "weight": "2.75",
  "width": null
}
```

`HTTP/1.1 200 OK`

```json
{
  "id": 144,
  "image_id": null,
  "promotional_price": "19.00",
  "created_at": "2013-01-03T09:11:51-03:00",
  "depth": null,
  "height": null,
  "values": [
    {
      "en": "X-Large"
    }
  ],
  "price": "25.00",
  "product_id": 1234,
  "stock_management": true,
  "stock": 5,
  "sku": "BSG1234D",
  "mpn": null,
  "age_group": null,
  "gender": null,
  "updated_at": "2013-06-01T12:15:11-03:00",
  "weight": "2.75",
  "width": null
}
```

### PUT /products/{product_id}/variants

Updates the entire `ProductVariant` collection owned by a specific `Product`. Use this endpoint to add, modify or remove `ProductVariant`s in one single batch operation.

If the operation is successful, all the variants sent in the request will be the current and only variants for the `Product`.

Each `ProductVariant` will be identified by its value combination. If a specified value combination doesn't exist, a new `ProductVariant` will be created, otherwise, the `ProductVariant` matching that value combination will be updated. Value combinations that aren't present in the request body, identify `ProductVariant`s that will be deleted.

#### PUT /products/1234/variants

```json
[
  {
    "values": [
      {
        "es": "Large"
      }
    ],
    "stock": 4,
    "price": 10.5
  },
  {
    "values": [
      {
        "es": "Medium"
      }
    ]
  }
]
```

`HTTP/1.1 200 OK`

Indicates that the entire collection has been processed successfully. Returns the current `ProductVariant`s collection of the `Product`.

```json
[
  {
    "id": 33739371,
    "image_id": null,
    "product_id": 17310851,
    "position": 27,
    "price": "10.50",
    "promotional_price": null,
    "stock_management": true,
    "stock": 4,
    "weight": "0.000",
    "width": "0.00",
    "height": "0.00",
    "depth": "0.00",
    "sku": null,
    "values": [
      {
        "es": "Large"
      }
    ],
    "barcode": null,
    "mpn": null,
    "age_group": null,
    "gender": null,
    "created_at": "2021-11-10T20:40:44+0000",
    "updated_at": "2021-11-10T20:40:44+0000"
  },
  {
    "id": 33739372,
    "image_id": null,
    "product_id": 17310851,
    "position": 28,
    "price": null,
    "promotional_price": null,
    "stock_management": false,
    "stock": null,
    "weight": "0.000",
    "width": "0.00",
    "height": "0.00",
    "depth": "0.00",
    "sku": null,
    "mpn": null,
    "age_group": null,
    "gender": null,
    "values": [
      {
        "es": "Medium"
      }
    ],
    "barcode": null,
    "mpn": null,
    "age_group": null,
    "gender": null,
    "created_at": "2021-11-10T20:40:44+0000",
    "updated_at": "2021-11-10T20:40:44+0000"
  }
]
```

`HTTP/1.1 400 Bad Request`

```json
{
  "code": 400,
  "message": "Bad Request",
  "description": "Invalid input format"
}
```

```json
{
  "code": 400,
  "message": "Bad Request",
  "description": "Variant values should not be empty"
}
```

```json
{
  "code": 400,
  "message": "Bad Request",
  "description": "There must be at least one variant"
}
```

```json
{
  "code": 400,
  "message": "Bad Request",
  "description": "Invalid values format"
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
  "code": 422,
  "message": "Unprocessable Entity",
  "description": "Validation error",
  "price": ["The price must be at least 0."],
  "stock": ["The stock must be an integer."]
}
```

```json
{
  "code": 422,
  "message": "Unprocessable Entity",
  "description": "Variant values should not be repeated"
}
```

```json
{
  "code": 422,
  "message": "Unprocessable Entity",
  "description": "Product is not allowed to have more than 1000 variants."
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
  "code": 422,
  "message": "Unprocessable Entity",
  "description": "The selected age group is invalid"
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
  "code": 422,
  "message": "Unprocessable Entity",
  "description": "The selected gender is invalid"
}
```

`HTTP/1.1 500 Internal Server Error`

```json
{
  "code": 500,
  "message": "Internal Server Error",
  "description": null
}
```

### PATCH /products/{product_id}/variants

Partially update a `ProductVariant` collection. This endpoint allows to modify many existing `ProductVariant`s that belong to the given `Product`.

This endpoint _will not_ add new `ProductVariant`s or remove existing `ProductVariant`s; it will just update their values.

#### Preconditions

Request body is an array of `ProductVariant` resources.

Each `ProductVariant` in the request body must:

- include the `ProductVariant` ID, set in the field `id`
- correspond to an existing `ProductVariant` with same ID than the one set in the field `id`
- belong to the `Product` which ID is set in `{product_id}` in the URL
- have a unique combination of `values` among all `ProductVariant`s of the `Product` (either `ProductVariant`s included in the request or previously persisted).

If any of the above preconditions is not met, the response:

- has HTTP status code `422`
- contains a JSON with error information including the IDs of the failed `ProductVariant`s:

```json
{
  "code": 422,
  "message": "Unprocessable Entity",
  "description": "Variants cannot be repeated",
  "duplicate_variant_ids": [33745216, 33745217, 33745218]
}
```

#### HTTP status code

`200`: All `ProductVariant`s in the request have been updated. Complete updated `ProductVariant` collection is returned.
`404`: `Product` with ID `{product_id}` was not found.
`422`: Changes are not processable because some of the above preconditions has not been met.

#### PATCH /products/1234/variants

```json
[
  {
    "id": 143,
    "values": [
      {
        "en": "Large"
      }
    ],
    "price": "19.00"
  },
  {
    "id": 144,
    "values": [
      {
        "en": "X-Large"
      }
    ],
    "price": "21.00"
  }
]
```

`HTTP/1.1 200 OK`

```json
[
  {
    "id": 143,
    "image_id": null,
    "promotional_price": null,
    "created_at": "2013-01-03T09:11:51-03:00",
    "depth": null,
    "height": null,
    "values": [
      {
        "en": "Large"
      }
    ],
    "price": "19.00",
    "product_id": 1234,
    "stock_management": false,
    "stock": null,
    "sku": null,
    "mpn": null,
    "age_group": null,
    "gender": null,
    "updated_at": "2013-06-01T09:15:11-03:00",
    "weight": null,
    "width": null
  },
  {
    "id": 144,
    "image_id": null,
    "promotional_price": null,
    "created_at": "2013-01-03T09:11:51-03:00",
    "depth": null,
    "height": null,
    "values": [
      {
        "en": "X-Large"
      }
    ],
    "price": "21.00",
    "product_id": 1234,
    "stock_management": false,
    "stock": null,
    "sku": null,
    "mpn": null,
    "age_group": null,
    "gender": null,
    "updated_at": "2013-06-01T09:15:11-03:00",
    "weight": null,
    "width": null
  }
]
```

### DELETE /products/{product_id}/variants/{id}

Remove a Product Variant

#### DELETE /products/1234/variants/112

`HTTP/1.1 200 OK`

```json
{}
```
