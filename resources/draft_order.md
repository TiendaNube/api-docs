Draft Order
=====

A draft order is created when the merchant manages a pre-sale made outside the platform. Draft orders also can be created through the API.

#### Table of Contents
[Create a draft order](#POST-draft-orders)

Properties
----------

| Property                   | Explanation                                                                                              |
| -------------------------- | -------------------------------------------------------------------------------------------------------- |
| id                         | The unique numeric identifier for the Draft Order.                                                       |
| store_id                   | The unique numeric identifier for the Store.                                                             |
| storefront                 | Name that refers to the means by which the draft order was managed.                                      |
| hash                       | Specifies the location of the Draft Order.                                                               |
| currency                   | The total spent's currency in [ISO 4217 format](http://en.wikipedia.org/wiki/ISO_4217).                  |
| lang                       | Draft Order's language used by the customer during the checkout process.                                 |
| contact_name               | Customer's full name.                                                                                    |
| contact_email              | Customer contact email.                                                                                  |
| contact_phone              | Customer contact phone.                                                                                  |
| cpf_cnpj                   | Customer identity number.                                                                                |
| owner_note                 | Store owner's note about the draft order.                                                                |
| shipping_method            | Shipping method chosen by the consumer during the checkout process.                                      |
| shipping_option            | The shipping option chosen by the customer during the checkout process.                                  |
| shipping_option_code       | The shipping option code selected by the consumers.                                                      |
| shipping_pickup_type       | "ship" if the order is going to be shipped; "pickup" if it's going to be picked up from a store branch.  |
| shipping_name              | Customer's name who will receive the order.                                                              |
| shipping_last_name         | Customer's lastname who will receive the order.                                                          |
| shipping_phone             | Customer's phone who will receive the order.                                                             |
| shipping_address           | Customer's shipping address where the order will be shipped.                                             |
| shipping_number            | Customer's shipping address number where the order will be shipped.                                      |
| shipping_floor             | Customer's shipping floor where the order will be shipped.                                               |
| shipping_locality          | Customer's shipping locality where the order will be shipped.                                            |
| shipping_city              | Customer's shipping city where the order will be shipped.                                                |
| shipping_zipcode           | Customer's shipping zipcode where the order will be shipped.                                             |
| shipping_province          | Customer's shipping province where the order will be shipped.                                            |
| shipping_country           | Customer's shipping country where the order will be shipped.                                             |
| shipping_extra             | Additional shipping information in JSON format.                                                          |
| shipping_cost              | The shipping cost the customer has to pay to the store owner.                                            |
| shipping_cost_owner        | The shipping cost the store owner has to pay to the shipping company.                                    |
| shipping_additional_days   | Additional days that are added to those of the shipment. It is set as 0.                                 |          
| gateway                    | Payment gateway code. It is set as not provided.                                                         |
| total                      | Total price of the draft order including shipping and discounts.                                         |
| initiated_by               | Unique identifier number that indicates the user who started the Draft Order.                            |
| internal_extra             | Additional draft order information. Includes the sales channel.                                          |
| checkout_url               | Url of the checkout corresponding to the draft order.                                                    |
| started_checkout           | Date the checkout process started in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601).           |
| updated_at                 | Date when the Draft Order was created in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601).       |
| created_at                 | Date when the Draft Order was last updated in [ISO 8601 format](http://es.wikipedia.org/wiki/ISO_8601).  |

Endpoints
---------

### POST /draft-orders

Create a Draft Order.

| Parameter          | Description                                                                                     |  Type    | Required |
|--------------------| ------------------------------------------------------------------------------------------------|----------|----------|
| contact_name       | Customer's name.                                                                                |  string  | Yes      |
| contact_lastname   | Customer's lastname.                                                                            |  string  | Yes      |
| contact_email      | Customer contact email.                                                                         |  E-mail  | Yes      |
| contact_phone      | Customer contact phone.                                                                         |  string  | No       |
| cpf_cnpj           | Customer identity number.                                                                       |  string  | No       |
| payment_status     | Draft Order's payment status. Possible values are "unpaid", "pending_confirmation" and "paid".  |  string  | Yes      |
| sale_channel       | Sales channel name.                                                                             |  string  | No       |
| note               | Store owner's note about the draft order.                                                       |  string  | No       |
| products           | Draft Order's products list ([Product](#Product)).                                              |  array   | Yes      |
| discount           | Discount amount applied to the draft order.                                                     |  string  | No       |
| shipping           | Shipping information ([Shipping](#Shipping)).                                                   |  array   | No       |
                                              
                                              
### Objects


#### Product
| Value              | Description                                                       | Type    | Required |
|--------------------|-------------------------------------------------------------------|---------|----------|
| variant_id         | The product variant ID.                                           | Integer | Yes      |
| quantity           | The product quantity.                                             | Integer | Yes      |


#### Shipping

| Value              | Description                                                      | Type   | Required |
|--------------------|------------------------------------------------------------------|--------|----------|                                     
| cost               | The shipping cost the customer has to pay to the store owner.    | string | No       |
| shipping_address   | Shipping address information ([Shipping Address](#Address)).     | array  | No       |


#### Address

| Value              | Description                                                      | Type   | Required |
|--------------------|------------------------------------------------------------------|--------|----------|                                                   
| address            | The customer's street.                                           | String | No       |
| number             | The address's number.                                            | String | No       |
| floor              | The address's complement.                                        | String | No       |
| locality           | The address's locality.                                          | String | No       | 
| city               | The address's city.                                              | String | No       |
| province           | The address's province.                                          | String | No       |
| zipcode            | The address's postal code.                                       | String | No       |


#### Payment Status

| Value                 | Description                                         |
|-----------------------|-----------------------------------------------------|
| unpaid                | The payment hasn't yet been made                    |
| pending_confirmation  | The payment confirmation is pending                 |
| paid                  | The payment was successfully confirmed and captured |

#### POST /draft-orders

`HTTP/1.1 201 Created`

```json
{
   "id": "39138174",
   "store_id": 1,
   "storefront": "api",
   "hash": "5be919b12255a1823d92869d40c9c3bda4503e37",
   "currency": "ARS",
   "lang": "es_AR",
   "contact_name": "My Example",
   "contact_email": "my.example@gmail.com",
   "contact_phone":"4678-1232",
   "cpf_cnpj": "12345678",
   "owner_note": "My first draft order created",
   "shipping_method": "draft",
   "shipping_option": "Te vamos a contactar para coordinar la entrega!",
   "shipping_option_code": "99999",
   "shipping_pickup_type": "ship",
   "shipping_name": "My",
   "shipping_last_name": "Example",
   "shipping_phone": "4678-1232",
   "shipping_address": "Sta Rosa",
   "shipping_number": "1535",
   "shipping_floor": "",
   "shipping_city": "Moron",
   "shipping_locality": "Castelar",
   "shipping_zipcode": "1712",
   "shipping_province": "Gran Buenos Aires",
   "shipping_country": 10,
   "shipping_extra": {
      "show_time": false,
      "show_price": true
   },
   "shipping_cost": 0,
   "shipping_cost_owner": 0,
   "shipping_additional_days": 0,
   "gateway": "not-provided",
   "total": 712,
   "initiated_by": 372661,
   "internal_extra": {
      "sale_channel": ""
   },
   "checkout_url": "https://mystore.mitiendanube.com/checkout/v3/order/proxy/39138177/945f71f08398e25a7ae6fcf961e9cc10417278c8",
   "started_checkout": "2020-10-01 19:52:04",
   "updated_at": "2020-10-01 19:52:05",
   "created_at": "2020-10-01 19:52:05"
}

```