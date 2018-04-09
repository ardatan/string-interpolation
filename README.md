# String Interpolation

[![Build Status](https://travis-ci.org/seripap/string-interpolation.svg?branch=master)](https://travis-ci.org/seripap/string-interpolation)

This package allows dynamic string interpolation. Given delimiter `{x}`, replace with `y`.

## Example

```js
const Interpolator = require('string-interpolation');

const replaceThis = `Hi, my name is {name}.`;
const interpolator = new Interpolator(options);

const data = {
    name: 'Dan',
};
interpolator.parse(replaceThis, data);
// Hi, my name is Dan.
```

## Options (optional)

- `delimiter`: Array; Must be an array of 2 items. This is the start and end values of the delimiter. Defaults to `{}`

### Defaults
```js
{
    delimiter: ['{','}'],
}
```

## Alternative Text

If data key is not defined, you can provide an alternative value. Just delineate the alternative text with a colon `:` after the data key. Due to current limitations, you cannot use reserved symbols (`|`, `:`) as an alternative text.

If you need to include reserved symbols as an alt text, feel free to open up a PR for a fix :).

```js
const replaceThis = `Hi, my name is {name:Altnerative Text}.`;
const interpolator = new Interpolator(replaceThis);

interpolator.parse(replaceThis);
// Hi, my name is Alternative Text.
```

## Modifiers

Modifiers are functions that transform interpolated text. These are applied by reducing strings parsed from the interpolator. By specifying a pipe `|`, the parser will transform interpolated text based on modifiers leading the pipe. This will also do transformations on alternative text if provided.

```js
const Interpolator = require('string-interpolator');

const replaceThis = `Hi, my name is {name|uppercase}.`;
const options = {
    data: {
        name: 'Dan',
    },
};
const interpolator = new Interpolator(options);

interpolator.parse(replaceThis);
// Hi, my name is DAN.
```

You can add as many modifiers as you'd like by seperating with a comma. Useful if you are using custom modifiers and want to reduce a string, not quite as useful with the example below.

```js
const Interpolator = require('string-interpolator');

// This will transform to title case, and then lowercase
const replaceThis = `Hi, my name is {name|title,lowercase}.`;
const options = {
    data: {
        name: 'Dan',
    },
};
const interpolator = new Interpolator(options);

interpolator.parse(replaceThis);
// Hi, my name is dan.
```


### Built in modifiers

| modifier | description |
|---|---|
| title | Converts to title case |
| uppercase | Converts to uppercase |
| lowercase | Converts to lowercase |

## Custom modifiers

You can also build your own modifiers. Modifiers are functions that receives interpolated strings and transforms them.

```js
const Interpolator = require('string-interpolator');

const replaceThis = `Hi, my name is {name|customModifier}.`;
const options = {
    data: {
        name: 'Dan',
    },
};
const interpolator = new Interpolator(options);
const customModifier = str => str.split('').reverse().join('');
interpolator.registerModifier('customModifier', modifier)

interpolator.parse(replaceThis);
// Hi, my name is naD.
```