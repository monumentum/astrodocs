---
id: astronode-ag-2
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
    title: {
        type: String,
        required: true,
    },
    author: {
        type: SchemaTypes.ObjectId,
        ref: 'user',
        required: true
    },
    likes: [{
        type: SchemaTypes.ObjectId,
        ref: 'user'
    }],
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
const slug = require('slug');
const { SchemaTypes, Schema } = require('mongoose');

const PostSchema = new Schema({
    slug: {
        type: String,
        required: true,
        unique: true,
    },
    title: {
        type: String,
        required: true,
    },
    author: {
        type: SchemaTypes.ObjectId,
        ref: 'user',
        required: true
    },
    likes: [{
        type: SchemaTypes.ObjectId,
        ref: 'user'
    }],
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

PostSchema.pre('save', function (next) {
    if (this.title) {
        this.title = slug(this.title);
    }

    next();
});

module.exports = PostSchema;
```

## Static Methods
We will need to methods during our calls, first on is `getBySlug` to get the post using the slug in the URL, and second one, is the `toggleLike` to add/remove like from an post.

```javascript
PostSchema.static('getBySlug', function (slug) {
    return this.findOne({ slug }).lean().exec()
});

PostSchema.static('toggleLike', function ({ userId, postId }) {
    return this.findById(postId).exec().then(post => {
        if (!post.likes) {
            post.likes = [ userId ];
        } else {
            const indexOfUser = post.likes.indexOf(userId);

            if (~indexOfUser) {
                post.likes.splice(indexOfUser, 1)
            } else {
                post.likes.push(userId);
            }
        }

        return post.save();
    });
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
                    "!express.extract:params.id",
                    "!mongoose.models.post.getBySlug"
                ]
            },
            "/like": {
                "put": [
                    "!express.extract:body",
                    "!mongoose.models.post.toggleLike"
                ]
            }
        }
    }
```

Here we are presented to two concepts of astronode: `Request Chain` and `Plugin Methods`.