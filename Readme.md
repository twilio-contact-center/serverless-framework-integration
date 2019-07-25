# Twilio Runtime Serverless Plugin

Serverless plugin for Twilio Runtime

## Getting started

### Pre-requisites

- Node.js v8.10.0 (this is the runtime version supported by Azure Functions)
- Serverless CLI v1.48.0+. You can run npm i -g serverless if you don't already have it.
- A Twilio account. If you don't have one you can [sign up quickly](https://www.twilio.com/try-twilio).

### Create a new Twilio service

- Create a new service using the standard Node.js template, specifying a unique name for your app: `serverless create -t twilio-nodejs -p <appName>`
- `cd` into the generated app directory: `cd <appName>`
- Install the app's NPM dependencies, which includes this plugin: `npm install`

### Deploy, test, and diagnose your Twilio runtime

Now that you have the service on your local machine you can deploy your service using the credentials you find in [the Twilio Console](https://www.twilio.com/console/). The best way to pass these to the serverless command is to define environment variables in you `serverless.yml`

```yaml
service: your-service # update this with your service name

provider:
  name: twilio

  # Twilio access credentials (mandatory)
  config:
    accountSid: ${env:TWILIO_ACCOUNT_SID}
    authToken: ${env:TWILIO_AUTH_TOKEN}
```

Note that you have to configure `provider.config` with a `accountSid` and `authToken` property.

### Supported configuration - `serverless.yml`

The Twilio runtime supports the following configuration options as of today:

```yaml
# serverless.yml

# Name of your service
service: your-service

# Provider definition
# all functions and assets
# share the same provider configuration
provider:
  # Twilio runtime as your preferred provider
  name: twilio

  # Auth credentials which you'll find at twilio.com/console
  config:
    accountSid: ${env:TWILIO_ACCOUNT_SID}
    authToken: ${env:TWILIO_AUTH_TOKEN}

  # Dependency definitions similar
  # to dependencies in a package.json
  # -> these dependencies will be available in the
  #    Twilio Node.js runtime
  dependencies:
    asciiart-logo: '*'

  # Twilio runtime supports several domains
  # your functions and assets will be available under
  # -> defaulting to 'dev'
  environment: ${env:TWILIO_RUNTIME_ENV, 'dev'}

  # Environment variables passed to your functions
  # available in the Twilio runtim via `process.env`
  environmentVars:
    MY_MESSAGE: 'THIS IS cool stuff'

# Twilio runtime has to be added a plugin
plugins:
  - twilio-runtime

functions:
  # Function name
  hello-world:
    # Path to the JS handler function in the project (without file extension '.js')
    handler: functions/hello-world
    # URL path of the function after deployment
    path: /hello-world-path
    # visibility of the function (can be "public" or "protected")
    access: public

resources:
  assets:
    # Asset name
    asset-image:
      # path to the asset in the project
      filePath: assets/image.jpg
      # URL path to the asset after deployment
      path: image.jpg
      # visibility of the asset
      access: public
```

### Supported commands

#### `serverless deploy`

Deploy all functions and assets defined in your `serverless.yml`

```
$ serverless deploy

sls deploy
Serverless: Packaging service...
Serverless: Excluding development dependencies...
Serverless: twilio-runtime: Creating Service
Serverless: twilio-runtime: Configuring "final-test" environment
Serverless: twilio-runtime: Creating 2 Functions
Serverless: twilio-runtime: Uploading 2 Functions
Serverless: twilio-runtime: Creating 1 Assets
Serverless: twilio-runtime: Uploading 1 Assets
Serverless: twilio-runtime: Waiting for deployment.
Serverless: twilio-runtime: Waiting for deployment. Current status: building
Serverless: twilio-runtime: Setting environment variables
Serverless: twilio-runtime: Activating deployment
Serverless: twilio-runtime: Function available at: xxx-123-dev.twil.io/hello-world
Serverless: twilio-runtime: Asset available at: xxx-123-dev.twil.io/image.jpg

```

#### `serverless info`

Get information about the currently deployed runtime.

```
$ serverless info

Service information
*******

Service: xxx
Service Sid: ZS...

Environment: dev
Environment unique name: dev-environment
Environment Sid: ZE...
Environment domain name: xxx-123-dev.twil.io
Environment variables:
  FOO: BAR

Assets:
  access: public
  creation date: 2019-07-25T11:03:46Z
  path: /image.jpg
  sid: ZN...
  url: https://xxx-123-dev.twil.io/image.jpg

Functions:
  access: public
  creation date: 2019-07-25T11:03:31Z
  path: /hello-world
  sid: ZN...
  url: https://xxx-123-dev.twil.io/hello-world
```

#### `serverless invoke`

Invoke a deployed function and see the result. Define the function using `--function` or `-f` parameter.

```
$ serverless invoke --function hello-world

sls invoke -f hello-world

  ,--------------------------------------------------------------------------.
  |                                                                          |
  |    _____          _ _ _         ____              _   _                  |
  |   |_   _|_      _(_) (_) ___   |  _ \ _   _ _ __ | |_(_)_ __ ___   ___   |
  |     | | \ \ /\ / / | | |/ _ \  | |_) | | | | '_ \| __| | '_ ` _ \ / _ \  |
  |     | |  \ V  V /| | | | (_) | |  _ <| |_| | | | | |_| | | | | | |  __/  |
  |     |_|   \_/\_/ |_|_|_|\___/  |_| \_\\__,_|_| |_|\__|_|_| |_| |_|\___|  |
  |                                                                          |
  |                                                                          |
  |                                                           version 1.0.0  |
  |                                                                          |
  |                                                                          |
  `--------------------------------------------------------------------------'
```
