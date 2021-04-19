# Startup Namer - Part 1

## What you'll learn

- How to write a Flutter app that looks natural on iOS, Android, and the web
- The basic structure of a Flutter app
- Finding and using packages to extend the functionality
- Using hot reload for a quicker development cycle

## What you'll build

You'll implement a simple app that generates proposed names for a startup company. The user can select and unselect names, saving the best ones. The code lazily generates 10 names at a time. As the user scrolls, more names are generated. There is no limit to how far a user can scroll.

The following image shows how the app works at the completion of part 1:

![image](https://user-images.githubusercontent.com/49060283/112730232-6b97c500-8f56-11eb-8f05-1e81f751f032.png)


- **Step 1:-** Create a simple, templated Flutter app and name it **startup_namer**.
- **Step 2:-** Delete all of the code from `lib/main.dart` and replace it with the following code, which displays "Hello World" in the center of the screen.

```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: const Text('Welcome to Flutter'),
        ),
        body: const Center(
          child: const Text('Hello World'),
        ),
      ),
    );
  }
}
```

- **Step 3:-** [Run the app.](https://flutter.dev/docs/get-started/test-drive#androidstudio) You should see either Android, iOS, or web output, depending on your device.


Android | IOS
------------ | -------------
![f9df7832965ede9f](https://user-images.githubusercontent.com/49060283/112729138-a991ea80-8f50-11eb-83a1-e10bc7b828c0.png) | ![20374605026d582](https://user-images.githubusercontent.com/49060283/112726817-55353d80-8f45-11eb-96f7-6adbeb993c07.png)


>**Tip:** The first time that you run on a physical device, it can take a while to load. Afterward, you can use [hot reload](https://flutter.dev/docs/get-started/test-drive#androidstudio) for quick updates. In supported IDEs, Save also performs a hot reload if the app is running. When running an app directly from the console using flutter run, enter r to perform hot reload.


## Observations 

> - This example creates a [Material](https://material.io/design/) app. Material is a visual-design language that's standard on mobile and the web. Flutter offers a rich set of [Material widgets](https://flutter.dev/docs/development/ui/widgets/material).
> - The main method uses arrow `(=>)` notation. Use arrow notation for one-line functions or methods.
> - The app extends [StatelessWidget](https://flutter.dev/docs/development/ui/interactive#stateful-and-stateless-widgets), which makes the app itself a widget. In Flutter, almost everything is a widget, including alignment, padding, and layout.
> - The [Scaffold](https://api.flutter.dev/flutter/material/Scaffold-class.html) widget, from the Material library, provides a default app bar, a title, and a body property that holds the widget tree for the home screen. The widget subtree can be quite complex.
> - A widget's main job is to provide a `build` method that describes how to display the widget in terms of other, lower-level widgets.
> - The body for this example consists of a [Center](https://api.flutter.dev/flutter/widgets/Center-class.html) widget containing a [Text](https://api.flutter.dev/flutter/widgets/Text-class.html) child widget. The Center widget aligns its widget subtree to the center of the screen.


In the next step, you'll start [using an open-source package](https://flutter.dev/docs/development/packages-and-plugins/using-packages#using-packages) named [english_words](https://pub.dev/packages/english_words), which contains a few thousand of the most-used English words, plus some utility functions.

You can find the `english_words` package, as well as many other open-source packages, at [pub.dev.](https://pub.dev/)

- **Step 4:-** The pubspec file manages the assets for a Flutter app. In `pubspec.yaml`, append `english_words: ^4.0.0-0` (english_words 4.0.0-0 or higher) to the dependencies list:

```
dependencies:
  flutter:
    sdk: flutter

  cupertino_icons: ^1.0.2
  english_words: ^4.0.0-0   # add this line
  ```

- **Step 5:-** While viewing the pubspec in Android Studio's editor view, click **"Packages get"**. This pulls the package into your project. You should see the following in the console:

```
flutter packages get
Running "flutter packages get" in startup_namer...
Process finished with exit code 0
```

- **Step 6:-** In `lib/main.dart`, import the new package:

```
import 'package:flutter/material.dart';
import 'package:english_words/english_words.dart';  // Add this line.
```

Next, you'll use the `english_words` package to generate the text instead of using "Hello World".

- **Step 7:-** Make the following changes:

```
import 'package:flutter/material.dart';
import 'package:english_words/english_words.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final wordPair = WordPair.random(); // Add this line.
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(                       // Drop the const, and
          //child: Text('Hello World'),     // Replace this text...
          child: Text(wordPair.asPascalCase),  // With this text.
        ),
      ),
    );
  }
}
```


>**Tip:** Pascal case (also known as upper camel case) means that each word in the string, including the first one, begins with an uppercase letter. So, uppercamelcase becomes UpperCamelCase.


If the app is running, hot reload to update the running app. (From the command line, you can enter r to hot reload.) Each time you click hot reload or save the project, you should see a different word pair, chosen at random, in the running app. That's because the word pairing is generated inside the build method, which runs each time the MaterialApp requires rendering, or when toggling the Platform in the Flutter Inspector.


Android | IOS
------------ | -------------
![image](https://user-images.githubusercontent.com/49060283/112730198-3ee3ad80-8f56-11eb-927d-a2df10c29431.png) | ![image](https://user-images.githubusercontent.com/49060283/112730232-6b97c500-8f56-11eb-8f05-1e81f751f032.png)


## Challenge

**Add an app icon**

For every app that you create using Flutter, you get a default Flutter logo as your app icon. 

The challenge is to replace the flutter icon with a custom icon.
