# esm modules

## ecmascript modules vs commonjs

Node.js has two module systems: CommonJS modules and ECMAScript modules.

Authors can tell Node.js to use the ECMAScript modules loader via:

1. the .mjs file extension
2. the package.json "type" field, 
3. or the --input-type flag. 

Outside of those cases, Node.js will use the CommonJS module loader. See Determining module system for more details.