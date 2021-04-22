Customer
========

A Customer of the store. Customer accounts store contact information for the customer, saving logged-in customers the trouble of having to provide it at every checkout.

Properties
----------

| Property             | Explanation                                                                                  |
| -------------------- | -------------------------------------------------------------------------------------------- |
| id                   | The unique numeric identifier for the Customer                                               |
| name                 | Name of the Customer                                                                         |
| email                | E-mail of the Customer                                                                       |
| phone                | Phone number of the customer (not necessarily the same as the address's phone)               |
| identification       | Customer's identification (in Brazil for example, it would be the CPF/CNPJ)                  |
| note                 | Store owner's notes about the customer                                                       |
| default_address      | Default shipping address of the Customer                                                     |
| addresses            | List of shipping addresses for the Customer                                                  |
| billing_address      | Billing address of the Customer                                                              |
| billing_number       | Billing number of the Customer                                                               |
| billing_floor        | Billing floor of the Customer                                                                |
| billing_locality     | Billing locality of the Customer                                                             |
| billing_zipcode      | Billing zipcode of the Customer                                                              |
| billing_city         | Billing city of the Customer                                                                 |
| billing_province     | Billing province of the Customer                                                             |
| billing_country      | Billing country code of the Customer                                                         |
| extra                | A JSON object containing custom information. Can be set via the API or through custom form fields of name "extra[key]" on the Customer's register form in the storefront. |
| total_spent          | The total amount of money that the Customer has spent at the store                           |
| total_spent_currency | The total spent's currency in [ISO 4217 format](http://en.wikipedia.org/wiki/ISO_4217)       |
| last_order_id        | The id of the Customer's last Order                                                          |
| active               | "true" if the Customer activated his account. "false" if he/she hasn't                       |
| created_at           | Date when the Customer was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)     | 
| updated_at           | Date when the Customer was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)|

Endpoints
---------

### GET /customers

Receive a list of all Customers.


| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| since_id       | Restrict results to after the specified ID                                                       |
| created_at_min | Show Customers created after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))       |
| created_at_max | Show Customers created before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))      |
| updated_at_min | Show Customers last updated after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))  |
| updated_at_max | Show Customers last updated before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)) |
| page           | Page to show                                                                                     |
| per_page       | Amount of results                                                                                |
| fields         | Comma-separated list of fields to include in the response                                        |
| q              | Search Customers containing the given text in their name, email or identification                |



#### GET /customers

`HTTP/1.1 200 OK`

