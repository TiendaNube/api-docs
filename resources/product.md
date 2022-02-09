Product
======

A Product is an item for sale in a Tiendanube/Nuvemshop's store. It can be either a good or a service.

Properties
----------

| Property       | Explanation                                                                                       |
| -------------- | ------------------------------------------------------------------------------------------------- |
| id             | The unique numeric identifier for the Product                                                     |
| name           | List of the names of the Product, in every language supported by the store                        |
| description    | List of the descriptions of the Product, as HTML, in every language supported by the store        |
| handle         | List of the url-friendly strings generated from the Product's names, in every language supported by the store |
| variants       | List of [Product Variant](https://github.com/tiendanube/api-docs/blob/master/resources/product_variant.md) objects representing the different version of the Product |
| images         | List of [Product Image](https://github.com/tiendanube/api-docs/blob/master/resources/product_image.md) objects representing the Product's images                 |
| categories     | List of [Category](https://github.com/tiendanube/api-docs/blob/master/resources/category.md) objects representing the Product's categories             |
| brand          | The Product's brand                                                                               |
| published      | *true* if the Product is published in the store. *false* otherwise                                |
| free_shipping  | *true* if the Product is elegible for free shipping. *false* otherwise                            |
| video_url      | String with a valid URL format. Only admits https links                                           | 
| seo_title      | The SEO friendly title for the Product. Up to 70 characters                                       |
| seo_description| The SEO friendly description for the Product. Up to 320 characters                                |
| attributes     | List of the names of the attributes whose values define the variants. E.g.: Color, Size, etc. It is important that the number of `attributes` is equal to the number of `values` within the variants.      |
| tags           | String with all the Product's tags, separated by commas                                           |
| created_at     | Date when the Product was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)     | 
| updated_at     | Date when the Product was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)|
| requires_shipping | *true* if the Product is physical. *false* if it is digital |

Endpoints
---------

### GET /products

Receive a list of all Products.


| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| since_id       | Restrict results to after the specified ID                                                       |
| language       | Specify search language                                                                          |
| q              | Search Products containing the given text in their names, descriptions or SKU                    |
| handle         | Show Products with a given URL                                                                   |
| category_id    | Show Products with a given category                                                              |
| published      | Show Products by published status. Valid values are "true" or "false"                            |
| free_shipping  | Show Products by free_shipping status. Valid values are "true" or "false"                        |
| max_stock | Show Products with less or equal stock than the specified value                                       |
| min_stock | Show Products with more or equal stock than the specified value                                       |
| has_promotional_price | Show Products that have a defined promotional price. Valid values are *true* or *false*   |
| has_weight | Show Products that have a defined weight. Valid values are *true* or *false*                         |
| has_all_dimensions | Show Products that have a defined depth, width and height. Valid values are *true* or *false*|
| has_weight_and_all_dimensions | Show Products that have a defined weight, depth, width and height. Valid values are *true* or *false* |
| created_at_min | Show Products created after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))      |
| created_at_max | Show Products created before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))     |
| updated_at_min | Show Products last updated after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)) |
| updated_at_max | Show Products last updated before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))|
| sort_by        | Sort Products by a particular criteria (I.E.: sort_by=created-at-ascending)
| page           | Page to show                                                                                     |
| per_page       | Amount of results                                                                                |
| fields         | Comma-separated list of fields to include in the response                                        |


| Sort Criteria                     | Explanation                                                                      |
| ----------------------------------| -------------------------------------------------------------------------------- |
| user                              | User manually defined sort                                                       |
| price-ascending, cost-ascending   | Sort by price ascending                                                          |
| price-descending, cost-descending | Sort by price descending                                                         |
| alpha-ascending, name-ascending   | Sort by Product name ascending                                                   |
| alpha-descending, name-descending | Sort by Product name descending                                                  |
| created-at-ascending              | Sort by created date ascending                                                   |
| created-at-descending             | Sort by created date descending                                                  |
| best-selling                      | Sort by number of sold products descending                                       |


#### GET /products

`HTTP/1.1 200 OK`

