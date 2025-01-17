# URL Parameters

So far we have created a basic app with a simple `GET` request defined which sends a string back as response.

What we are going to do now is accept parameters from the URL.

## Read URL parameters

Most of the time, we'll use segments in the path section of the URL to modify how our application works.

In `server.js` we are going to start by adding an array of fruits as this is an app about fruits.

```javascript
const express = require('express');
const app = express();

const fruits = ['apple', 'banana', 'pear'];

app.get('/fruits/:index', (req, res) => {
    res.send(fruits[req.params.index]);
});

app.listen(3000,() => {
    console.log('listening');
});
```

Now visit [http://localhost:3000/fruits/0](http://localhost:3000/fruits/0) to see what you get.

## Place routes in correct order

- Express starts at the top of your server.js file and attempts to match the url being used by the browser with routes in the order in which they're defined
- URL params (e.g. :foo, :example, :index) can be anything, a number, or even a string
    - Occasionally, a less specific route with a URL param will catch something that is supposed to go to something more specific

```javascript
const express = require('express');
const app = express();

const fruits = ['apple', 'banana', 'pear'];

app.get('/fruits/:index', (req, res) => { //:index can be anything, even awesome
    res.send(fruits[req.params.index]);
});

app.get('/fruits/awesome', (req, res) => { //this will never be reached
    res.send("Fruits are awesome!");
});

app.listen(3000,() => {
    console.log('listening');
});
```

If this happens, reorder them so that more specific routes come before less specific routes (those with params in them)

```javascript
const express = require('express');
const app = express();

const fruits = ['apple', 'banana', 'pear'];

app.get('/fruits/awesome', (req, res) => {
    res.send("Fruits are awesome!");
});

app.get('/fruits/:index', (req, res) => {
    res.send(fruits[req.params.index]);
});

app.listen(3000,() => {
    console.log('listening');
});
```

## Query Parameters

Just like you can send one parameter after each `/` like here `/fruits/:index`. The query parameters are sent as key-value pair. The query parameters are passed at the end of the URL after a question mark `?` to sort, filter or paginate the resource.

For example, get the accounts which are in active state and limit it by 50 accounts

```js
/accounts?status=active&limit=50
```

However, best practice for RESTful API design is that path params are used to identify a specific resource or resources, while query parameters are used to sort/filter those resources.

For example, let's create a new route that accepts a query parameter to send number of fruit available. Make the below change in `server.js`

```js
app.get('/fruits/count', (req, res) => {
    let fruit = req.query.fruit

    let count = fruits.filter((val) => {
        return val === fruit
    }).length;

    res.send(`The number of available ${fruit}/s are ${count}`);
})
```

Notice the way to grab a query parameter in express is `req.query` which is an object with key-value pairs.

And our request would look like[http://localhost:3000/fruits/count/?fruit=apple](http://localhost:3000/fruits/count/?fruit=apple)
