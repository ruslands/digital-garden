|   |   |
|---|---|
|VSCode automatically downloaded flutter and dart||
|Flutter was located in Public folder||
|In VSCode bottom right corner choose a device||
|In terminal run the command|flutter run --dart-define=url="$https://allpapers.online/app"|
|Create keystore is not exists|keytool -genkeypair -alias dummy -keystore /Users/konovalov/1.code/keystore.jks -storepass IDAZ5KOC -keypass IDAZ5KOC -noprompt -keyalg RSA -keysize 2048|
|Create new key in keystore|keytool -genkeypair -alias allpapers -keystore /Users/konovalov/1.code/keystore.jks -storepass IDAZ5KOC -keypass EHBFYEUW384u2 -keyalg RSA -keysize 2048 -validity 365000|
|Check key in keystore|keytool -list -keystore /Users/konovalov/1.code/keystore.jks -v|
|Export key|keytool -exportcert -alias upload -keystore /Users/konovalov/1.code/upload-keystore.jks \| openssl sha1 -binary \| openssl base64|
|export pem certificate|keytool -export -rfc -alias upload -file upload_certificate.pem -keystore /Users/konovalov/1.code/upload-keystore.jks|
|Delete key|keytool -delete -alias allpapers -keystore /Users/konovalov/1.code/keystore.jks|
|Create google-services.json in Firebase||

Версионирование

я поставлю ту, которую Вы скажете. Но "0.0.1" - говорит о том, что версия не публичная с исправленным багом

если первая публикуемая - то 1.0.0

если до этого было приложение, а сейчас его перевели на новые технологии (Вы перевели на webview) - 2.0.0

когда добавите авторизацию - 2.1.0

когда пофиксите авторизацию - 2.1.1

cd /Users/konovalov/.android/avd

cd /Users/konovalov/Library/Android/sdk

### Flutter version - 2.11.0-0.0.pre.870

Dart - 2.17.0

DevTools - 2.11.1

Kotlin Gradle plugin - 1.6.10

Groovy - 3.0.9

Gradle Version - 7.4.1

Java version - 1.8.0_292

flutter channel stable

flutter upgrade

### Kotlin Gradle plugin and Kotlin plugin

в android/build.gradle

ext.kotlin_version = '1.6.10' # должен соответствовать Kotlin

мб понизить до 1.6.10 либо обновить

Android Studio -> Tools -> Kotlin -> Configure Kotlin Plugin Updates

### Firebase