```json
[
    {
      "attributes": [
        {
          "en": "Size"
        }
      ],
      "categories": [
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
            "google_shopping_category": null,
              "updated_at": "2013-03-11T09:14:11-03:00"
        }
      ],
      "created_at": "2013-01-03T09:11:51-03:00",
      "description": {
          "en": "<p>The best Ball with the ultimate level of performance. It will catch any wild Pokémon without fail.</p>",
          "es": "<p>La mejor Bola con el nivel máximo de desempeño. Atrapará cualquier Pokémon sin fallar.</p>",
          "pt": "<p>A melhor Bola com o nível máximo de desempenho. Ele vai pegar qualquer Pokémon selvagem sem falhar.</p>"
      },
      "handle": {
          "en": "master-ball",
          "es": "master-ball",
          "pt": "master-ball"
      },
      "id": 1234,
      "images": [
        {
          "id": 101,
          "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/servine-640-0.jpg",
          "position": 1,
          "product_id": 1234
        },
        {
          "id": 112,
          "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/onyx-640-0.jpg",
          "position": 2,
          "product_id": 1234
        },
        {
          "id": 123,
          "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/stoutland-640-0.jpg",
          "position": 3,
          "product_id": 1234
        }
      ],
      "name": {
          "en": "Master Ball",
          "es": "Master Ball",
          "pt": "Master Ball"
      },
      "brand": null,
      "video_url": "https://www.youtube.com/watch?v=57aG16_gQcU",
      "seo_title": "Master Ball",
      "seo_description": "The best Ball with the ultimate level of performance. It will catch any wild Pokémon without fail.",
      "published": true,
      "free_shipping": false,
      "updated_at": "2013-03-11T09:14:11-03:00",
      "variants": [
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
             "en": "Large"
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
    }
]
```
#### GET /products?created_at_min=2013-01-01T00:00:00-03:00&fields=id,name

`HTTP/1.1 200 OK`

```json
[
    {
      "id": 1234,
      "name": {
          "en": "Master Ball",
          "es": "Master Ball",
          "pt": "Master Ball"
      }
    }
]
```

### GET /products/{id}

Receive a single Product

| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| fields         | Comma-separated list of fields to include in the response                                        |

#### GET /products/1234

`HTTP/1.1 200 OK`

```json
{
  "attributes": [
      {
          "en": "Size"
      }
  ],
  "categories": [
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
        "google_shopping_category": null,
          "updated_at": "2013-03-11T09:14:11-03:00"
    }
  ],
  "created_at": "2013-01-03T09:11:51-03:00",
  "description": {
      "en": "<p>The best Ball with the ultimate level of performance. It will catch any wild Pokémon without fail.</p>",
      "es": "<p>La mejor Bola con el nivel máximo de desempeño. Atrapará cualquier Pokémon sin fallar.</p>",
      "pt": "<p>A melhor Bola com o nível máximo de desempenho. Ele vai pegar qualquer Pokémon selvagem sem falhar.</p>"
  },
  "handle": {
      "en": "master-ball",
      "es": "master-ball",
      "pt": "master-ball"
  },
  "id": 1234,
  "images": [
    {
      "id": 101,
      "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/servine-640-0.jpg",
      "position": 1,
      "product_id": 1234
    },
    {
      "id": 112,
      "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/onyx-640-0.jpg",
      "position": 2,
      "product_id": 1234
    },
    {
      "id": 123,
      "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/stoutland-640-0.jpg",
      "position": 3,
      "product_id": 1234
    }
  ],
  "name": {
      "en": "Master Ball",
      "es": "Master Ball",
      "pt": "Master Ball"
  },
  "brand": null,
  "video_url": "https://www.youtube.com/watch?v=57aG16_gQcU",
  "seo_title": "Master Ball",
  "seo_description": "The best Ball with the ultimate level of performance. It will catch any wild Pokémon without fail.",
  "published": true,
  "free_shipping": false,
  "updated_at": "2013-03-11T09:14:11-03:00",
  "variants": [
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
}
```
### GET /products/sku/{sku}

Returns the first Product found where one of its variants has the given SKU.

#### GET /products/sku/1234abcd

### POST /products

Creates a new Product

#### POST /products

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

`HTTP/1.1 422 Unprocessable Entity`

