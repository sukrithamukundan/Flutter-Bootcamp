# Startup Namer Part - 4

## What you'll learn

- How to create and navigate to a second screen.
- How to change the look of an app using themes.

## What you'll build

Tapping the list icon in the upper right of the app bar navigates to a new page (called a route) that lists only the favorited names.

![image](https://user-images.githubusercontent.com/49060283/114319436-3b901a80-9b2f-11eb-98cf-87b327809602.png)

**Navigate to a new screen**

In this step, you'll add a new page (called a route in Flutter) that displays the favorites. You'll learn how to navigate between the home route and the new route.

In Flutter, the Navigator manages a stack containing the app's routes. Pushing a route onto the Navigator's stack, updates the display to that route. Popping a route from the Navigator's stack, returns the display to the previous route.

Next, you'll add a list icon to the AppBar in the build method for RandomWordsState. When the user clicks the list icon, a new route that contains the saved favorites is pushed to the Navigator, displaying the icon.

- **Step 1:-** Add the icon and its corresponding action to the `build` method:


```
class RandomWordsState extends State<RandomWords> {
  ...
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Startup Name Generator'),
        actions: <Widget>[      // Add 3 lines from here...
          IconButton(icon: Icon(Icons.list), onPressed: _pushSaved),
        ],                      // ... to here.
      ),
      body: _buildSuggestions(),
    );
  }
  ...
}
```


> Tip: Some widget properties take a single widget (**child**), and other properties, such as action, take an array of widgets (**children**), as indicated by the square brackets (**[]**).


- **Step 2:-** Add a `_pushSaved() `function to the RandomWordsState class.


```
  void _pushSaved() {
  }
  ```
  
  
- **Step 3:-** Hot reload the app. The list icon ( ![image](https://user-images.githubusercontent.com/49060283/114319697-2667bb80-9b30-11eb-9a57-d84be4e9e89f.png) ) appears in the app bar. Tapping it does nothing yet, because the `_pushSaved` function is empty.

Next, you'll build a route and push it to the Navigator's stack. This action changes the screen to display the new route. The content for the new page is built in MaterialPageRoute's `builder` property, in an anonymous function.

 - **Step 4:-** Call `Navigator.push`, as shown below, which pushes the route to the Navigator's stack. The IDE will complain about invalid code, but you will fix that in the next section.

```
void _pushSaved() {
  Navigator.of(context).push(
  );
}
```


Next, you'll add the MaterialPageRoute and its builder. For now, add the code that generates the ListTile rows. The `divideTiles()` method of ListTile adds horizontal spacing between each ListTile. The `divided` variable holds the final rows, converted to a list by the convenience function, `toList()`.

- **Step 5:-** Add the code, as shown below:


```
void _pushSaved() {
  Navigator.of(context).push(
    MaterialPageRoute<void>(   // Add 20 lines from here...
      builder: (BuildContext context) {
        final Iterable<ListTile> tiles = _saved.map(
          (WordPair pair) {
            return ListTile(
              title: Text(
                pair.asPascalCase,
                style: _biggerFont,
              ),
            );
          },
        );
        final List<Widget> divided = ListTile
          .divideTiles(
            context: context,
            tiles: tiles,
          )
          .toList();
      },
    ),                       // ... to here.
  );
}
```


The builder property returns a Scaffold, containing the app bar for the new route, named "Saved Suggestions." The body of the new route consists of a ListView containing the ListTiles rows; each row is separated by a divider.

- **Step 6:-** Add horizontal dividers, as shown below:


```
void _pushSaved() {
  Navigator.of(context).push(
    MaterialPageRoute<void>(
      builder: (BuildContext context) {
        final Iterable<ListTile> tiles = _saved.map(
          (WordPair pair) {
            return ListTile(
              title: Text(
                pair.asPascalCase,
                style: _biggerFont,
              ),
            );
          },
        );
        final List<Widget> divided = ListTile
          .divideTiles(
            context: context,
            tiles: tiles,
          )
              .toList();

        return Scaffold(         // Add 6 lines from here...
          appBar: AppBar(
            title: Text('Saved Suggestions'),
          ),
          body: ListView(children: divided),
        );                       // ... to here.
      },
    ),
  );
}
```


- **Step 7:-** Hot reload the app. Favorite some of the selections and tap the list icon in the app bar. The new route appears containing the favorites. Note that the Navigator adds a "Back" button to the app bar. You did not have to explicitly implement Navigator.pop. Tap the back button to return to the home route.

iOS - Main route | iOS - Saved suggestions route
------------ | -------------
![image](https://user-images.githubusercontent.com/49060283/114319487-624e5100-9b2f-11eb-9d36-9ed55cd80ab5.png) | ![image](https://user-images.githubusercontent.com/49060283/114319500-74c88a80-9b2f-11eb-9210-d414e97260e7.png)


**Change the UI using Themes**


In this step, you'll modify the app's theme. The theme controls the look and feel of your app. You can either use the default theme, which is dependent on the physical device or emulator, or customize the theme to reflect your branding.

You can easily change an app's theme by configuring the [ThemeData](https://api.flutter.dev/flutter/material/ThemeData-class.html) class. This app currently uses the default theme, but you'll change the app's primary color to white.

- **Step 8:-** Change the color in the MyApp class:


```
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Startup Name Generator',
      theme: ThemeData(          // Add the 3 lines from here... 
        primaryColor: Colors.white,
      ),                         // ... to here.
      home: RandomWords(),
    );
  }
}
```


- **Step 9:-** Hot reload the app. The entire background is now white, even the app bar.

As an exercise for the reader, use ThemeData to change other aspects of the UI. The [Colors](https://api.flutter.dev/flutter/material/Colors-class.html) class in the Material library provides many color constants you can play with, and hot reload makes experimenting with the UI quick and easy.

Android | iOS
------------ | -------------
![image](https://user-images.githubusercontent.com/49060283/114319612-cb35c900-9b2f-11eb-93a0-d5ef00ca8173.png) | ![image](https://user-images.githubusercontent.com/49060283/114319624-db4da880-9b2f-11eb-80c7-c628e1fbbcc4.png)


## Challenge
These days it's hard to come by a good app that doesn't feature any animations. 
**The challenge is to create delightful experiences for the user by incorporating animations into your Startup Namer.**

**Your animation must include:**
1. Pulsating(growing and shrinking) effect when tapping the Heart Icon.

2. Stagger List Animation (Sliding in one after the other) for the list of startup names.

**You can add other animations if you wish to.**

