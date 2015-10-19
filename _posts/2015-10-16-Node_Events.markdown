---
layout: post
title:  "Node Events"
date:   2015-10-16 17:31:45
categories: Node
image:
image-subtitle:
---

Callbacks are great, but when there are more than two events and/or they need to be called multiple times, events are used. They're more flexible and more powerful... Plus you can kind of switch them on and off as you please.

Instead of using asynchronous functions which execute upon the return of a result, a number of event listeners are set up to watch for certain signals or state changes. These listeners are also known as Observers. Node.js has a module called 'events' which needs to be required in order for this all to work.

Here's an example of event handlers and emitters:

    var events = require('events');
    var eventEmitter = new events.EventEmitter();

    var myFirstEventHandler = function first() {
      console.log('First event handler fired');
    }

    eventEmitter.on('firstEvent', myFirstEventHandler);

    eventEmitter.emit('firstEvent');

Run that with node and see what happens...

Also, check out what gets returned here if you type each line into a REPL environment:

    > eventEmitter.on('firstEvent', myFirstEventHandler);

    { domain: null,
      _events: { firstEvent: [Function: first] },
      _maxListeners: 10 }

The event emitter is returned, so this can be used for chaining calls together.

Event listeners can be added and removed from the array of event listeners using methods such as ``eventEmitter.addListener(event, listener)``.

Here's an example of removeAllListeners in use.

    if(counter === 15) {
      readStream.removeAllListeners('data');
    }

For a full list please see the <a href="https://nodejs.org/api/events.html">docs</a>.

Next up, streams!

Nat x
