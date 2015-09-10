# koa-repath

A [Jade](http://jade-lang.com/) middleware for [Koa](http://koajs.com/).

# How to use

```bash
npm install koa-repath --save
```

```javascript
var koa = require('koa');
var repath = require('koa-repath');

var app = koa();

repath.config({
  whitelist: ['*/*.js', '*/*.css', /^\/articles\/\w+/],
  on: true
});
// app.use(repath());

```

# Examples

```javascript
app.use(repath(/^\/blogs(\w+)/, '/blogs/$1');
// '/blogs123' ==> '/blogs/123'

app.use(repath(/^\/index/, '/v2/index/');
// '/index' ==> '/v2/index'
```

```javascript
app.use(repath(/^\/blogs(\w+)/, function (path, src, id) {
  if (+id > 123) {
    return '/v2/blogs/$1';
  } else {
    return '/blogs/$1';
  }
}));
```

```javascript
app.use(repath([{
  src: /^\/index/,
  dst: '/v2/index/'
}, {
  src: '/:src..:dst',
  dst: '/commits/:src/to/:dst'
}, {
  src: '/^\/blogs(\w+)/',
  dst: function (path, src, id) {
    if (+id > 123) return '/v2/blogs/$1';
    return '/blogs/$1';
  })
}]));
```

