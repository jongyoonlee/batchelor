# Batchelor
A lovely little Node.js module to perform batch requests with the Google REST API. Simply does it.

![Batchelor](http://wapi.staging.wapisasa.org/wp-content/uploads/2015/02/batchelor-smaller.png)

## Google API Batch Requests
This is a project to solve a need for a missing feature in the wonderfully epic and useful [google/google-api-nodejs-client](https://github.com/google/google-api-nodejs-client). Currently, this cannot be used to post media and has not been tested with posting anything but JSON.

In theory this library could be used with other APIs, but has only been tested with Google's APIs as that's what we need it for.

Feel free to get involved in development.

## Issues, Features and Bugs
This has been created for an internal project of [wapisasa](https://github.com/wapisasa) so it might not fit everyone's requirements. It might also be buggy as it's an alpha release. Any issues, feature requests or bugs that need attention please [let us know](https://github.com/wapisasa/batchelor/issues).

## Installation

This library has also been distributed on `npm`. Install it with the following command:

``` sh
$ npm install batchelor --save
```

## How to Use
#### GET Requests
``` node
var Batchelor = require('batchelor');
```
Once the module has been included, we initialise it with all our default options:
``` node
Batchelor.init({
	'uri':'https://www.googleapis.com/batch',
	'method':'POST',
	'auth': {
		'bearer': [... Google API Token ...]
	},
	'headers': {
		'Content-Type': 'multipart/mixed;'
	}
});
```
We can then start adding requests to our batch. This can be done 2 ways:

As a one-off object:
``` node
Batchelor.add({
	'method':'GET',
	'path':'/plusDomains/v1/people/me/activities/user'
})
```
Or an Array of objects:
``` node
Batchelor.add([
	{
		'method':'GET',
		'path':'/plusDomains/v1/people/me/activities/user'
	},
	{
		'method':'GET',
		'path':'/plusDomains/v1/people/me/circles'
	},
	{
		'method':'GET',
		'path':'/plusDomains/v1/people/me/people/circled'
	}
]);
```
Once you have added all of the requests you need, call `.run()`:
``` node
Batchelor.run(function(response){
	res.json(response);
});
```
#### POST Requests
The above examples show `GET` requests. To perform a `POST` requires a few more settings:
``` node
Batchelor.add({
	'method':'POST',
	'path':'/plusDomains/v1/people/me/activities',
	'parameters':{
		'Content-Type':'application/json;',
		'body':{'object':{'originalContent': 'A wonderful batch post!'},'access': {'items': [{'type': 'domain'}],'domainRestricted': true}}
	}
});
```
#### Callbacks
By default, all responses are returned through the callback function in the `Batchelor.run()` call. Alternatively, a callback can be supplied for each individual calls:
``` node
Batchelor.add({
	'method':'POST',
	'path':'/plusDomains/v1/people/me/activities',
	'parameters':{
		'Content-Type':'application/json;',
		'body':{'object':{'originalContent': 'Another wonderful batch post with callback!'},'access': {'items': [{'type': 'domain'}],'domainRestricted': true}}
	},
	'callback':function(response){
		console.log(response);
	}
});
```
#### Request and Response IDs
The module will assign a request a randomly generated unique `Content-ID` by default, but this can be supplied as part of the options to supply `Batchelor.add()`:
``` node
Batchelor.add({
	'method':'GET',
	'path':'/plusDomains/v1/people/me/activities/user',
	'requestId':'Batch_UniqueID_1'
})
```
#### A Little Gift
All methods return the `Batchelor` object. So you can chain calls together.

``` node
Batchelor.init({
	...
}).add([
	...
]).run(function(data){
	...
});
```
## To Do List-ish
These might get done if we end up needing them/have time:
* Limit requests per batch request
* Handle Media in API calls (no need for it here, feel free)

## Release History

* 0.0.3 More bug fixes: New contentId for every request
* 0.0.2 Bug fixes
* 0.0.1 Initial release

## Acknowledgement
Built on the clock by [@sparkyfied](https://github.com/sparkyfied) as an internal project of [wapisasa C.I.C.](http://www.wapisasa.com)
