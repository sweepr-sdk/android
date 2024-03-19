# Sweepr Mobile SDK

`SweeprMobile` is a framework to embed personalized consumer care into existing products. This repository contains the packages of the latest SDK as a dependencies, that could be referenced via Gradle.

For more, please visit our [website](https://sweepr.com/).

## Installation

### Gradle (Groovy DSL)

Following instructions are good for classic Android projects, where whole syntax was based on Groovy. They show, how to setup a reference to proper GitHub Package Repository with Sweepr Framework AAR files and how to add it as an application dependency.

1. Declare reference to Package Repository hosted on GitHub, applied inside the global `build.gradle` file (from the `root`, NOT from the `app` subfolder):

    ```gradle

    // top declaration to read credentials
    def sweeprProperties = new Properties()
    def sweeprPropertiesFile = rootProject.file("sweepr.properties")

    if (sweeprPropertiesFile.exists()) {
        sweeprProperties.load(new FileInputStream(sweeprPropertiesFile))
    } else {
        sweeprProperties.setProperty('github.username', System.getenv("GPR_USER") ?: "undefined-user")
        sweeprProperties.setProperty('github.token', System.getenv("GPR_API_KEY") ?: "undefined-token")
    }

    project.ext.sweeprProperties = sweeprProperties

    // ...
    allprojects {
        repositories {
            // ...
            maven {
                name = "SweeprGitHubPackages"
                url = uri("https://maven.pkg.github.com/sweepr-sdk/android")
                credentials {
                    username = sweeprProperties["github.username"]
                    password = sweeprProperties["github.token"]
                }
            }
        }
    }
    ```

    > This way credentials could be passed via proper values defined in `sweepr.properties` or through environment variables `GPR_USER` & `GPR_API_KEY`.

    Expected `sweepr.properties` file content is:

    ```text
    github.username=<login>
    github.token=<github_token>
    ```

2. Add proper dependency inside the application's `build.gradle` file:

    ```gradle
    // ...
    dependencies {
        // ...

        // Sweepr SDK
        implementation "com.sweepr:mobile-framework:2.4.5"
    }
    ```

### Gradle (Kotlin DSL)

Those instructions are suitable for new Android types of projects, which are based on Gradle with Kotlin-DSL-enabled syntax. Some sections have to be extended to let the project point to proper online repository hosted on GitHub with Sweepr Framework AAR files and add a dependency to application's bundle itself.

1. Declare reference to Package Repository hosted on GitHub, apply it inside the `settings.gradle.kts`:

    ```gradle

    // top declaration to read credentials
    val sweeprProperties = java.util.Properties()
    val sweeprPropertiesFile = File(rootProject.projectDir.absolutePath + "/sweepr.properties")

    if (sweeprPropertiesFile.exists()) {
        sweeprProperties.load(java.io.FileInputStream(sweeprPropertiesFile))
    } else {
        sweeprProperties.setProperty("github.username", System.getenv("GPR_USER") ?: "undefined-user")
        sweeprProperties.setProperty("github.token", System.getenv("GPR_API_KEY") ?: "undefined-token")
    }

    // ...
    dependencyResolutionManagement {
        // ...
        repositories {
            // ...
            maven {
                name = "SweeprGitHubPackages"
                url = uri("https://maven.pkg.github.com/sweepr-sdk/android")
                credentials {
                    username = sweeprProperties.getProperty("github.username")
                    password = sweeprProperties.getProperty("github.token")
                }
            }
        }
    }

    ```

    > This way credentials could be passed via proper values defined in `sweepr.properties` or through environment variables `GPR_USER` & `GPR_API_KEY`.

    Expected `sweepr.properties` file content is:

    ```text
    github.username=<login>
    github.token=<github_token>
    ```

1. Declare required versions inside the `libs.versions.toml` file:

    ```toml
    [versions]
    sweeprMobileFramework = "2.4.5"

    [libraries]
    sweepr-mobileframework = { group = "com.sweepr", name = "mobile-framework", version.ref = "sweeprMobileFramework" }
    ```

1. And finally, add a proper dependency inside the application's `build.gradle.kts`:

    ```gradle
    // ...
    dependencies {
        // ...
        implementation(libs.sweepr.mobileframework)
    }
    ```

-----
Sweepr Technologies Limited (2024)
