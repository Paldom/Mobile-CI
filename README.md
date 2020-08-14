# Mobile-CI

Continuous Integration and Delivery toolchain for native Android and iOS applications, built with Docker, Jenkins, Sonarqube, Fastlane, GitHub, Xcode, Android SDK, Cucumber and Firebase. Testing steps are based on a BDD workflow.

## BDD 

Automated BDD tests are written in Gherkin definitions (`.feature` files). Gherkin tests are implemented with native Android instrumented tests and iOS XCUITests, feature files are interpreted by GreenCoffee and Cucumberish.

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
3. Save build to artifactory (JFrog & GitHub in this case) with fastlane.
4. Upload build to Firebase App Distribution with fastlane.
5. Upload build to Play Store Beta and Testflight.
6. Create report.

## Architecture and docker images

- nginx: Used as a simple reverse proxy for this stack. But features of nginx can be expanded (e.g. with load balancing).
- Jenkins: Custom Jenkins master slave prepared with plugins and slaves for mobile DevOps. [dpal/docker-jenkins-mobile-ci](https://github.com/Paldom/docker-jenkins-mobile-ci)
- SonarQube: SonarQube with plugins installed for this mobile DevOps stack [dpal/docker-sonarqube-mobile-ci](https://github.com/Paldom/docker-sonarqube-mobile-ci)
- Artifactory: JFrog is used for current example. Nexus can be an alternative, but JFrog CLI commands need to be replaced.

## Requirements and prerequisites

Running this toolchain requires Docker, and a macOS slave with XCode and Android SDK installed.

## Instructions

1. Pull repository:

```sh
git clone https://github.com/Paldom/Mobile-CI.git
```

2. Run `docker-compose`:

```sh
docker-compose up -d
```

3. Set up a Jenkins slave

Follow the guide below about how to set up a Jenkins slave for Android and iOS on a physical device.

[Jenkins slave on Mac OS](https://github.com/Paldom/Mobile-CI/MAC_OS_JENKINS_SLAVE.md)

4. Configure Jenkins, environment variables, connect slave

Enable registration on Jenkins, create an admin user, after that enable security for login.

Find default configuration of `mac` slave and set the correct address, credentials and connection protocol.

Set up proper environment variable values for `mac` slave:

- `APPLE_ID` - Your Apple Developer ID.
- `FASTLANE_PASSWORD` - Your Apple Developer password.
- `ITC_TEAM_ID` - Itunes Connect Team ID.
- `KEYSTORE_ALIAS`, `KEYSTORE_ALIAS_PASSWORD`, `KEYSTORE_PASSWORD` - defaullt keystore setup, it is preferred to use different credentials for each of your android project.
- `MATCH_PASSWORD` - Password to encrypt/decrypt profiles generated with `match`.
- `TEAM_ID` - Apple Developer Team ID.

Set up the address of your Jenkins master at Manage Jenkins > Configure System > Jenkins Location > Jenkins URL.

5. Fork example repositories

Fork the following example repositories:
- **Android:** [Mobile-CI-Android-Example](https://github.com/Paldom/Mobile-CI-Android-Example)
- **iOS:** [Mobile-CI-iOS-Example](https://github.com/Paldom/Mobile-CI-iOS-Example)

Change the Team ID and Signing Identity according to your Apple Developer account in the `iOS` project.

6. Add your forked repositories to Jenkins, configure Jenkins to scan branches periodically. Commit and watch pipline running.

Add multi branch pipeline on Jenkins Admin, or with Jenkins Blue Interface (`/blue/create-pipeline`). Configure pipeline to scan branches periodically so that new branches or commits can trigger a run.