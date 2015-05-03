# Problem

Many Node.js modules include tests and example code in their repos. This code needs to require the module itself.

But whereas modules depending on `foobar` would write `require('foobar')`, the test and example code within `foobar` cannot require itself that way. They must use a relative path like `var foobar = require('../..')` instead.

This difference is minor but annoying in some case. Example code copied from `foobar` into your module won't work until you update the relative path(s) to `'foobar'`. And if `foobar` is authored in TypeScript, the line `var foobar = require('../..')` loses all ambient type information about the module.

# Solution

1. Add `require-self` as a devDependency to your module.
2. Call the `require-self` bin command in your module's `prepublish` npm script.

For example:
```json
{
    "name": "foobar",
	...
    "scripts": {
        "build": "...",
        "prepublish": "npm run build && require-self",
        "test": "mocha built/test/*.js"
    },
    "devDependencies": {
		...
        "require-self": "^0.0.4"
    }

```

Now all test and example code in the `foobar` module can use `var foobar = require('foobar');`.

# How it works

The `require-self` command adds a file to the module's `node_modules` folder, with the same name as the module, that simply does `module.exports = require('..');`


## License

The MIT License (MIT)

Copyright (c) 2015 Troy Gerwien

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.


