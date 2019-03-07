# Mobile-CI

Continuous Integration and Delivery toolchain for native Android and iOS applications, built with Docker, Jenkins, Sonarqube, Fastlane, GitHub, XCode, Android SDK, Appium, Cucumber, AWS Device Farm and Firebase. CI flow is based on a BDD workflow.

## BDD 

Automated BDD tests are written in Gherkin definitions (.feature files). Gherkin tests are implemented with native Android instrumented tests and iOS XCUITests, feature files are interpreted by GreenCoffee and Cucumberish.

### Gherkin example

Following test feature definitions are used for this example: [Mobile-CI-Gherkin](https://github.com/Paldom/Mobile-CI-Gherkin)

## Mobile applications

Test feature runs on following mobile applications:

- **Android:** [Mobile-CI-Android-Example](https://github.com/Paldom/Mobile-CI-Android-Example)
- **iOS:** [Mobile-CI-iOS-Example](https://github.com/Paldom/Mobile-CI-iOS-Example)

## Pipeline

CI and CD pipelines (defined in jenkinsfile) contain the following steps:

### CI Pipeline

Trigger: new `feature` branch (with pull request on `develop` branch).

1. Code analysis and lint check with Sonarqube.
2. Run Unit tests.
3. Run mocked UI test (tagged with @smoke) in cloud with native instrumented test.
4. Report feedback to GitHub.

### CD Pipline - TBD

Trigger: new `release` branch.

1. Code analysis and lint check with Sonarqube.
2. Run full e2e regression testsuit (all Gherkin tests) native instrumented test.
3. Save build to artifactory (GitHub in this case) with fastlane.
4. Upload build to Fabric with fastlane.
5. Upload build to Play Store Beta and Testflight.
6. Create report.

## Requirements and prerequisites

Running this toolchain requires Docker, and a macOS slave with XCode and Android SDK installed.

## Instructions

1. Pull repository:

```sh
git clone https://github.com/Paldom/Mobile-CI.git
```

2. Run `docker-compose`:

```sh
docker-compose up
```
