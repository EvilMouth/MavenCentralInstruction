![GitHub Workflow Status](https://img.shields.io/github/workflow/status/evilmouth/mavencentralinstruction/Publish)
![GitHub tag (latest by date)](https://img.shields.io/github/v/tag/evilmouth/mavencentralinstruction?label=latest_tag)

# How to publish lib to MavenCentral

## 1. config properties

> this will require **sonatype account** and **gpg key**, you can find it [here](https://getstream.io/blog/publishing-libraries-to-mavencentral-2021/)

in local.properties

```properties
signing.keyId=xxx // gpg public key
signing.password=xxx
signing.key=xxx // gpg private key base64 encode
ossrhUsername=xxx // sonatype account
ossrhPassword=xxx
sonatypeStagingProfileId=xxx // sonatype nexus oss staging profile id
```

or env

- SIGNING_KEY_ID=xxx
- SIGNING_PASSWORD=xxx
- SIGNING_KEY=xxx
- OSSRH_USERNAME=xxx
- OSSRH_PASSWORD=xxx
- SONATYPE_STAGING_PROFILE_ID=xxx

## 2. config gradle scripts

### 2.1 root/build.gradle

```groovy
buildscript {
  ...
}

// add below codeblock after buildscript{}
plugins {
    id("io.github.gradle-nexus.publish-plugin") version "1.1.0"
    id("org.jetbrains.dokka") version "1.5.30" // for javadoc
}
ext {
    PUBLISH_GROUP_ID = 'net.evilmouth'
    PUBLISH_VERSION = '1.0.0-SNAPSHOT'
    
    // optional
    PUBLISH_LICENSE_NAME = your project lincense // default APACHE LICENSE, VERSION 2.0
    PUBLISH_LICENSE_URL = your project lincense url
    PUBLISH_PUBLISH_DEVELOPER_NAME = your name
    PUBLISH_PUBLISH_DEVELOPER_EMAIL = your email
}
apply from: 'https://raw.githubusercontent.com/EvilMouth/MavenCentralInstruction/1.4.0/scripts/publish-root.gradle'
```

### 2.2 module/build.gardle

add below codeblock in end of every lib-module

```groovy
ext {
    PUBLISH_ARTIFACT_ID = project.name or custom
}
apply from: 'https://raw.githubusercontent.com/EvilMouth/MavenCentralInstruction/1.4.0/scripts/publish-module.gradle'
```

## optional

### apply helper script

```groovy
// in root/build.gradle
buildscripts {
  ext {
      BUILD_WITH_REMOTE_MAVEN = true
      BUILD_WITH_LOCAL_MAVEN = true
  }
  apply from: 'https://raw.githubusercontent.com/EvilMouth/MavenCentralInstruction/1.4.0/scripts/helper.gradle'
}
```

### Continuous integration with Github Actions

```yml
name: Publish

on:
  release:
    # We'll run this workflow when a new GitHub release is created
    types: [released]

jobs:
  publish:
    name: Release build and publish
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@v2
      - name: Set up JDK 11
        uses: actions/setup-java@v2
        with:
          distribution: adopt
          java-version: 11

        # Builds the release artifacts of the library
      - name: Release build
        run: ./gradlew assembleRelease

        # Generates other artifacts (javadocJar is optional)
      - name: Source jar and dokka
        run: ./gradlew sourcesJar javadocJar

        # Runs upload, and then closes & releases the repository
      - name: Publish to MavenCentral
        run: ./gradlew publishReleasePublicationToSonatypeRepository --max-workers 1 closeAndReleaseSonatypeStagingRepository
        env:
          OSSRH_USERNAME: ${{ secrets.OSSRH_USERNAME }}
          OSSRH_PASSWORD: ${{ secrets.OSSRH_PASSWORD }}
          SIGNING_KEY_ID: ${{ secrets.SIGNING_KEY_ID }}
          SIGNING_PASSWORD: ${{ secrets.SIGNING_PASSWORD }}
          SIGNING_KEY: ${{ secrets.SIGNING_KEY }}
          SONATYPE_STAGING_PROFILE_ID: ${{ secrets.SONATYPE_STAGING_PROFILE_ID }}
```
