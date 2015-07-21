# jsrouter

`jsrouter` is a minimal client side router based on `window.location.hash`. It utilizes [route-recognizer](https://github.com/tildeio/route-recognizer) to match routes and leans on this library for the API as well. For this reason, you will notice that the API is very similar to [Ember](http://emberjs.com/)'s [router](https://github.com/tildeio/router.js), though more minimal and with less lifecycle states.

## Installation

`npm install jsrouter`

## Usage

This is to provide a good idea of what the API looks like and what `jsrouter` can do. Check out the documentation for more details as well as an in depth breakdown of the API. (COMING SOON)

```js
import Router from 'jsrouter';

// create a new router
var router = new Router();

// define routes
router.map((match) => {
  match('/').to('home'); // where "/" is the route and "home" is the handler name
  match('/signup').to('createAccount');
  match('/signin').to('login');
  match('/profile/:id').to('profile'); // where id is a dynamic route segment
  match('/parent').to('parent', function(match) { // nested routes
    match('/child').to('child', function(match) { // matches /parent/child and calls both handlers
      match('/*splat').to('everythingElse'); // "*splat" will match everything
    });
  });
});

// define handlers
router.addHandler('home', {
  // enter/leave called when route is entered and left
  enter: function({path, lastPath, queryParams, params}) {
    // path === current path
    // lastPath === last path
    // queryParams === query parameters
    // params === dynamic matched segments or splats
  },
  leave: function({path, nextPath, queryParams, params}) {
  }
});
// etc...

// using the router
router.navigate('/');
router.navigate('/signup', {promo: 'test'}); // optionally takes data to store in window.history.replaceState
router.navigate('/profile/1');

router.back(); // window.history.back();
router.forward(); // window.history.forward();
```

## Configuration

The router takes an optional object as an argument that allows it to override default configuration and customize the behavior of the router.

```js
var router = new Router({
  unrecognizedRouteHandler: function() {
    this.navigate('/notFound');
  },
  // ...
  // etc.
});
```