```json
[
    {
      "created_at": "2013-01-03T09:11:51-03:00",
      "email": "john.doe@example.com",
      "id": 101,
      "identification": "28776255670",
      "last_order_id": 9001,
      "name": "John Doe",
      "note": null,
      "phone": null,
      "total_spent": "89.00",
      "total_spent_currency": "USD",
      "updated_at": "2013-03-11T09:14:11-03:00",
      "billing_address": "Evergreen Terrace",
      "billing_city": "Springfield",
      "billing_country": "US",
      "billing_floor": null,
      "billing_locality": null,
      "billing_number": "742",
      "billing_phone": "555-123-0413",
      "billing_province": "Oregon",
      "billing_zipcode": "97475",
      "extra": {
        "number_of_children": "2",
        "gender": "male"
      },
      "default_address": {
        "address": "Evergreen Terrace",
        "city": "Springfield",
        "country": "US",
        "created_at": "2013-01-03T09:11:51-03:00",
        "default": true,
        "floor": null,
        "id": 1234,
        "locality": null,
        "number": "742",
        "phone": "555-123-0413",
        "province": "Oregon",
        "updated_at": "2013-03-10T11:13:01-03:00",
        "zipcode": "97475"
      },
      "addresses": [
        {
          "address": "Evergreen Terrace",
          "city": "Springfield",
          "country": "US",
          "created_at": "2013-01-03T09:11:51-03:00",
          "default": true,
          "floor": null,
          "id": 1234,
          "locality": null,
          "number": "742",
          "phone": "555-123-0413",
          "province": "Oregon",
          "updated_at": "2013-03-10T11:13:01-03:00",
          "zipcode": "97475"
        }
      ]
    },
    {
      "created_at": "2013-04-07T09:11:51-03:00",
      "email": "john.doe@example.com",
      "id": 112,
      "identification": "28776255671",
      "last_order_id": null,
      "name": "Zé Ninguém",
      "note": null,
      "phone": "392502584",
      "total_spent": "0.00",
      "total_spent_currency": "USD",
      "updated_at": "2013-04-08T11:11:51-03:00",
      "billing_address": "Evergreen Terrace",
      "billing_city": "Springfield",
      "billing_country": "US",
      "billing_floor": null,
      "billing_locality": null,
      "billing_number": "742",
      "billing_phone": "555-123-0413",
      "billing_province": "Oregon",
      "billing_zipcode": "97475",
      "extra": {
        "number_of_children": "2",
        "gender": "male"
      },
      "default_address": {
        "address": "Praça Roberto Gomes Pedrosa",
        "city": "São Paulo",
        "country": "BR",
        "created_at": "2013-04-07T09:11:51-03:00",
        "default": true,
        "floor": null,
        "id": 1234,
        "locality": "Morumbi",
        "number": "1",
        "phone": "11 3749-8000",
        "province": "São Paulo",
        "updated_at": "2013-04-08T11:13:01-03:00",
        "zipcode": "05653-070"
      },
      "addresses": [
        {
          "address": "Praça Roberto Gomes Pedrosa",
          "city": "São Paulo",
          "country": "BR",
          "created_at": "2013-04-07T09:11:51-03:00",
          "default": true,
          "floor": null,
          "id": 1234,
          "locality": "Morumbi",
          "number": "1",
          "phone": "11 3749-8000",
          "province": "São Paulo",
          "updated_at": "2013-04-08T11:13:01-03:00",
          "zipcode": "05653-070"
        }
      ]
    }
]
```

#### GET /customers?since_id=105

`HTTP/1.1 200 OK`

```json
[
    {
      "created_at": "2013-01-03T09:11:51-03:00",
      "email": "john.doe@example.com",
      "id": 101,
      "identification": "28776255670",
      "last_order_id": 9001,
      "name": "John Doe",
      "note": null,
      "phone": null,
      "total_spent": "89.00",
      "total_spent_currency": "USD",
      "updated_at": "2013-03-11T09:14:11-03:00",
      "billing_address": "Evergreen Terrace",
      "billing_city": "Springfield",
      "billing_country": "US",
      "billing_floor": null,
      "billing_locality": null,
      "billing_number": "742",
      "billing_phone": "555-123-0413",
      "billing_province": "Oregon",
      "billing_zipcode": "97475",
      "extra": {
        "number_of_children": "2",
        "gender": "male"
      },
      "default_address": {
        "address": "Evergreen Terrace",
        "city": "Springfield",
        "country": "US",
        "created_at": "2013-01-03T09:11:51-03:00",
        "default": true,
        "floor": null,
        "id": 1234,
        "locality": null,
        "number": "742",
        "phone": "555-123-0413",
        "province": "Oregon",
        "updated_at": "2013-03-10T11:13:01-03:00",
        "zipcode": "97475"
      },
      "addresses": [
        {
          "address": "Evergreen Terrace",
          "city": "Springfield",
          "country": "US",
          "created_at": "2013-01-03T09:11:51-03:00",
          "default": true,
          "floor": null,
          "id": 1234,
          "locality": null,
          "number": "742",
          "phone": "555-123-0413",
          "province": "Oregon",
          "updated_at": "2013-03-10T11:13:01-03:00",
          "zipcode": "97475"
        }
      ]
    }
]
```

