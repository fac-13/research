## [Promises](https://www.youtube.com/watch?v=drvS9w-lTMc) and Fetch


![Promises](https://media.giphy.com/media/gx49Zi2dUyjmg/giphy.gif)

### pg-promise: using promises with DB queries

The [Peppr app](https://github.com/fac-12/peppr/blob/master/server/controllers/queries.js) uses promises when doing DB queries, such as:
```jsx=
const getUser = email => {
  return db.query('SELECT * FROM users WHERE email = $1', [email])
  .then(user => user[0])
  .catch(console.log)
}
```

This is possible without constructing a `new Promise` because the module [pg-promise](https://www.npmjs.com/package/pg-promise) has been required in their [db_connection.js file](https://github.com/fac-12/peppr/blob/master/server/database/db_connections.js). It allows us to **handle the result from a DB query** once it has finished, without having to use callbacks.

The Peppr app has a good example of connection promises together in their [auth.js file](https://github.com/fac-12/peppr/blob/master/server/controllers/auth.js):
```jsx=
//this requires the DB queries like getUser above
const queries = require('./queries'); 

queries
  .getUser(email)
  .then(user => {
    //a new promise is created
    return new Promise((resolve, reject) => {
      if(user){
        res.status(422).send({ error: 'Email is in use. Please login'});
          //the user already exists, so an error is thrown using reject
        reject('Email is in use. Please sign in');
      } 
        //the user doesn't exist yet, so their password is hashed
      else resolve(hashPassword(password));
    })
  })
  .then(hash => {
    return queries.addUser(name, email, hash)
  })
  .then (user => {
    //as this is the last then, it doesn't need to be asynchronous
    res.json({ token: userToken(user)});
  })
  .catch(console.log)
}
```


### request-promise module
To turn get requests (e.g. an external API call) into a promise easily, there is the [request-promise module](https://github.com/request/request-promise). 

```jsx=
const rp = require('request-promise');

rp('https://pokeapi.co/api/v2/pokemon/pikachu/')
    .then(function (htmlString) {
        // Process html...
    })
    .catch(function (err) {
        // Crawling failed...
    });
```
This also means you won't need to chunk-ify/parse the body. It comes back fully-formed and ready to be handled.



### Use promise in frontend 

Since it is not supportes by all browser, use [promise-polyfill](https://www.npmjs.com/package/promise-polyfill)


### Example of using promises in testing

>db_build.js:
```jsx=
const runDbBuild = () => {
  return db.any(build)
    .then(res => console.log('res', res))
    .catch(e => console.error('error', e));
};
```

>test:

```jsx=
test('check parent exists', (t) => {
  runDbBuild().then(() => {
    const email = 'k@a.com';
    return checkParent(email)
  }).then((queryRes) => {
    t.equal(queryRes[0].case, true, 'If parent exists then check_parent should return true- promise');
    t.end();
  }).catch((err) => {
    throw err;
  })
})
```
