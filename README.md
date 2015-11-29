# request-progress [![Build Status](https://secure.travis-ci.org/IndigoUnited/node-request-progress.png)](http://travis-ci.org/IndigoUnited/node-request-progress.png)

Tracks the download progress of a request made with [request](https://github.com/mikeal/request).


## Installation

`$ npm install request-progress`


## Usage

```js
var fs = require('fs');
var request = require('request');
var progress = require('request-progress');

// Note that the options argument is optional
progress(request('http://google.com/doodle.png'), {
    throttle: 2000,                    // Throttle the progress event to 2000ms, defaults to 1000ms
    delay: 1000,                       // Only start to emit after 1000ms delay, defaults to 0ms
    lengthHeader: 'x-transfer-length'  // Length header to use, defaults to content-length
})
.on('progress', function (state) {
    console.log('received size in bytes', state.received);
    // The properties bellow can be null if response does not contain
    // the content-length header
    console.log('total size in bytes', state.total);
    console.log('percent', state.percent);
    console.log('eta', state.eta);
})
.on('error', function (err) {
    // Do something with err
})
.pipe(fs.createWriteStream('doodle.png'));
```

Note that the `state` object emitted in the `progress` event is reused to avoid creating a new object for each event.   
If you wish to peek the `state` object at any time, it is available in `request.progressState`.


## License

Released under the [MIT License](http://www.opensource.org/licenses/mit-license.php).
