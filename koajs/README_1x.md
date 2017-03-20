Koajs, from the creator of expressjs, consice, small, modular and faster and also, has very incredibly **CONFUSING DOCUMENTATION** that take you nowhwere.

This readme help me focus on Koajs, main components to take firs step with koajs

Eco-system
-------


- Router:  [koa-router](https://github.com/alexmingoia/koa-router)
- Templating:[nunjucks](https://mozilla.github.io/nunjucks/templating.html)
- Usage?  https://github.com/koajs/examples/blob/master/blog/lib/render.js
- [koa-static](https://github.com/koajs/static) to serve static files


Setup
------

    npm install --save webpack koa koa-router co co-views co-body koa-logger koa-static

``` javascript
→ cat index.js
/**
 * Module dependencies.
 */
var render = require('./lib/render');
var config = require('config');
var logger = require('koa-logger');
var parse = require('co-body');
var serve = require('koa-static');
var router = require('./routes');
var koa = require('koa');
var app = koa();

// "database"

var posts = [];

// middleware

app.use(logger());

// serve files from ./public
app.use(serve(__dirname + '/public'));
app.use(serve(__dirname + '/bower_components'));

app.use(router.routes())

// route middleware

//app.use(route.get('/', list));
//app.use(route.get('/post/new', add));
//app.use(route.get('/post/:id', show));
//app.use(route.post('/post', create));

// listen
app.listen(3000);
console.log('listening on port 3000');
    
```

``` javascript
→ cat lib/render.js
/**
 * Module dependencies.
 */

var views = require('co-views');

// setup views mapping .html

module.exports = views(__dirname + '/../views', {
  map: { html: 'nunjucks' }
});
```

``` javascript
→ cat routes.js
var router = require('koa-router')();
var sessions = require('./controllers/sessions')
var projects = require('./controllers/projects')


router
    .get('/', function *(next){
        this.body = 'Hello world'
    })
    // Sessions
    .get('/auth/google', sessions.g_auth)
    .get('/auth/google/callback', sessions.g_auth_callback)
    .delete('/logout', sessions.logout)
    .get('/login', sessions.login)

    // Projects
    .get('/projects', projects.list)
    .get('/projects/:id', projects.show)
    .post('/projects', projects.add)
    .put('/projects/:id', projects.update)
    .delete('/projects/:id', projects.destroy)
    .post('/projects', projects.add)

;

module.exports = router
```


``` javascript
→ cat controllers/projects.js
module.exports = {

    list: function *(next){
            this.body = "list all projects"
          },
    show: function *(next){
            this.body = "show project :" + this.params.id
          },
    add: function *(next) {
            this.body = "add new project"
         },
    update: function *(next) {
            this.body = "update project" + this.params.id
         },
    destroy: function *(next) {
            this.body = "destroy project" + this.params.id
         }
}
```

```
→ cat views/layouts/main.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- The above 3 meta tags *must* come first in the head; any other head content must come *after* these tags -->
    <meta name="description" content="">
    <meta name="author" content="">
    <link rel="icon" href="../../favicon.ico">

    <title>Signin Template for Bootstrap</title>

    <!-- Bootstrap core CSS -->
    <link href="/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet">

    <!-- IE10 viewport hack for Surface/desktop Windows 8 bug -->
    <link href="../../assets/css/ie10-viewport-bug-workaround.css" rel="stylesheet">

    <!-- Custom styles for this template -->
    <link href="/css/signin.css" rel="stylesheet">

    <!-- Just for debugging purposes. Don't actually copy these 2 lines! -->
    <!--[if lt IE 9]><script src="../../assets/js/ie8-responsive-file-warning.js"></script><![endif]-->
    <script src="../../assets/js/ie-emulation-modes-warning.js"></script>

    <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.3/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
  </head>

  <body>

    <div class="container">

     {% block body %}

     {% endblock %}

    </div> <!-- /container -->


    <!-- IE10 viewport hack for Surface/desktop Windows 8 bug -->
    <script src="../../assets/js/ie10-viewport-bug-workaround.js"></script>
  </body>
</html>
```

``` html
→ cat views/sessions/login.html
{% extends "views/layouts/main.html" %}

{% block body %}
<a  class='form-signin' href="/auth/google"> Sign in with google </a>
{% endblock %}
```


```
→ tree -L 2
.
├── bower_components
│   ├── bootstrap
│   └── jquery
├── config
│   └── default.json
├── controllers
│   ├── projects.js
│   └── sessions.js
├── index.js
├── lib
│   └── render.js
├── node_modules
├── package.json
├── public
│   └── css
├── routes.js
├── secret.md
├── src
│   └── client.js
└── views
    ├── layouts
    ├── projects
    └── sessions
```

