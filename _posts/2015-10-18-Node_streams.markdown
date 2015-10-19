---
layout: post
title:  "Node Streams"
date:   2015-10-18 19:14:52
categories: Node
image:
image-subtitle:
---

Streams are a way of inputting and outputting data. They can be readable or writable (or both) and can be paused and resumed.

Using the filesystem module we just saw, we can create a stream to read data as below. The data is read in in chunks, so the readStream on data event event is fired multiple times. A counter has been implemented which increments each time another chunk of data is received.

    var fs = require('fs');

    var readStream = fs.createReadStream('lorem.txt');
    readStream.setEncoding('utf8');

    var counter = 0;

    readStream.on('data', function(myData) {
      counter ++;
    });

    readStream.on('end', function() {
      console.log(counter);
    });

Another event listener is listening for a signal that the data has finished being read, and at this point it outputs the value of the counter variable to the console. You can therefore see how many chunks the data was streamed in.

Write streams can be created as below

    var writeStream = fs.createWriteStream('copylorem.txt');

    writeStream.write('Hello world');

Some other useful commands you might like to use with streaming are

    readStream.pipe(writeStream); // to pipe the data read in straight into the write stream

    readStream.pause(); // pauses the reading of the stream

Nat x
