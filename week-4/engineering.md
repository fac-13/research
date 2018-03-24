[**Our Presentetion Here**](https://docs.google.com/presentation/d/1qSAybw_4o6k1GRaUxbqkjJraFRMGAOdXiMvnvLwF3m4/edit?usp=sharing)

## Modules


### What is a module?

Modularisation is the process of refactoring code and restructuring programs so that related functions sharing common dependencies are bundled together as objects (modules) providing public/global access via an _interface_.

* Writing modular code facilitates DRY principle by emphasising _reusability_ and _portability_.
* Modular code avoids naming confilcts and aids semantic variable naming by providing namespaces within which functions are called, e.g. `numbers.add(1, 3)` and `strings.add('Hello', ' ', 'World')` - Here, the modules `numbers`and `strings`provide namespace so that we do not have to invent a contrived name for the function that adds arguments together in either case.
* Another way of putting the previous point is to say that modularisation prevents pollution of the global scope.
* As such, it provides a means of maintaining access control via the interface, which represents the _public_ face of an entity which might hold many _private_ properties or methods.


### Modules before ES6 and Node.js

While modules are a built-in feature of Node and ES6, web developers have been utilising modular patterns for decades (modules and packages are a common feature of many other languages).

As such, while we can very easily apply modular patterns and principles on the client side, there is at present no widely implemented system for _importing_ and _exporting_ those modules in the browser (tools such as _browserify_ and _webpack_ can facilitate this process).

The basic module pattern in JavaScript utilises a named IIFE which returns an object:

```js
var module = (function () {

  var privateMethod = function (message) {
    console.log(message);
  };

  var publicMethod = function (text) {
    privateMethod(text);
  };
  
  return {
    publicMethod: publicMethod
  };

})();
```

In the above example, the object returned by the module - its interface - supplies only the function `publicMethod`, while `privateMethod`remains inaccessible from the outside: a call to `module.privateMethod`fails. As such, we can use the module system to keep certain data safe either for security reasons or to maintain legibility, stucture etc. as indicated above.

### Modules in Node.js

Node.js adopts the module pattern as a standard feature. In this way, node programming emphasises reusability and separation of concerns. The node ecosystem reflects this fact, with a vast library of modules available to developers.

With node, we can break the modules into _core_, _3rd party_, and _custom_:

* Core modules are available to import by default
* 3rd Party modules must be deliberately installed in the user environment before their features can be accessed
* Custom modules are user defined

It is a general rule of node development that there may likely be a module available that implements the functionality you are seeking - the emphasis is on accessing and using what is already available in the ecosystem, since it will be well tested.

The node moudule ecosystem uses an 'import'/'export' syntax, whereby -
* when a module is a dependency it can be loaded using the `require` keyword and either the name of the module (if it is a core or installed 3rd party module) or the path to the file containing the relevant code (if it is a user defined module) as follows: `var something = require(module_name / path/to/file)`
* when a user defined module is an interface it can be exported using the `exports` keyword as follows: `module.exports = { name_of_function etc. }`

### Modules and the Browser

We can use the module pattern on the client side by writing modules as specified above and then passing them as values to other parsts of the program. 

Module functionality is now being rolled out in Safari and Chrome browsers using the `export` and `ìmport` statements.

_Browserify_ allows you to bundle dependencies for use in the browser. This means that all the 3rd party modules available inthe node ecosystem can be used in the browser environment...




## Asyncronous functions

#### SYNCHRONOUS

You are in a queue to get a movie ticket. You cannot get one until everybody in front of you gets one, and the same applies to the people queued behind you.

#### ASYNCHRONOUS

You are in a restaurant with many other people. You order your food. Other people can also order their food, they don't have to wait for your food to be cooked and served to you before they can order. In the kitchen restaurant workers are continuously cooking, serving, and taking orders. People will get their food served as soon as it is cooked.

### Research Topics

> Why should you use asyncronous forms of functions wherever possible in Node?

Node runs on a single threaded event loop and leverages asynchronous calls for doing various things, like I/O operations. When you send a database query, Node will continue executing the code that comes after it, then jump back when the result is available.

Synchronous functions make the complete node runtime process stop processing anything else during that call.

> What are error-first callbacks?
> and why is it important to follow that pattern in your own code?

Node relies on asynchronous code to stay fast, so having a dependable callback pattern is crucial. Without one, developers would be stuck maintaining different signatures and styles between each and every module. The error-first pattern was introduced into Node core to solve this very problem, and has since spread to become today’s standard. While every use-case has different requirements and responses, the error-first pattern can accommodate them all.

> Why should you avoid using `throw` in callbacks?

Exceptions are only a **synchronous mechanism**, which is logical: in an asynchronous environment, the exception could be thrown when the handler block is already out of scope and thus meaningless.

> When might you use the syncronous form of a function instead?

It's only acceptable in node command line apps and during app startup etc.




## Input/output

### PATH

```
var web = Desktop/FAC/week4//index.html
var website = Desktop/FAC/week4/index.html

path.normalize(web)       // gives Desktop/FAC/week4/index.html
path.dirname(website)     // gives Desktop/FAC/week4/
path.dirname(website)     // gives index.html
path.extname(website)     // gives html
```

### FS
```   
fs.writeFile()            // CREATE AND UPDATE FILES
fs.writeFileSync()
fs.appendFile() 


fs.readFile()                // READ FILES


fs.rename()                // RENAME FILES 


fs.unlink()                // DELETE FILES

```





## Working with URLs
- ### **_querystring_**
```javascript
const querystring = require('querystring');
```


#### ```querystring.parse(str[, sep[, eq[, options]]])```

- sep: The substring used to delimit key and value pairs in the query string. Defaults to `'&'`

- eq: The substring used to delimit keys and values in the query string. Defaults to `'='`.
```javascript=
querystring.parse('foo=bar&abc=xyz&abc=123')    //{foo: 'bar', abc: ['xyz', '123']}
```

#### ```querystring.stringify(obj[, sep[, eq[, options]]])```



```javascript
querystring.stringify({ foo: 'bar', baz: ['qux', 'quux'], corge: '' });
// returns 'foo=bar&baz=qux&baz=quux&corge='

querystring.stringify({ foo: 'bar', baz: 'qux' }, ';', ':');
// returns 'foo:bar;baz:qux'
```


#### ```querystring.escape(str)```
#### ```querystring.unescape(str)```

---

- ### **_url_**
```javascript
const url = require('url');
````

`href:'http://user:pass@host.com:8080/p/a/t/h?query=string#hash'`
|protocol|auth|hostname|port|pathname|search|path|query|hash|
|:------:|:--:|:------:|:--:|:------:|:---:|:---:|:---:|:--:|
|http|user:pass|host.com|8080|/p/a/t/h|?query=string|/p/a/t/h?query=string|query=string|#hash|
    
    

#### ```url.parse(urlStr, [parseQueryString], [slashesDenoteHost])```
- `url.parse(urlStr, true, [slashesDenoteHost])` : parse the query string using the `querystring` module
- `url.parse(urlStr, [parseQueryString], true)` : 
    - `//foo/bar` as `{ host: 'foo', pathname: '/bar' }`
- `url.parse(urlStr, [parseQueryString], false)` : 
    - `//foo/bar` as `{ pathname: '//foo/bar' }`
    ---
#### ```url.format(urlObj)```
Take a parsed URL object, and return a formatted URL string.

-   `href` will be ignored.
-   `protocol` is treated the same with or without the trailing `:` (colon).
    -   The protocols `http`, `https`, `ftp`, `gopher`, `file` will be postfixed with `://` (colon-slash-slash).
    -   All other protocols `mailto`, `xmpp`, `aim`, `sftp`, `foo`, etc will be postfixed with `:` (colon)
-   `auth` will be used if present.
-   `hostname` will only be used if `host` is absent.
-   `port` will only be used if `host` is absent.
-   `host` will be used in place of `hostname` and `port`
-   `pathname` is treated the same with or without the leading `/` (slash)
-   `search` will be used in place of `query`
-   `query` (object; see `querystring`) will only be used if `search` is absent.
-   `search` is treated the same with or without the leading `?` (question mark)
-   `hash` is treated the same with or without the leading `#` (pound sign, anchor)
   
#### `url.resolve(from, to)`

```js
url.resolve('/one/two/three', 'four')         // '/one/two/four'
url.resolve('http://example.com/', '/one')    // 'http://example.com/one'
url.resolve('http://example.com/one', '/two') // 'http://example.com/two'
```