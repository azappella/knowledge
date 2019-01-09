# configuration management

see https://www.npmjs.com/package/node-env-run

## store config in env variables

Configuration should come from [environment variables](https://12factor.net/config)

Use [dotenv](https://github.com/motdotla/dotenv) + [dotenv-expand](https://github.com/motdotla/dotenv-expand) package (with the added advantage that variable expansion works as it would in bash).

Example .env

```
NODE_ENV=development
PROTOCOL=http
HOST=localhost
PORT=3000

SERVER_URL=${PROTOCOL}://${HOST}:${PORT}
```

Now, you can start the application with the proper configuration, using either of the three following methods

1) using dotenv + dotenv expand in the application

```
# as early as possible in your application
const dotenv = require('dotenv')
const dotenvExpand = require('dotenv-expand')

const config = dotenv.config()
dotenvExpand(config);
```
2) set the script local environment variables before launching the app itself

```
# will only set the env vars within the scope of the script
$ env $(grep -v '^#' .env | xargs) npm start
```

3) set the environment variables in your environment

```
$ export $(grep -v '^#' .env | xargs)  # will set env vars globally on the system
$ npm start
```

Alternatively, use [wtgtybhertgeghgtwtg/env-and-files](https://github.com/wtgtybhertgeghgtwtg/env-and-files) to load env variables and files.

## store config based on runtime environment (à la create-react-app)

Create-react-app automatically reads configuration files for the three pre-defined environments: development, test, and production.

Default (lowest priority) values may be defined in .env, and the value of NODE_ENV is set automatically based on the script used:

```
start → development
test → test
build → production
```

There is a configuration file corresponding to each of these environments: .env.development, .env.test, and .env.production. The values from these files have higher priority than the values in .env. Each file (including the default file) supports another layer of overrides by adding .local to the end. The full order of precedence is (highest first):

1. shell
2. .env.{environment}.local
3. .env.{environment}
4. .env.local
5. .env

[Source](https://medium.com/@tacomanator/environments-with-create-react-app-7b645312c09d)

## store config based on runtime environment

Use [node-config](https://github.com/lorenwest/node-config) package.

The config folder structure is as follows:

- local.js — contains full configuration, should be put under .gitignore and never appear on a repo
- default.js — contains the top-level generic configuration keys that are shared by all enviroments with the secret values blanked out
- <env.js> — contains the specific overrides for a particular environment. Environment is defined using NODE_ENV environment variable
- custom-environment-variables.js — maps the secret configuration keys to env variables to be later injected at runtime

[Source](https://itnext.io/node-js-configuration-and-secrets-management-acd84375ca7)

