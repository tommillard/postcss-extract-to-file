# PostCSS extract to file plugin

A basic PostCSS plugin to extract certain rules from one file and write them to a different file.

It's intended use case it to remove all fallback CSS into its own file so that it can be loaded only for devices that require it. Think of all the OldIE rules for desktop or the non-W3C flexbox implementations for mobile.

## Install
### NPM 

`npm i postcss-extract-to-file --save-dev`

### Git
`git clone https://github.com/benfrain/postcss-extract-to-file.git`


## There be dragons!!
**NOTE: At this point the plugin is in 0.0.1 release. That means it's been tested in my own dev bubble and little else. Use with caution for production!!**

**NOTE 2: I intend to port this to TypeScript or Babel shortly for better backwards compatibility in older Node versions. Presently, you'll need to use a modern NODE version (I'm rocking v4.2.6 at time of writing)**

## Overview
The plugin takes 3 options: `remaining`, `extracted` and `extractors`. It processes your CSS, matches relevant rules (that you set in "extractors") and extracts them to the "extracted" file location. Rules that haven't matched are written to the "remaining" file location. Your existing file (the one that had both extracted and remaining in) continues to it's original location. That's it!

**It is recommended that this plugin is used just before or just after your minifier (e.g. [cssnano](http://cssnano.co))**

## Options
#### remaining
This is the location you want the rules that haven't matched to be written to.

#### extracted
This is the location you want the rules that have matched to be written to.

#### extractors
This is an object of possible strings (e.g. CSS selectors) that, should they be present in a rule will cause them to be extracted.

## Example configuration
Given an PostCSS plugin options object like this:

````
"postcss-extract-to-file": {
    "remaining": "./output/remaining.css",
    "extracted": "./output/extracted.css",
    "extractors": [
        ".no-w3cflexbox",
        ".os_",
        ".no-supportsnativescrolling",
        ".no-svginhtml",
        ".no-flex",
        ".browser_ie",
        ".no-csstransitions",
        ".ie-8",
        ".ie-9"
    ]
},
````

Any rules that match a string in the "extractors" array will be extracted and written to the output destination and filename set in "extracted". The remaining rules will be written to the "remaining" location.

## Tests
This plugin uses [AVA](https://github.com/sindresorhus/ava) for tests. If you want to check this plugin is working as it should on your system, from CLI, browse to `node_modules/postcss-extract-to-file/` and run `npm test`.


