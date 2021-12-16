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
## 元组（tuple）
一旦确定后长度就不可变
## 字典（Dict）
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
> 注意：结构体中一旦有private的属性并且这个属性没有默认值时，swift将不会为我们自动生成init函数，而是需要我们自己定义。

结构体中的函数想要修改当前结构体的属性时
- var（变量） 直接函数不能修改 需要给函数加上`mutating`前缀才能修改
- let（常量）对于这种属性，那么结构体的任何函数都无法对其做修改
## 计算属性（computed property）
> 注意：计算属性不可以为常量
```swift
struct Employee {
    let name: String
    var vacationAllocated = 14
    var vacationTaken = 0

    var vacationRemaining: Int {
        vacationAllocated - vacationTaken
    }
}
```
上述代码中的`vacationRemaining`即为计算属性
```swift
var vacationRemaining: Int {
    get {
        vacationAllocated - vacationTaken
    }

    set {
        vacationAllocated = vacationTaken + newValue
    }
}
```
通过设置计算属性的`get`和`set`可以自定义计算属性的读写
e.g.


```swift
var archer = Employee(name: "Sterling Archer", vacationAllocated: 14)
archer.vacationTaken += 4
archer.vacationRemaining = 5
print(archer.vacationAllocated)
```
> 注意：如果写了计算属性，那么会影响到和他有关的数据
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
> 注意：和Struct不同，所有的Class需要自己定义init函数！同时class内部的函数不能有mutating前缀。

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

# 解构
看如下代码
```swift
class User {
    let id: Int

    init(id: Int) {
        self.id = id
        print("User \(id): I'm alive!")
    }

    deinit {
        print("User \(id): I'm dead!")
    }
}

var users = [User]()

for i in 1...3 {
    let user = User(id: i)
    print("User \(user.id): I'm in control!")
    users.append(user)
}

print("Loop is finished!")
users.removeAll()
print("Array is clear!")
```
上述代码输出如下：
```sh
User 1: I'm alive!
User 1: I'm in control!
User 2: I'm alive!
User 2: I'm in control!
User 3: I'm alive!
User 3: I'm in control!
Loop is finished!
User 1: I'm dead!
User 2: I'm dead!
User 3: I'm dead!
Array is clear!
```
> 可以看到，对于类来讲，当同一类的最后一个实例被清除时，会调用解构方法。（注意解构`deinit`方法和`init`方法一样不需要加`func`前缀，这两个都是特殊的）

# 协议（Protocol）
感觉有点像typescript中的`interface`
e.g.
```swift
protocol Purchaseable {
	var price: Double { get set }
	var currency: String { get set }
}
```

```swift
protocol Vehicle {
    func estimateTime(for distance: Int) -> Int
    func travel(distance: Int)
}
```
```swift
struct Bicycle: Vehicle {
    func estimateTime(for distance: Int) -> Int {
        distance / 10
    }

    func travel(distance: Int) {
        print("I'm cycling \(distance)km.")
    }
}

let bike = Bicycle()
commute(distance: 50, using: bike)
```
> **Tip:** 你可以遵从任意数量的协议（Protocol），把他们一个一个用逗号分隔开列在结构体/类名称之后即可。如果你还需要继承某一个父类，那么你需要将父类写在第一个，后面更上协议就可以。

# 不透明返回类型（opaque return types）
告诉swift我的这个函数返回的类型是某种类型的返回，但是具体值你自己解决
```swift
func getRandomNumber() -> Int {
    Int.random(in: 1...6)
}

func getRandomBool() -> Bool {
    Bool.random()
}

print(getRandomNumber() == getRandomNumber())
```
上述代码可以运行因为Int实现了Equatable协议
但是换成如下：
```swift
func getRandomNumber() -> Equatable {
    Int.random(in: 1...6)
}

func getRandomBool() -> Equatable {
    Bool.random()
}
print(getRandomNumber() == getRandomNumber())
```
上述代码不能运行，因为虽然都是`getRandomNumber()`但是返回的是个Equatable，而具体值是啥swift不知道
此时，`opaque return types`就起作用了，修改成如下代码
```swift
func getRandomNumber() -> some Equatable {
    Int.random(in: 1...6)
}

func getRandomBool() -> some Equatable {
    Bool.random()
}
print(getRandomNumber() == getRandomNumber())
```
可以看到，在每个函数的返回的类型前面加了`some`前缀后就可以再次比较了（需要是同一个函数的返回，不同函数的返回依然无法比较）
# 扩展（Extensions）

```swift
var quote = "   The truth is rarely pure and never simple   "

let trimmed = quote.trimmingCharacters(in: .whitespacesAndNewlines)

extension String {
    func trimmed() -> String {
        self.trimmingCharacters(in: .whitespacesAndNewlines)
    }
}
```
写了扩展之后，我们可以在任何地方仅仅通过如下方式就能获取一个删除了空格符的字符串：
```swift
let trimmed = quote.trimmed()
```
# 协议扩展（protocol extension）
举例如下，如果我们有如下协议：

```swift
protocol Person {
    var name: String { get }
    func sayHello()
}
```

