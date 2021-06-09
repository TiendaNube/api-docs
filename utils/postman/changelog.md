
# Change Log
All notable changes to the collection will be documented in this file.
 
## [1.3.2] - 2021-06-08
  
Removed the Discounts API collection until the update of the next version of the specification.

### Changed
- Discounts API methods deleted from the collection.
  - **POST Create a Promotion**
  - **PUT Create a Discount**
  - **DELETE Remove a discount associated with a cart**
  - **DELETE Remove a discount associated with a cart from a specific line item**

- Discounts API variables removed from the collection.
  - `promotion_id`
  - `line_item`

### Fixed
- Folder Shipping Fulfillment Event
  - **GET Receive a list of all fulfillment events for an order:** 
    - Headers `Authentication` and `User-Agent` included.
  - **GET Receive a single fulfillment event:** 
    - Headers `Authentication` and `User-Agent` included.
  - **POST Create a new fulfillment event:** 
    - Headers `Authentication` and `User-Agent` included.
  - **DELETE Remove a fulfillment event:** 
    - Headers `Authentication` and `User-Agent` included.

- Other missing parameters used in the collection were included.
  - `transaction_id`
  - `fullfillment_id`
  - `script_id`
  - `metafield_id`
  - `customer_id`
  - `coupon_id`
  - `category_id`
  - `sku_id`

## [1.3.1] - 2021-05-24
  
Fix PUT and DELETE methods on Discounts API.

### Changed
- Updated the format of the paragraphs of Changelog.md


### Fixed
- Folder Discounts
  - **PUT Create a Discount:** 
    - `cart_id` and `line_items` replaced from integer to string on body request example.
  - **DELETE Remove a discount associated with a cart from a specific line item:** 
    - path parameter updated with `/line-items/{{line_item}}`
 
## [1.3.0] - 2021-05-19
  
Abandoned Checkout API methods included according with documentation available at https://github.com/TiendaNube/api-docs/blob/master/resources/abandoned_checkout.md

### Added
- Folder Abandoned Checkout
  - POST Create a discount coupon to the abandoned cart
  - GET Receive all abandoned carts
  - GET Receive a specific abandoned cart

### Changed

### Fixed
 
## [1.2.0] - 2021-05-13
  
Discounts API methods included according with documentation available at http://tiendanube.github.io/api-docs/resources/discounts/api/

### Added
- Folder Discounts
  - POST Create a Promotion
  - PUT Create a Discount
  - DELETE Remove a discount associated with a cart
  - DELETE Remove a discount associated with a cart from a specific line item

### Changed

### Fixed
 
## [1.1.0] - 2021-03-19
Inclusion of all methods available in the docs
 
### Added
   
### Changed
- Folder Customer
- Folder Draft Order
- Folder Metafield
- Folder Order
- Folder Order
- Folder Payment Provider
- Folder Product
- Folder Product Image
- Folder Product Variant
- Folder Script
- Folder Shipping Carrier
- Folder Shipping Carrier Option
- Folder Shipping Fulfillment Event
- Folder Store
- Folder Transaction
- Folder Webhook
- Folder Authorization
- Folder Category
- Folder Coupon

### Fixed

## [1.0.0] - 2021-03-09
Creation of the collection.
 
### Added
- postman.json
- README.md

### Changed
 
### Fixed
