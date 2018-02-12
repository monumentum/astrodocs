---
id: astronode-qs-2
title: Astronode Config
sidebar_label: Config File
---

The astronode config file is the responsible for configure port, host, plugins, authentication and all core stuff for our framework.

## Using a base config file
Inside astronode we have a function that create a base astronode config file for your project. So let's use this command: `./node_modules/.bin/astronode setup --engine express --data mongoose`

The **--engine** and **--data** flag will create plugin template for both.

## Configuring Engine and Data Plugin
By default the first command will create a file like it:
```javascript
{
    "port": 3000,
    "host": "0.0.0.0",
    "engine": "express",
    "data": "mongoose",
    "application": {
        "modules": "app",
        "controllerPattern": "/controller.js$",
        "modelPattern": "/model.js$",
        "ignored": []
    },
    "plugins": [
        {
            "name": "express",
            "module": "express"
        },
        {
            "name": "mongoose",
            "module": "mongoose",
            "config": {
                "uri": "localhost",
                "port": 1111,
                "database": "astronode"
            }
        }
    ]
}
```

Now you need to put correct informations change **module** name of **express** plugin to **astronode-express-plugin** and **mongoose** to **astronode-mongoose-plugin**, remember to change mongoose config too, you will get something like:

```javascript
{
    "port": 3000,
    "host": "0.0.0.0",
    "engine": "express",
    "data": "mongoose",
    "application": {
        "modules": "app",
        "controllerPattern": "/controller.js$",
        "modelPattern": "/model.js$",
        "ignored": []
    },
    "plugins": [
        {
            "name": "express",
            "module": "astronode-express-plugin"
        },
        {
            "name": "mongoose",
            "module": "astronode-mongoose-plugin",
            "config": {
                "uri": "localhost",
                "port": 27017,
                "database": "astroblog"
            }
        }
    ]
}
```

Now we need create a module to generate default API's