### GET /customers/{id}

Receive a single Customer

| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| fields         | Comma-separated list of fields to include in the response                                        |

#### GET /customers/101

`HTTP/1.1 200 OK`

```json
{
  "created_at": "2013-01-03T09:11:51-03:00",
  "email": "john.doe@example.com",
  "id": 101,
  "identification": "28776255670",
  "last_order_id": 9001,
  "name": "John Doe",
  "note": null,
  "phone": null,
  "total_spent": "89.00",
  "total_spent_currency": "USD",
  "updated_at": "2013-03-11T09:14:11-03:00",
  "billing_address": "Evergreen Terrace",
  "billing_city": "Springfield",
  "billing_country": "US",
  "billing_floor": null,
  "billing_locality": null,
  "billing_number": "742",
  "billing_phone": "555-123-0413",
  "billing_province": "Oregon",
  "billing_zipcode": "97475",
  "extra": {
    "number_of_children": "2",
    "gender": "male"
  },
  "default_address": {
    "address": "Evergreen Terrace",
    "city": "Springfield",
    "country": "US",
    "created_at": "2013-01-03T09:11:51-03:00",
    "default": true,
    "floor": null,
    "id": 1234,
    "locality": null,
    "number": "742",
    "phone": "555-123-0413",
    "province": "Oregon",
    "updated_at": "2013-03-10T11:13:01-03:00",
    "zipcode": "97475"
  },
  "addresses": [
    {
      "address": "Evergreen Terrace",
      "city": "Springfield",
      "country": "US",
      "created_at": "2013-01-03T09:11:51-03:00",
      "default": true,
      "floor": null,
      "id": 1234,
      "locality": null,
      "number": "742",
      "phone": "555-123-0413",
      "province": "Oregon",
      "updated_at": "2013-03-10T11:13:01-03:00",
      "zipcode": "97475"
    }
  ]
}
```

### POST /customers

Create a new Customer

#### POST /customers

| Parameter                | Explanation                                                                             |
| ------------------------ | ----------------------------------------------------------------------------------------|
| send_email_invite        | Send an email to notify the customer of their registration                              |
| password                 | User's password                                                                         |

```json
{
    "name": "First Last",
    "email": "first.last@example.com",
    "addresses": [
      {
        "address": "My Street",
        "city": "My City",
        "country": "BR",
        "locality": "Morumbi",
        "number": "123",
        "phone": "11 1234-5678",
        "province": "São Paulo",
        "zipcode": "05653-071"
      }
    ],
    "send_email_invite": true,
    "password": "mysupersecretpassword"
}
```

`HTTP/1.1 201 Created`

```json
{
  "created_at": "2013-06-01T09:11:51-03:00",
  "email": "john.doe+modified@example.com",
  "id": 101,
  "identification": "28776255670",
  "last_order_id": 9001,
  "name": "John Doe",
  "note": null,
  "total_spent": "89.00",
  "total_spent_currency": "USD",
  "updated_at": "2013-06-01T09:11:51-03:00",
  "billing_address": null,
  "billing_city": null,
  "billing_country": null,
  "billing_floor": null,
  "billing_locality": null,
  "billing_number": null,
  "billing_phone": null,
  "billing_province": null,
  "billing_zipcode": null,
  "extra": {
    "number_of_children": "2",
    "gender": "male"
  },
  "default_address": {
    "address": "My Street",
    "city": "My City",
    "country": "BR",
    "created_at": "2013-06-01T09:11:51-03:00",
    "default": true,
    "floor": null,
    "id": 1240,
    "locality": null,
    "number": "123",
    "phone": "11 1234-5678",
    "province": "São Paulo",
    "updated_at": "2013-06-01T09:11:51-03:00",
    "zipcode": "05653-071"
  },
  "addresses": [
    {
      "address": "My Street",
      "city": "My City",
      "country": "BR",
      "created_at": "2013-06-01T09:11:51-03:00",
      "default": true,
      "floor": null,
      "id": 1240,
      "locality": null,
      "number": "123",
      "phone": "11 1234-5678",
      "province": "São Paulo",
      "updated_at": "2013-06-01T09:11:51-03:00",
      "zipcode": "05653-071"
    }
  ]
}
```

