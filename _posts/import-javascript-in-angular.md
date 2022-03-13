# How to import a JavaScript library in Angular

In this example we show how to import a JS library in your Angular project generated by the CLI.

## Add the library to your project

Include the JS file in your path, e.g.: `src/assets/js`.

## Modify angular-cli.json

Add the library to your `angular-cli.json`, e.g:
```json
  "scripts": [
        "../node_modules/jquery/dist/jquery.min.js",
        "../src/assets/css/hl/highlight.pack.js",
        "../src/assets/js/autotrack.js"
      ],
```