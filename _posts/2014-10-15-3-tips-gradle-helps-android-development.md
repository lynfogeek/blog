---
layout: post
title: 3 ways Gradle can help in your daily work
categories: [General, Android, Build system]
tags: [androiddev, gradle]
fullview: false
comments: true
---

This post is the first one of a small series of tips to optimize your development environment. And to start from the beginning, why not talking about our dear friend Gradle?

Here I will show you 3 easy customizations that make your daily life easier. I will explain you how to:

 - [Create multiples Build Variants](#buildvariants)
 - [Auto-increment your app’s version code and name after a build release](#autoincrement)
 - [Sign your application from a configuration file or from a prompt](#signapplication)

## <a name="buildvariants"></a> Create multiples Build Variants

 Gradle allows you to easily manage build variants. These ones are composed by a build type (Debug, Release) and product flavor you have created. Different product flavors can be used to build a free and pro version of your application, or different APKs according to the Android version, for example.

 I use these build variants to create different APKs for my applications in development and release mode. The apps have a different `applicationId`, different resources such as the launcher and the app name (helpful to know which variant you are using). They also connect to different web services and log the errors to the console or send them to Crashlytics, according to the flavor.

 In your build.gradle:

        android {
          productFlavors {
            prod {
                applicationId "com.example.myapplication"
            }
            staging {
              applicationId "com.example.myapplication.dev"
            }
          }
        }

![Build variant]({{site.url}}/assets/buildvariants.png)
Once your project has been synced in Android Studio, you will be able to choose the Build Variant on the bottom right of your IDE.

Don’t forget to create your prod/ and staging/ sub-folders in order to compile different source code and resources into your APK.

##  <a name="autoincrement"></a> Auto-increment your app’s version code and name

In order to not forget to increment the `versionCode` attribute in my `AndroidManifest.xml` before submitting an app on the Play Store, I have found and adapted the following solution to handle it automatically after each Release build.

To do so, first, include on the top of your build.gradle file the following import:

    import java.util.regex.Pattern

Then, copy this task and read the comment to understand it!

    task('increaseVersionCodeRelease') << { //create a new task
        def manifestFile = rootProject.file("app/src/main/AndroidManifest.xml") //locate your Manifest
        def manifestText = manifestFile.getText() //get the file's content
        //search for the current versionCode
        def pattern = Pattern.compile("versionCode=\"(\\d+)\"")
        def matcher = pattern.matcher(manifestText)
        matcher.find()
        // increment it and update the file
        def versionCode = Integer.parseInt(matcher.group(1)) +1
        def manifestContent = matcher.replaceAll("versionCode=\"" + versionCode + "\"")
        manifestFile.write(manifestContent)
        // get the new content
        manifestText = manifestFile.getText()

        // search for the current versionName
        def pattern2 = Pattern.compile("versionName=\"(\\d+).(\\d+).(\\d+)\"") //I use Semantic Versioning
        def matcher2 = pattern2.matcher(manifestText)
        matcher2.find()
        def versionCodeName = Integer.parseInt(matcher2.group(1))
        def versionCodeName2 = Integer.parseInt(matcher2.group(2))
        // increment it and update the file
        def versionCodeName3 = Integer.parseInt(matcher2.group(3)) +1
        def manifestContent2 = matcher2.replaceAll("versionName=\"" + versionCodeName +"." + versionCodeName2 +"." + versionCodeName3 + "\"")
        manifestFile.write(manifestContent2)
    }

In order to get this task executed when the ProdRelease build processes, add this:

    tasks.whenTaskAdded { task ->
        if (task.name == 'assembleProdRelease') {
            task.dependsOn 'increaseVersionCodeRelease'
        }
    }

**Note:** It only increments the latest number of the version and you could create another flavor with its own task to increment the first number of the version, when you are building a new major release.

## <a name="signapplication"></a> Sign your application from a configuration file or from a prompt

There are many ways to sign your application APK. And I will describe below 2 of them. The first one uses a configuration file. Create a file containing the following:

    storeFile=/path_to_your/keystore
    storePassword=p@ss
    keyAlias=MyAliasName
    keyPassword=p@ssw0rd

And then add this in your build.gradle.

    signingConfigs {
    release {
        def propsFile = rootProject.file('app/keys.properties')
        def props = new Properties()
        props.load(new FileInputStream(propsFile))

        storeFile file(props['storeFile'])
        storePassword props['storePassword']
        keyAlias props['keyAlias']
        keyPassword props['keyPassword']
    }
    }

Note that in this example my configuration file is named keys.properties and located into the `app` sub-directory.

If you don’t want your password to be saved in a file, another solution is ask the user to input it from the console:

    signingConfigs {
      release {
         storeFile file(System.console().readLine("\nEnter keystore path:"))
         storePassword System.console().readLine("\nEnter keystore password:")
         keyAlias System.console().readLine("\nEnter key alias: ")
         keyPassword System.console().readLine("\nEnter key password:")
      }
    }

If you want to learn more about Gradle you can read [the official guide](http://tools.android.com/tech-docs/new-build-system/user-guide).
Do you have some other tips to share? Feel free to comment below!