### PUT /customers/{id}

Modify an existing Customer

#### PUT /customers/5123

```json
{
  "created_at": "2013-01-03T09:11:51-03:00",
  "email": "john.doe+modified@example.com",
  "id": 101,
  "identification": "28776255670",
  "last_order_id": 9001,
  "name": "John Doe",
  "note": null,
  "phone": "911",
  "total_spent": "89.00",
  "total_spent_currency": "USD",
  "billing_address": "Evergreen Terrace",
  "billing_city": "Springfield",
  "billing_country": "US",
  "billing_floor": null,
  "billing_locality": null,
  "billing_number": "742",
  "billing_phone": "555-123-0413",
  "billing_province": "Oregon",
  "billing_zipcode": "97475",
  "extra": {
    "number_of_children": "2",
    "gender": "male"
  },
  "updated_at": "2013-03-11T09:14:11-03:00",
  "default_address": {
    "address": "Evergreen Terrace",
    "city": "Springfield",
    "country": "US",
    "created_at": "2013-01-03T09:11:51-03:00",
    "default": true,
    "floor": null,
    "id": 1234,
    "locality": null,
    "number": "742",
    "phone": "555-123-0413",
    "province": "Oregon",
    "updated_at": "2013-03-10T11:13:01-03:00",
    "zipcode": "97475"
  },
  "addresses": [
    {
      "address": "Evergreen Terrace",
      "city": "Springfield",
      "country": "US",
      "created_at": "2013-01-03T09:11:51-03:00",
      "default": true,
      "floor": null,
      "id": 1234,
      "locality": null,
      "number": "742",
      "phone": "555-123-0413",
      "province": "Oregon",
      "updated_at": "2013-03-10T11:13:01-03:00",
      "zipcode": "97475"
    }
  ]
}
```

`HTTP/1.1 200 OK`

```json
{
  "created_at": "2013-01-03T09:11:51-03:00",
  "email": "john.doe+modified@example.com",
  "id": 101,
  "identification": "28776255670",
  "last_order_id": 9001,
  "name": "John Doe",
  "note": null,
  "phone": "911",
  "total_spent": "89.00",
  "total_spent_currency": "USD",
  "updated_at": "2013-06-01T09:14:11-03:00",
  "billing_address": "Evergreen Terrace",
  "billing_city": "Springfield",
  "billing_country": "US",
  "billing_floor": null,
  "billing_locality": null,
  "billing_number": "742",
  "billing_phone": "555-123-0413",
  "billing_province": "Oregon",
  "billing_zipcode": "97475",
  "extra": {
    "number_of_children": "2",
    "gender": "male"
  },
  "default_address": {
    "address": "Evergreen Terrace",
    "city": "Springfield",
    "country": "US",
    "created_at": "2013-01-03T09:11:51-03:00",
    "default": true,
    "floor": null,
    "id": 1234,
    "locality": null,
    "number": "742",
    "phone": "555-123-0413",
    "province": "Oregon",
    "updated_at": "2013-03-10T11:13:01-03:00",
    "zipcode": "97475"
  },
  "addresses": [
    {
      "address": "Evergreen Terrace",
      "city": "Springfield",
      "country": "US",
      "created_at": "2013-01-03T09:11:51-03:00",
      "default": true,
      "floor": null,
      "id": 1234,
      "locality": null,
      "number": "742",
      "phone": "555-123-0413",
      "province": "Oregon",
      "updated_at": "2013-03-10T11:13:01-03:00",
      "zipcode": "97475"
    }
  ]
}
```
