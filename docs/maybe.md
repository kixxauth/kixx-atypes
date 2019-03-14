# Maybe
Used when something may return a null value, or we want to represent the return value of a function as something or nothing. Maybe it's something, maybe it's nothing.

Typically we would want to do something like this:
```js
const partial = (fn, args) => {
    return fn(...args);
};

const readFileSync = (path) => {
    try {
        return fs.readFileSync(path, 'utf8');
    } catch () {
        return null
    }
};

const splitLines = (text) => {
    return text.split(EOL);
};

const rejoinLines = (delimeter, lines) => {
    return lines.join(delimeter);
};

const writeFileSync = (fpath, text) => {
    return [fpath, text];
};

const transformFile = (fpath, delimeter) => {
    return pipe(
        readFileSync,
        splitLines,
        partial(rejoinLines, [ delimeter ]),
        partial(writeFileSync, [ fpath ])
    )(fpath);
};
```

The problem is that if the file does not exist, `readFileSync()` will return `null` after catching the `ENOENT` error. After that, our `pipe()` composition will blow up unless we add `if (x === null)` checks to all of the following functions.

By returning a Maybe from `readFileSync()` and combining it with `map()`, the remaining piped functions after `readFileSync()` will simply not be called and the whole composition will return a Maybe.Nothing instance.

```js
class Maybe {
    extract() {
        return this.value;
    }

    static nothing() {
        return new Nothing();
    }

    static just(x) {
        return new Just(x);
    }
}

class Nothingness {}

class Nothing extends Maybe {
    constructor() {
        this.value = Maybe.nothingness;
    }

    map(fn) {
        return this;
    }
}

class Just extends Maybe {
    constructor(value) {
        this.value = value;
    }

    map(fn) {
        return new Just(fn(this.value));
    }
}

Maybe.Just = Just;
Maybe.Nothing = Nothing;
Maybe.nothingness = new Nothingness();

const partial = (fn, args) => {
    return fn(...args);
};

// Our new map() helper can be used on any object
// with a .map() method, including a normal Array.
const map = (fn, a) => {
    return a.map(fn);
};

const readFileSync = (path) => {
    try {
        return Maybe.just(fs.readFileSync(path, 'utf8'));
    } catch () {
        return Maybe.nothing();
    }
};

const splitLines = (text) => {
    return text.split(EOL);
};

const rejoinLines = (delimeter, lines) => {
    return lines.join(delimeter);
};

const writeFileSync = (fpath, text) => {
    return [fpath, text];
};

const transformFile = (fpath, delimeter) => {
    return pipe(
        readFileSync,
        map(splitLines),
        map(partial(rejoinLines, [ delimeter ])),
        map(partial(writeFileSync, [ fpath ])),
    )(fpath).extract();
};
```

Just knowing that `readFileSync()` returns a Maybe instance, we can then `map()` the rest of the composition and everything will 'just work'.

## Importing
```js
// The modern CommonJS way
const { Maybe } = require('kixx-atypes');

// The hip way.
import { Maybe } from 'kixx-atypes';

// The old school way
var KixxAtypes = require('kixx-atypes');
var Maybe = KixxAtypes.Maybe;
```

## Usage

## Specifications
- [Functor](https://github.com/fantasyland/fantasy-land#functor)
- [Bifunctor](https://github.com/fantasyland/fantasy-land#bifunctor)
- [Applicative](https://github.com/fantasyland/fantasy-land#applicative)
- [Chain](https://github.com/fantasyland/fantasy-land#chain)
- [Monad](https://github.com/fantasyland/fantasy-land#monad)
- [Extend](https://github.com/fantasyland/fantasy-land#extend)
- [Comonad](https://github.com/fantasyland/fantasy-land#comonad)
