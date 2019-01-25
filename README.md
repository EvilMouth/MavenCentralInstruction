# novoda-push
使用[novoda](https://github.com/novoda/bintray-release)上传项目到`bintray`并打包到`jcenter`

## 目录结构
```
.
├── README.md
├── build.gradle            // root build.gradle
├── gradle
│   └── push.gradle         // new file
├── gradle.properties       // root properties
├── library
│   ├── build.gradle        // module build.gradle
│   └── gradle.properties   // module properties
└── local.properties        // local file for configuring your key
```

## 使用说明

### 配置
- 根据目录结构添加或更新你的文件

### 上传
- 直接双击`fuck`命令（位于`Gradle projects`中`publishing`包下）
- 或者命令行执行```./gradlew fuck```

## 注意事项
- 使用`kotlin`编写的项目可能会遇到`.kt`文件无法生成`javadoc`情况，可以在`root build.gradle`文件下添加
```
tasks.getByPath(":your module:releaseAndroidJavadocs").enabled = false
```

- 同样`javadoc`问题
```
tasks.withType(Javadoc) {
    options.addStringOption('Xdoclint:none', '-quiet')
    options.addStringOption('encoding', 'UTF-8')
    options.addStringOption('charSet', 'UTF-8')
}
```

- gradlew权限问题
```gradle
chmod +x gradlew
```

## 快捷apply
```
apply from: 'https://raw.githubusercontent.com/izyhang/novoda-push/master/gradle/push.gradle'
```
