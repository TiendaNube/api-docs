---
layout: default
---

# Data Objects

## Cart Object

| Property | Type |
| --- | --- |
| id | Integer |
| coupons | Boolean |
| products | Array\<[Product]\> |
| shipping | [Shipping] |
| payment | [Payment] |
| package | [Package] |
| currency | String |
| language | String |
| discounts | Array\<[Discount]\> |
| totalDiscountAmount | String |

## Product Object

| Property | Type |
| --- | --- |
| barcode | String |
| depth | String |
| free_shipping | Boolean |
| height | String |
| id | Integer |
| name | String |
| price | String |
| price\_long | String |
| price\_short | String |
| product\_id | Integer |
| properties | Array |
| quantity | Integer |
| sku | String |
| subtotal\_long | String |
| subtotal\_short | String |
| url | String |
| variant\_id | Integer |
| variant\_values | Array |
| weight | String |
| width | String |
| categories | Array\<[Category]\> |

## Category Object

| Property | Type |
| --- | --- |
| id | Integer |
| name | [I18n] |
| description | [I18n] |
| handle | [I18n] |
| parent | Integer |
| subcategories | Array\<Integer\> |
| seo_title | [I18n] |
| seo_description | [I18n] |
| created_at | String |
| updated_at | String |

## I18n Object

| Property | Type |
| --- | --- |
| Language code (Ex: es) | String |

## Shipping Object

| Property | Type |
| --- | --- |
| city | String |
| cost | String |
| country | String |
| postalcode | String |

## Package Object

| Property | Type |
| --- | --- |
| weight | String |

## Payment Object

| Property | Type |
| --- | --- |
| creditCardCompany | String |
| method | String |
| installments | Integer |

## Discount Object

| Property | Type |
| --- | --- |
| script_type | String |
| scope_type | String |
| scope_value_id | Integer |
| total_amount | Integer |
| original_price | Integer |
| final_price | Integer |
| begin_date | String |
| end_date | String | 
| quantity | Integer |

[Product]: #product-object
[Category]: #category-object
[I18n]: #i18n-object
[Shipping]: #shipping-object
[Package]: #package-object
[Payment]: #payment-object
[Discount]: #discount-object

# Example

```json
{
    "id": 397256730,
    "currency": "ARS",
    "coupons": false,
    "language": "es",
    "shipping": {
        "country": null,
        "city": null,
        "postalcode": null
    },
    "package": {
        "weight": "0.600"
    },
    "payment": {
        "creditCardCompany": null,
        "method": null,
        "installments": 1
    },
    "products": [
        {
            "id": 467422732,
            "depth": "20.00",
            "height": "5.00",
            "name": "Mi first product",
            "price": "12.00",
            "product_id": 17310718,
            "image": {
                "id": 0,
                "product_id": 0,
                "src": "https://static.tiendanube.com/stores_files/img/no-photo-1024-1024.png",
                "position": 0,
                "alt": [],
                "created_at": "2021-06-30T20:47:29+0000",
                "updated_at": "2021-06-30T20:47:29+0000"
            },
            "quantity": 4,
            "free_shipping": false,
            "weight": "0.00",
            "width": "30.00",
            "variant_id": 33739098,
            "variant_values": [],
            "properties": [],
            "sku": null,
            "barcode": null,
            "url": "http://www.mystore.com/productos/mi-primero-producto/",
            "price_short": "$12",
            "price_long": "$12 ARS",
            "subtotal_short": "$48",
            "subtotal_long": "$48 ARS",
            "categories": [
                {
                    "id": 9090471,
                    "name": {
                        "es": "Category with subcategory"
                    },
                    "description": {
                        "es": ""
                    },
                    "handle": {
                        "es": "category-with-subcategory"
                    },
                    "parent": null,
                    "subcategories": [
                        9090472
                    ],
                    "seo_title": {
                        "es": ""
                    },
                    "seo_description": {
                        "es": ""
                    },
                    "created_at": "2021-07-20T20:07:12+0000",
                    "updated_at": "2021-07-20T20:09:12+0000"
                },
                {
                    "id": 9090472,
                    "name": {
                        "es": "Subcategory"
                    },
                    "description": {
                        "es": ""
                    },
                    "handle": {
                        "es": "subcategory"
                    },
                    "parent": 9090471,
                    "subcategories": [
                        9090473
                    ],
                    "seo_title": {
                        "es": ""
                    },
                    "seo_description": {
                        "es": ""
                    },
                    "created_at": "2021-07-20T20:07:28+0000",
                    "updated_at": "2021-07-20T20:09:12+0000"
                }
            ]
        },
    ],
    "discounts": [
        {
            "script_type": "2x1",
            "scope_type": "products",
            "scope_value_id": 17310718,
            "total_amount": 24,
            "original_price": 12,
            "final_price": 7.2,
            "begin_date": null,
            "end_date": null,
            "quantity": 5
        }
    ],
    "totalDiscountAmount": "24.00"
}
```