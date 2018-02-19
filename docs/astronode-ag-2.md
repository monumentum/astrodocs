---
id: astronode-ag-1
title: Complex Models
sidebar_label: Manage Complex Models
---

Now that we are introduced with Routing File, let's create a custom controller! To make it, we will upgrade our start project to Medium copy :)

## New Modules
Let's create an post module: `./node_modules/.bin/astronode module post`

**post/model.js**
```javascript
const { SchemaTypes } = require('mongoose');

module.exports = {
    slug: {
        type: String,
        required: true,
        unique: true,
    },
    author: {
        type: SchemaTypes.ObjectId,
        ref: 'user',
        required: true
    },
    body: {
        type: String,
    },
    createdAt: {
        type: Date,
        default: Date.now,
    },
    updatedAt: {
        type: Date,
        default: Date.now,
    }
}
```

## Complex Schemas
You will need to handle values and change it or create custom methods to some instances. Example: In user model you need hash the password before save in your database, in post you need to create a slug and in a future comment model you will need a custom method to get commends by POST id. Our `astronode-mongoose-plugin` supports you to export a Mongoose Schema in the place of a simple object.

```javascript
const { SchemaTypes, Schema } = require('mongoose');

const PostSchema = new Schema({
    slug: {
        type: String,
        required: true,
        unique: true,
    },
    author: {
        type: SchemaTypes.ObjectId,
        ref: 'user',
        required: true
    },
    body: {
        type: String,
    },
    createdAt: {
        type: Date,
        default: Date.now,
    },
    updatedAt: {
        type: Date,
        default: Date.now,
    }
});

PostSchema.pre('save', post => {
    if (post.title) {
        post.title = slugfy(post.title);
    }

    next();
});

module.exports = PostSchema;
```

## Slug as id?
In theory you will use the slug in your URL and will hit the API with it to get the post, so, we need a custom method `getBySlug`:

```javascript
PostSchema.static('getBySlug', slug => {
    return PostSchema.findOne({ slug }).lean().exec()
});
```

Now we can override our method `getById` in defaultAPI by our `getBySlug`:

```javascript
    "/post": {
        "defaultAPI": {
            "module": "post"
        },
        "routes": {
            "/:id": {
                "get": [
                    "!express.extract:id",
                    "!mongoose.models.post.getBySlug"
                ]
            }
        }
    }
```

Here we are presented to two concepts of astronode: `Request Chain` and `Plugin Methods`.