---
id: astronode-ag-1
title: Astronode Route File
sidebar_label: Understanding Routes
---

Let's talk a little about route file before we start to make some new complex stuff! You sse in previous section that this file is where we mapping our routes, but now we will teach you how do that :)

## Route Structure
An route structure contain a **path** as **key** and a **map** as value.

**example**
```javascript
    {
        "/route": {}
    }
```

Inside this map we can add **methods** as **keys** and their **middlewares** and **call functions** as configuration. See some examples.
```javascript
    {
        "/route": {
            "get": "callingSimpleFunction",
            "post": [ "calling", "sequence", "of", "functions"],
            "put": {
                "call": [ "amethod" ],
                "middlewares": [ "somemiddleware" ]
            }
        }
    }
```

### Route Configuration
The route configuration file holds a similar structure than the Route Structure, we have **BASE paths** as **keys** and a **map** as value, but this map is different, they have two basic properties: **defaultAPI** and **routes**.

### routes
Route is exactly a Route Structure, you can add your path and custom calls here! Remember that everything that you put here will override defaultAPI values.

#### defaultAPI
By default, astronode can create a base CRUD for you. He creates:
- GET in `/` that get all the documents of an entity.
- POST in `/` that create a document for an entity.
- PUT in `/:id` that update a document for an entity.
- DELETE in `/:id` that remove a document for an entity.
- GET in `/:id` that get a document by your ID.

If you insert this configuration, is mandatory that you add a `.module` value, points what module will controll that route. You can see it in our early example in quick start when we give an `astronode module user`

```javascript
{
    "/user": {
        "defaultAPI": {
            "module": "user"
        }
    }
}
```

Another property that you can insert is `.middlewares`, it will cover the necessity of include middlewares in routes created by default.

```javascript
{
    "/user": {
        "defaultAPI": {
            "module": "user",
            "middlewares": {
                "/": {
                    "get": [ "somemiddleware" ]
                }
            }
        }
    }
}
```

