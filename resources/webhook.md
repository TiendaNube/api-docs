Webhook
=======

A Webhook is a tool that allows you to receive a notification for a certain event. It allows you to register an *http* or *https* URL which will receive the event data, stored in JSON. Webhooks can be registered for the following events:

| Category       | Events                                                                                           |
| -------------- | ------------------------------------------------------------------------------------------------ |
| App            | uninstalled/suspended/resumed                                                                    |
| Category       | created/updated/deleted                                                                          |
| Order          | created/updated/paid/fulfilled/cancelled                                                         |
| Product        | created/updated/deleted                                                                          |
| Domain        | updated                                                                                          |
| Theme          | updated                                                                                          |

To register for the product created event, for example, you should send `product/created` in the event field.

The `app/suspended` and `app/resumed` events refer to the [suspension of API access due to lack of payment](https://github.com/TiendaNube/api-docs/#suspension-of-api-access-due-to-lack-of-payment).

You are not allowed to use a localhost/tiendanube/nuvemshop domain for webhooks.

To aid in your development you can use [RequestCatcher](https://requestcatcher.com/). PostCatcher is a web app that generates a unique url that you can use as a webhook endpoint and display any POST requests sent to it for you to examine.

Properties
----------

| Property       | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| id             | The unique numeric identifier for the Webhook                                                    |
| url            | The URL where the webhook should send the POST request when the event occurs.                    |
| event          | The event that will trigger the webhook                                                          |
| created_at     | Date when the Webhook was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)    | 
| updated_at     | Date when the Webhook was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)|

### Verifying a webhook

Webhooks created through the API can be verified by calculating a digital signature.

Each Webhook request includes a X-Linkedstore-HMAC-SHA256 header which is generated using the app's secret, along with the data sent in the request.

__Note that some frameworks will change the header to HTTP_X_LINKEDSTORE_HMAC_SHA256__.

See PHP example below:

```php
<?php

define('APP_SECRET', 'secret');

function verify_webhook($data, $hmac_header) {
  return $hmac_header == hash_hmac('sha256', $data, APP_SECRET);
}

$hmac_header = $_SERVER['HTTP_X_LINKEDSTORE_HMAC_SHA256'];
$data = file_get_contents('php://input');
$verified = verify_webhook($data, $hmac_header);
echo 'Webhook verified: ' . var_export($verified, true);
```

### Parameters

When doing the POST request, all webhooks will send the following parameters in JSON format:

* __store_id__: Store from where the event originated
* __event__: Event's name (product/created, product/updated, etc.)

Also, every webhook will send custom parameters, as follows:

#### app/uninstalled

* __id__: App's id

#### category/created - category/updated - category/deleted

* __id__: Category's id

#### order/created - order/updated - order/paid - order/fulfilled - order/cancelled

* __id__: Order's id

#### product/created - product/updated - product/deleted

* __id__: Product's id

#### domain/updated

* no additional parameter is sent along with this event. To get the list of domains one may refer to [Store](https://github.com/tiendanube/api-docs/blob/master/resources/store.md) resources.

#### theme/updated

* __old_theme__: Old Theme's code
* __new_theme__: New Theme's code

#### Example webhook content

```javascript
{
  'store_id':123,
  'event':'product/created',
  'id':1948209
}
```

### Retry policies

A webhook notification expects a 200 status code in response. If this does not happen (it gets another response code or no response at all) we will retry to deliver the notification 14 times along the next three days in incremental lapses of time. After this, the notification is lost.

Endpoints
---------

### GET /webhooks

Receive a list of all Webhooks for your application.


| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| since_id       | Restrict results to after the specified ID                                                       |
| url            | Show webhooks tags with a given URL                                                              |
| event          | Show webhooks with a given event                                                                 |
| created_at_min | Show Webhooks created after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))      |
| created_at_max | Show Webhooks created before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))     |
| updated_at_min | Show Webhooks last updated after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)) |
| updated_at_max | Show Webhooks last updated before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))|
| page           | Page to show                                                                                     |
| per_page       | Amount of results                                                                                |
| fields         | Comma-separated list of fields to include in the response                                        |


#### GET /webhooks

`HTTP/1.1 200 OK`

```json
[
    {
      "created_at": "2013-01-03T09:11:51-03:00",
      "event": "app/uninstalled",
      "id": 101,
      "updated_at": "2013-03-11T09:14:11-03:00",
      "url": "http://myapp.com/uninstall"
    },
    {
      "created_at": "2013-04-07T09:11:51-03:00",
      "event": "order/created",
      "id": 5670,
      "updated_at": "2013-04-08T11:11:51-03:00",
      "url": "http://myapp.com/order_created_hook"
    }
]
```

#### GET /webhooks?since_id=1234

`HTTP/1.1 200 OK`

```json
[
    {
      "created_at": "2013-04-07T09:11:51-03:00",
      "event": "order/created",
      "id": 5670,
      "updated_at": "2013-04-08T11:11:51-03:00",
      "url": "http://myapp.com/order_created_hook"
    }
]
```

### GET /webhooks/#{id}

Receive a single Webhook

| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| fields         | Comma-separated list of fields to include in the response                                        |

#### GET /webhooks/5670

`HTTP/1.1 200 OK`

```json
{
  "created_at": "2013-04-07T09:11:51-03:00",
  "event": "order/created",
  "id": 5670,
  "updated_at": "2013-04-08T11:11:51-03:00",
  "url": "http://myapp.com/order_created_hook"
}
```

### POST /webhooks

Create a new Webhook

#### POST /webhooks

```json
{
    "url": "foobar",
    "event" : "invalid_event"
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
    "event": [
      "Invalid event specified. Events allowed: app/uninstalled, cart/created, cart/updated, category/created, category/updated, category/deleted, order/created, order/updated, order/paid, order/fulfilled, order/cancelled, product/created, product/updated, product/deleted"
    ],
    "url": [
      "invalid url specified"
    ]
}
```

#### POST /webhooks

```json
{
  "event": "product/created",
  "url": "http://myapp.com/product_created_hook"
}
```

`HTTP/1.1 201 Created`

```json
{
    "created_at": "2013-06-01T15:12:15-03:00",
    "event": "product/created",
    "id": 8901,
    "updated_at": "2013-06-01T15:12:15-03:00",
    "url": "http://myapp.com/product_created_hook"
}
```

### PUT /webhooks/#{id}

Modify an existing Webhook

#### PUT /webhooks/5670

```json
{
  "created_at": "2013-04-07T09:11:51-03:00",
  "event": "category/created",
  "id": 5670,
  "updated_at": "2013-04-08T11:11:51-03:00",
  "url": "http://myapp.com/category_created_hook"
}
```

`HTTP/1.1 200 OK`

```json
{
  "created_at": "2013-04-07T09:11:51-03:00",
  "event": "category/created",
  "id": 5670,
  "updated_at": "2013-06-01T12:11:14-03:00",
  "url": "http://myapp.com/category_created_hook"
}
```

### DELETE /webhooks/#{id}

Remove a Webhook

#### DELETE /webhooks/5670

`HTTP/1.1 200 OK`

```json
{}
```
