# koa-repath

A `rewrite` middleware for [Koa](http://koajs.com/).

# How to install

```bash
npm install koa-repath --save
```

# How to test
```bash
npm run test
```


# How to use

```javascript
var koa = require('koa');
var repath = require('koa-repath');

var app = koa();

// config repath globally.
repath.config({
  whitelist: ['*/*.js', '*/*.css', /^\/articles\/\w+/],
  on: true
});
// app.use(repath());

```


Example 1:

```javascript
app.use(repath(/^\/blogs(\w+)/, '/blogs/$1'));
// '/blogs123' ==> '/blogs/123'

app.use(repath(/^\/index/, '/v2/index/'));
// '/index' ==> '/v2/index'
```


Example 2

```javascript
app.use(repath(/^\/blogs(\w+)/, function (path, src, id) {
  if (+id > 123) {
    return '/v2/blogs/$1';
  } else {
    return '/blogs/$1';
  }
}));

// '/blogs124' ==> '/v2/blogs/124'
// '/blogs120' ==> '/blogs/120'
```

Example-3

```javascript
app.use(repath([{
  src: /^\/index/,
  dst: '/v2/index/'
}, {
  src: '/:src..:dst',
  dst: '/commits/:src/to/:dst'
  // dst: '/commits/$1/to/$2'
}, {
  src: '/^\/blogs(\w+)/',
  dst: function (path, src, id) {
    if (+id > 123) return '/v2/blogs/$1';
    return '/blogs/$1';
  })
}]));

// '/index' ==> '/v2/index'
// '/foo..bar' ==> '/commits/foo/to/bar'
```


# Options

- `whitelist`: white list which will be bypassed; default `[]`
- `on`: is turned rewrite; default: `true`



