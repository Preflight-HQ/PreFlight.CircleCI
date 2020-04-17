# Integrate Preflight with CircleCI

## Description
This documentation provides you, how to run or trigger your Preflight tests using CircleCI v2.1 orbs.   

## Prerequisites
To access Preflight api and run your Preflight tests, you need to get a Preflight Client Id and Client Secret from [Account Settings > API](https://app.preflight.com/account-settings/api) under your Preflight account.

## Usage
This is full example configuration

```
version: 2.1
orbs:
  preflight: preflight/test-runner@1.0.1
jobs:
  build:
    docker: 
      - image: circleci/node:9.0.0
    steps:
      - preflight/run-tests:
          clientId: '<preflight-client-id>'
          clientSecret: '<preflight-client-secret>'
          groupId: '<test-group-id>'
          environmentId: '<test-environment-id>'
          platforms: '[{ "platform": "windows", "browser": "chrome" }]'
          sizes: '[{"width":1440,"height":900}]'
          captureScreenshots: true
          waitResults: false
```

`clientId (optional)` : Preflight Client Id. You can pass as a paramater or you can add it as an environment variable under your CircleCI account. Environment name should be `PF_CLIENT_ID`. This is optional if you set a client id environment variable under your CircleCI account. Otherwise you should pass it as a parameter.
 
`clientSecret (optional)` : Preflight Client Secret. You can pass as a paramater or you can add it as an environment variable under your CircleCI account. Environment name should be `PF_CLIENT_SECRET`. This is optional if you set a client secret environment variable under your CircleCI account. Otherwise you should pass it as a parameter.

`groupId (required)` : Preflight group id. You can get it from [Test Settings > Groups](https://app.preflight.com/tests/settings/groups) under your Preflight account.

`environmentId (optional)` : Preflight environment id. You can get it from [Test Settings > Environments](https://app.preflight.com/tests/settings/environments) under your Preflight account.

`platforms (optional)` : Platforms and browsers you want to run your Preflight tests.  
  * Example usage `[{ "platform": "windows", "browser": "chrome" }]`
  * You can pass more than one browser option. Ex. `[{ "platform": "windows", "browser": "chrome" }, { "platform": "windows", "browser": "firefox" }]`
  * Platform options : `"windows"`, `"mac"(Temporary out of service)`
  * Browser options : `"chrome"`, `"internetexplorer"`, `"edge"`, `"firefox"`, `"safari"(Temporary out of service)`

`sizes (optional)` :  Size you want to run your Preflight tests.
  * Example usage `[{"width":1440,"height":900}]`
  * You can pass more than one size option. Ex. `[{"width":1920,"height":1080}, {"width":1440,"height":900}]`
  * Size options : `{"width":1920,"height":1080}` `{"width":1440,"height":900}` `{"width":1024,"height":768}` `{"width":480,"height":640}`

`captureScreenshots (optional)` :  Enables taking screenhots of the each step. Default value is `true`

`waitResults (optional)` :  If you set it as `true`, your CircleCI build waits your Preflight test results. Default value is `false`


## Developer Notes

###### CircleCI Orb Creation
  1. Download CircleCI Cli from command line (Mac).
    `curl -fLSs https://circle.ci/cli | bash`

  2. Create an api token in the CircleCI [account](https://circleci.com/account/api)

  3. Open a CircleCI terminal Set api token with command line. Run the command
    `circleci setup`

  4. Run this commands to create a namespace
    `circleci namespace create <namespace_name> github <org-name>`

  5. Create an orb under CircleCI Registry
    `circleci orb create <namespace_name>/<method-name> (for ex. preflight/test-runner)`


###### Deployment
If you need to change something in the orb.yml file, you can follow below steps for new version. 
  
  1. Open a terminal and navigate to your source code.
  
  2. Run this command to publish CircleCI Registry.
      `circleci orb publish orb.yml preflight/test-runner@1.0.0`

