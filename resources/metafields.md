Metafields
========

The metafields are Namespaced Key - Value store for Apps.

The metafields can only be associated with the following entities: 

* Product
* Product_Variant
* Category
* Page

To do that you need to set the owner_resource to one of the above, an example would be owner_resource='Product'.

This allows for Reviews apps for example to add SEO features combined with HTML/CSS editing by our customers


Properties
----------

| Property             | Explanation                                                                                  |
| -------------------- | -------------------------------------------------------------------------------------------- |
| id                   | The unique numeric identifier for the metafield.                                                |
| namespace                 | The namespace where the metafield makes sense. It can be any string that starts with a letter followed only by: a-Z A-B 0-9 or _.         |
| key                 | String that identifies the metafield in some namespace. It can be any string that starts with a letter followed only by: a-Z A-B 0-9 or _.                                                           |
| description                | The key (string) with some description explaining the metafield meaning.                           |
| value           | Metafield's value (string).                                                         |
| owner_resource             | Type of entity to which is associated the metafield.                                                               |
| owner_id             | Entity id to which is associated the metaField.                                                               |
| created_at     | Date when the Metafield was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)     | 
| updated_at     | Date when the Metafield was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)|

Endpoints
---------

### GET /metafields/#{owner_resource}

* #{owner_resource} examples are: products, product_variants, categories and pages

Receive a list of all metafield.


| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| owner_id | Entity id to which is associated the metaField.                                                                   |
| namespace   | The namespace where the metafield was defined.filter.                                                                  |
| key | metafield's key .                                                                |
| created_at_min | Show Products created after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))      |
| created_at_max | Show Products created before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))     |
| updated_at_min | Show Products last updated after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)) |
| updated_at_max | Show Products last updated before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))|
| page           | Page to show                                                                                     |
| per_page       | Amount of results                                                                                |
| fields         | Comma-separated list of fields to include in the response                                        |


#### GET /metafields/products

`HTTP/1.1 200 OK`


```json
[
    {
        "id":10691,
        "key":"key3",
        "value":"3",
        "namespace":"namespace3",
        "description":"description3",
        "owner_id":2856879,
        "owner_resource":"Product",
        "created_at":"2015-01-02T19:48:44+0000",
        "updated_at":"2015-01-02T19:48:44+0000"
    },
    {
        "id":10692,
        "key":"key4",
        "value":"4",
        "namespace":"namespace4",
        "description":"description4",
        "owner_id":2856879,
        "owner_resource":"Product",
        "created_at":"2015-01-02T19:48:44+0000",
        "updated_at":"2015-01-02T19:48:44+0000"
    }
]
```

#### GET /metafields/products?per_page=3&owner_id=2856934&created_at_min=2013-01-01T00:00:00-03:00&fields=owner_id,key,value

`HTTP/1.1 200 OK`


```json
[
    {
        "key":"key3",
        "value":"3",
        "owner_id":2856934
    },
    {
        "key":"key4",
        "value":"4",
        "owner_id":2856934
    },
    {
        "key":"key5",
        "value":"5",
        "owner_id":2856934
    }
]
```

### GET /metafields/#{id}

Receive a single metafield

#### GET /metafields/11896


`HTTP/1.1 200 OK`

```json
{
    "id":11896,
    "key":"key0",
    "value":"0",
    "namespace":"namespace0",
    "description":"description0",
    "owner_id":2856959,
    "owner_resource":"Product",
    "created_at":"2015-01-02T20:01:50+0000",
    "updated_at":"2015-01-02T20:01:50+0000"
}
```

### POST /metafields

Create a new metafield

#### POST /metafields

| Parameter                | Explanation                                                                             |
| ------------------------ | ----------------------------------------------------------------------------------------|
| key                     | The value is mandatory and it can be any string that starts with a letter followed only by: a-Z A-B 0-9 or _.|
| value                    | The value is mandatory and it can be any string.
| namespace                    | The value is mandatory and it can be any string that starts with a letter followed only by: a-Z A-B 0-9 or _.                            |
| description                    | Is optional and can have some description that shows the use of this metafield                            |
| owner_id                    | The value is mandatory and must exist an entity of the owner_resource type with that id.                            |
| owner_resource                    | The value is mandatory and can be only the allowed values mentioned at the beginning of this documentation.                            |


```json
{
    "key": "key"
    "value": "value",
    "namespace": "namespace",
    "description": "description",
    "owner_id": "2857023",
    "owner_resource": "Product"
}
```

`HTTP/1.1 201 Created`

```json
{
    "id":12877,
    "namespace":"namespace",
    "description":"description",
    "key":"key","value":"value",
    "owner_id":2857023,
    "owner_resource": "Product",
    "created_at":"2015-01-02 20:27:51",
    "updated_at":"2015-01-02 20:27:51",
    "deleted_at":null
}
```

### PUT /metafields/#{id}

Modify an existing metafield

#### PUT /metafields/32967

```json
{
    "value": "modified"
}
```

`HTTP/1.1 200 OK`


```json
{
    "id":13226,
    "key":"key0",
    "value":"modified",
    "namespace":"namespace0",
    "description":"description0",
    "owner_id":2857047,
    "owner_resource":"Product",
    "created_at":"2015-01-02T20:32:08+0000",
    "updated_at":"2015-01-02T20:32:09+0000"
}
```

### DELETE /metafields/#{id}

Delete an existing metafield

#### DELETE /metafields/13226

`HTTP/1.1 200 OK`

```json
{
  
}
```
