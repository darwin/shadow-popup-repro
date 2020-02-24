# shadow-popup-repro

This is a minimal version of  [chromex/examples/shadow](https://github.com/binaryage/chromex/tree/master/examples/shadow).

The release build produces shared.js and popup.js - we are supposed to include it via script tags in popup.html in that order. 
The problem is that dev compilation produces shared.js and popup.js, but popup.js already contains all code from shared.js, so
including it after shared.js gives us tons of warnings about duplicate namespaces. Including only popup.js works as expected.

### repro steps

```bash
git clone https://github.com/darwin/shadow-popup-repro.git
cd shadow-popup-repro

shadow-cljs release extension
# inspect shared.js and popup.js in resources/unpacked/out 
# popup.js only contains `console.log("hello from popup");`

rm -rf resources/unpacked/out
shadow-cljs compile extension
# inspect shared.js and popup.js in resources/unpacked/out
# popup.js contains full boilerplate which is also present in shared.js
```

Tested with shadow-cljs 2.8.85
