# Electron

<!-- MarkdownTOC -->

- [Fixing errors when loading 3rd party libs](#fixing-errors-when-loading-3rd-party-libs)

<!-- /MarkdownTOC -->

## Fixing errors when loading 3rd party libs

If you try and load 3rd party libs in your `index.html`,
you may see errors like this in the dev console:

```
script.js:3 Uncaught ReferenceError: $ is not defined
```

This is caused by the libs seeing that they are running in a CommonJS environment and therefore they ignore `window`, so you won't see `$` globally when using jQuery for example.

There are a variety of solutions to this, which can be found in [this StackOverflow thread](http://stackoverflow.com/questions/32621988/electron-jquery-is-not-defined) or in the [Electron GitHub Issue thread](https://github.com/electron/electron/issues/254).

[Source](http://stackoverflow.com/questions/32621988/electron-jquery-is-not-defined) & [Source](https://github.com/electron/electron/issues/254)

