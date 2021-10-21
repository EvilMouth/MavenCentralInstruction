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
}
ext {
    PUBLISH_GROUP_ID = 'net.evilmouth'
    PUBLISH_VERSION = '1.0.0-SNAPSHOT'
}
apply from: 'https://raw.githubusercontent.com/EvilMouth/MavenCentralInstruction/1.0.0/scripts/publish-root.gradle'
```

### 2.2 module/build.gardle

add below codeblock in end of every lib-module

```groovy
ext {
    PUBLISH_ARTIFACT_ID = project.name or custom
}
apply from: 'https://raw.githubusercontent.com/EvilMouth/MavenCentralInstruction/1.0.0/scripts/publish-module.gradle'
```

## optional

### helper.gradle

```groovy
buildscripts {
  ext {
      BUILD_WITH_REMOTE_MAVEN = true
      BUILD_WITH_LOCAL_MAVEN = true
  }
  apply from: 'https://raw.githubusercontent.com/EvilMouth/MavenCentralInstruction/1.0.0/scripts/helper.gradle'
}
```
