# Startup Namer - Part 2

## What you'll learn

- How to implement a stateful widget
- How to create an infinite, lazily loaded list

The following animated GIF shows how the app works at the completion of part 2:

![6556f8b61acd6a89](https://user-images.githubusercontent.com/49060283/112726269-7cd6d680-8f42-11eb-8548-ab99ec93e462.gif)



Stateless widgets are immutable, meaning that their properties can't change—all values are final.

Stateful widgets maintain state that might change during the lifetime of the widget. Implementing a stateful widget requires at least two classes:  a [`StatefulWidget`](https://api.flutter.dev/flutter/widgets/StatefulWidget-class.html) that creates an instance of a [`State`](https://api.flutter.dev/flutter/widgets/State-class.html) class. The `StatefulWidget` object is, itself, immutable and can be thrown away and regenerated, but the `State`object persists over the lifetime of the widget.

In this task, you'll add a stateful widget, `RandomWords`, which creates its State class, `_RandomWordsState`. You'll then use `RandomWords` as a child inside the existing MyApp stateless widget.


- **Step 1:-** Create boilerplate code for a stateful widget.

It can go anywhere in the file outside of `MyApp`, but the solution places it at the bottom of the file. In `lib/main.dart`, position your cursor after all of the code, enter **Return** a couple times to start on a fresh line. In your IDE, start typing `stful`. The editor asks if you want to create a `Stateful` widget. Press **Return** to accept. The boilerplate code for two classes appears, and the cursor is positioned for you to enter the name of your statefull widget.


- **Step 2:-** Enter `RandomWords` as the name of your widget.

As you can see in the code below, the `RandomWords` widget does little else beside creating its `State` class.

Once you've entered `RandomWords` as the name of the stateful widget, the IDE automatically updates the accompanying `State` class, naming it `_RandomWordsState`. By default, the name of the `State` class is prefixed with an underscore. Prefixing an identifier with an underscore enforces privacy in the Dart language and is a recommended best practice for `State` objects.

The IDE also automatically updates the state class to extend `State<RandomWords>`, indicating that you're using a generic `State` class specialized for use with `RandomWords`. Most of the app's logic resides here⁠—it maintains the state for the `RandomWords` widget. This class saves the list of generated word pairs, which grows infinitely as the user scrolls and, in next task, favorites word pairs as the user adds or removes them from the list by toggling the heart icon.

Both classes now look as follows:

```class RandomWords extends StatefulWidget {
  @override
  _RandomWordsState createState() => _RandomWordsState();
}

class _RandomWordsState extends State<RandomWords> {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}
```


- **Step 3:-** Update the `build()` method in `_RandomWordsState.`

Replace `return Container();` with the following two lines:

```
class _RandomWordsState extends State<RandomWords> {
  @override                                  
  Widget build(BuildContext context) {
    final wordPair = WordPair.random();      // NEW
    return Text(wordPair.asPascalCase);      // NEW
  }                                         
}
```


- **Step 4:-** Remove the word-generation code from `MyApp` by making the following changes:

```
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final wordPair = WordPair.random();  // DELETE

    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          //child: Text(wordPair.asPascalCase), // REPLACE with... 
          child: RandomWords(),                 // ...this line
        ),
      ),
    );
  }
}
```



- **Step 5:-** Hot reload the app. The app should behave as before, displaying a word pairing each time you hot reload or save the app.

> Tip: If you see a warning on a hot reload that you might need to restart the app, you should consider restarting the app. It might be a false positive, but restarting ensures that your changes are reflected in the app's UI.


In the next step, you'll expand `_RandomWordsState` to generate and display a list of word pairings. As the user scrolls, the list (displayed in a `ListView` widget) grows infinitely. The `builder` factory constructor in `ListView` allows you to lazily build a list view on demand.


- **Step 6:-** Add some state variables to the `_RandomWordsState` class.

Add a `_suggestions` list for saving suggested word pairings. Also, add a `_biggerFont` variable for making the font size larger.

```
class _RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];                 // NEW
  final _biggerFont = const TextStyle(fontSize: 18); // NEW
  ...
}
```

Next, you'll add a `_buildSuggestions()` function to the `_RandomWordsState` class. This method builds the `ListView` that displays the suggested word pairing.

The [`ListView`](https://api.flutter.dev/flutter/widgets/ListView-class.html) class provides a builder property, `itemBuilder`, that's a factory builder and callback function specified as an anonymous function. Two parameters are passed to the function—the `BuildContext` and the row iterator, `i`. The iterator begins at 0 and increments each time the function is called, once for every suggested word pairing. This model allows the suggestion list to continue growing as the user scrolls.


- **Step 7:-** Add the entire `_buildSuggestions` function.

In the `_RandomWordsState` class, add the following function, deleting the comments, if you prefer:

```
  Widget _buildSuggestions() {
    return ListView.builder(
      padding: const EdgeInsets.all(16),
      // The itemBuilder callback is called once per suggested 
      // word pairing, and places each suggestion into a ListTile
      // row. For even rows, the function adds a ListTile row for
      // the word pairing. For odd rows, the function adds a 
      // Divider widget to visually separate the entries. Note that
      // the divider may be difficult to see on smaller devices.
      itemBuilder: (BuildContext _context, int i) {
        // Add a one-pixel-high divider widget before each row 
        // in the ListView.
        if (i.isOdd) {
          return Divider();
        }

        // The syntax "i ~/ 2" divides i by 2 and returns an 
        // integer result.
        // For example: 1, 2, 3, 4, 5 becomes 0, 1, 1, 2, 2.
        // This calculates the actual number of word pairings 
        // in the ListView,minus the divider widgets.
        final int index = i ~/ 2;
        // If you've reached the end of the available word
        // pairings...
        if (index >= _suggestions.length) {
          // ...then generate 10 more and add them to the 
          // suggestions list.
          _suggestions.addAll(generateWordPairs().take(10));
        }
        return _buildRow(_suggestions[index]);
      }
    );
  }
  ```
  The `_buildSuggestions` function calls `_buildRow` once per word pair. That function displays each new pair in a `ListTile`, which allows you to make the rows more attractive in the next task.


- **Step 8:-** Add a `_buildRow` function to `_RandomWordsState`:

```
  Widget _buildRow(WordPair pair) {
    return ListTile(
      title: Text(
        pair.asPascalCase,
        style: _biggerFont,
      ),
    );
  }
  ```


- **Step 9:-** Update the `build` method for `_RandomWordsState`.

Change it to use `_buildSuggestions()`, rather than directly calling the word-generation library. ( [`Scaffold`](https://docs.flutter.io/flutter/material/Scaffold-class.html)implements the basic Material Design visual layout.)

```
  @override
  Widget build(BuildContext context) {
    //final wordPair = WordPair.random(); // Delete these... 
    //return Text(wordPair.asPascalCase); // ... two lines.

    return Scaffold (                     // Add from here... 
      appBar: AppBar(
        title: Text('Startup Name Generator'),
      ),
      body: _buildSuggestions(),
    );                                      // ... to here.
  }
  ```


- **Step 10:-** Update the `build` method for `MyApp`, changing the title, removing the `AppBar`, and changing the home property to a `RandomWords` widget.

```
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Startup Name Generator',
      home: RandomWords(),
    );
  }
  ```


- **Step 11:-** Restart the app. You should see a list of word pairings no matter how far you scroll.


IOS | Android
------------ | -------------
![ae47ef0ac2f492b8](https://user-images.githubusercontent.com/49060283/113668680-3adf2a80-96d0-11eb-9fe8-2881768fad71.png) | ![df2b3cb779e0020e](https://user-images.githubusercontent.com/49060283/113668684-3ca8ee00-96d0-11eb-9700-df517e8664f9.png)






