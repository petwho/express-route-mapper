# express-route-mapper

Manage routes and aliases in Rails fashion. 


### Installation

    $ npm install express-route-mapper
    
### Example

You can clone the example app in github repository

### Usage

*Include module and register all routes in app.js*

``` javascript
  app.set('routemap', require('express-route-mapper'));
  app.get('routemap').map('config', app);
```

Setting `routemap` local variable for View to retrieve URI from path name and aliase (See later session):

``` javascript
  app.use(function(req, res, next){
    app.locals.routemap = app.get('routemap');
    req.routemap = app.locals.routemap;
    next();
  });
```

Here we first require the module and then specify the configuration file location the module should look into under the `route` directory. You could also define your own path.

*Resource:*

If you want to register routes for a resource then the syntax will look like this:

In `routes/config.js`
``` javascript
module.exports = [
  {resource: 'photos'}
]
```

when registering a route with resource, route mapper register all related routes following RESTful API.

The above map will register the following routes:

 Path name     |   URI        | Callbak/HTTP Method
:--------------|-------------:|:------------------:
 list_photos   |  /photos     |  index  [GET]
 show_photos   |  /photos/:id |  show   [GET]
 new_photos    |  /photos/new |  new    [GET]
 create_photos |  /photos     |  create [POST]
 update_photos |  /photos/:id |  update [PUT]
 delete_photos |  /photos/:id |  delete [DELETE]

Then in `routes/photos` you need to define the appopreate callbacks. An example would look like this:

```javascript
module.exports = {
  index: {
    callback: function(req, res) {
      res.send("List photos!");
    }
  }
}
```

*Custom route name and alias*

In Rails you can create route like following:

match: 'signin', to: 'session#new', via: get

Here we could do the similar things:

```javascript
{get: '/signin', to: 'session#new', as: 'signin'},
```

The above registry will map a GET request to */signin* path to the `new` callback defined in `routes/session` file.


*Retrieve URI*

Using path name or alias you can retrieve the URI which you could later change it without affecting the result. This allow you to reference to an URIs without worrying about later changes will break those links.

Give the above declaration, you could for example simply do things like the followings:

\- In Route `routes/users.js`

``` javscript
show: {
param: ':user_id',
callback: function(req, res, next) {
    if(req.session.userId != req.user_id) {
        return res.redirect(routemap.get('signin');
    }
    else {
    // Do stuff
    }
}
```

#License

(The MIT License)

Copyright (c) 2013 Tran Khanh &lt;trankhanhsvn@gmail.com&gt;

Website: [www.hoclaptrinh.org](hoclaptrinh.org)

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
