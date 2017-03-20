# Teardrop [![Travis][build-badge]][build]

[build-badge]: https://img.shields.io/travis/OpenClubDev/teardrop/master.svg?style=flat-square
[build]: https://travis-ci.org/OpenClubDev/teardrop

<img src="https://github.com/OpenClubDev/teardrop/raw/master/logo/teardrop.png" height="150"/>

Declarative routing for [React](https://facebook.github.io/react).

React Router keeps your UI in sync with the URL. Make the URL your first thought, not an after-thought.

## Installation

Using [npm](https://www.npmjs.com/):

    $ npm install --save teardrop@5.0.0

Then with a module bundler like [webpack](https://webpack.github.io/), use as you would anything else:

```js
// using an ES6 transpiler, like babel
import { BrowserRouter, Match, Link, Miss } from 'teardrop'

// not using an ES6 transpiler
var BrowserRouter = require('teardrop').BrowserRouter
var Match = require('teardrop').Match
var Link = require('teardrop').Link
var Miss = require('teardrop').Miss
```

You can find the library on `window.Teardrop`.

## Docs

We're going to try and get the docs sorted out here.

## v5 FAQ

### Why rename and fork teardrop?

We weren't happy with the direction of teardrop, so we've decided to branch out and provide our own version.

### Will you do huge API overhauls?

No, we're not about that. We'll try and ensure everything is backwards compatible for as long as possible.

### Why Match, Miss instead of Route?

We like it.

### What about route transition hooks?

Transition's can be done within render of a Match.

```js
<Match render={() => {
  doStuffHere();
  return <ComponentHere />
}}/>
```

One use case was loading data and waiting to render the next screen until the data landed. With a component, you can save the previous children, render them while loading, and then render your new children when you're done. We'll have an example of this eventually.

### I liked seeing all my routes in one place, now what?

Check out the "Route Config" example.
`website/examples/RouterConfig.js`

### Example

Right now, because we're lacking pretty documentation, you'll have to use this example and the modules/__tests__ folder to figure out what's going on. Doccos are in progress. :)

#### Server Router example
```javascript:
import { ServerRouter, MatchGroup, Match, Miss, Redirect, createServerRenderContext } from 'teardrop';

const location = '/test' // Set this using your server
const routerContext = createServerRenderContext();

render() {
  return (
    <ServerRouter location={URL from server} context={routerContext}>
    {/* This component will return every time the pattern is matched - anywhere in the app */}
      <Match
        exact={true|false}
        pattern="*"
        render={({ params, location, pathname, isExact}) => {
          // params is an object containing params from your pattern
          // location is the current location object
          // pathname is the current pathname
          // isExact is true when the pattern is an "exact" match (ie. "/" === "/")
          // pattern is also returned in case you want it
          return <div />
        }}
      />
      <MatchGroup>
        // Put as many matches in here as you want, it'll only match ONE of them
        <Match pattern="hi" render={Hi} />
        <Match pattern="hello" render={Hello} />
        <Match pattern="hey" render={Hey} />
      </MatchGroup>
      { /* Simple redirect component */ }
      <Match pattern="/old-link" render={() => {
        <Redirect to="/new-link" />
        }} />
      { /* Nothing matched? We produce a Miss */ }
      <Miss render={ErrorPage} />
    </ServerRouter>
    );
}

```

#### Client Router example

```javascript:
import { BrowserRouter, MatchGroup, Match, Miss, Redirect, createServerRenderContext } from 'teardrop';

render() {
  return (
    <BrowserRouter>
    {/* This component will return every time the pattern is matched - anywhere in the app */}
      <Match
        exact={true|false}
        pattern="*"
        render={({ params, location, pathname, isExact}) => {
          // params is an object containing params from your pattern
          // location is the current location object
          // pathname is the current pathname
          // isExact is true when the pattern is an "exact" match (ie. "/" === "/")
          // pattern is also returned in case you want it
          return <div />
        }}
      />
      <MatchGroup>
        // Put as many matches in here as you want, it'll only match ONE of them
        <Match pattern="hi" render={Hi} />
        <Match pattern="hello" render={Hello} />
        <Match pattern="hey" render={Hey} />
      </MatchGroup>
      { /* Simple redirect component */ }
      <Match pattern="/old-link" render={() => {
        <Redirect to="/new-link" />
        }} />
      { /* Nothing matched? We produce a Miss */ }
      <Miss render={ErrorPage} />
    </BrowserRouter>
    );
}

```

### Changelog

**5.0.1**

 - Now sending `router` and `location` props to the Router's only child.

 **5.0.2**

 - Exporting `MatchGroup` for ease of use.
