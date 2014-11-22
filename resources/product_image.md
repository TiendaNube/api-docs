Product Image
=============

Product images could well be the single most important design aspect of any store. Without the ability to touch, hold, smell, taste or otherwise handle the products they are interested in, potential customers have only images to interact with.

The product images have the following restrictions:

* Must weight less than 10MB
* Must be in one of the following formats: .gif, .jpg, .png

Properties
----------

| Property       | Explanation                                                                                             |
| -------------- | ------------------------------------------------------------------------------------------------------- |
| id             | The unique numeric identifier for the Product Image                                                     |
| product_id     | The id of the product associated with the image                                                         |
| src            | URL of the product image                                                                                |
| position       | Number indicating the position of the image in the product's image list. 1 is the first and the main product image |
| created_at     | Date when the Product Image was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)     | 
| updated_at     | Date when the Product Image was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)|

Endpoints
---------

### GET /products/#{product_id}/images

Receive a list of all Product Images for a given product.


| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| since_id       | Restrict results to after the specified ID                                                       |
| src            | Show Product Images with a given URL                                                             |
| position       | Show Product Images at a given position                                                          |
| page           | Page to show                                                                                     |
| per_page       | Amount of results                                                                                |
| fields         | Comma-separated list of fields to include in the response                                        |


#### GET /products/1234/images

`HTTP/1.1 200 OK`

```json
[
    {
      "id": 101,
      "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/servine-640-0.jpg",
      "position": 1,
      "product_id": 1234,
      "created_at": "2013-01-03T09:11:51-03:00",
      "updated_at": "2013-03-11T09:14:11-03:00"
    },
    {
      "id": 112,
      "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/onyx-640-0.jpg",
      "position": 2,
      "product_id": 1234,
      "created_at": "2013-01-03T09:11:51-03:00",
      "updated_at": "2013-03-11T09:14:11-03:00"
    },
    {
      "id": 123,
      "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/stoutland-640-0.jpg",
      "position": 3,
      "product_id": 1234,
      "created_at": "2013-01-03T09:11:51-03:00",
      "updated_at": "2013-03-11T09:14:11-03:00"
    }
]
```

#### GET /product/1234/images?since_id=105

`HTTP/1.1 200 OK`

```json
[
    {
      "id": 112,
      "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/onyx-640-0.jpg",
      "position": 2,
      "product_id": 1234,
      "created_at": "2013-01-03T09:11:51-03:00",
      "updated_at": "2013-03-11T09:14:11-03:00"
    },
    {
      "id": 123,
      "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/stoutland-640-0.jpg",
      "position": 3,
      "product_id": 1234,
      "created_at": "2013-01-03T09:11:51-03:00",
      "updated_at": "2013-03-11T09:14:11-03:00"
    }
]
```

### GET /products/#{product_id}/images

Receive a single Product Image

| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| fields         | Comma-separated list of fields to include in the response                                        |

#### GET /products/1234/images/112

`HTTP/1.1 200 OK`

```json
{
    "id": 112,
    "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/onyx-640-0.jpg",
    "position": 2,
    "product_id": 1234,
    "created_at": "2013-01-03T09:11:51-03:00",
    "updated_at": "2013-03-11T09:14:11-03:00"
}
```

### POST /products/#{product_id}/images

Create a new Product Image

#### POST /products/1234/images

```json
{
    "invalid_name": "foobar"
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
    "src": [
      "can't be blank"
    ]
}
```

#### POST /products/1234/images

```json
{
    "src": "http://example.com/charmander.jpg"
}
```

`HTTP/1.1 201 Created`

```json
{
    "id": 134,
    "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/charmander-640-0.jpg",
    "position": 4,
    "product_id": 1234,
    "created_at": "2013-04-12T09:14:11-03:00",
    "updated_at": "2013-04-12T09:14:11-03:00"
}
```

#### POST /products/1234/images

```json
{
    "filename": "mewtwo.gif",
    "position": 5,
    "attachment": "R0lGODlhIAAgAPIAAPj4+MCgyHh4eGBgWLgomEBAQP///wAAACH/C05FVFNDQVBFMi4wAwEAAAAh
/hFwb2tlZ3VpZGUuZmlsYi5kZQAh+QQJEQAGACwAAAAAIAAgAAADj2i63P4wykmrvTjrzbv/YNgU
BUN+BUCuQFB2JNCmgeulbV2/y3rhup2pReBJgDuhgkYoUnA5mm0ZaBojyAJBadAWr8fgylXSgrEF
Mim5M1uka3H8VEm7BOmx3T59lvFVRWJcbyRNgjo+GAMCjQOHAzoDGoyOBgOYmAGTG5kOkZsil0Gc
IQMyNaUgmaqirhcJACH5BAkRAAYALAAAAAAgACAAAAOLaLrc/jDKSau9OOvNu/9gKEpFwZRfAZQs
EJhdCbhq8Hqqa9tww1a5Hc9RI/QgQd7w5CIYSTNd7XYKOI+PZIGwXGyNWKSQ9UIZtmExq6DclZ5A
KXtc4qWzPAGbPJ9TKSgFelZGY10VAyxOhW5mFgMCkQMDiwM7AxuTmAaamgGTI5aXoTM2myGdI6oa
CQA7"
}
```

`HTTP/1.1 201 Created`

```json
{
    "id": 145,
    "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/mewtwo-640-0.jpg",
    "position": 5,
    "product_id": 1234,
    "created_at": "2013-04-12T09:15:11-03:00",
    "updated_at": "2013-04-12T09:15:11-03:00"
}
```

### PUT /products/#{product_id}/images/#{id}

Modify an existing Product Image

#### PUT /products/1234/images/145

```json
{
  "id": 145,
  "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/mewtwo-640-0.jpg",
  "position": 1,
  "product_id": 1234
}
```

`HTTP/1.1 200 OK`

```json
{
  "id": 145,
  "src": "http://d26lpennugtm8s.cloudfront.net/stores/001/234/products/mewtwo-640-0.jpg",
  "position": 1,
  "product_id": 1234,
  "created_at": "2013-01-03T09:11:51-03:00",
  "updated_at": "2013-03-11T09:14:16-03:00"
}
```

### DELETE /products/#{product_id}/images/#{id}

Remove a Product Image

#### DELETE /products/1234/images/145

`HTTP/1.1 200 OK`

```json
{}
```
