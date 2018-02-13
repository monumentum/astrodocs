---
id: astronode-qs-3
title: My first module
sidebar_label: Creating a Module
---

A module is basic a folder that will hold you controller, model and whatever dedicated to a single instance of an entity in your project. In this Quick Start we will construct a User module

## Creating a User module
You can use command `./node_modules/.bin/astronode module user` to create a base module **OR**
you can do manually going to `/app` creating a folder called `/user` and inside this folder create one file called `model.js`.

**model.js**
```javascript
module.exports = {
    email: {
        type: String,
        required: true,
        unique: true,
    },
    password: {
        type: String,
        required: true,
    },
    name: {
        type: String
    }
}
```

## Astronode Route File
If you use the command line you will notice that you has an `astronode.route.json` with something like:
```javascript
{
    "/user": {
        "defaultAPI": {
            "module": "user"
        }
    }
}
```

If you didn't use the command line, you will need create this file manually and add the same code bellow. This file is necessary to mapping the routes with modules. You can understand more about it in advanced guide.