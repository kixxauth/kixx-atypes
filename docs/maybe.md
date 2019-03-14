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

## Member Functions

### Maybe.isMaybe()

`Maybe.isMaybe :: (x) -> Boolean` Takes any type and returns a Boolean.

If `x` is not an instance of Maybe, `Maybe.isMaybe()` will check to see if `x` implements the expected methods and has the expected properties of a `Maybe`.

### Maybe.isJust()

`Maybe.isJust :: (x) -> Boolean` Takes any type and returns a Boolean.

If `x` is not an instance of Maybe, `Maybe.isJust()` will check to see if `x` implements the expected methods and has the expected properties of a `Maybe.Just`.

### Maybe.isNothing()

`Maybe.isNothing :: (x) -> Boolean` Takes any type and returns a Boolean.

If `x` is not an instance of Maybe, `Maybe.isNothing()` will check to see if `x` implements the expected methods and has the expected properties of a `Maybe.Nothing`.

### Maybe.of()

### Maybe.just()

### Maybe.nothing()

### Maybe.maybe()

## Static Members

### Maybe.Just
A reference to the Just type class; a subclass of Maybe.

### Maybe.Nothing
A reference to the Nothing type class; a subclass of Maybe.

## Maybe.Just Methods

### Just map()

### Just bimap()

### Just ap()

### Just chain()

### Just extend()

### Just extract()

### Just.of()

## Maybe.Nothing Methods

### Nothing map()
`m.map :: (Function f) -> m`

Takes a function `f` as an argument and returns the same Nothing instance. `f` will not be invoked.

```js
const f = () => 1
const m = Maybe.nothing();
const x = m.map(f);
assert(x === m);
```

### Nothing bimap()
`m.bimap :: (Function a, Function b) -> m`

Takes a sad Function `a` and a happy Function `b`. The sad Function `a()` will be invoked with the `Maybe.nothingness` value as the only argument, but the return value will be ignored.

```js
const sad = () => 2
const happy = () => 2
const m = Maybe.nothing();
const x = m.bimap(sad, happy);
assert(x === m);
```

### Nothing ap()
`m.ap :: (Maybe b) -> m`

Takes a Maybe intance `b` as the only argument and returns the same Nothing instance. `b.map()` will not be invoked.

```js
const a = Maybe.nothing()
const b = Maybe.just();
const x = a.ap(b);
assert(x === a);
```

### Nothing chain()
`m.chain :: (Function f) -> m`

Takes a function `f` as the only argument and returns the same Nothing instance. `f` will not be invoked.

```js
const f = () => 1
const m = Maybe.nothing();
const x = m.chain(f);
assert(x === m);
```

### Nothing extend()
`m.extend :: (Function f) -> Nothing`

Takes a function `f` as an argument and returns the same Nothing instance. `f` will not be invoked.

```js
const f = () => 1
const m = Maybe.nothing();
const x = m.extend(f);
assert(x === m);
```

### Nothing extract()
`m.extract :: () -> v`

Takes no arguments and returns the Nothingness instance.

```js
const m = Maybe.nothing();
assert(m.extract() === Maybe.nothingness);
```

### Nothing.of()
`Nothing.of :: (x) -> Nothing`

Ignores the input value if there is one, and returns a new Nothing instance.

```js
const x = Maybe.Nothing.of(1);
assert(Maybe.isNothing(x));
```

## Specifications
- [Functor](https://github.com/fantasyland/fantasy-land#functor)
- [Bifunctor](https://github.com/fantasyland/fantasy-land#bifunctor)
- [Apply](https://github.com/fantasyland/fantasy-land#apply)
- [Applicative](https://github.com/fantasyland/fantasy-land#applicative)
- [Chain](https://github.com/fantasyland/fantasy-land#chain)
- [Monad](https://github.com/fantasyland/fantasy-land#monad)
- [Extend](https://github.com/fantasyland/fantasy-land#extend)
- [Comonad](https://github.com/fantasyland/fantasy-land#comonad)
