kixx-atypes
============
Algebraic Data Types for EcmaScript.

__TODO:__ Everything.

[![NPM](https://nodei.co/npm/kixx-atypes.png)](https://www.npmjs.com/package/kixx-atypes)

## Summary
Algebraic Data Types make programming in a functional way possible. It's *mostly* not possible to make functional programming very useful in JavaScript without them. Have you ever read a blog posts on functional programming and thought "I already know map() and reduce(), and there is no way I can make use of this other garbage"? Algebraic Data Types solve that.

Here is a [Mostly Adequate Guide to programming with containers](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/ch08.html) (AKA algebraic data types), which will make a great introduction.

## Requirements
kixx-atypes is written with ES6 style classes and requires a JavaScript engine that supports ES2015 or higher. Or, you can use a compiler like Babel to translate your code and dependencies (including kixx-atypes) to ES5.

## Installation
```
npm install --save kixx-atypes
```

kixx-atypes exports a single object, with the constructor for each type referenced on it. Here are some examples of importing the Identity type:

```js
// The modern CommonJS way
const { Identity } = require('kixx-atypes');

// The hip way.
import { Identity } from 'kixx-atypes';

// The old school way
var KixxAtypes = require('kixx-atypes');
var Identity = KixxAtypes.Identity;
```

## Documentation
- Identity
- Maybe
- Either
- IO
- List
- Task

Copyright and License
---------------------
Copyright: (c) 2019 by Kris Walker (www.kixx.name)

Unless otherwise indicated, all source code is licensed under the MIT license. See MIT-LICENSE for details.

