kixx-atypes
============
Algebraic Data Types for EcmaScript.

__TODO:__ Everything.

[![NPM](https://nodei.co/npm/kixx-atypes.png)](https://www.npmjs.com/package/kixx-atypes)

## Summary
Algebraic Data Types make programming in a functional way possible. It's *mostly* not possible to make functional programming very useful in JavaScript. Have you ever followed one of those blog posts on functional programming and thought "I already know map() and reduce(), and there is no way I can make use of this other garbage"? Algebraic Data Types solve that.

Here is a [Mostly Adequate Guide to programming with containers](https://mostly-adequate.gitbooks.io/mostly-adequate-guide/ch08.html) (AKA algebraic data types), which will make a great introduction.

## Installation
```
npm install --save kixx-atypes
```

## Algebraic Types
- Identity
- Maybe
- Either
- IO
- List
- Task

### Identity
```js
// The modern CommonJS way
const { Identity } = require('kixx-atypes');

// The hip way.
import { Identity } from 'kixx-atypes';

// The old school way
var KixxAtypes = require('kixx-atypes');
var Identity = KixxAtypes.Identity;
```

### Maybe
```js
// The modern CommonJS way
const { Maybe } = require('kixx-atypes');

// The hip way.
import { Maybe } from 'kixx-atypes';

// The old school way
var KixxAtypes = require('kixx-atypes');
var Maybe = KixxAtypes.Maybe;
```

### Either
```js
// The modern CommonJS way
const { Either } = require('kixx-atypes');

// The hip way.
import { Either } from 'kixx-atypes';

// The old school way
var KixxAtypes = require('kixx-atypes');
var Either = KixxAtypes.Either;
```

### IO
```js
// The modern CommonJS way
const { IO } = require('kixx-atypes');

// The hip way.
import { IO } from 'kixx-atypes';

// The old school way
var KixxAtypes = require('kixx-atypes');
var IO = KixxAtypes.IO;
```

### List
```js
// The modern CommonJS way
const { List } = require('kixx-atypes');

// The hip way.
import { List } from 'kixx-atypes';

// The old school way
var KixxAtypes = require('kixx-atypes');
var List = KixxAtypes.List;
```

### Task
```js
// The modern CommonJS way
const { Task } = require('kixx-atypes');

// The hip way.
import { Task } from 'kixx-atypes';

// The old school way
var KixxAtypes = require('kixx-atypes');
var Task = KixxAtypes.Task;
```

Copyright and License
---------------------
Copyright: (c) 2019 by Kris Walker (www.kixx.name)

Unless otherwise indicated, all source code is licensed under the MIT license. See MIT-LICENSE for details.

