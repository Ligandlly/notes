![Flutter](https://p0.ssl.qhimg.com/t01c3f5bf72e7d1ac67.png)

## widgets

The runApp() function takes the given Widget and makes it the root of the widget tree. 

## StatefulWidget

A `StatefulWidget` can be reloaded by calling the method `setState()`. This will trigger the `build(BuildContext context)` to be called again.

**To create a new `StatefulWidget`, we need 2 classes.** One is derived from `StateFulWidget`, another is derived from `State<NameOfWidget>` .

The method  `createState()` is required for a class derived from `StatefulWidget`.

```dart
class MyApp extends StatefulWidget {
  MyApp({this.TextInput});
  final Widget TextInput;
  MyAppState createState() => new MyAppState(); // Required
}

class MyAppState extends State<MyApp> {
  bool checkBoxValue = false;
  @override
  Widget build(BuildContext ctxt) {
    return new MaterialApp(
      title: "MySampleApplication",
      home: new Scaffold(
        appBar: new AppBar(
          title: new Text("Hello Flutter App"),
        ),
        body: new Center(
          child: new Column(
            children: <Widget>[
              widget.TextInput,
              new Checkbox(
                  value: checkBoxValue,
                  onChanged: (bool newValue){
                    setState(() {
                      checkBoxValue = newValue;
                    }); // setState()
                  }  // (bool newValue){}
              )  // CheckBox()
            ],  // <Widget>[]
          ),  // Column()
        )  // Centor()
      ),  // Scaffold()
    );  // MaterialApp()
  }
}
```

# Flutter