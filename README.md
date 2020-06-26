# Integrate Preflight with CircleCI

## Description
This documentation provides you, how to run or trigger your Preflight tests using CircleCI v2.1 orbs.   

## Prerequisites
To access Preflight api and run your Preflight tests, you need to get a Preflight Client Id and Client Secret from [Account Settings > API](https://app.preflight.com/account-settings/api) under your Preflight account.

`Client Id` : You should add it as an environment variable under your CircleCI account. Environment variable name should be `PF_CLIENT_ID`.
 
`Client Secret` : You should add as an environment variable under your CircleCI account. Environment variable name should be `PF_CLIENT_SECRET`. 

## Usage

You can run a test with below configuration

```
version: 2.1
orbs:
  preflight: preflight/test-runner@1.0
workflows:
  run-a-test:
    jobs:
      - preflight/run_a_test:
          testId: "<test-id>"
          environmentId: ""
          platforms: ""
          sizes: ""
          captureScreenshots: true
          waitResults: true
```

You can run a group of tests with below configuration.

```
version: 2.1
orbs:
  preflight: preflight/test-runner@1.0
workflows:
  run-group-test:
    jobs:
      - preflight/run_group_test:
          groupId: '<group-id>'
          environmentId: ""
          platforms: ""
          sizes: ""
          captureScreenshots: true
          waitResults: true
```

You can run all your PreFlight tests with below configuration.

```
version: 2.1
orbs:
  preflight: preflight/test-runner@1.0
workflows:
  run-all-tests:
    jobs:
      - preflight/run_all_tests:
          environmentId: ""
          platforms: ""
          sizes: ""
          captureScreenshots: true
          waitResults: true
```


`testId` : Pass the Test Id to run. If test id and group id are not passed all the tests will be run.

`groupId` : Pass the Group Id to run. If test id and group id are not passed all the tests will be run. You can get it from [Test Settings > Groups](https://app.preflight.com/tests/settings/groups) under your PreFlight account.

`environmentId (optional)` : Environment ID for your test group. You can get it from [Test Settings > Environments](https://app.preflight.com/tests/settings/environments) under your PreFlight account.

`platforms (optional)` : Platforms and browsers you want to run your PreFlight tests.  
  * Example usage `win-chrome`
  * You can pass more than one browser option. Ex. `win-chrome, win-firefox`
  * Platform options : `win`
  * Browser options : `chrome`, `ie`, `edge`, `firefox`

`sizes (optional)` :  Size you want to run your PreFlight tests.
  * Example usage. (WidthxHeight) `1440x900`
  * You can pass more than one size option. Ex. `1920x1080, 1440x900`
  * Size options : `1920x1080, 1440x900, 1024x768, 480x640`

`captureScreenshots (optional)` :  Enables taking screenhots of the each step.

`waitResults (optional)` :  If you set it as `true`, your build waits your PreFlight test results.


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
      `circleci orb publish orb.yml preflight/test-runner@<version-number>`
      or
      `circleci orb publish increment orb.yml preflight/test-runner patch`

