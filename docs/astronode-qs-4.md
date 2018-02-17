---
id: astronode-qs-4
title: Astronode Initialization
sidebar_label: Starting the Framework
---

Now all that you need to do is check if your mongo instance are running ok, and if the port that we will use are unlocked :) After it, all that you need to do is: `./node_module/.bin/astronode run`. There are methods to initializate it by other ways, you can check it out in Extended Section.

## Gold Tip
To improve the left it semantic, we recommend you to add this into your package.json:

```javascript
{
    "scripts": {
        "start": "astronode run"
    }
}
```

Now all that you need to do is: `npm run start`