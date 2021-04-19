# Startup Namer - Part 3


## What you'll learn
- How to add interactivity to a stateful widget


## What you'll build 
End users can select and unselect names, saving the best ones.

The following image shows how the app would look like after completing part 3.

![image](https://user-images.githubusercontent.com/49060283/114085862-8e6b9700-98cf-11eb-84a6-1a08ed17bbba.png)


In this step, you'll add heart icons to each row. 

- **Step 1:-** Add a `_saved` `Set` to `_RandomWordsState`. This `Set` stores the word pairings that the user favorited. `Set` is preferred to `List` because a properly implemented `Set` doesn't allow duplicate entries.

```
class _RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];
  final _saved = <WordPair>{};     // NEW
  final _biggerFont = TextStyle(fontSize: 18.0);
  ...
}
```


- **Step 2:-**
 In the `_buildRow` function, add an `alreadySaved` check to ensure that a word pairing has not already been added to favorites.

```
Widget _buildRow(WordPair pair) {
  final alreadySaved = _saved.contains(pair);  // NEW
  ...
}
```


In `_buildRow()` , you'll also add heart-shaped [icons](https://api.flutter.dev/flutter/widgets/Icon-class.html) to the [`ListTile`](https://api.flutter.dev/flutter/material/ListTile-class.html) objects to enable favoriting. In the next step, you'll add the ability to interact with the heart icons.

- **Step 3:-** Add the icons after the text, as shown below:

```
Widget _buildRow(WordPair pair) {
  final alreadySaved = _saved.contains(pair);
  return ListTile(
    title: Text(
      pair.asPascalCase,
      style: _biggerFont,
    ),
    trailing: Icon(   // NEW from here... 
      alreadySaved ? Icons.favorite : Icons.favorite_border,
      color: alreadySaved ? Colors.red : null,
    ),                // ... to here.
  );
}
```


- **Step 4:-** Hot reload the app.

You should now see open hearts on each row, but they are not yet interactive.

![image](https://user-images.githubusercontent.com/49060283/114086606-70526680-98d0-11eb-8574-732f035d6131.png)


In the next step, you'll make the heart icons tappable. When the user taps an entry in the list, toggling its favorited state, that word pairing is added or removed from a set of saved favorites.


To do that, you'll modify the `_buildRow` function. If a word entry has already been added to favorites, tapping it again removes it from favorites. When a tile has been tapped, the function calls `setState()` to notify the framework that state has changed.


- **Step 5:-** Add `onTap` to the `_buildRow` method, as shown below:

```
Widget _buildRow(WordPair pair) {
  final alreadySaved = _saved.contains(pair);
  return ListTile(
    title: Text(
      pair.asPascalCase,
      style: _biggerFont,
    ),
    trailing: Icon(
      alreadySaved ? Icons.favorite : Icons.favorite_border,
      color: alreadySaved ? Colors.red : null,
    ),
    onTap: () {      // NEW lines from here...
      setState(() {
        if (alreadySaved) {
          _saved.remove(pair);
        } else { 
          _saved.add(pair); 
        } 
      });
    },               // ... to here.
  );
}
```


> Tip: In Flutter's reactive style framework, calling **setState()** triggers a call to the **build()** method for the **State** object, resulting in an update to the UI.


- **Step 6:-** Hot reload the app.

You should be able to tap any tile to favorite or unfavorite the entry. Tapping a tile generates an implicit ink splash animation emanating from the tap point.


IOS | Android
------------ | -------------
![image](https://user-images.githubusercontent.com/49060283/114085862-8e6b9700-98cf-11eb-84a6-1a08ed17bbba.png) | ![image](https://user-images.githubusercontent.com/49060283/114085912-9deae000-98cf-11eb-8b2f-e0cd1cc32493.png)


## Observations

> - [Trailing](https://api.flutter.dev/flutter/material/ListTile/trailing.html) is a widget to display after the title(right-aligned). Typically an Icon Widget.
> - Leading is a widget to display before the title(left-aligned). Typically an Icon Widget.
> - **setState()** When the widget’s state changes, the state object calls setState(), telling the framework to redraw the widget.


## Challenge

You've already seen how a simple package such as the English words package works, and how it gets downloaded and incorporated into our project. You're now ready to go ahead and  **incorporate an audio file playing package** into our project.
 
The Challenge for today is to incorporate **Button click sound effect when tapping the heart icon.**
 
**Hint:**

1. Download the sound(https://www.zapsplat.com/sound-effect-category/button-clicks/)

2. Store it in the assets folder

3. Find a suitable package from pub.dev (https://pub.dev/packages/audioplayers you can use this package)

4. Add dependency in your pubspec.yaml file

5. Import and use it in your project.







