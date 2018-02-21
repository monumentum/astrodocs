---
id: astronode-ag-3
title: Astronode Parser Rules
sidebar_label: Rules of Parsing
---

In last section we talk about 2 concepts of astronode `Request Chain` and `Plugin Methods`, in this section we are talk a little about plugins, controllers, middlewares and how parsing works for the Route Config.

## Request Chain
Every Route Structure, could receive your request callback in 4 ways:

```javascript
{
    // As a string
    "get": "mymethod",

    // As an array
    "post": ["mymethod"],

    // As a string into call prop
    "put": {
        call: "mymethod"
    },

    // As an array into call prop
    "delete": {
        call: [ "mymethod" ]
    }
}
```

When we use an array, we are declaring a `Request Chain`, by default astronode works with a base handle at the end of each request that return an error in case of `.catch` or an json in case of `success`, we can construct a chain of promises waiting that in the end a value be dispatched to the end user.

### Promise structure
The first call will always receive an req parameter, the other ones will receive the return of the first call, example:

```javascript
const firstCall = req => {
    return 1
}

const secondCall = valueFromFirstCall => {
    // valueFromFirstCall === 1;
    return value
}
```

This structure could be use to make webhooks, or threat values as a plugin/middleware (like our post example in last section).

## Lookup Order
When we map functions inside Route File as an string exists a lookup order. If we add a string in a place that refer to a controller, they will look to the controllers, if we put into a middleware place, it will look for a middleware, but if we use the notation `!` we are afirming that this guy is a **plugin**. So `Plugin Methods` is any method that could be accessed by `!` reference, like `mongoose.methods` that export all interfaced used to create defaultAPIs.