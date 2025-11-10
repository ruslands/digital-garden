# UIKit Framework: Complete Guide

## Table of Contents
1. [Introduction](#introduction)
2. [UIKit vs SwiftUI](#uikit-vs-swiftui)
3. [Core Classes and Hierarchy](#core-classes-and-hierarchy)
4. [App Lifecycle](#app-lifecycle)
5. [AppDelegate](#appdelegate)
6. [SceneDelegate](#scenedelegate)
7. [Scene Lifecycle](#scene-lifecycle)
8. [Events and States](#events-and-states)
9. [Resources](#resources)

---

## Introduction

UIKit is Apple's framework for building user interfaces for iOS, tvOS, and watchOS applications. It provides the fundamental infrastructure needed to construct and manage a graphical, event-driven user interface.

**Key Features:**
- Comprehensive set of UI components
- Event handling and user interaction
- Layout and constraint system
- Animation and transitions
- Integration with system services

---

## UIKit vs SwiftUI

### Framework Comparison

| Framework | Documentation | Canvas Preview |
|-----------|---------------|----------------|
| **SwiftUI** | [SwiftUI Concepts Tutorial](https://developer.apple.com/tutorials/swiftui-concepts) | ✅ Has canvas preview |
| **UIKit** | [UIKit Documentation](https://developer.apple.com/documentation/uikit) | ❌ Does not have canvas preview |

### Interoperability

**Key Point**: UIKit works seamlessly with the SwiftUI framework, so you can implement parts of your UIKit app in SwiftUI or mix interface elements between the two frameworks.

**Integration Options**:
- Place UIKit views and view controllers inside SwiftUI views
- Place SwiftUI views inside UIKit views and view controllers
- Gradually migrate from UIKit to SwiftUI
- Use the best of both frameworks in the same app

**Example Use Cases**:
- Embedding a `UIViewController` in a SwiftUI view using `UIViewControllerRepresentable`
- Embedding a SwiftUI view in a `UIViewController` using `UIHostingController`
- Wrapping existing UIKit components for use in SwiftUI

---

## Core Classes and Hierarchy

### UIResponder

**Description**: Base class for objects that respond to and handle events.

**Subclasses Include**:
- `UIApplication`
- `UIView`
- `UIViewController`
- And many other UIKit classes

**Purpose**: Provides the foundation for the responder chain, which handles events like touches, gestures, and remote control events.

---

### UIApplication

**Description**: Represents the app's main event loop and is responsible for managing the app's lifecycle.

**Key Characteristics**:
- **Root of the App**: The root of our app
- **Singleton**: Like the messenger that tells the application delegate when different events happen in the app's life cycle, like when it's done launching or when the system's about to terminate it
- **Single Instance**: Note that each app only has one `UIApplication` instance, no matter what version of iOS we're using

**Responsibilities**:
- Managing the app's lifecycle
- Handling system events
- Coordinating with the app delegate
- Managing the app's windows and scenes

---

### UIWindow

**Description**: Represents a window in the app's user interface.

**Key Characteristics**:
- Contains the root view controller of the app's user interface
- Acts as a container for views
- Manages the view hierarchy
- Handles touch events and coordinates with the responder chain

**Usage**: Typically, you create one window per scene, and it serves as the root container for all your views.

---

### UIWindowScene

**Description**: Represents a scene that manages one or more windows.

**Related Types**:
- `UIWindowScene.Geometry`: Defines the geometry of a window scene
- `UIWindowScene.GeometryPreferences`: Preferences for window geometry
- `UIWindowScene.GeometryPreferences.Mac`: Mac-specific geometry preferences
- `UIWindowScene.GeometryPreferences.iOS`: iOS-specific geometry preferences

**Additional Scene-Related Types**:
- `UISceneSizeRestrictions`: Defines size restrictions for scenes
- `UIScreenshotService`: Service for handling screenshots
- `UISceneWindowingBehaviors`: Behaviors for window management
- `UIStatusBarManager`: Manages the status bar appearance
- `UITitlebar`: Represents the title bar (macOS)
- `UIWindowScene.ActivationAction`: Actions for scene activation
- `UIWindowScene.ActivationConfiguration`: Configuration for scene activation
- `UIWindowScene.ActivationInteraction`: Interactions for scene activation
- `UIWindowScene.ActivationRequestOptions`: Options for activation requests
- `UIWindowSceneDestructionRequestOptions`: Options for scene destruction
- `UIWindowSceneDragInteraction`: Drag interactions for scenes

---

### UIScene

**Description**: Represents one instance of your app's UI running on a device.

**Key Characteristics**:
- The user can create multiple scenes for each app
- Each scene can be shown and hidden separately
- Each scene has its own life cycle
- Each scene can be in a different state of execution

**Related Types**:
- `UISceneConfiguration`: Configuration for a scene
- `UISceneSession`: Session information for a scene
- `UISceneActivationConditions`: Conditions for scene activation

---

### UIView

**Description**: The base class for all user interface elements.

**Key Features**:
- Provides essential functionality for drawing, layout, and interaction
- Manages a rectangular area on the screen
- Handles touch events and gestures
- Supports animations and transitions

**Common Subclasses**:
- `UIControl`: Interactive elements (buttons, sliders, switches)
- `UIScrollView`: Scrollable content
- `UITableView`: Lists of data
- `UICollectionView`: Customizable grids
- `UIImageView`: Image display
- `UITextView`: Multi-line text
- `UITextField`: Single-line text input

---

### UIViewController

**Description**: Manages a view hierarchy for displaying content on the screen.

**Key Responsibilities**:
- Handles events and transitions related to the view lifecycle
- Manages the view's appearance and disappearance
- Coordinates between the model and view
- Handles user interactions

**Lifecycle Methods**:
- `viewDidLoad()`: Called after the view is loaded into memory
- `viewWillAppear(_:)`: Called before the view appears
- `viewDidAppear(_:)`: Called after the view appears
- `viewWillDisappear(_:)`: Called before the view disappears
- `viewDidDisappear(_:)`: Called after the view disappears

---

### UIControl

**Description**: Subclasses of `UIView` that respond to user interactions.

**Examples**:
- `UIButton`: Button controls
- `UISlider`: Slider controls
- `UISwitch`: Switch controls
- `UITextField`: Text input controls
- `UISegmentedControl`: Segmented controls

**Key Features**:
- Target-action pattern for event handling
- State management (enabled, selected, highlighted)
- Accessibility support

---

### UIScrollView

**Description**: Allows users to scroll and zoom content that is larger than the visible area.

**Key Features**:
- Scrolling in any direction
- Zooming support
- Content insets
- Scroll indicators

**Common Subclasses**:
- `UITableView`: Vertical scrolling lists
- `UICollectionView`: Customizable grids
- `UITextView`: Scrollable text content

---

### UITableView / UICollectionView

**Description**: Displays a scrollable list or grid of data.

**Comparison**:
- **UITableView**: For a list of data (single column)
- **UICollectionView**: For a customizable grid (multiple columns, custom layouts)

**Key Features**:
- Efficient memory management through cell reuse
- Delegate and data source patterns
- Support for sections and headers/footers
- Built-in animations

---

### UIAlertController

**Description**: Presents a modal alert or action sheet to the user.

**Types**:
- **Alert**: Modal dialog with buttons
- **Action Sheet**: Bottom sheet with options (iOS)

**Usage**: For displaying messages, confirming actions, or presenting options to the user.

---

### UINavigationBar / UITabBar

**Description**: Provides navigation or tab-based interfaces for organizing and navigating content.

**UINavigationBar**:
- Used with `UINavigationController`
- Provides back button and title
- Supports navigation stack

**UITabBar**:
- Used with `UITabBarController`
- Provides tab-based navigation
- Supports multiple view controllers

---

### UITextView / UITextField

**Description**: Allows users to input and edit text.

**UITextField**:
- Single-line text input
- Placeholder text
- Keyboard types
- Input validation

**UITextView**:
- Multi-line text input
- Rich text support
- Scrollable content
- Text selection and editing

---

### UIImage / UIImageView

**Description**: Represents images and displays them in the user interface.

**UIImage**:
- Image data representation
- Supports various formats (PNG, JPEG, etc.)
- Can be created from files, data, or assets

**UIImageView**:
- Displays `UIImage` objects
- Supports content modes (aspect fit, aspect fill, etc.)
- Animation support

---

### UIColor

**Description**: Represents colors used in the user interface.

**Features**:
- RGB, HSB, and grayscale color spaces
- System colors (adapt to light/dark mode)
- Dynamic colors
- Color literals

---

### UIFont

**Description**: Represents fonts used for displaying text.

**Features**:
- System fonts
- Custom fonts
- Font sizes and weights
- Dynamic type support

---

### UIBezierPath / CAShapeLayer

**Description**: Used for drawing custom shapes and paths.

**UIBezierPath**:
- Defines vector-based paths
- Can be used for drawing and clipping
- Supports curves and complex shapes

**CAShapeLayer**:
- Core Animation layer for shapes
- More performant for animations
- Part of the Core Animation framework

---

## App Lifecycle

### App States

The current state of your app determines what it can and can't do at any time:

| State | Description | Capabilities |
|-------|-------------|--------------|
| **Foreground Active** | App is visible and receiving events | Full access to system resources, including CPU |
| **Foreground Inactive** | App is in foreground but not receiving events | Limited interaction |
| **Background** | App is offscreen | Must do as little work as possible, preferably nothing |
| **Suspended** | App is in memory but not executing | No code execution |
| **Not Running** | App has not been launched or was terminated | No execution |

**Key Points**:
- A foreground app has the user's attention, so it has priority over system resources, including the CPU
- A background app must do as little work as possible, and preferably nothing, because it's offscreen
- As your app changes from state to state, you must adjust its behavior accordingly

### State Transitions

When your app's state changes, UIKit notifies you by calling methods of the appropriate delegate object.

---

## AppDelegate

### Overview

**Description**: The `AppDelegate` class has been a crucial part of iOS apps since the early days. It handles the overall lifecycle of our app, including launch, termination, and handling system-level events.

**Responsibilities**:
- Setting up the initial app environment
- Managing app-level data and resources
- Handling push notifications
- Responding to system events
- Managing app-level configuration

### Traditional Role

**Historical Context**: Traditionally, the `AppDelegate` was responsible for managing a single instance of UI. It provided the app's window object that acted as the root view for the entire app.

### Current Role (iOS 13+)

**Changes**: Starting from iOS 13, with the introduction of scenes:
- The app delegate doesn't get notified about what's going on with the user interface life cycle events anymore
- Apps can now have multiple interfaces or scenes
- The `AppDelegate` still handles app-level events but not scene-specific UI events

**Key Method**: `application(_:didFinishLaunchingWithOptions:)`

**Purpose**: Initialize the app and configure its initial state.

**When Called**: At launch time, UIKit automatically creates the `UIApplication` object and your app delegate. It then starts your app's main event loop.

**What to Do Here**:
- Initialize your app's data structures
- Verify that your app has the resources it needs to run
- Perform any one-time setup when your app launches for the first time (e.g., install templates or user-modifiable files in a writable directory)
- Connect to any critical services that your app uses (e.g., connect to the Apple Push Notification service if your app supports remote notifications)
- Check the launch options dictionary for information about why your app launched

**Important**: UIKit doesn't present your app's interface until `didFinishLaunchingWithOptions` method returns.

---

## SceneDelegate

### Overview

**Description**: The `SceneDelegate` focuses on managing the lifecycle of individual scenes. Introduced in iOS 13, it focuses specifically on managing the lifecycle and configuration of multiple instances of UI within our app.

**Key Responsibilities**:
- Setting up the initial UI for each scene
- Responding to changes within those scenes
- Managing scene activation, deactivation, and termination
- Handling events related to scenes

### Scene-Based Architecture

**Key Concept**: With the introduction of scenes, the `SceneDelegate` allows for multiple instances of UI within a single app. Each scene has its own window object, and the `SceneDelegate` is responsible for managing the UI and events specific to that scene.

**Multiple Scenes**:
- The ability to create multiple scenes for an app in iOS 13 is actually limited to iPads and iPadOS
- You can't create more than the one app scene on an iPhone in iOS
- On iPad, users can create and manage multiple instances of your app's user interface simultaneously, and switch between them using the app switcher

**Single Scene on iPhone**: 
- Formally, the app uses scenes, as this is the default starting from iOS 13
- But support for multiple scenes (for iPad) is not enabled
- That is, you always have one "scene" for the entire app on iPhone

### Why SceneDelegate?

**Reason**: Now apps can have multiple scenes on an iPad. Each scene has its own unique state going on, with one scene up front and another can be in the background. So, it doesn't make sense anymore to have the app delegate be in charge of handling the user interface life cycle events. Instead, each scene has its own `UIWindowSceneDelegate` object that takes care of those events for the scene it's connected to.

### Key Methods

**`scene(_:willConnectTo:options:)`**:
- Called when UIKit connects a scene to your app
- Configure your scene's initial UI and load the data your scene needs
- This is where you set up the window and root view controller

**`sceneWillEnterForeground(_:)`**:
- Called when transitioning to the foreground-active state
- Configure your UI and prepare to interact with the user
- See [Preparing your UI to run in the foreground](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_foreground)

**`sceneDidEnterBackground(_:)`**:
- Called when entering the background state
- Finish crucial tasks, free up as much memory as possible, and prepare for your app snapshot
- See [Preparing your UI to run in the background](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background)

**`sceneDidDisconnect(_:)`**:
- Called at scene disconnection
- Clean up any shared resources associated with the scene

### Interaction with AppDelegate

**Cooperation**: Although the `SceneDelegate` handles scene-related events, it still interacts with the `AppDelegate` when necessary. The `AppDelegate` retains its responsibility for app-level events and behaviors.

**Example**: The `AppDelegate`'s `application(_:didFinishLaunchingWithOptions:)` method is called when the app is initially launched, even before any scenes are created. You can use this method to perform global app setup tasks, initialize services, or set up shared resources that are not specific to a particular scene.

### Debugging Tip

**Note**: In the `SceneDelegate` file, you can connect different screens for debugging. For this, you need to pass the controller class of the page you're interested in to the `window?.rootViewController` variable.

### Protocol

**`UIWindowSceneDelegate`**: Protocol that the `SceneDelegate` conforms to for handling window scene-specific events.

---

## Scene Lifecycle

### Scene Transitions

Use scene transitions to perform the following tasks:

1. **When UIKit connects a scene to your app**: Configure your scene's initial UI and load the data your scene needs
2. **When transitioning to the foreground-active state**: Configure your UI and prepare to interact with the user
3. **Upon leaving the foreground-active state**: Save data and quiet your app's behavior
4. **Upon entering the background state**: Finish crucial tasks, free up as much memory as possible, and prepare for your app snapshot
5. **At scene disconnection**: Clean up any shared resources associated with the scene

### Additional Considerations

In addition to scene-related events, you must also respond to the launch of your app using your `UIApplicationDelegate` object. For information about what to do at app launch, see [Responding to the launch of your app](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app).

---

## Events and States

### System Events

| Event | Handler | Description |
|-------|---------|-------------|
| **Low Memory** | AppDelegate | [Responding to Memory Warnings](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle/responding_to_memory_warnings) |
| **Launch** | AppDelegate | At launch time, UIKit automatically creates the `UIApplication` object and your app delegate. It then starts your app's main event loop. |
| **Termination** | AppDelegate | Handles app termination events |

### App States

| State | Description |
|-------|-------------|
| **Foreground Inactive** | App is in foreground but not receiving events |
| **Foreground Active** | App is visible and receiving events |
| **Background** | App is offscreen |
| **Suspended** | App is in memory but not executing |
| **Not Running** | App has not been launched or was terminated |

### Trait Collections

**Description**: Trait collections reflect a combination of device settings, interface settings, and user preferences.

**Example**: You use traits to detect whether Dark Mode is active for the current view or view controller.

**Common Traits**:
- User interface style (light/dark mode)
- Size classes (compact/regular)
- Display scale
- Accessibility settings

---

## Storyboard

### Launch Storyboard

**Description**: When the user first launches your app on a device, the system displays your launch storyboard until your app is ready to display its UI.

**Purpose**: Displaying the launch storyboard assures the user that your app launched and is doing something.

**Timing**: If your app initializes itself and readies its UI quickly, the user may see your launch storyboard only briefly.

**Best Practice**: Design a launch storyboard that matches your app's initial screen to provide a smooth transition.

---

## Summary

### AppDelegate vs SceneDelegate

| Aspect | AppDelegate | SceneDelegate |
|--------|-------------|---------------|
| **Scope** | App-level | Scene-level |
| **Introduced** | iOS 2.0 | iOS 13.0 |
| **Responsibilities** | App lifecycle, system events | Scene lifecycle, UI management |
| **Window Management** | Traditional (pre-iOS 13) | Modern (iOS 13+) |
| **Multiple Instances** | No | Yes (iPad only) |

### Key Takeaways

1. **UIKit and SwiftUI are Interoperable**: You can mix both frameworks in the same app
2. **Scene-Based Architecture**: iOS 13+ uses scenes for UI management, with `SceneDelegate` handling scene lifecycle
3. **AppDelegate Still Matters**: Handles app-level events, even in scene-based apps
4. **Multiple Scenes**: Only available on iPad, iPhone always has one scene
5. **Lifecycle Management**: Understanding app and scene states is crucial for proper resource management

---

## Resources

### Official Documentation

- [UIKit Documentation](https://developer.apple.com/documentation/uikit)
- [SwiftUI Concepts Tutorial](https://developer.apple.com/tutorials/swiftui-concepts)
- [Managing Your App's Life Cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)
- [Scenes](https://developer.apple.com/documentation/uikit/app_and_environment/scenes)
- [Preparing Your UI to Run in the Foreground](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_foreground)
- [Preparing Your UI to Run in the Background](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background)
- [Responding to the Launch of Your App](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app)
- [Responding to Memory Warnings](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle/responding_to_memory_warnings)

### Community Resources

- [Understanding Scene Delegate & App Delegate - Medium](https://medium.com/@kalyan.parise/understanding-scene-delegate-app-delegate-7503d48c5445)

---

## Best Practices

1. **Use SceneDelegate for UI Management**: In iOS 13+, use `SceneDelegate` for scene-specific UI lifecycle
2. **Keep AppDelegate for App-Level Events**: Use `AppDelegate` for app-wide initialization and system events
3. **Handle State Transitions Properly**: Respond appropriately to foreground/background transitions
4. **Manage Memory Efficiently**: Free up resources when entering background state
5. **Design Smooth Launch Experience**: Use launch storyboards that match your initial UI
6. **Leverage Trait Collections**: Adapt your UI based on device settings and user preferences
7. **Mix UIKit and SwiftUI Wisely**: Use both frameworks where they make sense, but plan your architecture
