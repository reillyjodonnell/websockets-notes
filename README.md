# websockets

<p align="center">
  <a href="#what-are-websockets">What are websockets</a> &nbsp;&bull;&nbsp;
  <a href="#how-do-they-work">How do they work</a> &nbsp;&bull;&nbsp;
  <a href="#how-to-use-them">How to use them</a> &nbsp;&bull;&nbsp;
</p>
<br>

## What are websockets
Allow bi-directional connection between cilent and server that stays open until either closes it. I.e. server can send stuff to client without being requested from client.
- *bi-directional*: client/ server communicate between each other
- *full-duplex*: can send and receive data simultaneously
- *persistent*: connection stays open until server or client closes it

## How do they work
Client requests to upgrade to websockets when sending HTTP request. Looks like this:
```
GET /socket HTTP/1.1
Host: localhost:3000
Connection: Upgrade
Upgrade: websocket
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
```

## How to use them

### Bun
Bun abstracts this upgrade with a nice `upgrade` method:
```ts
Bun.serve({
  fetch(req, server) {
    // upgrade the request to a WebSocket
    if (server.upgrade(req)) {
      return; // do not return a Response
    }
    return new Response("Upgrade failed :(", { status: 500 });
  },
  websocket: {}, // handlers
});

```

### Node
`ws` lib

```ts
import * as WebSocket from 'ws';

const server = new WebSocket.Server({ port: PORT });

server.on('connection', ws => {
    ws.on('message', message => {
        console.log(`Received message => ${message}`);
    });
    ws.send('Hello! You are connected.');
});
```


### Zig
Way more fucking work lmao
