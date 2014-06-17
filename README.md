Tienda Nube/Nuvem Shop API
====================

This is a REST-style API that uses JSON for serialization and OAuth 2 for authentication.

Where do I start?
----------------

Want to get started with API integration? Here's a quick check list:

1. Register as a partner in [Tienda Nube](http://www.tiendanube.com/partners) or [Nuvem Shop](http://www.nuvemshop.com.br/parceiros).
2. Once inside your partner's admin panel, go to the "Apps" section and create your app.
3. Read up on [how to authenticate](#authentication) your app with us.
4. Read the API docs to understand what you can do with your app.

Making a request
----------------

All URLs start with `https://api.tiendanube.com/v1/{store_id}` or `https://api.nuvemshop.com.br/v1/{store_id}`. **SSL only**. The path is prefixed with the store id and the API version. If we change the API in backward-incompatible ways, we'll bump the version marker and maintain stable support for the old URLs.

So if you want to access the store with id 123456 via the API the url will be `https://api.tiendanube.com/v1/123456` or `https://api.nuvemshop.com.br/v1/123456`.

To make a request for all the store's products you would do the following in curl:

```shell
curl -H 'Authentication: bearer ACCESS_TOKEN ' \
  -H 'User-Agent: MyApp (name@email.com)' \
  https://api.tiendanube.com/v1/123456/products
```

where `ACCESS_TOKEN` is the store's access token for your app (see Authentication).

To create something, you have to include the `Content-Type` header and the JSON data:

```shell
curl -H 'Authentication: bearer ACCESS_TOKEN ' \
  -H 'Content-Type: application/json' \
  -H 'User-Agent: MyApp (name@email.com)' \
  -d '{ "name": "My new product" }' \
  https://api.tiendanube.com/v1/123456/products
```

Authentication
--------------

We follow the OAuth 2 framework for letting users authorize your application to use Tienda Nube/Nuvem Shop on their behalf. Quite briefly, when a user installs it you can obtain an access token (a secret string denoting your rights over his store), which you have to include in the header of every request (as shown in the above example).

Read the [authentication guide](https://github.com/tiendanube/api-docs/blob/master/resources/authentication.md) to get started.


Identify your app
-----------------

In every API request, you must include a `User-Agent` header with the name of your application and a link to it or your email address so we can get in touch with you. Here's a couple of examples:

    User-Agent: Super app (http://superapp.com/contact)
    or
    User-Agent: Awesome app (awesome@app.com)

If you don't supply this header, you will get a `400 Bad Request` response.

Just JSON
-----------------

All data is sent and received as JSON. Our format is to have no root element and we use snake\_case to describe attribute keys. This means that you have to send `Content-Type: application/json; charset=utf-8` when POSTing or PUTing data into Tienda Nube/Nuvem Shop.

You'll receive a `415 Unsupported Media Type` response code if you leave out the `Content-Type` header.

Use HTTP caching (Coming Soon)
----------------

You must make use of the HTTP freshness headers to lessen the load on our servers (and increase the speed of your application!). Most requests we return will include an `ETag` or `Last-Modified` header. When you first request a resource, store this value, and then submit them back to us on subsequent requests as `If-None-Match` and `If-Modified-Since`. If the resource hasn't changed, you'll see a `304 Not Modified` response, which saves you the time and bandwidth of sending something you already have.

Also, cached requests will not consume your API rate limit (see Rate Limiting).


Client errors
-------------

These are the possible types of client errors on API calls that receive request bodies:

* Sending invalid JSON will result in a `400 Bad Request` response.

```
HTTP/1.1 400 Bad Request
Content-Length: 34

{"error": "Problems parsing JSON"}
```

* Sending invalid fields will result in a `422 Unprocessable Entity` response.

```
HTTP/1.1 422 Unprocessable Entity
Content-Length: 47

{
  "src": [
      "can't be blank"
    ]
}
```

Server errors
-------------

If Tienda Nube/Nuvem Shop is having trouble, you might see a 5xx error. `500` means that the app is entirely down, but you might also see `502 Bad Gateway`, `503 Service Unavailable`, or `504 Gateway Timeout`. It's your responsibility in all of these cases to retry your request later.


Rate limiting
-------------

You can perform up to 500 requests per 5 minutes period from the same app to the same store. If you exceed this limit, you'll get a [429 Too Many Requests](http://tools.ietf.org/html/draft-nottingham-http-new-status-02#section-4) response for subsequent requests. We use the following headers to provide information:

* `X-Rate-Limit-Limit` : Total requests that con be done in a given period.
* `X-Rate-Limit-Remaining` : Remaining requests in the current period.
* `X-Rate-Limit-Reset` : Remaining seconds for the start of the next period.

Pagination
----------

Requests that return multiple items will be paginated to 30 items by default. You can specify further pages with the `page` parameter. You can also set a custom page size up to 200 with the `per_page` parameter.

```shell
curl -H 'Authentication: bearer ACCESS_TOKEN ' \
  -H 'User-Agent: MyApp (name@email.com)' \
  https://api.tiendanube.com/v1/123456/products?page=2&per_page=100
```

Note that page numbering is 1-based and that ommiting the `page` parameter will return the first page.

To check the total count of the results you can use the `X-Total-Count` header:

```
X-Total-Count: 156
```

To check the next and previous links for pagination you can use the `Link` header:

```
Link: <https://api.tiendanube.com/v1/123456/products?page=3&per_page=100>; rel="next",
  <https://api.tiendanube.com/v1/123456/products?page=50&per_page=100>; rel="last"
```

The possible `rel` values are:

* `next` : Shows the URL of the immediate next page of results.
* `last` : Shows the URL of the last page of results.
* `first` : Shows the URL of the first page of results.
* `prev` : Shows the URL of the immediate previous page of results.

You **should** use the `Link` URLs instead of building your own.

HTTP Verbs
----------

Where possible, API v1 strives to use appropriate HTTP verbs for each action.

* `HEAD` : Can be issued against any resource to get just the HTTP header info.
* `GET` : Used for retrieving resources.
* `POST` : Used for creating resources.
* `PUT` :  Used for replacing resources or collections. For PUT requests with no body attribute, be sure to set the Content-Length header to zero.
* `DELETE` : Used for deleting resources.

Cross Origin Resource Sharing (Coming Soon)
-----------------------------

The API supports Cross Origin Resource Sharing (CORS) for AJAX requests. you can read the [CORS W3C working draft](http://www.w3.org/TR/cors), or [this intro](http://code.google.com/p/html5security/wiki/CrossOriginRequestSecurity) from the HTML 5 Security Guide.

Here’s a sample request for a browser hitting http://example.com:

```shell
curl -i https://api.tiendanube.com -H "Origin: http://example.com"
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: ETag, Link, X-Rate-Limit-Limit, X-Rate-Limit-Remaining, X-OAuth-Scopes, X-Accepted-OAuth-Scopes
Access-Control-Allow-Origin: *
```

This is what the CORS preflight request looks like:

```shell
curl -i https://api.tiendanube.com -H "Origin: http://example.com" -X OPTIONS
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: ETag, Link, X-Rate-Limit-Limit, X-Rate-Limit-Remaining, X-Rate-Limit-Reset, X-OAuth-Scopes, X-Accepted-OAuth-Scopes
Access-Control-Max-Age: 86400
Access-Control-Allow-Headers: Authorization, Content-Type, If-Match, If-Modified-Since, If-None-Match, If-Unmodified-Since, X-Requested-With
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Origin: *
```

Languages and Internationalization
----------------------------------
Stores can potentially have multiple languages. This means that some properties of the Product and Category endpoints will be objects detailing the value for each language.

For example,

```shell
curl -H 'Authentication: bearer ACCESS_TOKEN ' \
  -H 'User-Agent: MyApp (name@email.com)' \
  -D - \
  https://api.tiendanube.com/v1/123456/categories/1234
```

```
HTTP/1.1 200 OK
Date: Thu, 05 Sep 2013 18:18:22 GMT
Content-Type: application/json; charset=UTF-8
Content-Length: 376
X-Main-Language: es
```

```json
{
    "id": 1234,
    "name": {
        "es": "Nombre de la Categoría",
        "pt": "Nome da Categoria"
    },
    "description": {
        "es": "",
        "pt": ""
    },
    "handle": {
        "es": "nombre-de-la-categoria",
        "pt": "nome-da-categoria"
    },
    "parent": null,
    "subcategories": [

    ],
    "created_at": "2013-09-05T17:59:19+0000",
    "updated_at": "2013-09-05T17:59:19+0000"
}
```

The properties affected by internationalization will always be an object, even if there is only one language active. If you only care about the main language, you can obtain its code by reading the `X-Main-Language` header.


When POSTing or PUTing data you can provide an object with a value for each language:

```shell
curl -H 'Authentication: bearer ACCESS_TOKEN ' \
  -H 'Content-Type: application/json' \
  -H 'User-Agent: MyApp (name@email.com)' \
  -d '{ "name": {"es": "Ejemplo", "pt": "Exemplo"} }' \
  https://api.tiendanube.com/v1/123456/products
```

Or you can simply provide a string value, which will be applied to all languages:

```shell
curl -H 'Authentication: bearer ACCESS_TOKEN ' \
  -H 'Content-Type: application/json' \
  -H 'User-Agent: MyApp (name@email.com)' \
  -d '{ "name": "Example" }' \
  https://api.tiendanube.com/v1/123456/products
```

Front-end integration
---------------------

When using [scripts](https://github.com/tiendanube/api-docs/blob/master/resources/script.md) to integrate your app with the storefront, you may want to bind yourself to certain events or access related objects.

While support for this is still being improved, there is one single event you may bind yourself to: `LS.registerOnChangeVariant(callback)` where `callback` is a function that receives a single argument, a `variant`, that contains information about the chosen product variant. In particular, `variant.element` contains a string detailing a jQuery selector that you may use in order to obtain a container for the chosen variant's form; this is important when working with themes that have a "quick shop" feature, such as LinkedMan or LinkedWoman, as there may be more than one product form present in a page.


Suspension of API access due to lack of payment
-----------------------------------------------

Being a monthly subscription service, it's possible that a store will not renew its service. In this case, the store will go offline and the API will be inaccessible. The API will also be inaccessible if the app has a recurring price and it's not paid in time.

In either case, all API calls will return a `402 Payment Required` response, [Scripts](https://github.com/tiendanube/api-docs/blob/master/resources/script.md) will not be included and [Webhooks](https://github.com/tiendanube/api-docs/blob/master/resources/webhook.md) will not be called. Please make sure you handle this error code to notify the user that he needs to resume his payment instead of displaying a generic server error.

Once the required payment is made, the API becomes accessible again.

If your app needs to know when access to the API is suspended or resumed (because you may have missed a webhook and want to do a full resync, for example), you can register to the `app/suspended` and `app/resumed` events using a Webhook.


API resources
-----------------

* [Store](https://github.com/tiendanube/api-docs/blob/master/resources/store.md)
* [Product](https://github.com/tiendanube/api-docs/blob/master/resources/product.md)
* [Product Variant](https://github.com/tiendanube/api-docs/blob/master/resources/product_variant.md)
* [Product Image](https://github.com/tiendanube/api-docs/blob/master/resources/product_image.md)
* [Category](https://github.com/tiendanube/api-docs/blob/master/resources/category.md)
* [Order](https://github.com/tiendanube/api-docs/blob/master/resources/order.md)
* [Customer](https://github.com/tiendanube/api-docs/blob/master/resources/customer.md)
* [Script](https://github.com/tiendanube/api-docs/blob/master/resources/script.md)
* [Webhook](https://github.com/tiendanube/api-docs/blob/master/resources/webhook.md)
* [Coupons](https://github.com/tiendanube/api-docs/blob/master/resources/coupon.md)


Help us make it better
----------------------

Please tell us how we can make the API better. If you have a specific feature request or if you found a bug, please use GitHub issues. Feel free to fork these docs and send a pull request with improvements.

To talk with us about the API, feel free to write to <mailto:api@tiendanube.com>.
