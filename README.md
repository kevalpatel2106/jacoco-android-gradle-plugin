# jacoco-android-gradle-plugin
[![Gradle Plugin Portal](https://img.shields.io/maven-metadata/v/https/plugins.gradle.org/m2/io.github.kevalpatel2106.gradle/jacoco-android/maven-metadata.xml.svg?label=Gradle%20Plugin%20Portal)](https://plugins.gradle.org/plugin/com.kevalpatel2106.gradle.jacoco-android)

A Gradle plugin that adds fully configured `JacocoReport` tasks for unit tests of each Android application and library project variant. See the readme for the original plugin [here](https://github.com/arturdm/jacoco-android-gradle-plugin).

### This is the fork of [jacoco-android-gradle-plugin](https://github.com/arturdm/jacoco-android-gradle-plugin) by [arturdm](https://github.com/arturdm)
### How it is different from the original plugin?

This plugin fixes the changes related to report configs, that breaks the build for the original plugin ([v0.1.5](https://github.com/arturdm/jacoco-android-gradle-plugin/releases/tag/jacoco-android-release-0.1.5)) Gradle 8+. The changes can be found in [this](https://github.com/kevalpatel2106/jacoco-android-gradle-plugin/commit/d3a1ede7460d289b1ed47e274e7bbcb17fbbb1fb) commit. 


## Usage
```groovy
buildscript {
  repositories {
    ...
    maven { url 'https://plugins.gradle.org/m2/' }
  }
  dependencies {
    ...
    classpath 'io.github.kevalpatel2106.gradle:jacoco-android:0.1.5'
  }
}

apply plugin: 'com.android.application'
apply plugin: 'io.github.kevalpatel2106.gradle.jacoco-android'

jacoco {
  toolVersion = "0.8.4"
}

tasks.withType(Test) {
  jacoco.includeNoLocationClasses = true
}

android {
  ...
  productFlavors {
    free {}
    paid {}
  }
}
```

The above configuration creates a `JacocoReport` task for each variant and an additional `jacocoTestReport` task that runs all of them.
```
jacocoTestPaidDebugUnitTestReport
jacocoTestFreeDebugUnitTestReport
jacocoTestPaidReleaseUnitTestReport
jacocoTestFreeReleaseUnitTestReport
jacocoTestReport
```

The plugin excludes Android generated classes from report generation by default. You can use `jacocoAndroidUnitTestReport` extension to add other exclusion patterns if needed.
```groovy
jacocoAndroidUnitTestReport {
  excludes += ['**/AutoValue_*.*',
              '**/*JavascriptBridge.class']
}
```

You can also toggle report generation by type using the extension.
```groovy
jacocoAndroidUnitTestReport {
  csv.required false
  xml.required true
  html.outputLocation = layout.buildDirectory.dir('jacocoHtml')
}
```

By default your report will be in `[root_project]/[project_name]/build/jacoco/`
But you can change the local reporting directory :
```groovy
jacocoAndroidUnitTestReport {
  destination "/path/to/the/new/local/directory/"
}
```

To generate all reports run:
```shell
$ ./gradlew jacocoTestReport
```

Reports for each variant are available at `$buildDir/reports/jacoco` in separate subdirectories, e.g. `build/reports/jacoco/jacocoTestPaidDebugUnitTestReport`.

## Examples
* [example](example)
* https://github.com/codecov/example-android
* https://github.com/devinciltd/lib
