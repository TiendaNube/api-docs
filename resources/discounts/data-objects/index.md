---
layout: default
---

# Data Objects

## Root Object

| Property | Type |
| --- | --- |
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
