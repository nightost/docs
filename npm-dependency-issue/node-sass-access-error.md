## node-sass ACCESS error
###  Error Log
```javascript
gyp ERR! configure error
gyp ERR! stack Error: EACCES: permission denied, mkdir '/Users/nightost/Documents/workspace/aisports-platform/aisports-web/src/main/webapp/assets/node_modules/node-sass/build'
gyp ERR! System Darwin 17.3.0
gyp ERR! command "/Users/nightost/.nvm/versions/node/v8.9.4/bin/node" "/Users/nightost/Documents/workspace/aisports-platform/aisports-web/src/main/webapp/assets/node_modules/node-gyp/bin/node-gyp.js" "rebuild" "--verbose" "--libsass_ext=" "--libsass_cflags=" "--libsass_ldflags=" "--libsass_library="
gyp ERR! cwd /Users/nightost/Documents/workspace/aisports-platform/aisports-web/src/main/webapp/assets/node_modules/node-sass
gyp ERR! node -v v8.9.4
gyp ERR! node-gyp -v v3.6.2
gyp ERR! not ok
```
### Solution
[How to Prevent Permissions Errors](https://docs.npmjs.com/getting-started/fixing-npm-permissions)
MAC/Linux select option two