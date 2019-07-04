# http.client
An HTTP client wrapped around Axios

## Example

```js
var requests = [
	httpClient.get('/api/v1/endpoint1'),
	httpClient.get('/api/v1/endpoint2')
];

httpClient.all(requests)
.then(httpClient.spread(function (response1, response2) {
	console.log(response1);
	console.log(response2);
}))
.catch(function (error) {
	console.log(error);
	cancelRequests();
});

function cancelRequests() {
	requests.forEach(function (request) {
		request.cancel();
	});
}
```

---------------------------------------------------------------------------------

```js
var request = httpClient.get/delete/head/options(
	'/api/v1/endpoint',
	{
		parmas: {
			id: '1234567890'
		}
	}
)
.then(function (response) {
	console.log(response);
})
.catch(function (error) {
	console.log(error);
});

// can be cancelled before request finishes
request.cancel();
```

---------------------------------------------------------------------------------

```js
var request = httpClient.post/put/patch(
	'/api/v1/endpoint',
	new URLSearchParams(new FormData(document.getElementById('my-form-id'))),
	{
		params: {
			id: '1234567890'
		}
	}
)
.then(function (response) {
	console.log(response);
})
.catch(function (error) {
	console.log(error);
});

// can be cancelled before request finishes
request.cancel();
```

---------------------------------------------------------------------------------

```js
var polling = httpClient.poll(
	'/api/v1/endpoint',
	{
		method: 'get', // method; get or post. get is default
		minTimeout: 1000, // starting value for the timeout in milliseconds
		maxTimeout: 64000, // maximum length of time between requests
		multiplier: 2, // if set to 2, timerInterval will double each time the response hasn't changed (up to maxTimeout)
		maxCalls: 0, // maximum number of calls. 0 = no limit
		autoStop: 0, // automatically stop requests after this many returns of the same data. 0 = disabled
		runAtOnce: true, // whether to fire initially or wait
		data: null // hash of values to be passed to the page - e.g. { name: "John", greeting: "hello" } or function or null
	},
	function (response) {
		console.log(response);
		// to cancel polling
		polling.cancel();
	},
	function (error) {
		console.log(error);
		// to cancel polling
		polling.cancel();
	}
);

// can be cancelled
polling.cancel();
```

### Request method aliases

##### httpClient.poll(url, config, successCallback, failureCallback)
##### httpClient.request(config)
##### httpClient.get(url[, config])
##### httpClient.delete(url[, config])
##### httpClient.head(url[, config])
##### httpClient.options(url[, config])
##### httpClient.post(url[, data[, config]])
##### httpClient.put(url[, data[, config]])
##### httpClient.patch(url[, data[, config]])

###### NOTE
When using the alias methods `url`, `method`, and `data` properties don't need to be specified in config.

## Request config
It's same like at [axios config](https://github.com/axios/axios/blob/master/README.md#request-config) less at `poll`

#### Config for polling (`poll`)
```js
{
	method: 'get',     // method; get or post
	data: null,        // hash of values to be passed to the page - e.g. { name: "John", greeting: "hello" } or function
	minTimeout: 1000,  // starting value for the timeout in milliseconds
	maxTimeout: 60000, // maximum length of time between requests
	multiplier: 2,     // if set to 2, timerInterval will double each time the response hasn't changed (up to maxTimeout)
	maxCalls: 0,       // maximum number of calls. 0 = no limit.
	autoStop: 0,       // automatically stop requests after this many returns of the same data. 0 = disabled
	runAtOnce: false   // whether to fire initially or wait
}
```
