# ChuckT client

ChuckT is a client (browser) JavaScript library for triggering and/or listening
to events over the [SockJS](https://github.com/sockjs/sockjs-client) websocket
API. This client is intended to be used in conjunction with a server-side
implementation:

 * [chuckt-node](https://github.com/epixa/chuckt-node)

ChuckT is inspired in part by both the socket-io event system as well as the core
EventEmitter in node.js.

# Usage:

### Initialization

Include the chuckjs client library:

`<script src="chuckt.js"></script>`

Make your SockJS socket communicate in chuckt:

```javascript
<script>
  var sock = new SockJS('http://example.com/socket');
  var chuckt = epixa.chucktify(sock);
</script>
```

If you intend to receive non-chuckt messages through your socket, then
instantiate ChuckT directly:

```javascript
<script>
  var sock = new SockJS('http://example.com/socket');
  var chuckt = new epixa.ChuckT(sock);
  sock.onmessage = function(e) {
    chuckt.process(e.data); // will ignore the message if not chuckt
    // .. your own message handling here
  };
</script>
```

### Listen to events fired from the backend

```javascript
<script>
  chuckt.on('some-event', function(foo) {
    console.log(foo);
  });
</script>
```

Note: A backend event may pass any number of arguments to frontend listeners.

### Emit an event to the backend

You can pass any number of arguments as well as a callback function:

```javascript
<script>
  chuckt.emit('another-event', 'arg1', 'arg2', function() {
    console.log('backend acknowledged event receipt');
  });
</script>
```

You can also emit events without any arguments at all:

```javascript
<script>
  chuckt.emit('another-event-2', function() {
    console.log('backend acknowledged event receipt');
  });
</script>
```

The backend can pass arguments to the callback too:

```javascript
<script>
  chuckt.emit('another-event-3', function(foo, bar) {
    console.log(foo, bar); // will exist if the backend passes them
  });
</script>
```

Of course you can also just fire off an event and forget it:

```javascript
<script>
  chuckt.emit('another-event-4');
</script>
```
