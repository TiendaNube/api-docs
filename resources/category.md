Category
======

A Category lets the store owner group his/her products to make the store easier to browse.

Properties
----------

| Property       | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| id             | The unique numeric identifier for the Category                                                   |
| name           | List of the names of the Category, in every language supported by the store                      |
| descripton     | List of the descriptions of the Category, as HTML, in every language supported by the store      |
| handle         | List of the url-friendly strings generated from the Category's names, in every language supported by the store |
| parent         | Id of the Category's parent. *null* if it has no parent                                          |
| subcategories  | The ids of the Category's first level subcategories                                              |
| created_at     | Date when the Category was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)   | 
| updated_at     | Date when the Category was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)|

Endpoints
---------

### GET /categories

Receive a list of all Categories.


| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| since_id       | Restrict results to after the specified ID                                                       |
| language       | Specify search language (required when serching for handle)                                      |
| handle         | Show Categories with a given URL                                                                   |
| parent_id      | Show Categories with a given parent category                                                       |
| created_at_min | Show Categories created after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))      |
| created_at_max | Show Categories created before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))     |
| updated_at_min | Show Categories last updated after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)) |
| updated_at_max | Show Categories last updated before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))|
| page           | Page to show                                                                                     |
| per_page       | Amount of results                                                                                |
| fields         | Comma-separated list of fields to include in the response                                        |


#### GET /categories

`HTTP/1.1 200 OK`

```json
[
    {
      "created_at": "2013-01-03T09:11:51-03:00",
      "description": {
          "en": "",
          "es": "",
          "pt": ""
      },
      "handle": {
          "en": "poke-balls",
          "es": "poke-balls",
          "pt": "poke-balls"
      },
      "id": 4567,
      "name": {
          "en": "Poké Balls",
          "es": "Poké Balls",
          "pt": "Poké Balls"
      },
      "parent": null,
      "subcategories": [],
      "updated_at": "2013-03-11T09:14:11-03:00"
    }
]
```

#### GET /categories?fields=id,name,subcategories

`HTTP/1.1 200 OK`

```json
[
    {
      "id": 4567,
      "name": {
          "en": "Poké Balls",
          "es": "Poké Balls",
          "pt": "Poké Balls"
      },
      "subcategories": []
    }
]
```

### GET /categories/{id}

Receive a single Category

| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| fields         | Comma-separated list of fields to include in the response                                        |

#### GET /categories/4567

`HTTP/1.1 200 OK`

```json
{
  "created_at": "2013-01-03T09:11:51-03:00",
  "description": {
      "en": "",
      "es": "",
      "pt": ""
  },
  "handle": {
      "en": "poke-balls",
      "es": "poke-balls",
      "pt": "poke-balls"
  },
  "id": 4567,
  "name": {
      "en": "Poké Balls",
      "es": "Poké Balls",
      "pt": "Poké Balls"
  },
  "parent": null,
  "subcategories": [],
  "updated_at": "2013-03-11T09:14:11-03:00"
}
```

### POST /categories

Create a new Category

#### POST /categories

```json
{
    "invalid_name": "foobar"
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
    "name": [
      "can't be blank"
    ]
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
    "code": 422,
    "message": "Unprocessable Entity",
    "description": "Store has reached maximum limit of 5000 allowed categories"
}
```

#### POST /categories

```json
{
    "name": {
      "en": "Gen I",
      "es": "Gen I",
      "pt": "Gen I"
    },
    "parent": 4567    
}
```

`HTTP/1.1 201 Created`

```json
{
  "created_at": "2013-06-01T12:15:11-03:00",
  "description": {
      "en": "",
      "es": "",
      "pt": ""
  },
  "handle": {
      "en": "gen-i",
      "es": "gen-i",
      "pt": "gen-i"
  },
  "id": 5678,
  "name": {
      "en": "Gen I",
      "es": "Gen I",
      "pt": "Gen I"
  },
  "parent": 4567,
  "subcategories": [],
  "updated_at": "2013-06-01T12:15:11-03:00"
}
```

### PUT /categories/{id}

Modify an existing Category

#### PUT /categories/5678

```json
{
  "id": 5678,
  "parent": null
}
```

`HTTP/1.1 200 OK`

```json
{
  "created_at": "2013-06-01T12:15:11-03:00",
  "description": {
      "en": "",
      "es": "",
      "pt": ""
  },
  "handle": {
      "en": "gen-i",
      "es": "gen-i",
      "pt": "gen-i"
  },
  "id": 5678,
  "name": {
      "en": "Gen I",
      "es": "Gen I",
      "pt": "Gen I"
  },
  "parent": null,
  "subcategories": [],
  "updated_at": "2013-06-01T12:15:11-03:00"
}
```

### DELETE /categories/{id}

Remove a Category

#### DELETE /categories/4567

`HTTP/1.1 200 OK`

```json
{}
```
