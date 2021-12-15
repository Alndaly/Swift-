# 类型
## 列表
```swift
var scores = Array<Int>()
//或者
scores = [String]()
//又或者
var scores: [String] = []
```
### 添加元素
```swift
scores.append(100)
```
### 删除元素
```swift
scores.remove(at: 1)
```
## 字典
```swift
var employee2 = [String: String]()
```
```swift
let employee2 = [
    "name": "Taylor Swift",
    "job": "Singer", 
    "location": "Nashville"
]
```
> 注意，和其他语言不同，外层用的是`[]`

## Set
set的操作和列表基本一致
```swift
let people = Set(["Denzel Washington", "Tom Cruise", "Nicolas Cage", "Samuel L Jackson"])
```
优点：相比列表而言在查询数据时数据会快很多很多
e.g.
`people.contain("Tome Curise")`
缺点：数组的顺序无法保证
## 枚举
```swift
enum Weekday {
    case monday
    case tuesday
    case wednesday
    case thursday
    case friday
}
```
也可以如下定义
```swift
enum Weekday {
    case monday, tuesday, wednesday, thursday, friday
}
```
调用
```swift
var day = Weekday.monday
day = Weekday.tuesday
day = Weekday.friday
```
如果一个数据已经被定义成了一种枚举类型，那么下次再次修改他的枚举类型的时候可以直接调用`.***`
e.g.
```swift
var day = Weekday.monday
day = .tuesday
day = .friday
```
# 闭包（Closure）
```swift
let sayHello = {
    print("Hi there!")
}

sayHello()
```
Swift给这种定义方式起了一个名字——闭包表达式，是一种非常好的方式说明我们创造了一个闭包——一组我们在任何时候都能够传递以及调用的代码。
如果要在闭包函数中传参数，那么需要在`{}`内部加入参数。
e.g
```swift
let sayHello = { (name: String) -> String in
    "Hi \(name)!"
}
```
> 注意：在闭包函数中，传参和函数返回返回类型结束后一定要加上`in`表示接下来是函数的具体代码体部分！

#  变量监视器
```swift
struct App {
 var contacts = [String]() {
	 willSet {
		 print("Current value is: \(contacts)")
		 print("New value will be: \(newValue)")
	 }
	 didSet {
		 print("There are now \(contacts.count) contacts.")
		 print("Old value was \(oldValue)")
	 }
 }
}

var app = App()
app.contacts.append("Adrian E")
app.contacts.append("Allen W")
app.contacts.append("Ish S")
```
`willSet`变量将要改变时调用
`didSet`变量刚改变结束时调用

# 静态方法
struct中的静态方法/参数不能调用非静态方法/参数
struct中的非静态方法调用静态方法的数据要通过 使用类型的名字比如`School.studentCount`，也可以使用 `Self`来指代当前的struct。
> 注意：self和Self是不一样的！前者代表的是struct==实例==的值，而后者代表的是==当前的struct类型==！

# 结构体（Struct）
> 注意：结构体中的函数不需要加`func`
# 类（Class）
和Struct很相似，但两者都非常重要
```swift
class Employee {
    let hours: Int

    init(hours: Int) {
        self.hours = hours
    }
}
```
> 注意：和Struct不同，所有的Class需要自己定义init函数！

Class 正常copy之后两者代表的是同一个东西，一处修改另外的会同步修改（有点像指向同一处内存的概念）
看如下代码：
```swift
class User {
    var username = "Anonymous"
	var user1 = User()
	var user2 = user1
	user2.username = "Taylor"
	print(user1.username) 
	//Taylor
	print(user2.username)
	//Taylor
}
```
可以看到修改了user2的username但是user1的username也一起改变了

想要避免这种情况需要自定义一个copy函数用于重新创建一个user实例（其实也蛮像Python的）
```swift
class User {
    var username = "Anonymous"

    func copy() -> User {
        let user = User()
        user.username = username
        return user
    }
}
```
上述代码通过`let User = User()`新创建了一个User实例同时通过`user.username=username`的方式将原user的数据“复制”了一份到新的user。

## 继承（inherit）
> **Tip:**如果你确定知道某一个类之后不应该被继承，那么你可以加上`final`的前缀。这意味着这个类能继承自其他父类但是不能被继承为别的类的父类。

e.g.
父类：
```swift
class Vehicle {
    let isElectric: Bool

    init(isElectric: Bool) {
        self.isElectric = isElectric
    }
}
```
子类：
> 继承了父类的子类需要在init函数中调用`super.init()`来调用父类的init函数
```swift
class Car: Vehicle {
    let isConvertible: Bool

    init(isElectric: Bool, isConvertible: Bool) {
        self.isConvertible = isConvertible
        super.init(isElectric: isElectric)
    }
}
```
>**Tip:** 如果一个子类没有任何自己的新的变量（没有定义自己的init函数），那么会自动继承父类的init函数。（怎么看都像Python）
## 方法的重写
重写父类的方法时，需要加上`override`前缀
```swift
override func printSummary() {
    print("I'm a developer who will sometimes work \(hours) a day, but other times spend hours arguing about whether code should be indented using tabs or spaces.")
}
```
