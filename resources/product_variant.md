Product Variant
===============

Product variants allow you to group a shoe with different sizes and colors in the same product. 

Properties
----------

| Property          | Explanation                                                                                     |
| ----------------- | ----------------------------------------------------------------------------------------------- |
| id                | The unique numeric identifier for the Product Variant                                           |
| product_id        | The id of the product associated with the variant                                               |
| price             | Price of the Product Variant. *null* indicates the product will initiate a contact instead of a checkout process |
| promotional_price | Lower price to display as a sale. The value of price will be displayed crossed out for comparison                |
| stock_management  | Specifies whether or not Tienda Nube/Nuvem Shop tracks the number of items in stock for this product variant. Valid values are *true* if Tienda Nube/Nuvem Shop tracks the stock, *false* if it doesn't.|
| stock             | Stock of the Product Variant. If `stock_management` is false the stock will be null             |
| weight            | Weight of the Product Variant in kilograms                                                      |
| width             | Width of the Product Variant in centimetres                                                     |
| height            | Height of the Product Variant in centimetres                                                    |
| depth             | Depth of the Product Variant in centimetres                                                     |
| sku               | Unique identifier of the Product Variant in your store                                          |
| values            | List of the values of the attributes whose values define the variant. E.g.: Large, Medium, etc. |
| created_at        | Date when the Product Variant was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)     | 
| updated_at        | Date when the Product Variant was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)|

Endpoints
---------

### GET /products/#{product_id}/variants

Receive a list of all Product Variants for a given product.

| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| since_id       | Restrict results to after the specified ID                                                       |
| created_at_min | Show Product Variants created after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))       |
| created_at_max | Show Product Variants created before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))      |
| updated_at_min | Show Product Variants last updated after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))  |
| updated_at_max | Show Product Variants last updated before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)) |
| page           | Page to show                                                                                     |
| per_page       | Amount of results                                                                                |
| fields         | Comma-separated list of fields to include in the response                                        |

#### GET /products/1234/variants

`HTTP/1.1 200 OK`

```json
[
    {
      "id": 101,
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
      "updated_at": "2013-03-11T09:14:11-03:00",
      "weight": "2.00",
      "width": null      
    },
    {
      "id": 112,
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
      "updated_at": "2013-03-11T09:14:11-03:00",
      "weight": "2.25",
      "width": null      
    },
    {
      "id": 133,
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
      "updated_at": "2013-03-11T09:14:11-03:00",
      "weight": "2.25",
      "width": null      
    },
    {
      "id": 133,
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
      "updated_at": "2013-03-11T09:14:11-03:00",
      "weight": "2.50",
      "width": null
    }
]
```

### GET /products/#{product_id}/variants/#{id}

Receive a single Product Variant

| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| fields         | Comma-separated list of fields to include in the response                                        |

#### GET /products/1234/variants/112

`HTTP/1.1 200 OK`

```json
{
      "id": 112,
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
      "updated_at": "2013-03-11T09:14:11-03:00",
      "weight": "2.25",
      "width": null      
}
```

### POST /products/#{product_id}/variants

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

`HTTP/1.1 201 Created`

```json
{
      "id": 144,
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
      "updated_at": "2013-06-01T09:15:11-03:00",
      "weight": null,
      "width": null      
}
```

### PUT /products/#{product_id}/variants/#{id}

Modify an existing Product Variant

#### PUT /products/1234/variants/144

```json
{
      "id": 144,
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
      "updated_at": "2013-06-01T09:15:11-03:00",
      "weight": "2.75",
      "width": null
}
```

`HTTP/1.1 200 OK`

```json
{
      "id": 144,
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
      "updated_at": "2013-06-01T12:15:11-03:00",
      "weight": "2.75",
      "width": null      
}
```

### DELETE /products/#{product_id}/variants/#{id}

Remove a Product Variant

#### DELETE /products/1234/variants/112

`HTTP/1.1 200 OK`

```json
{}
```
