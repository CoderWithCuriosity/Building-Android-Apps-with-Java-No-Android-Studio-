Getting Started With Java
To build Android apps with Java, you need:
1️⃣ Android SDK (Command-line only)
2️⃣ Environment variables
3️⃣ A minimal Android project
4️⃣ Build APK (debug)
5️⃣ Install on phone (test)first, start by checking Java Version, Javac Version

STEP 1: CONFIRM JAVA IS READY

java -version
javac -version

❌ If not found:
Install JDK 11 or JDK 17
After install, re-open CMD


STEP 2: INSTALL ANDROID SDK (NO STUDIO)
2.1 Download Android Command Line Tools

Download Android SDK Command Line Tools (Windows) from Google.

Extract to:

C:\Android\cmdline-tools\latest\


Folder must look like:

C:\Android
 └─ cmdline-tools
    └─ latest
       ├─ bin
       ├─ lib

Some devices are like this C:\Users\USER\AppData\Local\Android\ like in the AppData\Local\Android that's where the android is.

Set 3: Set Environment Variables
setx ANDROID_HOME "C:\Android" 

or You can use your AppData\Local, it depends though, it's because i have installed android studio, that's why my android home could be in that app data.

setx PATH "%PATH%;%ANDROID_HOME%\platform-tools;%ANDROID_HOME%\cmdline-tools\latest\bin"

Step3: Install Required SDK Packages
sdkmanager "platform-tools" "platforms;android-34" "build-tools;34.0.0"
sdkmanager --licenses


Step 4: Confirm ADB is installed
Verify:
adb version

if it adb isn't included, just use download adb and set it's environment variables

STEP 3: CREATE A JAVA ANDROID PROJECT (MINIMAL)

Create folders:

MyJavaApp/
 ├─ app/
 │  └─ src/main/java/com/example/app/MainActivity.java
 │  └─ src/main/AndroidManifest.xml
 ├─ build.gradle
 └─ settings.gradle

 3.1 AndroidManifest.xml
<manifest package="com.example.app"
    xmlns:android="http://schemas.android.com/apk/res/android">

    <application
        android:label="JavaApp">

        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>

    </application>
</manifest>


3.2 MainActivity.java (NO XML UI)
package com.example.app;

import android.app.Activity;
import android.os.Bundle;
import android.widget.TextView;

public class MainActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        TextView tv = new TextView(this);
        tv.setText("Hello Native Java Android!");
        tv.setTextSize(24);

        setContentView(tv);
    }
}


STEP 4: SET UP GRADLE (JAVA ONLY)
4.1 settings.gradle
rootProject.name = "MyJavaApp"
include ':app'



4.2 Root build.gradle
buildscript {
    repositories {
        google()
        mavenCentral()
    }
    dependencies {
        classpath "com.android.tools.build:gradle:8.2.0"
    }
}

4.3 app/build.gradle
plugins {
    id 'com.android.application'
}

android {
    namespace "com.example.app"
    compileSdk 34

    defaultConfig {
        applicationId "com.example.app"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }
}

dependencies {
}

STEP 5: BUILD REAL APK (DEBUG)

but before that 
You don’t have the Gradle Wrapper yet. 

gradlew is not a global command.

It only exists if:

You generated the Gradle Wrapper, or

You downloaded it manually

Right now your project folder does not contain:

gradlew
gradlew.bat
gradle/wrapper/


So Windows can’t find it.

FIX OPTION A (RECOMMENDED): CREATE GRADLE WRAPPER
STEP 1: CHECK IF GRADLE IS INSTALLED GLOBALLY

Run:

gradle -v

If you see version info → GOOD
If NOT recognized → install Gradle first
STEP 2: INSTALL GRADLE (IF NEEDED)

Download Gradle Binary ZIP

Extract to:

C:\Gradle


You should have:

C:\Gradle\bin\gradle.bat


Add to PATH:

setx PATH "%PATH%;C:\Gradle\bin"


Close & reopen CMD.

Verify:

gradle -v

STEP 3: GENERATE GRADLE WRAPPER

From your project root (android folder):

gradle wrapper


if there's an error run 

Run:

gradle init

When prompted, choose:

Type of project: basic

Language: Groovy

DSL: Groovy

Project name: android

Generate build scans: no

After this, your folder will contain:

build.gradle
settings.gradle
gradle/


✅ Now it is a valid Gradle project.

Example: 
PS C:\Users\USER\Desktop\Projects\android> gradle init

Found existing files in the project directory: 'C:\Users\USER\Desktop\Projects\android'.
Directory will be modified and existing files may be overwritten.  Continue? (default: no) [yes, no] yes

Select type of build to generate:
  1: Application
  2: Library
  3: Gradle plugin
  4: Basic (build structure only)
Enter selection (default: Application) [1..4] 4

Project name (default: android): 

Select build script DSL:
  1: Kotlin
  2: Groovy
Enter selection (default: Kotlin) [1..2] 2

Generate build using new APIs and behavior (some features may change in the next minor release)? (default: no) [yes, no] no


