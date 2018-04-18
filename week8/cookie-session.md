# Express Middleware
Middleware functions are functions that have access to the request object (req), the response object (res), and the next middleware function in the application’s request-response cycle. 

Middleware functions can perform the following tasks:
- Execute any code.
- Make changes to the request and the response objects.
- End the request-response cycle.
- Call the next middleware function in the stack.

> The idea behind it is “wrapping” your application logic with additional request processing logic, and then chaining as much of those wrappers as you like. So when your server receives a request, it would be first processed by your middlewares, and then after you generate a response it will also be processed by the same set:
![image of middleware](http://dracony.org/wp-content/uploads/2015/03/middleware.png)


The only difference between a route handler and middleware is that middleware calls `next` - which is the next middleware function. 

**Important**
If the current middleware function does not end the request-response cycle, it must call next() to pass control to the next middleware function. Otherwise, the request will be left **hanging**.
Also this means the order of middleware and route handlers is important.


### Using cookie-session to track the number of views a page has
[npm page for cookie-session](https://www.npmjs.com/package/cookie-session)

This sets up a signed cookie using cookie-session which is a form of middleware. 

_A user session can be stored in two main ways with cookies: on the server or on the client. This module stores the session data on the client within a cookie, while a module like express-session stores only a session identifier on the client within a cookie and stores the session data on the server, typically in a database._



The user is logged in when they visit /login, they are logged out when they visit /logout. They can only view the content on the views page when they are logged in. 


When telling the app to use cookie-session we supply and object with key-value pairs specified in the cookie-session docs. 
```js
// setting up
const express = require('express');
const app = express();
const cookieSession = require('cookie-session');

// Use cookie session - an object is passed into cookie session as per the cookie session docs.
// Name - the name that you want to call your cookie. The default is session
// Secret is what you would normally store in your env, its used to sign the cookie
app.use(cookieSession({
    name: 'session',
    secret: 'orange'
}));

// When the user visits login we add a cookie to the req 
// req.session.views is just adding a cookie called session and adding a views property to it
// You can add anything to the cookie
// We also add username 
app.get('/login', (req, res) => {
    req.session.views = (req.session.views || 0) + 1;
    req.session.username = 'helen';
    res.send('You are logged in');
});

// When visitign views the content is only displayed if a cookie exists otherwise the user is told to log in 
// We use req.session.length > 0 to check for the cookie as when we logged our session cookie it was an empty object session{} and so had a length of 0
app.get('/views', (req, res) => {
    if (req.session.length > 0) {
        return res.send(`N of views ${req.session.views}, and out name is ${req.session.username}`);
    } else {
        res.send('Acess denied, Go log in!!!');
    }
});

// To log the user out and delete the cookie we set the value of the cookie to null
app.get('/logout', (req, res) => {
    req.session = null;
    res.send('You are now logged out')
})

// Handles the user visiting pages that don't exist
app.use((req, res) => {
    res.send('Oops couldn\'t find that page ...404');

})

// Handling a server error
app.use((err, req, res, next) => {
    res.send(`Ooops error ...500!! Your error is ${err.message}`);
})

app.listen(3000, () => {
    console.log('app running on 3000');
})
```

### Resources 
https://hackernoon.com/middleware-the-core-of-node-js-apps-ab01fee39200
https://expressjs.com/en/guide/using-middleware.html
