---
layout: default
---

# Data Objects

## Cart Object

| Property | Type |
| --- | --- |
| id | Integer |
| coupons | Boolean |
| products | Array  <Product> |
| shipping | Object |
| payment | Object |
| package | Object |
| currency | String |
| language | String |

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
            "subtotal_long": "$48 ARS"
        }
    ]
}
```