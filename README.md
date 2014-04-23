# electric fence

This is a small hapi module to allow you to serve files and directories
from a local path mapped to a url.

## How to use

Simply give it a local path and a remote base url

```
plugin.require({electricfence: {path: 'public', url: '/'}});
```
Those are the defaults, so if you pass it nothing those will be used.

## Why?

"But Gar," you say, "hapi already has directory and file handlers!"

Yes, but if your server already has a a catchall route such as:

```javascript
server.route({
    method: 'get',
    path: '/{posts*}',
    handler: postHandler
});
```

You can't then add

```javascript
server.route({
    method: 'get',
    path: '/{path*}',
    handler: {directory: {path: 'public'}}
});
```

The paths will conflict.  What electricfence allows you to do is just this.

## How it works

electricfence adds explicit file and directory handlers for anything that's in the local path you give it.  This means for examply if you have a ``js`` and ``css`` directory in ``./public``, and you also have a ``robots.txt`` file, electricfence will add these handlers


```javascript
server.route({
    method: 'get',
    path: '/css/{path*}',
    handler: {directory: {path: 'public/css'}}
});

server.route({
    method: 'get',
    path: '/js/{path*}',
    handler: {directory: {path: 'public/js'}}
});

server.route({
    method: 'get',
    path: '/robots.txt',
    handler: {file: {path: 'public/robots.txt'}}
});
```