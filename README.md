# novoda-push
使用[novoda](https://github.com/novoda/bintray-release)上传项目到`bintray`并打包到`jcenter`

## 目录结构
```
.
├── README.md
├── build.gradle        // root build.gradle
├── gradle
│   ├── push.gradle     // add this file to your path
│   └── push.sh         // add this file to your path
├── library
│   └── build.gradle    // your library build.gradle
└── local.properties    // configure your key
```

## 使用说明
- 直接双击`gradle projects`中`fuck`命令
- 或者命令行执行```./gradlew fuck```
