Store
======

The Store resource contains general settings and information about a Tiendanube/Nuvemshop's store.

Properties
----------

| Property          | Explanation                                                                                   |
| ----------------- | --------------------------------------------------------------------------------------------- |
| id                | The unique numeric identifier for the Store                                                   |
| name              | List of the names of the Store, in every language supported by the store                      |
| description       | List of the descriptions of the Store, in every language supported by the store               |
| type              | Store type. Examples are "clothing", "sports", "electronic"                                   |
| email             | Store owner's e-mail                                                                          |
| logo              | Store logo URL, starting with // (or null if it has no logo)                                  |
| contact_email     | Store's contact e-mail                                                                        |
| facebook          | Store's Facebook URL                                                                          |
| twitter           | Store's Twitter URL                                                                           |
| google_plus       | Store's G+ URL                                                                                |
| instagram         | Store's Instagram URL                                                                         |
| pinterest         | Store's Pinterest URL                                                                         |
| blog              | Store's blog URL                                                                              |
| address           | Store's address                                                                               |
| phone             | Store's phone                                                                                 |
| business_id       | Business identifier (different for each country) of the company who owns the store            |
| business_name     | Business name of the company who owns the store                                               |
| business_address  | Business address of the company who owns the store                                            |
| customer_accounts | "optional" if the customer is allowed to checkout as guest. "mandatory" if not.               |
| plan_name         | Name of the Tiendanube/Nuvemshop's plan the store is on                                     |
| country           | Store's country in [ISO 3166-1 format](http://en.wikipedia.org/wiki/ISO_3166-1)               |
| languages         | Store available languages with its currency and whether or not is active                      |
| domains           | List of store's domains                                                                       |
| original_domain   | Original `tiendanube.com` or `nuvemshop.com.br` domain for the Store                          |
| current_theme	    | Store's current theme                          						    |
| main_language     | Store's main language                                                                         |
| main_currency     | Store's main currency in [ISO 4217 format](http://en.wikipedia.org/wiki/ISO_4217)             |
| admin_language    | Store's admin language                                                                        |
| created_at        | Date when the Store was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)   | 

Endpoints
---------

### GET /store

Receive a single Store.


| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| fields         | Comma-separated list of fields to include in the response                                        |


#### GET /store

`HTTP/1.1 200 OK`

```json
{
  "address": null,
  "admin_language": "pt",
  "blog": null,
  "business_id": null,
  "business_name": null,
  "business_address": null,
  "contact_email": "contact@pokestore.com",
  "country": "BR",
  "created_at": "2013-01-01T05:12:51-03:00",
  "customer_accounts": "optional",
  "description": {
      "en": "",
      "es": "",
      "pt": ""
  },
  "domains": [
    "www.pokestore.com",
    "www.another.com"
  ],
  "email": "owner@pokestore.com",
  "facebook": "http://www.facebook.com/pokestore",
  "google_plus": "http://plus.google.com/+pokestore",
  "id": 1234,
  "instagram": "http://www.instagram.com/pokestore",
  "languages": {
    "en": {
      "currency" : "USD",
      "active": true
    },
    "es": {
      "currency" : "ARS",
      "active": false
    },
    "pt": {
      "currency" : "BRL",
      "active": true
    }
  },
  "logo": "//d26lpennugtm8s.cloudfront.net/stores/046/themes/common/logo-ff622335866ee56df3bceed2e9d41469.png",
  "main_currency": "BRL",
  "current_theme": "luxury",
  "main_language": "pt",
  "name": {
      "en": "Poké Store",
      "es": "Poké Tienda",
      "pt": "Poké Loja"
  },
  "original_domain" : "pokeloja.nuvemshop.com.br",
  "phone": null,
  "pinterest": "http://www.pinterest.com/pokestore",
  "plan_name": "Business",
  "type": null,
  "twitter": "http://www.twitter.com/pokestore"  
}
```
