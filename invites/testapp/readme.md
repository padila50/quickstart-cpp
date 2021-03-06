Firebase Invites Quickstart
==============================

The Firebase Invites Test Application (testapp) demonstrates both
sending and receiving Firebase Invites using the Firebase Invites C++
SDK. This application has no user interface and simply logs actions
it's performing to the console.

Introduction
------------

- [Read more about Firebase Invites](https://firebase.google.com/docs/invites/)
- [Read more about Firebase Dynamic Links](https://firebase.google.com/docs/dynamic-links/)

Building and Running the testapp
--------------------------------

### iOS
  - Link your iOS app to the Firebase libraries.
    - Get CocoaPods version 1 or later by running,
        ```
        $ sudo gem install cocoapods --pre
        ```
    - From the testapp directory, install the CocoaPods listed in the Podfile
      by running,
        ```
        $ pod install
        ```
    - Open the generated Xcode workspace (which now has the CocoaPods),
        ```
        $ open testapp.xcworkspace
        ```
    - For further details please refer to the
      [general instructions for setting up an iOS app with Firebase](https://firebase.google.com/docs/ios/setup).
  - Register your iOS app with Firebase.
    - Create a new app on
      [firebase.google.com/console](https://firebase.google.com/console/),
      and attach your iOS app to it.
      - For Invites, you will need an App Store ID. Use something random such
        as 12345678."
      - You can use "com.google.ios.invites.testapp" as the iOS Bundle ID
        while you're testing.
    - Add the GoogleService-Info.plist that you downloaded from Firebase
      console to the testapp root directory. This file identifies your iOS app
      to the Firebase backend.
    - Firebase Invites uses Google Sign-In to send invites needs to be
      configured.
      - Enable the Keychain Sharing capability on iOS 10 or above.
        You can enable this capability on your project in Xcode 8 by going to
        your project's settings, Capabilities, and turning on Keychain Sharing.
      - Configure a URL type to handle the Google Sign-In callback.
        In your project's Info tab, under the URL Types section, find the URL
        Schemes box containing YOUR\_REVERSED\_CLIENT\_ID. Replace this with the
        value of the REVERSED\_CLIENT\_ID string in GoogleService-Info.plist.
    - Configure the app to handle dynamic links / app invites.
      - In your project's Info tab, under the URL Types section, find the URL
        Schemes box and add your app's bundle ID (the default scheme used
        by dynamic links).
      - Copy the dynamic links domain for your project under the Dynamic Links
        tab of the [Firebase console](https://firebase.google.com/console/)
        Then, in your project's Capabilities tab:
        - Enable the Associated Domains capability.
        - Add applinks:YOUR_DYNAMIC_LINKS_DOMAIN
          For example "applinks:xyz.app.goo.gl".

  - Download the Firebase C++ SDK linked from
    [https://firebase.google.com/docs/cpp/setup]() and unzip it to a
    directory of your choice.
  - Add the following frameworks from the Firebase C++ SDK to the project:
    - frameworks/ios/universal/firebase.framework
    - frameworks/ios/universal/firebase_invites.framework
    - You will need to either,
       1. Check "Copy items if needed" when adding the frameworks, or
       2. Add the framework path in "Framework Search Paths"
          - e.g. If you downloaded the Firebase C++ SDK to
            `/Users/me/firebase_cpp_sdk`,
            then you would add the path
            `/Users/me/firebase_cpp_sdk/frameworks/ios/universal`.
          - To add the path, in XCode, select your project in the project
            navigator, then select your target in the main window.
            Select the "Build Settings" tab, and click "All" to see all
            the build settings. Scroll down to "Search Paths", and add
            your path to "Framework Search Paths".
  - _Known Issue_: There is an issue where bundles from the FirebaseInvites
    Cocoapod are not properly copied into the Xcode project. To work around the
    issue, you can manually copy the bundles into your project. Navigate to the
    Pods/FirebaseInvites/Frameworks/frameworks/FirebaseInvites.framework/Resources
    subdirectory of your project's folder, and drag the
    GINInviteResources.bundle and GPPACLPickerResources.bundle folders into your
    Xcode project, selecting "Copy items if needed" when adding them.
  - In Xcode, build & run the sample on an iOS device or simulator.
  - The testapp has no user interface. The output of the app can be viewed
    via the console.  In Xcode,  select
    "View --> Debug Area --> Activate Console" from the menu.

### Android
  - Register your Android app with Firebase.
    - Create a new app on
      [developers.google.com](https://firebase.google.com/console/),
      and attach your Android app to it.
      - You can use "com.google.android.invites.testapp" as the Package Name
        while you're testing.
      - To [generate a SHA1](https://developers.google.com/android/guides/client-auth)
        run this command on Mac and Linux,
        ```
        keytool -exportcert -list -v -alias androiddebugkey -keystore ~/.android/debug.keystore
        ```
        or this command on Windows,
        ```
        keytool -exportcert -list -v -alias androiddebugkey -keystore %USERPROFILE%\.android\debug.keystore
        ```
      - If keytool reports that you do not have a debug.keystore, you can
        [create one with](http://developer.android.com/tools/publishing/app-signing.html#signing-manually),
        ```
        keytool -genkey -v -keystore ~/.android/debug.keystore -storepass android -alias androiddebugkey -keypass android -dname "CN=Android Debug,O=Android,C=US"
        ```
    - Add the `google-services.json` file that you downloaded from Firebase
      console to the root directory of testapp. This file identifies your
      Android app to the Firebase backend.
    - For further details please refer to the
      [general instructions for setting up an Android app with Firebase](https://firebase.google.com/docs/android/setup).
  - Download the Firebase C++ SDK linked from
    [https://firebase.google.com/docs/cpp/setup]() and unzip it to a
    directory of your choice.
  - Configure the location of the Firebase C++ SDK by setting the
    firebase\_cpp\_sdk.dir Gradle property to the SDK install directory.
    For example, in the project directory:
      ```
      > echo "systemProp.firebase\_cpp\_sdk.dir=/User/$USER/firebase\_cpp\_sdk" >> gradle.properties
      ```
  - Ensure the Android SDK and NDK locations are set in Android Studio.
    - From the Android Studio launch menu, go to `File/Project Structure...` or
      `Configure/Project Defaults/Project Structure...`
      (Shortcut: Control + Alt + Shift + S on windows,  Command + ";" on a mac)
      and download the SDK and NDK if the locations are not yet set.
  - Open *build.gradle* in Android Studio.
    - From the Android Studio launch menu, "Open an existing Android Studio
      project", and select `build.gradle`.
  - Install the SDK Platforms that Android Studio reports missing.
  - Build the testapp and run it on an Android device or emulator.
  - See [below](#using_the_test_app) for usage instructions.

# Using the Test App

- Install and run the test app on your iOS or Android device or emulator.
- The application has minimal user interface. The output of the app can be viewed
  via the console:
  - __iOS__: Open select "View --> Debug Area --> Activate Console" from the menu
    in Xcode.
  - __Android__: View the logcat output in Android studio or by running
    "adb logcat" from the command line.

- When you first run the app, it will check for an incoming dynamic link or
  invitation, and report whether it was able to fetch an invite.
- Afterwards, it will open a screen that allows you to send an invite for the
  current app via e-mail or SMS.
  - You may have to log in to Google first.
- To simulate receiving an invitation from a friend, you can send yourself an
  invite, uninstall the test app, then click the link in your e-mail.
  - This would normally send you to the Play Store or App Store to download the
    app. Because this is a test app, it will link to a nonexistent store page.
- After clicking the invite link, re-install and run the app on your device or
  emulator, and see the invitation fetched on the receiving side.

Support
-------

[https://firebase.google.com/support/]()

License
-------

Copyright 2016 Google, Inc.

Licensed to the Apache Software Foundation (ASF) under one or more contributor
license agreements.  See the NOTICE file distributed with this work for
additional information regarding copyright ownership.  The ASF licenses this
file to you under the Apache License, Version 2.0 (the "License"); you may not
use this file except in compliance with the License.  You may obtain a copy of
the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
License for the specific language governing permissions and limitations under
the License.
