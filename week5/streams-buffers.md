# Streams
Streams are collections of data — just like arrays or strings. The difference is that streams might not be available all at once, and they don’t have to fit in memory.

There are four fundamental stream types in Node.js: Readable, Writable, Duplex, and Transform streams: 

-   [Readable](https://nodejs.org/api/stream.html#stream_class_stream_readable) \- streams from which data can be read (for example [`fs.createReadStream()`](https://nodejs.org/api/fs.html#fs_fs_createreadstream_path_options)).
-   [Writable](https://nodejs.org/api/stream.html#stream_class_stream_writable) \- streams to which data can be written (for example [`fs.createWriteStream()`](https://nodejs.org/api/fs.html#fs_fs_createwritestream_path_options)).
-   [Duplex](https://nodejs.org/api/stream.html#stream_class_stream_duplex) \- streams that are both Readable and Writable (for example [`net.Socket`](https://nodejs.org/api/net.html#net_class_net_socket)).
-   [Transform](https://nodejs.org/api/stream.html#stream_class_stream_transform) \- Duplex streams that can modify or transform the data as it is written and read (for example [`zlib.createDeflate()`](https://nodejs.org/api/zlib.html#zlib_zlib_createdeflate_options)).

#### Reading from a stream
```
var fs = require('fs');
var readableStream = fs.createReadStream('file.txt');
var data = '';

readableStream.on('data', function(chunk) {
    data+=chunk;
});

readableStream.on('end', function() {
    console.log(data);
});
```

By default the data you read from a stream is a `Buffer` object. If you are reading strings this may not be suitable for you. So, you can set encoding on the stream by calling `Readable.setEncoding()` To read a string set the encoding to UTF8

### Writing 
To write data to a writable stream you need to call `write()` on the stream instance.


```javascript
var fs = require('fs');
var readableStream = fs.createReadStream('file1.txt');
var writableStream = fs.createWriteStream('file2.txt');

readableStream.setEncoding('utf8');

readableStream.on('data', function(chunk) {
    writableStream.write(chunk);
});
```




The `pipe()` function reads data from a readable stream as it becomes available and writes it to a destination writable stream.

The snippet below makes use of the `pipe()` function to write the content of `file1`to `file2`


```javascript
var fs = require('fs');
var readableStream = fs.createReadStream('file1.txt');
var writableStream = fs.createWriteStream('file2.txt');

readableStream.pipe(writableStream);
```

`pipe()` returns the destination stream. So, you can use this to chain multiple streams together







# Buffers
The `Buffer` class is a global within Node.js
    * So wouldn't need to use `require('buffer').Buffer`
* The buffers module comes with different methods etc that can be used for reading or manipulating streams of binary data.
* Can use `Buffer.alloc(10)` - allocate a Buffer of length 10 of 0s
* Can pass in strings, and will turn it into a string of binary data
    * E.g. `var buffer = Buffer.from('hello') = <Buffer 68 65 6c 6c 6f>`
    * To turn it back use `toString()` - e.g. `buffer.toString() = 'hello'`
* Can also pass in array 
    * E.g. `var buff = Buffer.from([1, 2]); = <Buffer 01 02>`
    * To turn it back use `toJSON()` - e.g. `buff.toJSON() = { type: 'Buffer', data: [ 1, 2 ] }`
    * And then use `buff.toJSON().data = [1, 2]`
* But not only strings, you can convert other things: computers have specified rules on how images and videos should be converted or encoded and stored in binaries.

## Buffers in relation to Streams
* We’ve seen that a stream of data is the movement of data from one point to the other
* buffer is a small physical location in your computer, usually in the RAM, where data are temporally gathered, wait, and are eventually sent out for processing during streaming.
* We can think of the whole stream and buffer process as a bus station. In some bus stations, a bus is not allowed to depart until a certain amount of passengers arrive or until a specific departure time. Also, the passengers may arrive at different times with different speed.
    * So with Node.js - it can’t control the speed or time of data arrival, the speed of the stream. It only can decide when it’s time to send out the data. If it’s not yet time, Node.js will put them in the buffer — the “waiting area” — a small location in the RAM, until it’s time to send them out for processing.

* A typical example where you could see buffer in action is when you’re streaming a video online. If your internet connection is fast enough, the speed of the stream will be fast enough to instantly fill up the buffer and send it out for processing, then fill another one, and send it out, then another, and yet another… till the stream is finished.

## Interacting with buffers
* While in the buffer, we can manipulate or interact with the binary data being streamed. The Buffer implementation in Node.js provides us with a whole list of what is doable (as mentioned quickly before).

## _extra:_ Character Encoding
* Just as there are rules that define what number should represent a character, there are also rules that define **how **that number should be represented in binaries. Specifically, **how many bits** to use to represent the number. This is called **Character Encoding**.
* One of the definitions for Character Encoding is the **UTF-8**. UTF-8 states that characters should be encoded in **bytes. **A byte is a set of eight bits — eight 1s and 0s. So eight 1s and 0s should be used to represent the Code Point of any character in binary.

# Resources
## Streams
https://medium.freecodecamp.org/node-js-streams-everything-you-need-to-know-c9141306be93
https://www.sitepoint.com/basics-node-js-streams/

## Buffers
https://nodejs.org/api/buffer.html
https://www.w3schools.com/nodejs/ref_buffer.asp
https://medium.freecodecamp.org/do-you-want-a-better-understanding-of-buffer-in-node-js-check-this-out-2e29de2968e8