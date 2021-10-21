# How to publish lib to MavenCentral

## 1. config local.properties

```properties
signing.keyId=xxx
signing.password=xxx
signing.key=xxx
ossrhUsername=xxx
ossrhPassword=xxx
sonatypeStagingProfileId=xxx
```

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
