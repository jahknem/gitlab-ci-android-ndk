Fork for older NDK Version.

## About
This Docker image contains the Android SDK, NDK and most common packages necessary for building Android apps in a CI tool like GitLab CI. Make sure your CI environment's caching works as expected, this greatly improves the build time, especially if you use multiple build jobs. It is based on [jangrewe/gitlab-ci-android](https://github.com/jangrewe/gitlab-ci-android) with the NDK and CMake added as additional packages.

Obligatory [Docker Hub] (https://hub.docker.com/r/jahknem/gitlab-ci-android-ndk) link.

## Usage
A `.gitlab-ci.yml` with caching of your project's dependencies would look like this:

```
image: jahknem/gitlab-ci-android-ndk:latest

variables:
  GRADLE_OPTS: "-Dorg.gradle.daemon=false"

before_script:
- export GRADLE_USER_HOME=$(pwd)/.gradle
- chmod +x ./gradlew

cache:
  key: ${CI_COMMIT_REF_NAME}
  paths:
  - .gradle/

stages:
  - build

build:
  stage: build
  script:
  - ./gradlew assembleDebug
  artifacts:
    paths:
    - app/build/outputs/apk/*.apk
```
