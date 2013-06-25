Scopes
------

Apps should only ask access scopes they need. If an app only needs to read products, it should only ask for the `read_products` scope.

__If you ask for any write scope, the read scope is implied.__

As [Webhooks](https://github.com/TiendaNube/api-docs/blob/master/resources/webhook.md) rely on other resources, you will only be able to register webhooks for the resources you were granted permission to use.

The available scopes for the API are:

* read_content / write_content
    - Page
* read_products / write_products
    - Product
    - Product Variant
    - Product Image
    - Category
* read_customers / write_customers
    - Customer
* read_orders / write_orders
    - Order
* read_scripts / write_scripts
    - Script



