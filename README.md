# novoda-push
使用[novoda](https://github.com/novoda/bintray-release)上传项目到`bintray`并打包到`jcenter`

## 目录结构
```
.
├── README.md
├── build.gradle        // root build.gradle
├── gradle
│   └── push.gradle     // add this file to your path
├── library
│   └── build.gradle    // your library build.gradle
└── local.properties    // configure your key
```

## 使用说明
- 直接双击`fuck`命令（位于`Gradle projects`中`publishing`包下）
- 或者命令行执行```./gradlew fuck```

## 注意事项
使用`kotlin`编写的项目可能会遇到`.kt`文件无法生成`javadoc`情况，可以在`root build.gradle`文件下添加
```
tasks.getByPath(":your module:releaseAndroidJavadocs").enabled = false
```
