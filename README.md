Caronte
=======

Caronte is an HTTP programmable proxying library that supports 
websockets. It is suitable for implementing components such as
proxies and load balancers.

### Core Concept

A new proxy is created by calling `createProxyServer` and passing
an `options` object as argument ([valid properties are available here](https://github.com/yawnt/caronte/blob/master/lib/caronte.js#L26-L39)) 

```javascript
var caronte = require('caronte');

var proxy = caronte.createProxyServer(options);
```

An object will be returned with four values:

* web `req, res, [options]` (used for proxying regular HTTP(S) requests)
* ws `req, socket, head, [options]` (used for proxying WS(S) requests)
* ee (an EventEmitter2 that emits events, you can hook into them to customize behaviour)
* listen `port` (a function that wraps the object in a webserver, for your convenience)

Is it then possible to proxy requests by calling these functions

```javascript
require('http').createServer(function(req, res) {
  proxy.web(req, res, { target: 'http://mytarget.com:8080' });
});
```

When a request is proxied it follows two different pipelines ([available here](https://github.com/yawnt/caronte/tree/master/lib/caronte/passes))
which apply trasformations to both the `req` and `res` object. 
The first pipeline (ingoing) is responsible for the creation and manipulation of the stream that connects your client to the target.
The second pipeline (outgoing) is responsible for the creation and manipulation of the stream that, from your target, returns datas 
to the client.

You can easily add a `pass` (stages) into both the pipelines (XXX: ADD API).

In addition, every stage emits a corresponding event so introspection during the process is always available.

### Contributing and Issues

* Search on Google/Github 
* If you can't find anything, open an issue 
* If you feel comfortable about fixing the issue, fork the repo
* Commit to your local branch (which must be different from `master`)
* Submit your Pull Request (be sure to include tests and update documentation)

### Test

```
$ npm test
```

### License

>The MIT License (MIT)
>
>Copyright (c) 2013 Nodejitsu Inc.
>
>Permission is hereby granted, free of charge, to any person obtaining a copy
>of this software and associated documentation files (the "Software"), to deal
>in the Software without restriction, including without limitation the rights
>to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
>copies of the Software, and to permit persons to whom the Software is
>furnished to do so, subject to the following conditions:
>
>The above copyright notice and this permission notice shall be included in
>all copies or substantial portions of the Software.
>
>THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
>IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
>FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
>AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
>LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
>OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
>THE SOFTWARE.

