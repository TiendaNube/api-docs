Script
======

The Script resource allows the app to register custom Javascript to be run in the store or in the checkout page. Also, if the user deletes your app, then he will not have to edit his theme to remove your JavaScript. When an app is uninstalled from a store, all of the scripts the app created are automatically removed along with it.

You should have the following things into consideration:

* You cannot depend of any JavaScript available in the store's theme. Not even jQuery.

* Another applications may be installed and can include other JavaScript in addition to yours.

Ideally, your javascript should be inside a closure to avoid any conflict:

```javascript
(function(){
    // Your JavaScript
})();
```

Use AJAX to load specific configurations for the store. Access your own defined URL with the store ID specified in that URL. Your app will serve those store settings in JSON format, which you can then use in your JavaScript file.

If you are going to use jQuery, you should load it in your JS using `jQuery.noConflict` as some stores already have jQuery in their themes. You should not assume the store has a cutting-edge jQuery version. 

```javascript
var loadScript = function(url, callback){

  /* JavaScript that will load the jQuery library on Google's CDN.
     We recommend this code: http://snipplr.com/view/18756/loadscript/.
     Once the jQuery library is loaded, the callback function will be executed. */

};

var myAppJavaScript = function($){
  /* Your app's JavaScript here.
     $ in this scope references the jQuery object we'll use.
     Don't use 'jQuery', or 'jQuery191', here. Use the dollar sign
     that was passed as argument.*/
  $('body').append("<p>I'm using jQuery version "+$.fn.jquery+'</p>');
};

if ((typeof jQuery === 'undefined') || (parseFloat(jQuery.fn.jquery) < 1.7)) {
  loadScript('//ajax.googleapis.com/ajax/libs/jquery/1.10.1/jquery.min.js', function(){
    var jQuery1101 = jQuery.noConflict(true);
    myAppJavaScript(jQuery1101);
  });
} else {
  myAppJavaScript(jQuery);
}

```

Properties
----------

| Property       | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| id             | The unique numeric identifier for the Script                                                     |
| src            | Specifies the location of the Script                                                             |
| event          | DOM event which triggers the loading of the script. Valid values are **onload** (default)        |
| where          | A comma-separated list of places where the javascript will run. Valid values are **store** (default) or **checkout** |
| created_at     | Date when the Script was created in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)     | 
| updated_at     | Date when the Script was last updated in [ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)|

Endpoints
---------

### GET /scripts

Receive a list of all Scripts.


| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| since_id       | Restrict results to after the specified ID                                                       |
| src            | Show Scripts with a given URL                                                                |
| created_at_min | Show Scripts created after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))       |
| created_at_max | Show Scripts created before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))      |
| updated_at_min | Show Scripts last updated after date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601))  |
| updated_at_max | Show Scripts last updated before date ([ISO 8601 format](http://en.wikipedia.org/wiki/ISO_8601)) |
| page           | Page to show                                                                                     |
| per_page       | Amount of results                                                                                |
| fields         | Comma-separated list of fields to include in the response                                        |


#### GET /scripts

`HTTP/1.1 200 OK`

```json
[
    {
      "created_at": "2013-01-03T09:11:51-03:00",
      "event": "onload",
      "id": 101,
      "src": "http://myapp.com/foo.js",
      "updated_at": "2013-03-11T09:14:11-03:00",
      "where": "store,checkout"
    },
    {
      "created_at": "2013-04-07T09:11:51-03:00",
      "event": "onload",
      "id": 5123,
      "src": "http://myapp.com/bar.js",
      "updated_at": "2013-04-08T11:11:51-03:00",
      "where": "store"
    },
    {
      "created_at": "2013-04-08T12:09:48-03:00",
      "event": "onload",
      "id": 6412,
      "src": "http://myapp.com/yet_another_script.js",
      "updated_at": "2013-04-08T11:11:53-03:00",
      "where": "checkout"
    }
]
```

#### GET /scripts?since_id=1234

`HTTP/1.1 200 OK`

```json
[
    {
      "created_at": "2013-04-07T09:11:51-03:00",
      "event": "onload",
      "id": 5123,
      "src": "http://myapp.com/foo.js",
      "updated_at": "2013-04-08T11:11:51-03:00",
      "where": "store"
    },
    {
      "created_at": "2013-04-08T12:09:48-03:00",
      "event": "onload",
      "id": 6412,
      "src": "http://myapp.com/bar.js",
      "updated_at": "2013-04-08T11:11:53-03:00",
      "where": "checkout"
    }
]
```

### GET /scripts/#{id}

Receive a single Script

| Parameter      | Explanation                                                                                      |
| -------------- | ------------------------------------------------------------------------------------------------ |
| fields         | Comma-separated list of fields to include in the response                                        |

#### GET /scripts/1234

`HTTP/1.1 200 OK`

```json
{
  "created_at": "2013-01-03T09:11:51-03:00",
  "event": "onload",
  "id": 101,
  "src": "http://myapp.com/foo.js",
  "updated_at": "2013-03-11T09:14:11-03:00",
  "where": "store,checkout"
}
```

### POST /scripts

Create a new Script.

__A Script will only appear in checkout if the src starts with 'https://' in order not to break the SSL lock.__

#### POST /scripts

```json
{
    "invalid_name": "foobar",
    "where" : "invalid_where",
}
```

`HTTP/1.1 422 Unprocessable Entity`

```json
{
    "event": [
      "can't be blank"
    ],
    "src": [
      "can't be blank"
    ],
    "where": [
      "is not included in the list"
    ]
}
```

#### POST /scripts

```json
{
    "src": "http://myapp.com/new.js",
    "event" : "onload",
    "where" : "store",
}
```

`HTTP/1.1 201 Created`

```json
{
    "created_at": "2013-06-01T15:12:15-03:00",
    "event": "onload",
    "id": 8901,
    "src": "http://myapp.com/new.js",
    "updated_at": "2013-06-01T15:12:15-03:00",
    "where": "store"
}
```

### PUT /scripts/#{id}

Modify an existing Script

#### PUT /scripts/5123

```json
{
  "created_at": "2013-04-07T09:11:51-03:00",
  "event": "onload",
  "id": 5123,
  "src": "http://myapp.com/another_bar.js",
  "updated_at": "2013-06-01T12:05:34-03:00",
  "where": "store"
}
```

`HTTP/1.1 200 OK`

```json
{
  "created_at": "2013-04-07T09:11:51-03:00",
  "event": "onload",
  "id": 5123,
  "src": "http://myapp.com/another_bar.js",
  "updated_at": "2013-06-01T12:11:14-03:00",
  "where": "store"
}
```

### DELETE /scripts/#{id}

Remove a Script

#### DELETE /scripts/5123

`HTTP/1.1 200 OK`

```json
{}
```
