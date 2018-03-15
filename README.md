# isemail-es5

Node email address validation library

This library is a port of the PHP `is_email` function by Dominic Sayers. It's almost entirely based off of [`hapijs/isemail`](https://github.com/hapijs/isemail)...except that it's been compiled to `ES5`.

`hapijs` has openly chosen not to compile their code to `ES5`

> It is up to the developer to make the code work in their environment. Across the org, we do not include a single line of cleint-side transformation code.
- [`hapijs/isemail#27`](https://github.com/hapijs/isemail/issues/27)

> The hapijs organization hosts modules that are designed for server-side use, and we as an organization decided to relatively quickly drop support for older versions of Node.js. We use the latest features to ease development, and browser support is not within scope.
- [`hapijs/isemail#158](https://github.com/hapijs/isemail/issues/158)

However, there are those that might need an `ES5` version of `isemail` and don't want to specify transpilation for only this package in the `node_modules` folder in a webpack config, for example.

Install
=======

```sh
$ npm install isemail-es5
```

Test
====

The tests were pulled from `is_email`'s extensive [test suite][tests] on October 15, 2013. Many thanks to the contributors! Additional tests have been added to increase code coverage and verify edge-cases.

Run any of the following.

```sh
$ lab
$ npm test
$ make test
```

_remember to_ `npm install` to get the development dependencies!

API
===

validate(email, [options])
--------------------------

Determines whether the `email` is valid or not, for various definitions thereof. Optionally accepts an `options` object. Options may include `errorLevel`.

Use `errorLevel` to specify the type of result for `validate()`. Passing a `false` literal will result in a true or false boolean indicating whether the email address is sufficiently defined for use in sending an email. Passing a `true` literal will result in a more granular numeric status, with zero being a perfectly valid email address. Passing a number will return `0` if the numeric status is below the `errorLevel` and the numeric status otherwise.

The `tldBlacklist` option can be either an object lookup table or an array of invalid top-level domains. If the email address has a top-level domain that is in the whitelist, the email will be marked as invalid.

The `tldWhitelist` option can be either an object lookup table or an array of valid top-level domains. If the email address has a top-level domain that is not in the whitelist, the email will be marked as invalid.

The `allowUnicode` option governs whether non-ASCII characters are allowed. Defaults to `true` per RFC 6530.

Only one of `tldBlacklist` and `tldWhitelist` will be consulted for TLD validity.

The `minDomainAtoms` option is an optional positive integer that specifies the minimum number of domain atoms that must be included for the email address to be considered valid. Be careful with the option, as some top-level domains, like `io`, directly support email addresses.

As of `3.1.1`, the `callback` parameter is deprecated, and will be removed in `4.0.0`.

### Examples

```js
$ node
> var Isemail = require('isemail');
undefined
> Isemail.validate('test@iana.org');
true
> Isemail.validate('test@iana.org', {errorLevel: true});
0
> Isemail.validate('test@e.com', {errorLevel: true});
6
> Isemail.validate('test@e.com', {errorLevel: 7});
0
> Isemail.validate('test@e.com', {errorLevel: 6});
6
```

<sup name="footnote-1">&#91;1&#93;</sup>: if this badge indicates the build is passing, then isemail has 100% code coverage.

[skeggse]: https://github.com/skeggse "Eli Skeggs"
[tests]: http://isemail.info/_system/is_email/test/?all‎ "is_email test suite"
