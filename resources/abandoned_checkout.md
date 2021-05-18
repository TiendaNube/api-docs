
Abandoned Checkout
=====
Insert here a description.

Describe business information here.


#### Table of Contents


>  [Create a discount coupon](#POST-/checkouts/{cart_id}/coupons)
  

Notes
----------
> ***Note:*** The application must have `read_orders` or `write_orders` permission (depends on whether they just want to do GET or POST).

> ***Note:*** A cart can be created up to `6 hours` after the purchase has been abandoned.

> ***Note:*** The carts are accessible for `30 days` after being created.

> ***Note:*** A purchase is only converted into a cart if the customer has arrived for the second checkout step.

Endpoints
---------

### POST /checkouts/{cart_id}/coupons

  Add a discount coupon to the cart.
  
| Parameter   | Description                                                       | Type    | Required |
|-------------|-------------------------------------------------------------------|---------|----------|
| `coupon_id` | ID of the [Coupon](https://github.com/TiendaNube/api-docs/blob/master/resources/coupon.md) that applies to this abandoned cart. | Integer | Yes      |



```json
{
    "coupon_id": 1029384756
}
```

`HTTP/1.1 200 OK`

```json
{
    "id": 987654321,
    "token": "b5d911460455d44411c7f259da4309b3d099291e",
    "store_id": 1234567,
    "abandoned_checkout_url": "https://abandonedcheckouturl/checkout/v3/order/proxy/987654321/b5d911460455d44411c7f259da4309b3d099291e",
    "contact_email": "hetoca2855@threepp.com",
    "contact_name": "3245U67Jyhrtegf 23fewsdf",
    "contact_phone": "",
    "contact_identification": "83092433084",
    "shipping_name": "3245U67Jyhrtegf",
    "shipping_phone": "",
    "shipping_address": null,
    "shipping_number": null,
    "shipping_floor": null,
    "shipping_locality": null,
    "shipping_zipcode": "75474567",
    "shipping_city": "City",
    "shipping_province": "State",
    "shipping_country": "BR",
    "shipping_min_days": -5,
    "shipping_max_days": -4,
    "billing_name": "3245U67Jyhrtegf",
    "billing_phone": "",
    "billing_address": "zewrtryewr",
    "billing_number": "654",
    "billing_floor": "",
    "billing_locality": "aersrdgfd",
    "billing_zipcode": "75474567",
    "billing_city": "City",
    "billing_province": "State",
    "billing_country": "BR",
    "shipping_cost_owner": "38.15",
    "shipping_cost_customer": "38.15",
    "coupon": [],
    "promotional_discount": {
        "id": null,
        "store_id": 1234567,
        "order_id": 987654321,
        "created_at": "2021-05-18T16:28:17+0000",
        "total_discount_amount": "0.00",
        "contents": [],
        "promotions_applied": []
    },
    "subtotal": "1999.99",
    "discount": "0.00",
    "discount_coupon": "0.00",
    "discount_gateway": "0.00",
    "total": "2038.14",
    "total_usd": "766.71",
    "checkout_enabled": true,
    "weight": "13.000",
    "currency": "BRL",
    "language": "pt",
    "gateway": null,
    "gateway_id": null,
    "shipping": "api_129734",
    "shipping_option": "Express Delivery",
    "shipping_option_code": "xp-dlv-1",
    "shipping_option_reference": "ref123 xp dlv",
    "shipping_pickup_details": null,
    "shipping_tracking_number": null,
    "shipping_tracking_url": null,
    "shipping_store_branch_name": null,
    "shipping_pickup_type": "ship",
    "shipping_suboption": [],
    "extra": {},
    "storefront": "store",
    "note": "",
    "created_at": "2021-05-17T14:24:54+0000",
    "updated_at": "2021-05-18T16:28:17+0000",
    "completed_at": null,
    "next_action": "noop",
    "payment_details": {
        "method": "redirect",
        "credit_card_company": null,
        "installments": 1
    },
    "attributes": [],
    "customer": null,
    "products": [
        {
            "id": 192837465,
            "depth": "0.00",
            "height": "0.00",
            "name": "Product Abc123",
            "price": "1999.99",
            "product_id": 564738291,
            "image": {
                "id": 291837465,
                "product_id": 564738291,
                "src": "https://d2r9epyceweg5n.cloudfront.net/stores/001/679/811/products/bc123-757914__480-3bf9322c22a0206aee16202452236251-1024-1024.png",
                "position": 2,
                "alt": [],
                "created_at": "2021-05-05T20:07:08+0000",
                "updated_at": "2021-05-10T14:00:17+0000"
            },
            "quantity": 1,
            "free_shipping": false,
            "weight": "13.00",
            "width": "0.00",
            "variant_id": 344345678,
            "variant_values": [
                "Azul",
                "17"
            ],
            "properties": [],
            "sku": null,
            "barcode": null
        }
    ]
}
```

## HTTP Errors List - POST Examples

`HTTP/1.1 401 Unauthorized`

```json
{
    "code": 401,
    "message": "Unauthorized",
    "description": "Invalid access token"
}
```

`HTTP/1.1 404 Not Found`

```json
{
    "code": 404,
    "message": "Not Found",
    "description": null
}
```

`HTTP/1.1 415 Unsupported Media Type`

```json
{
    "code": 415,
    "message": "Unsupported Media Type",
    "description": "Content-Type should be application/json"
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
    "code": 422,
    "message": "Unprocessable Entity",
    "description": "Validation error",
    "coupon_id": [
        "The coupon id field is required."
    ]
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
    "code": 422,
    "message": "Unprocessable Entity",
    "description": "The checkout already has an assigned coupon."
}
```