```json
{
    "code": 422,
    "message": "Unprocessable Entity",
    "description": "Store has reached maximum limit of 100000 allowed products"
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
    "code": 422,
    "message": "Unprocessable Entity",
    "description": "Product is not allowed to have more than 250 images"
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

```json
{
    "code": 422,
    "message": "Unprocessable Entity",
    "description": "The video url field is not a secure url"
}
```

#### POST /products

```json
{
    "images": [
      {
        "src": "http://images1.wikia.nocookie.net/__cb20101106022321/pokemon/images/f/f1/UltraBallArt.png"
      }
    ],
    "name": {
      "en": "Ultra Ball",
      "es": "Ultra Ball",
      "pt": "Ultra Ball"
    },
    "video_url": "https://www.youtube.com/watch?v=57aG16_gQcU",
    "variants": [
        {
            "price": "10.00",
            "stock_management": true,
            "stock": 12,
            "weight": "2.00"
        }
    ]
}
```

`HTTP/1.1 201 Created`

```json
{
    "attributes": [],
    "categories": [
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
          "google_shopping_category": null,
            "updated_at": "2013-03-11T09:14:11-03:00"
      }
    ],
    "created_at": "2013-06-01T12:15:11-03:00",
    "description": {
        "en": "",
        "es": "",
        "pt": ""
    },
    "handle": {
        "en": "ultra-ball",
        "es": "ultra-ball",
        "pt": "ultra-ball"
    },
    "id": 2435,
    "images": [
      {
        "id": 231,
        "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/UltraBallArt.jpg",
        "position": 1,
        "product_id": 2435
      }
    ],
    "name": {
        "en": "Ultra Ball",
        "es": "Ultra Ball",
        "pt": "Ultra Ball"
    },
    "brand": null,
    "video_url": "https://www.youtube.com/watch?v=57aG16_gQcU",
    "seo_title": "Ultra Ball",
    "seo_description": "",
    "published": true,
    "free_shipping": false,
    "created_at": "2013-06-01T12:15:11-03:00",
    "variants": [
        {
         "id": 101,
         "promotional_price": null,
         "created_at": "2013-06-01T12:15:11-03:00",
         "depth": null,
         "height": null,
         "values": [],
         "price": "10.00",
         "product_id": 1234,
         "stock_management": true,
         "stock": 12,
         "sku": null,
         "updated_at": "2013-03-11T09:14:11-03:00",
         "weight": "2.00",
         "width": null      
       }
    ]
}
```

### PUT /products/{id}

Modify an existing Product

#### PUT /products/5123

```json
{
  "categories": [4567],
  "id": 1234,
  "published": false
}
```

`HTTP/1.1 200 OK`

```json
{
    "attributes": [
        {
            "en": "Size"
        }
    ],
    "categories": [
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
          "google_shopping_category": null,
            "updated_at": "2013-03-11T09:14:11-03:00"
      }
    ],
    "created_at": "2013-01-03T09:11:51-03:00",
    "description": {
        "en": "<p>The best Ball with the ultimate level of performance. It will catch any wild Pokémon without fail.</p>",
        "es": "<p>La mejor Bola con el nivel máximo de desempeño. Atrapará cualquier Pokémon sin fallar.</p>",
        "pt": "<p>A melhor Bola com o nível máximo de desempenho. Ele vai pegar qualquer Pokémon selvagem sem falhar.</p>"
    },
    "handle": {
        "en": "master-ball",
        "es": "master-ball",
        "pt": "master-ball"
    },
    "id": 1234,
    "images": [
      {
        "id": 101,
        "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/servine-640-0.jpg",
        "position": 1,
        "product_id": 1234
      },
      {
        "id": 112,
        "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/onyx-640-0.jpg",
        "position": 2,
        "product_id": 1234
      },
      {
        "id": 123,
        "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/stoutland-640-0.jpg",
        "position": 3,
        "product_id": 1234
      }
    ],
    "name": {
        "en": "Master Ball",
        "es": "Master Ball",
        "pt": "Master Ball"
    },
    "brand": null,
    "video_url": null,
    "seo_title": "Master Ball",
    "seo_description": "The best Ball with the ultimate level of performance. It will catch any wild Pokémon without fail.",
    "published": false,
    "free_shipping": false,
    "updated_at": "2013-06-01T12:15:11-03:00",
    "variants": [
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
               "en": "Large"
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
}
```

### DELETE /products/{id}

Remove a Product

#### DELETE /products/1234

`HTTP/1.1 200 OK`

```json
{}
```