这意味着所有实现`Person`协议的都必须要有`sayHello()`方法，但是我们也可以增加一个默认的扩展。就像下面这样：

```swift
extension Person {
    func sayHello() {
        print("Hi, I'm \(name)")
    }
}
```

这样实现`Person`协议的类或者结构体都可以自己决定是否增加`sayHello()`方法，他们可以选择仅仅依赖我们增加的默认的扩展。

所以我们可以写出像下面这样的代码：

```swift
struct Employee: Person {
    let name: String
}
```

但是由于这串代码实现了`Person`协议，这个协议又有一个默认的扩展，所以我们可以直接调用`sayHello()`方法。

```swift
let taylor = Employee(name: "Taylor Swift")
taylor.sayHello()
```
再举个例子：
```swift
protocol Politician {
	var isDirty: Bool { get set }
	func takeBribe()
}
extension Politician {
	func takeBribe() {
		if isDirty {
			print("Thank you very much!")
		} else {
			print("Someone call the police!")
		}
	}
}
```
# *更深层次的协议扩展（非新手内容）*
## Equatable
看如下代码
```swift
struct User: Equatable {
    let name: String
}

let user1 = User(name: "Link")
let user2 = User(name: "Zelda")
print(user1 == user2)
//false
print(user1 != user2)
//true
```
实现了Equatable协议的的Swift构造体，在新建不同的实例时，可以相互比较，会将不同的实例中的所有属性一一对比，如果完全相同才会返回`true`
## Comparable
```swift
struct User: Equatable, Comparable {
    let name: String
	
	static func <(lhs: User, rhs: User) -> Bool {
		lhs.name < rhs.name
	}
}



let user1 = User(name: "Taylor")
let user2 = User(name: "Adele")
print(user1 < user2)
```
由于Comparable协议继承自Equatable协议
所以可以把Equatable协议去掉，代码如下
```swift
struct User: Equatable, Comparable {
    let name: String
	
	static func <(lhs: User, rhs: User) -> Bool {
		lhs.name < rhs.name
	}
}



let user1 = User(name: "Taylor")
let user2 = User(name: "Adele")
print(user1 < user2)
```
# optionals
## if let
```swift
let opposites = [
    "Mario": "Wario",
    "Luigi": "Waluigi"
]

let peachOpposite = opposites["Peach"]
```
上述代码会直接返回nil
但不推荐这样
应该通过如下代码去返回（以确保你知道这个属性可能是nil并且对应做了处理）
```swift
if let marioOpposite = opposites["Mario"] {
    print("Mario's opposite is \(marioOpposite)")
}else{
	print("Mario is not in the dict")
}
```
上述代码的含义是：
如果`opposites`中存在`Mario`属性那么就打印`Mario's opposite is \(marioOpposite)`否则就打印`Mario is not in the dict`
e.g.
```swift
var username: String? = nil

if let unwrappedName = username {
    print("We got a user: \(unwrappedName)")
} else {
    print("The optional was empty.")
}
```
另一个很具有迷惑性的例子：
```swift
func square(number: Int) -> Int {
    number * number
}

var number: Int? = nil
print(square(number: number))
```
通过明显的不同变量我们可以像下面这么写
```swift
if let unwrappedNumber = number {
    print(square(number: unwrappedNumber))
}
```
但有时候（绝大多数时候）都会像下面这样写（看起来很让人迷惑）
```swift
if let number = number {
    print(square(number: number))
}
```
其含义是：如果number这个属性存在值，那么就将其作为参数，传递给square函数并打印函数的结果。

## Guard（守卫）
```swift
var myVar: Int? = 3

if let unwrapped = myVar {
    print("Run if myVar has a value inside")
}else{
	...
}

guard let unwrapped = myVar else {
    print("Run if myVar doesn't have a value inside")
}
```
`if let`执行的是如果`myVar`这个变量存在值时的代码，否则执行`else`下的代码；而`guard let`恰恰相反，`{}`中执行的是`myVar`这个变量不存在值时的情况，注意此处必须`return`或者抛出错误（所以称他为守卫），如果有值的话，那么这个值赋给`upwrapped`然后接着运行程序。
## nil coalescing
e.g.1
```swift
let tvShows = ["Archer", "Babylon 5", "Ted Lasso"]
let favorite = tvShows.randomElement() ?? "None"
```
e.g.2
```swift
let input = ""
let number = Int(input) ?? 0
print(number)
```
## （nil值可以链式传递）nil chain
```swift
struct Book {
    let title: String
    let author: String?
}

var book: Book? = nil
let author = book?.author?.first?.uppercased() ?? "A"
print(author)
```
上述代码`book`是`nil `所以一路向后传递一直等于`nil `最终`uppercased()`返回的是nil，所以`author`就是`A`d
## 用try?处理错误
```swift
enum UserError: Error {
    case badID, networkFailed
}

func getUser(id: Int) throws -> String {
    throw UserError.networkFailed
}

if let user = try? getUser(id: 23) {
    print("User: \(user)")
}
```
上述代码代表`user`将会是`optianal`的，如果`getUser(id:23)`返回成功了那么`user`就会有一个具体的值，否则就是`nil`。
