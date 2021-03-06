# libp2p chat in the browser

This is a TypeScript example of using [libp2p][libp2p] in a browser for
bidirectional communication.

This example leverages the [Parcel.js bundler][parcel] to compile
and serve the libp2p chat in the browser. Parcel uses
[Babel][babel] to handle transpilation of the code. You can use
other bundlers such as Webpack or Browserify, but we will not be covering them
here.

Some interesting discussion and background on this example can be found in [it's
pull request][pr].

## ARCHIVED

This repository has been archived, because it was intended as a one-time
example, not something to be maintained over time. There will be more and more
security issues in the transitive dependencies, so if you want to use this code
please make sure that you bring everything up to date.

## Setup

In order to run the example, first install the dependencies from same directory
as this README:

```sh
cd ./examples/chat-in-the-browser
npm install
```

## Signaling Server

This example uses the `libp2p-webrtc-star` module, which enables libp2p browser
nodes to establish direct connections to one another via a central signaling
server. For this example, we are using the signaling server that ships with
`libp2p-webrtc-star`.

You can start the server by running `npm run server`. This will start a
signaling server locally on port `9090`. If you'd like to run a signaling server
outside of this example, you can see instructions on how to do so in the
[`libp2p-webrtc-star` README][webstar].

When you run the server, you should see output that looks something like this:

```log
$ npm run server

> libp2p-in-browser@1.0.0 server
> star-signal

Listening on: http://0.0.0.0:9090
```

## Running the examples

Once you have started the signaling server, you can run the Parcel server.

```sh
npm start
```

The output should look something like this:

```log
$ npm start

> chat-in-the-browser@1.0.0 start
> parcel index.html

Server running at http://localhost:1234
✨  Built in 1000ms.
```

This will compile the code and start a server listening on port
[http://localhost:1234](http://localhost:1234). Now open your browser to
`http://localhost:1234`. You should see a log of your node's Peer ID, the
discovered peers from the Bootstrap module, and connections to those peers as
they are created.

Now, if you open a second browser tab to `http://localhost:1234`, you should
discover your node from the previous tab. This is due to the fact that the
`libp2p-webrtc-star` transport also acts as a Peer Discovery interface. Your
node will be notified of any peer that connects to the same signaling server you
are connected to. Once libp2p discovers this new peer, it will attempt to
establish a direct WebRTC connection.

**Note**: In the example we assign libp2p to `window.libp2p`, in case you would
like to play around with the API directly in the browser. You can of course make
changes to `index.js` and Parcel will automatically rebuild and reload the
browser tabs.

**Note**: During startup, the example raises exceptions "protocol selection
failed". This is normal. The reason is that the example tries to chat with the
bootstrap servers, but they don't support the chat protocol.

## TypeScript

This example also shows how to use Libp2p using TypeScript. Libp2p itself
currently does not support TypeScript, but this example includes a few minimal
type declarations so that it can be used, even with strict compilation.

[libp2p]: https://github.com/libp2p/js-libp2p
[parcel]: https://parceljs.org/
[babel]: https://babeljs.io/
[pr]: https://github.com/libp2p/js-libp2p/pull/616
[webstar]: https://github.com/libp2p/js-libp2p-webrtc-star
