# Swift4

## struct

e.g.

```swift
struct StructName {
    var dataMember: Int

    func memberFunction(parameter: Int) -> Int{
        // function body
        return parameter
    }
}

// creat objects
let object = StructName(dataMember: 1)
object.memberFunction(parameter: 2)
```

### Initializer

1. Default value:
```swift
struct Odometer {
     var count: Int = 0
 }
 let odometer = Odometer() // odometer.count = 0
```

2. Member wise  initializer 成员级构造器

```swift
 /* 续 */
 let odo = Odometer(count: 27000) // odometer.count = 27000
```

3. 自定构造器

```swift
struct Temperature {
  var celsius: Double
  init(celsius: Double) {
        self.celsius - celsius
      }
    
  init(fahrenheit: Double) {
    celsius = (fahrenheit - 32) / 1.8
  }

  init(kelvin: Double) {
    celsius = kelvin - 273.15
  }
}

let currentTemperature = Temperature(celsius: 18.5)
let boiling = Temperature(fahrenheit: 212.0)
let freezing = Temperature(kelvin: 273.15)
```



### mutating func

> 如果要更改数据成员，需要函数声明为`mutating func`.一般的函数`func`不能更改数据成员  

### 属性观察器 `willset`, `didset`



## Class

> 类可以继承，结构不能  

1. 创建子类

``` swift
class {

}
```
2. 覆盖
	* 方法

```swift
 class Vehicle{
    func makeNoise(){
        print("Di Di")
    }
 }
 class Train: Vehicle{
 override func makeNoise(){
             print("choo choo")
 }
}
```
		* 属性

```swift
         class Car: Vehicle {
             var gear = 1
             override var description: String {
                 return super.description + " in gear \(gear)"
             }
         }
        
```

3. 在子类的构造函数中需要初始化所有属性

```swift
    class Student: Person {
      var favoriteSubject: String
    
     init(name: String, favoriteSubject: String) {
        self.favoriteSubject = favoriteSubject
        super.init(name: name)
      }
    }
```
4. 拷贝类会生成引用



## 数组，集合

```swift
var myArray = [Int](repeating: 0, count: 100)
myArray.count // 100
myArray.isEmpty // false
```

## 循环

```swift
for index in 1...5 {
}
for index in 1..<5 {
}
```



## 可选类型

1. define
```swift
    var serverResponseCode: Int? = 404
    var serverResponseCode: Int? = nil
```

2. use: need force-unwrap operator "!" to unwrap

```swift
    var pub: Int? = 1
    
    if pub != nil { // 需要验证，解包nil类型会出错
        print(pub) // Expression implicitly coerced from 'Int?' to 'Any'
        print(pub!)
    }
```

    推荐：

```swift
    if let unwrappedPublicationYear = book.publicationYear {
      print("The book was published in \(unwrappedPublicationYear)")
    }
    else {
      print("The book does not have an official publication date.")
    }
```

​    

## 类型转化

 `as!`会尝试将类型向下转换

``` swift
var items: [Any] = [5, "Bill", 6.7, true]
if let firstItem = items[0] as? Int {
  print(firstItem+4) //9
}
```

## Guard

```swift
func singHappyBirthday() {
  if birthdayIsToday {
    if invitedGuests > 0 {
      if cakeCandlesLit {
        print("Happy Birthday to you!")
      } else {
        print("The cake's candles haven't been lit.")
      }
    } else {
      print("It's just a family party.")
    }
  } else {
    print("No one has a birthday today.")
  }
}
```

```swift
func singHappyBirthday() {
  guard birthdayIsToday else {
    print("No one has a birthday today.")
    return
  }
    
guard invitedGuests > 0 else {
    print("It's just a family party.")
    return
  }

  guard cakeCandlesLit else {
    print("The cake's candles haven't been lit.")
    return
  }

  print("Happy Birthday to you!")
}
```

```swift
guard condition else {
    // false: excute some code
    return
}
// true: excute some code
```



## enum

```swift
enum CompassPoint {
    case north
    case east
    case west
    case sounth
}
let heading: CompassPoint = .west
switch heading {
    case .east:    
}
```


#swift/syntax#