SSO Errors - [https://firebase.google.com/docs/auth/admin/errors](https://firebase.google.com/docs/auth/admin/errors)

### Push params - [https://firebase.google.com/docs/reference/fcm/rest/v1/projects.messages](https://firebase.google.com/docs/reference/fcm/rest/v1/projects.messages)

### Allpapers  
  
rm -rf "${HOME}/Library/Caches/CocoaPods"  
Quit Xcode Delete ~/Library/Developer/Xcode/DerivedData and delete the derived data. Then run.

flutter clean && rm ios/Podfile.lock pubspec.lock && rm -rf ios/Pods ios/Runner.xcworkspace ios/Podfile.lock && flutter pub get && pod update && pod install

flutter clean && rm ios/Podfile.lock pubspec.lock && rm -rf ios/Pods ios/Runner.xcworkspace ios/Podfile.lock && flutter pub get

pod deintegrate

pod cache clean --all

pod install

### [https://pub.dev/packages/upgrader](https://pub.dev/packages/upgrader) # updater

### [https://fluttershapemaker.com](https://fluttershapemaker.com) # svg to flutter code

### sudo rm -rf /Library/Java/JavaVirtualMachines/jdk<version>.jdk

### sudo rm -rf /Library/PreferencePanes/JavaControlPanel.prefPane  
sudo rm -rf /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin  
sudo rm -rf /Library/LaunchAgents/com.oracle.java.Java-Updater.plist  
sudo rm -rf /Library/PrivilegedHelperTools/com.oracle.java.JavaUpdateHelper  
sudo rm -rf /Library/LaunchDaemons/com.oracle.java.Helper-Tool.plist  
sudo rm -rf /Library/Preferences/com.oracle.java.Helper-Tool.plist

sharedPreferences for Android

keyChain for iOS

### [https://mkyong.com/java/how-to-install-java-on-mac-osx/](https://mkyong.com/java/how-to-install-java-on-mac-osx/)

### ###CREATE ENVIRONMENT###

[https://gist.github.com/ThePredators/064c46403290a6823e03be833a2a3c21](https://gist.github.com/ThePredators/064c46403290a6823e03be833a2a3c21)

[https://gist.github.com/patrickhammond/4ddbe49a67e5eb1b9c03](https://gist.github.com/patrickhammond/4ddbe49a67e5eb1b9c03)

### export PATH="$PATH:/Users/rus/work/flutter/bin"

export ANT_HOME=/usr/local/opt/ant

export MAVEN_HOME=/usr/local/opt/maven

export GRADLE_HOME=/usr/local/opt/gradle

export ANDROID_HOME=/Users/konovalov/Library/Android/sdk

export ANDROID_NDK_HOME=/usr/local/opt/android-ndk

export SDK_REGISTRY_TOKEN=sk.eyJ1IjoidnBha2V0ZSIsImEiOiJja3FxOHd4dXcxNDlnMnVtaDl4bzU3a25jIn0.tOMcH863GguhwNZzkaW6pQ

export PATH=$ANT_HOME/bin:$PATH

export PATH=$MAVEN_HOME/bin:$PATH

export PATH=$GRADLE_HOME/bin:$PATH

export PATH=$ANDROID_HOME/tools:$PATH

export PATH=$ANDROID_HOME/platform-tools:$PATH

export PATH=$ANDROID_HOME/build-tools/30.0.3:$PATH

Add to .zshrc

### echo $PATH

### /usr/libexec/java_home

### export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk-14.0.2.jdk/Contents/Home

### export PATH=$JAVA_HOME/bin:$PATH

### ./gradlew run in android folder to update gradle

### export PATH=$PATH:/opt/gradle/gradle-6.5.1/bin

git fetch --all

git reset --hard frontend/master

### rm -rf flutter

### export PATH="$PATH:`pwd`/flutter/bin"

gradlew clean

flutter clean

rm -rf android/.gradle && rm -rf build

flutter pub get

rm -r pub-cache

### ###FLUTTER DOWNGRADE###

git reset --hard 1aafb3a8b9b0c36241c5f5b34ee914770f015818

Flutter doctor

### ### DEBUG ###

запустить ADV

flutter build apk --flavor dev

### flutter run --flavor dev --release

### flutter run --flavor dev --debug

### ###FLUTTER COMMANDS###

### flutter clean

flutter packages get

flutter packages upgrade

### flutter run

### flutter build apk  --flavor prod

### flutter build apk  --flavor dev

### flutter build appbundle --target-platform android-arm,android-arm64,android-x64 --flavor prod

### flutter build ios  --flavor prod

flutter run --dart-define=url="$нужный урл"

### ###INTEGRATION TESTING###

Запуск через Android Studio + Эмулятор

flutter drive --driver=test_driver/integration_test.dart --target=integration_test/app_test.dart --flavor dev

###FLAVORS FOR IOS###

Xcode settings - [https://medium.com/@animeshjain/build-flavors-in-flutter-android-and-ios-with-different-firebase-projects-per-flavor-27c5c5dac10b](https://medium.com/@animeshjain/build-flavors-in-flutter-android-and-ios-with-different-firebase-projects-per-flavor-27c5c5dac10b)

### flutter build ios --flavor prod

### ###SIGN-IN FIREBASE###

### # generate SHA-1 for Google sign-in in Firebase

### [https://stackoverflow.com/questions/51845559/generate-sha-1-for-flutter-app](https://stackoverflow.com/questions/51845559/generate-sha-1-for-flutter-app)

### # for facebook sign-in

### [https://developers.facebook.com/docs/facebook-login/android/?locale=en](https://developers.facebook.com/docs/facebook-login/android/?locale=en)

Upload SHA-1 to google app settings. Need for sign-in authentification

### [https://developer.android.com/studio/build/building-cmdline?hl=ru#build_bundle](https://developer.android.com/studio/build/building-cmdline?hl=ru#build_bundle)

### [https://developer.android.com/studio/build/building-cmdline?hl=ru](https://developer.android.com/studio/build/building-cmdline?hl=ru)

### [https://stackoverflow.com/questions/55255516/firebase-login-not-working-in-flutter-android-app](https://stackoverflow.com/questions/55255516/firebase-login-not-working-in-flutter-android-app)

###SIGN APP FOR GOOGLE###

#picker

zip -d vpakete/vpakete-picker/build/app/outputs/bundle/prodRelease/app-prod-release.aab META-INF/\*

### jarsigner -verbose -sigalg SHA256withRSA -digestalg SHA-256 -keystore my-release-key.jks vpakete/vpakete-picker/build/app/outputs/bundle/prodRelease/app-prod-release.aab key

#courier

zip -d vpakete/vpakete-courier/build/app/outputs/bundle/prodRelease/app-prod-release.aab META-INF/\*

### jarsigner -verbose -sigalg SHA256withRSA -digestalg SHA-256 -keystore my-release-key.jks vpakete/vpakete-courier/build/app/outputs/bundle/prodRelease/app-prod-release.aab key

#vpakete

zip -d vpakete/vpakete-flutter/build/app/outputs/bundle/prodRelease/app-prod-release.aab META-INF/\*

### jarsigner -verbose -sigalg SHA256withRSA -digestalg SHA-256 -keystore my-release-key.jks vpakete/vpakete-flutter/build/app/outputs/bundle/prodRelease/app-prod-release.aab key

#ALLPAPERS

zip -d allpapers-flutter/build/app/outputs/bundle/prodRelease/app-prod-release.aab META-INF/\*

### jarsigner -verbose -sigalg SHA256withRSA -digestalg SHA-256 -keystore my-release-key.jks allpapers/allpapers-flutter/build/app/outputs/bundle/prodRelease/app-prod-release.aab key_allpapers

The 'Pods-Runner' target has transitive dependencies that include statically linked binaries: (/Users/rus/work/webapps/vpakete/app_client/ios/.symlinks/flutter/ios-release/Flutter.framework)

flutter clean

rm -r ios/Flutter/Flutter.framework

pod install

In Xcode go to project navigator -> Select Runner -> go to General Tab -> on Deployment Info change the target to whatever version you have in Podfile platform :ios, 'version'

# will create 'Generated.xcconfig'

flutter clean && flutter pub get

The sandbox is not in sync with the Podfile.lock. Run 'pod install' or update your CocoaPods installation.

[https://stackoverflow.com/questions/21366549/error-the-sandbox-is-not-in-sync-with-the-podfile-lock-after-installing-re](https://stackoverflow.com/questions/21366549/error-the-sandbox-is-not-in-sync-with-the-podfile-lock-after-installing-re)

I Project Cleanup

In the project navigator, select your project

Select your target

Remove all libPods*.a in Build Phases > Link Binary With Libraries

II. Update CocoaPods

Launch Terminal and go to your project directory.

Update CocoaPods using the command pod install

/Volumes/240_TOSHIBA/xcode/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/bitcode_strip: can't open file: /Users/rus/work/webapps/vpakete/app_client/ios/Flutter/Flutter.framework/Flutter (No such file or directory)

sudo gem uninstall cocoapods

sudo gem install cocoapods -v 1.6.0

pod setup

sudo gem list cocoapods

pod --version

sudo gem install cocoapods -v 1.9.3

gem uninstall cocoapods

gem install cocoapods

Library/Caches

/Users/rus/Library/Containers

# помогает при ошибках с локализацией xcode

sudo xcode-select --reset

cd /Applications/ # remove apps

cd /Library/Containers # remove unnecessary apps

du -h -d 1  ~/Library/Containers/com.docker.docker # check size of containers

export PATH="$PATH/Volumes/240_TOSHIBA/xcode/Xcode.app/Contents/Developer/usr/bin/xcodebuild"

[https://stackoverflow.com/questions/59159232/can-i-install-xcode-on-an-external-hard-drive-along-with-the-iphone-simulator-ap](https://stackoverflow.com/questions/59159232/can-i-install-xcode-on-an-external-hard-drive-along-with-the-iphone-simulator-ap)

# available tools

ls /Library/Developer/CommandLineTools/usr/bin/

Xcode different versions

[https://stackoverflow.com/questions/10335747/how-to-download-xcode-dmg-or-xip-file](https://stackoverflow.com/questions/10335747/how-to-download-xcode-dmg-or-xip-file)

# codesign

[http://www.manpagez.com/man/1/codesign/](http://www.manpagez.com/man/1/codesign/)

Worked out using: brew install cocoapods --build-from-source

then: brew link --overwrite cocoapods