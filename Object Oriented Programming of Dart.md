# Object Oriented Programming of Dart

[TOC]

## Classes

### ?.

Use `?.` instead of `.` to avoid an exception when the leftmost operand is null:

```dart
// if p.y is non-null, set its y value to 4
p?.y = 4;
```

### Using Constructors

### Getting an object's type

`runtimeType`is a property which returns a `Type` object.  

```dart
a.runtimeType
```

### Instance variables

All instance variables generate an implicit *`getter`* method. Non-final instance variables also generate an implicit *`setter`* method. 

Values are default to `null`. 

Instance variables are initialized before constructors and initializer list. 

```dart
class Point {
  num x; // Declare instance variable x, initially null.
  num y; // Declare y, initially null.
  num z = 0; // Declare z, initially 0.
}

void main() {
  var point = Point();
  point.x = 4; // Use the setter method for x.
  assert(point.x == 4); // Use the getter method for x.
}
```

### Constructors

<u>**Constructors aren’t inherited.**</u> 

```dart
class Point {
  num x, y;
  Point(num x, num y) {
    this.x = x;
    this.y = y;
  }
}
```

A equal syntactic sugar:

```dart
class Point {
  num x, y;

  // Syntactic sugar for setting x and y
  // before the constructor body runs.
  Point(this.x, this.y);
}

void main() {
  p1 = Point(2, 3); // p1.x = 2, p1.y = 3
}
```

#### Named constructors

Use a named constructor to implement multiple constructors for a class or to provide extra clarity:

```dart
class Point {
  num x, y;
  Point(this.x, this.y);

  // Named constructor
  Point.origin() {
    x = 0;
    y = 0;
  }
}

void main() {
  O = Point.origin(); // O.x = O.y = 0
}
```

#### Getters and setters

 using the `get` and `set` keywords

```dart
class Rectangle {
  num left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Define two calculated properties: right and bottom.
  num get right => left + width;
  set right(num value) => left = value - width;
  num get bottom => top + height;
  set bottom(num value) => top = value - height;
}

void main() {
  var rect = Rectangle(3, 4, 20, 15);
  assert(rect.left == 3);
  rect.right = 12;
  assert(rect.left == -8);
}
```

#### Implicit interfaces

If you want to create a class A using B's API without inherenting B, you can use implicit interface 

```dart
// A person. The implicit interface contains greet().
class Person {
  // In the interface, but visible only in this library.
  final _name;

  // Not in the interface, since this is a constructor.
  Person(this._name);

  // In the interface.
  String greet(String who) => 'Hello, $who. I am $_name.';
}

// An implementation of the Person interface.
class Impostor implements Person {
  get _name => '';

  String greet(String who) => 'Hi $who. Do you know who I am?';
}

String greetBob(Person person) => person.greet('Bob');

void main() {
  print(greetBob(Person('Kathy')));
  print(greetBob(Impostor()));
}

// implements multiple interfaces:
class Point implements Comparable, Location {...}
```

#### Extanding a class

Use `extends` to create a subclass, and `super` to refer to the superclass. 

#### Overriding members

Subclasses can override instance methods, getters, and setters. You can use the `@override` annotation to indicate that you are intentionally overriding a member:

```dart
class SmartTelevision extends Television {
  @override
  void turnOn() {...}
  // ···
}
```

#### mixins

A mixin don't have constructors:

```dart
mixin Musical {
  bool canPlayPiano = false;
  bool canCompose = false;
  bool canConduct = false;

  void entertainMe() {
    if (canPlayPiano) {
      print('Playing piano');
    } else if (canConduct) {
      print('Waving hands');
    } else {
      print('Humming to self');
    }
  }
}
```

To use mixins, use the `with` keyword followed by one or more mixin names:

```dart
class Musician extends Performer with Musical {
  // ···
}

class Maestro extends Person
    with Musical, Aggressive, Demented {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}
```

#### Optional parameters

```dart
void enableFlags({bool bold, bool hidden}) {
  // ...
}

void main() {
  enableFlags(bold: true, hidden: false);
}
```

You can use  `@required`  to indicate this  is a required parameter, like:

```dart
void enableFlage({bool bold, @required bool hidden}) {
  // ...
}
```

# Object Oriented Programming of Dart