> Task :init
Learn more about Gradle by exploring our Samples at https://docs.gradle.org/9.2.1/samples

BUILD SUCCESSFUL in 57s
1 actionable task: 1 executed


This will create:

gradlew
gradlew.bat
gradle/
 └─ wrapper/
    ├─ gradle-wrapper.jar
    └─ gradle-wrapper.properties

STEP 4: BUILD USING WRAPPER (WINDOWS)

Now run:

gradlew.bat assembleDebug


or:

.\gradlew assembleDebug


✅ This will work now.

From project root:

gradlew assembleDebug

if an error occur, this may be due to This is NOT an error anymore — PowerShell is just being strict.

PowerShell does not run local files automatically.

before running the gradlew command ensure you have include(":app") in the settings.gradle.
if you don't have then add it 

include(":app")

PS C:\Users\USER\Desktop\Projects\android> .\gradlew projects
Calculating task graph as no cached configuration is available for tasks: projects

> Task :projects

Projects:

------------------------------------------------------------
Root project 'android'
------------------------------------------------------------

Location: C:\Users\USER\Desktop\Projects\android

Project hierarchy:

Root project 'android'
\--- Project ':app'

Project locations:

project ':app' - \app

To see a list of the tasks of a project, run gradlew <project-path>:tasks
For example, try running gradlew :app:tasks

BUILD SUCCESSFUL in 3s
1 actionable task: 1 executed
Configuration cache entry stored.
PS C:\Users\USER\Desktop\Projects\android> .\gradlew :app:tasks                                                                             
Calculating task graph as no cached configuration is available for tasks: :app:tasks                                                        

> Task :app:tasks

------------------------------------------------------------
Tasks runnable from project ':app'
------------------------------------------------------------

Help tasks
----------
artifactTransforms - Displays the Artifact Transforms that can be executed in project ':app'.
buildEnvironment - Displays all buildscript dependencies declared in project ':app'.
dependencies - Displays all dependencies declared in project ':app'.
dependencyInsight - Displays the insight into a specific dependency in project ':app'.
help - Displays a help message.
javaToolchains - Displays the detected java toolchains.
outgoingVariants - Displays the outgoing variants of project ':app'.
projects - Displays the sub-projects of project ':app'.
properties - Displays the properties of project ':app'.
resolvableConfigurations - Displays the configurations that can be resolved in project ':app'.
tasks - Displays the tasks runnable from project ':app'.

To see all tasks and more detail, run gradlew tasks --all

To see more detail about a task, run gradlew help --task <task>

BUILD SUCCESSFUL in 2s
1 actionable task: 1 executed
Configuration cache entry stored.

.\gradlew --stop
.\gradlew clean --refresh-dependencies

.\gradlew :app:tasks --all


add android:exported="true" due to In Android Gradle Plugin (AGP) 8+, you should NOT set package in the manifest if you already have namespace in build.gradle.

then run

.\gradlew :app:assembleDebug




Output:

app/build/outputs/apk/debug/app-debug.apk


🎉 REAL ANDROID APK BUILT USING JAVA ONLY

Next steps

Test it on a device or emulator:

Copy the APK to your phone or use adb install app-debug.apk.

Clean up warnings (optional but recommended):

Remove package="com.example.app" from your AndroidManifest.xml since the namespace is already set in app/build.gradle.

Build release APK when ready:

.\gradlew :app:assembleRelease



what i notice is tht the app/build.gradle has this
plugins {
    id 'com.android.application'
}

android {
    namespace "com.example.app"
    compileSdk 34

    defaultConfig {
        applicationId "com.example.app"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }
}

dependencies {
}



and the setting.gradle is 
rootProject.name = "MyJavaApp"
include ':app'


and the com/example/app there is 
plugins {
    id 'com.android.application'
}

android {
    namespace "com.example.app"
    compileSdk 34

    defaultConfig {
        applicationId "com.example.app"
        minSdk 21
        targetSdk 34
        versionCode 1
        versionName "1.0"
    }
}

dependencies {
}



and in the root the build.gradle
buildscript {
    repositories {
        google()
        mavenCentral()
    }
    dependencies {
        classpath "com.android.tools.build:gradle:8.2.0"
    }
}

allprojects {
    repositories {
        google()
        mavenCentral()
    }
}



and the setings.gradle is 
/*
 * This file was generated by the Gradle 'init' task.
 *
 * The settings file is used to specify which projects to include in your build.
 * For more detailed information on multi-project builds, please refer to https://docs.gradle.org/9.2.1/userguide/multi_project_builds.html in the Gradle documentation.
 */

rootProject.name = 'android'
include(":app")


that is what i noticed on the functioning app

PART 5: TEST ON REAL PHONE
Enable Developer Mode

Settings → About Phone

Tap Build Number 7 times

Enable USB Debugging

cd app/build/outputs/apk/debug/
then you install
Install APK
adb install app-debug.apk


Or just click APK → install.

then for the release 
.\gradlew :app:assembleRelease 