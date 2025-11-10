plist (property list) file contains critical information about the configuration and capabilities of an iOS mobile app—such as iOS versions that are supported and device compatibility—which the operating system uses to interact with the app. This file is automatically created when the mobile app is compiled.

swinject:  is a lightweight dependency injection framework for Swift.

fastlane: app automation, build deploy, signing

Complications: are small interface elements that appear on a watch face and provide quick access to data that people frequently use. 

[https://qonversion.io](https://qonversion.io)

[https://adapty.io](https://adapty.io)

[https://apphud.com](https://apphud.com)

[https://www.revenuecat.com](https://www.revenuecat.com)

|   |   |
|---|---|
|Certificates|Need to create a CSR [https://developer.apple.com/help/account/create-certificates/create-a-certificate-signing-request/](https://developer.apple.com/help/account/create-certificates/create-a-certificate-signing-request/)<br><br>iOS Distribution|
|Identifiers||
|Devices||
|Profiles|[https://abhimuralidharan.medium.com/what-is-a-provisioning-profile-in-ios-77987a7c54c2#:~:text=the%20provisioning%20profile%20in%20the,bundle%20identifier%20in%20the%20app](https://abhimuralidharan.medium.com/what-is-a-provisioning-profile-in-ios-77987a7c54c2#:~:text=the%20provisioning%20profile%20in%20the,bundle%20identifier%20in%20the%20app).<br><br>Unlike Android, you can’t install any app on an iOS device. It has to be signed by Apple first. However, when you’re developing an app, you probably want to test it before sending it to Apple for approval.<br><br>Provisioning profile act as a link between the device and the developer account.<br><br>Provisioning profiles allow you to install apps onto your devices. A provisioning profile includes signing certificates, device identifiers, and an App ID.<br><br>During development, you choose which devices can run your app and which app services your app can access. A provisioning profile is downloaded from your developer account and embedded in the app bundle, and the entire bundle is code-signed. A Development Provisioning Profile must be installed on each device on which you wish to run your application code. If the information in the provisioning profile doesn’t match certain criteria, your app won’t launch.|
|Keys||
|Services||

|   |   |
|---|---|
|sudo gem install cocoapods|install cocoapods|
|brew install cocoapods|install cocoapods if first way didn't work|
|chmod -R 0600 /Users/konovalov/.netrc|give some permissions|
|pod install|Install dependencies, execute from project directory. This is to be used the first time you want to retrieve  <br>the pods for the project, but also every time you edit your Podfile to add, update or remove a pod.|
|pod deintegrate && pod install|If some errors popup with pods|

.xcworkspace - pod generated file

|   |   |
|---|---|
|close xcode  <br>open .xcworkspace instead of .xcodeproj||
|Set iOS deployment target in project and pods|Every pod and project has settings for deployment target|
|Sandbox: rsync.samba(8912) deny(1) file-write-create|[https://stackoverflow.com/questions/76590131/error-while-build-ios-app-in-xcode-sandbox-rsync-samba-13105-deny1-file-w](https://stackoverflow.com/questions/76590131/error-while-build-ios-app-in-xcode-sandbox-rsync-samba-13105-deny1-file-w)|
|Simulator error, xcode ask to install simulator|[https://forums.developer.apple.com/forums/thread/735155](https://forums.developer.apple.com/forums/thread/735155)<br><br>sudo killall -9 com.apple.CoreSimulator.CoreSimulatorService|
|||

Read:

[https://habr.com/ru/companies/docdoc/articles/742962/](https://habr.com/ru/companies/docdoc/articles/742962/)

[https://habr.com/ru/companies/docdoc/articles/748130/](https://habr.com/ru/companies/docdoc/articles/748130/)

[https://habr.com/ru/companies/docdoc/articles/735946/](https://habr.com/ru/companies/docdoc/articles/735946/)

[https://habr.com/ru/companies/docdoc/articles/732102/](https://habr.com/ru/companies/docdoc/articles/732102/)

1. rm -rf "${HOME}/Library/Caches/CocoaPods"
2. quit xcode
3. rm -rf ~/Library/Developer/Xcode/DerivedData
4. Run xcode
5. rm -rf ios/Pods ios/Runner.xcworkspace ios/Podfile.lock
6. pod update && pod install

xcodebuild -showBuildSettings -scheme Metabolism -sdk iphonesimulator | grep ARCHS

ARCHS = x86_64

ARCHS_STANDARD = arm64 x86_64

ARCHS_STANDARD_32_64_BIT = arm64 i386 x86_64

ARCHS_STANDARD_32_BIT = i386

ARCHS_STANDARD_64_BIT = arm64 x86_64

ARCHS_STANDARD_INCLUDING_64_BIT = arm64 x86_64

ARCHS_UNIVERSAL_IPHONE_OS = arm64 i386 x86_64

VALID_ARCHS = arm64 arm64e i386 x86_64

Add arm64 under Excluded Architectures. Build Settings > Architectures > Excluded Architectures > Debug > Select "Any iOS Simulator SDK" > fill in "arm64" as the value. Do the same for Release.

Equivalent line of code: "EXCLUDED_ARCHS[sdk=iphonesimulator*]" = arm64;

Debugging

|   |   |
|---|---|
|Open simulator|Xcode -> Open developer tool -> Simulator|
|Check logs||
|Clean build|Delete files from DerivedData folder|
|Reset packages|File - Packages - Reset packages cache|

Добрый день!  
Я в поисках ios разработчика на парт тайм с опытом работы с BLE устройствами.

Проект - metabolism.cloud  
Проект сделан на MVVM-C, Realm, SwiftUI, UIkit.  
Требуется доработать BLE взаимодействие с устройством.

Если готовы взяться проект, то давайте запланируем звонок.  
 

Project Structure

Application

Classes

Controllers

Helpers

Views

Utilities

SupportFile

1. Application: This folder likely contains the main entry point of your application, such as AppDelegate or SceneDelegate in case of SwiftUI apps. It might also include any global configurations or setups needed for your application to run.

2. Classes: This folder typically contains the model classes used in your project. Models represent the data and the logic behind your data. They are often the backbone of your application.

3. Controllers: This folder usually contains the view controllers of your application. In the MVVM-C (Model-View-ViewModel-Coordinator) architecture, view controllers are part of the view layer. They handle user interactions, update the views, and communicate with the view models.

4. Helpers: This folder may contain various helper classes or extensions that provide utility functions or enhance the functionality of existing classes. Helpers can include things like date formatters, string manipulations, or network request handlers.

5. Views: This folder typically contains custom views or view components used throughout your application. Views are responsible for presenting the user interface and handling user interactions. In the MVVM-C architecture, views are usually passive and delegate user input to view controllers.

6. Utilities: This folder often contains utility classes or functions that don't belong to any specific domain of your application but are used across multiple parts of your project. Utilities can include things like logging utilities, file managers, or encryption helpers.

7. SupportFile: This folder might contain miscellaneous files that support your application, such as configuration files, asset catalogs, or localization files. It can also include third-party libraries, fonts, or any other resources needed for your project.

Application

Classes

Controllers

Helpers

Views

- Base

- BaseCollectionViewCell
- BaseTableViewCell
- BaseOneTypeTextTableViewCell
- BaseTopDropDownTableViewCell
- BasePickerTableViewCell
- BaseCheckBoxTableViewCell
- BasePlainSelectableTableViewCell
- BaseSpaceCollectionViewCell
- BaseVerticalStackSpaceView
- BaseVerticalStackLabelView

- Navigation

- UIBarButtonItem

- ActionTableCellModel
- IndicatorCheckboxTableViewCell
- BlueButton
- DateSelectionView

Utilities

- Coders
- NeverFunctions

SupportFile

- Animations

- device_search

- Fonts

Алексей Папин - 1500

Владимир Сорокин - 1500 / 1700