# Linkedin 样式指南

首先推荐阅读下 [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)。

## Table Of Contents

* [Swift Style Guide](linkedin-yang-shi-zhi-nan.md#swift-style-guide)
  * [Table Of Contents](linkedin-yang-shi-zhi-nan.md#table-of-contents)
  * [1. Code Formatting:代码格式化](linkedin-yang-shi-zhi-nan.md#1-code-formatting代码格式化)
  * [2. Naming:命名](linkedin-yang-shi-zhi-nan.md#2-naming命名)
  * [3. Coding Style](linkedin-yang-shi-zhi-nan.md#3-coding-style)
    * [3.1 General](linkedin-yang-shi-zhi-nan.md#31-general)
    * [3.2 Access Modifiers](linkedin-yang-shi-zhi-nan.md#32-access-modifiers)
    * [3.3 Custom Operators:自定义操作符](linkedin-yang-shi-zhi-nan.md#33-custom-operators自定义操作符)
    * [3.4 Switch Statements and `enum`s](linkedin-yang-shi-zhi-nan.md#34-switch-statements-and-enums)
    * [3.5 Optionals](linkedin-yang-shi-zhi-nan.md#35-optionals)
    * [3.6 Protocols](linkedin-yang-shi-zhi-nan.md#36-protocols)
    * [3.7 Properties](linkedin-yang-shi-zhi-nan.md#37-properties)
    * [3.8 Closures:闭包](linkedin-yang-shi-zhi-nan.md#38-closures闭包)
    * [3.9 Arrays](linkedin-yang-shi-zhi-nan.md#39-arrays)
    * [3.10 Error Handling](linkedin-yang-shi-zhi-nan.md#310-error-handling)
    * [3.11 Using `guard` Statements](linkedin-yang-shi-zhi-nan.md#311-using-guard-statements)
  * [4. Documentation/Comments](linkedin-yang-shi-zhi-nan.md#4-documentationcomments)
    * [4.1 Documentation](linkedin-yang-shi-zhi-nan.md#41-documentation)
    * [4.2 Other Commenting Guidelines:其他的注释规则](linkedin-yang-shi-zhi-nan.md#42-other-commenting-guidelines其他的注释规则)

## 1. Code Formatting:代码格式化

* **1.1** 使用 4 个空格来代替 Tabs
* **1.2** 避免过长的行，可以在 XCode 中进行设置单行最大长度：\(Xcode-&gt;Preferences-&gt;Text Editing-&gt;Page guide at column: 160 is helpful for this\)
* **1.3** 保证每个文件结尾都存在一个新行 Ensure that there is a newline at the end of every file.
* **1.4** 避免无意义的尾随空格: \(Xcode-&gt;Preferences-&gt;Text Editing-&gt;Automatically trim trailing whitespace + Including whitespace-only lines\).
* **1.5** 避免将单独的左花括号放置到一行，我们参考了：[1TBS style](https://en.m.wikipedia.org/wiki/Indent_style#Variant:_1TBS).

```swift
class SomeClass {
    func someMethod() {
        if x == y {
            /* ... */
        } else if x == z {
            /* ... */
        } else {
            /* ... */
        }
    }

    /* ... */
}
```

* **1.6** 在写变量的类型声明、字典类型的键、函数参数、协议的声明或者父类的时候，不要在冒号前添加空格。

```swift
// specifying type
let pirateViewController: PirateViewController

// dictionary syntax (note that we left-align as opposed to aligning colons)
let ninjaDictionary: [String: AnyObject] = [
    "fightLikeDairyFarmer": false,
    "disgusting": true
]

// declaring a function
func myFunction<T, U: SomeProtocol where T.RelatedType == U>(firstArgument: U, secondArgument: T) {
    /* ... */
}

// calling a function
someFunction(someArgument: "Kitten")

// superclasses
class PirateViewController: UIViewController {
    /* ... */
}

// protocols
extension PirateViewController: UITableViewDataSource {
    /* ... */
}
```

* **1.7** 一般来说，逗号后面都要跟随一个空格。

```swift
let myArray = [1, 2, 3, 4, 5]
```

* **1.8** 在二元操作符譬如`+`, `==`, 或者 `->`的前后需要加上空格，但是对于`(` 、\`\)的前后不需要加空格。

```swift
let myValue = 20 + (30 / 2) * 3
if 1 + 1 == 3 {
    fatalError("The universe is broken.")
}
func pancake() -> Pancake {
    /* ... */
}
```

* **1.9** 我们默认使用 Xcode 推荐的格式化风格\(CTRL-I\) ，在声明某个函数的时候会多行排布参数。

```swift
// Xcode indentation for a function declaration that spans multiple lines
func myFunctionWithManyParameters(parameterOne: String,
                                  parameterTwo: String,
                                  parameterThree: String) {
    // Xcode indents to here for this kind of statement
    print("\(parameterOne) \(parameterTwo) \(parameterThree)")
}

// Xcode indentation for a multi-line `if` statement
if myFirstVariable > (mySecondVariable + myThirdVariable)
    && myFourthVariable == .SomeEnumValue {

    // Xcode indents to here for this kind of statement
    print("Hello, World!")
}
```

* **1.10** 在调用多参数函数的时候，会把多个参数放置到单独的行中：

```swift
someFunctionWithManyArguments(
    firstArgument: "Hello, I am a string",
    secondArgument: resultFromSomeFunction()
    thirdArgument: someOtherLocalVariable)
```

* **1.11** 对于大型的数组或者字典类型，应该将其分割到多行内，`[` 与 `]`类比于花括号进行处理。对于闭包而言也应该同样适合于该规则。

```swift
someFunctionWithABunchOfArguments(
    someStringArgument: "hello I am a string",
    someArrayArgument: [
        "dadada daaaa daaaa dadada daaaa daaaa dadada daaaa daaaa",
        "string one is crazy - what is it thinking?"
    ],
    someDictionaryArgument: [
        "dictionary key 1": "some value 1, but also some more text here",
        "dictionary key 2": "some value 2"
    ],
    someClosure: { parameter1 in
        print(parameter1)
    })
```

* **1.12** 尽可能地使用本地变量的方式来避免多行的判断语句。

```swift
// PREFERRED
let firstCondition = x == firstReallyReallyLongPredicateFunction()
let secondCondition = y == secondReallyReallyLongPredicateFunction()
let thirdCondition = z == thirdReallyReallyLongPredicateFunction()
if firstCondition && secondCondition && thirdCondition {
    // do something
}

// NOT PREFERRED
if x == firstReallyReallyLongPredicateFunction()
    && y == secondReallyReallyLongPredicateFunction()
    && z == thirdReallyReallyLongPredicateFunction() {
    // do something
}
```

## 2. Naming:命名

* **2.1** Swift 中不需要再使用 Objective-C 那样的前缀，譬如使用 `GuybrushThreepwood` 而不是`LIGuybrushThreepwood`。
* **2.2** 对于类型名即`struct`, `enum`, `class`, `typedef`, `associatedtype`等等使用 `PascalCase` 。
* **2.3** 对于函数名、方法名、变量名、常量、参数名等使用`camelCase`。
* **2.4** 在使用首字母缩写的时候尽可能地全部大写，并且注意保证全部代码中的统一。不过如果缩写被用于命名的起始，那么就全部小写。

```swift
// "HTML" is at the start of a variable name, so we use lowercase "html"
let htmlBodyContent: String = "<p>Hello, World!</p>"
// Prefer using ID to Id
let profileID: Int = 1
// Prefer URLFinder to UrlFinder
class URLFinder {
    /* ... */
}
```

* **2.5** 对于静态常量使用 `k` 前缀 + PascalCase。

```swift
class MyClassName {
    // use `k` prefix for constant primitives
    static let kSomeConstantHeight: CGFloat = 80.0

    // use `k` prefix for non-primitives as well
    static let kDeleteButtonColor = UIColor.redColor()

    // don't use `k` prefix for singletons
    static let sharedInstance = MyClassName()

    /* ... */
}
```

* **2.6** 对于泛型或者关联类型，使用`PascalCase`描述泛型，如果泛型名与其他重复，那么可以添加一个`Type`后缀名到泛型名上。

```swift
class SomeClass<T> { /* ... */ }
class SomeClass<Model> { /* ... */ }
protocol Modelable {
    associatedtype Model
}
protocol Sequence {
    associatedtype IteratorType: Iterator
}
```

* **2.7** 命名必须要是不模糊的并且方便表述的

```swift
// PREFERRED
class RoundAnimatingButton: UIButton { /* ... */ }

// NOT PREFERRED
class CustomButton: UIButton { /* ... */ }
```

* **2.8** 不要使用缩写，可以选择较为简短的单词。

```swift
// PREFERRED
class RoundAnimatingButton: UIButton {
    let animationDuration: NSTimeInterval

    func startAnimating() {
        let firstSubview = subviews.first
    }

}

// NOT PREFERRED
class RoundAnimating: UIButton {
    let aniDur: NSTimeInterval

    func srtAnmating() {
        let v = subviews.first
    }
}
```

* **2.9** 对于不是很明显的类型需要将类型信息包含在属性名中。

```swift
// PREFERRED
class ConnectionTableViewCell: UITableViewCell {
    let personImageView: UIImageView

    let animationDuration: NSTimeInterval

    // it is ok not to include string in the ivar name here because it's obvious
    // that it's a string from the property name
    let firstName: String

    // though not preferred, it is OK to use `Controller` instead of `ViewController`
    let popupController: UIViewController
    let popupViewController: UIViewController

    // when working with a subclass of `UIViewController` such as a table view
    // controller, collection view controller, split view controller, etc.,
    // fully indicate the type in the name.
    let popupTableViewController: UITableViewController

    // when working with outlets, make sure to specify the outlet type in the
    // variable name.
    @IBOutlet weak var submitButton: UIButton!
    @IBOutlet weak var emailTextField: UITextField!
    @IBOutlet weak var nameLabel: UILabel!

}

// NOT PREFERRED
class ConnectionTableViewCell: UITableViewCell {
    // this isn't a `UIImage`, so shouldn't be called image
    // use personImageView instead
    let personImage: UIImageView

    // this isn't a `String`, so it should be `textLabel`
    let text: UILabel

    // `animation` is not clearly a time interval
    // use `animationDuration` or `animationTimeInterval` instead
    let animation: NSTimeInterval

    // this is not obviously a `String`
    // use `transitionText` or `transitionString` instead
    let transition: String

    // this is a view controller - not a view
    let popupView: UIViewController

    // as mentioned previously, we don't want to use abbreviations, so don't use
    // `VC` instead of `ViewController`
    let popupVC: UIViewController

    // even though this is still technically a `UIViewController`, this variable
    // should indicate that we are working with a *Table* View Controller
    let popupViewController: UITableViewController

    // for the sake of consistency, we should put the type name at the end of the
    // variable name and not at the start
    @IBOutlet weak var btnSubmit: UIButton!
    @IBOutlet weak var buttonSubmit: UIButton!

    // we should always have a type in the variable name when dealing with outlets
    // for example, here, we should have `firstNameLabel` instead
    @IBOutlet weak var firstName: UILabel!
}
```

* **2.10** 在编写函数参数的时候，要保证每个参数都易于理解其功能。
* **2.11** 根据 [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/), 对于`protocol`，如果其描述的是正在做的事情，譬如`Collection`，那么应该命名为名词。而如果是用于描述某种能力，譬如`Equatable`, `ProgressReporting`，那么应该添加 `able`, `ible`, 或者 `ing` 这样的后缀。如果你的协议并不符合上述两种情形，那么应该直接添加一个`Protocol`后缀，譬如：

```swift
// here, the name is a noun that describes what the protocol does
protocol TableViewSectionProvider {
    func rowHeight(atRow row: Int) -> CGFloat
    var numberOfRows: Int { get }
    /* ... */
}

// here, the protocol is a capability, and we name it appropriately
protocol Loggable {
    func logCurrentState()
    /* ... */
}

// suppose we have an `InputTextView` class, but we also want a protocol
// to generalize some of the functionality - it might be appropriate to
// use the `Protocol` suffix here
protocol InputTextViewProtocol {
    func sendTrackingEvent()
    func inputText() -> String
    /* ... */
}
```

## 3. Coding Style

### 3.1 General

* **3.1.1** 尽可能地使用`let`来代替`var`。
* **3.1.2** 尽可能地使用 `map`, `filter`, `reduce`的组合来进行集合的转换等操作，并且尽可能地避免使用带有副作用的闭包。

```swift
// PREFERRED
let stringOfInts = [1, 2, 3].flatMap { String($0) }
// ["1", "2", "3"]

// NOT PREFERRED
var stringOfInts: [String] = []
for integer in [1, 2, 3] {
    stringOfInts.append(String(integer))
}

// PREFERRED
let evenNumbers = [4, 8, 15, 16, 23, 42].filter { $0 % 2 == 0 }
// [4, 8, 16, 42]

// NOT PREFERRED
var evenNumbers: [Int] = []
for integer in [4, 8, 15, 16, 23, 42] {
    if integer % 2 == 0 {
        evenNumbers(integer)
    }
}
```

* **3.1.3** 尽可能地显式声明不方便进行类型推测的变量或者常量的类型名。
* **3.1.4** 如果你的函数需要返回多个参数，那么尽可能地使用`Tuple`来代替`inout`参数。如果你会多次使用某个元组，那么应该使用`typealias`设置别名。如果返回的参数超过三个，那么应该使用结构体或者类来替代。

```swift
func pirateName() -> (firstName: String, lastName: String) {
    return ("Guybrush", "Threepwood")
}

let name = pirateName()
let firstName = name.firstName
let lastName = name.lastName
```

* **3.1.5** 在创建 delegates/protocols 的时候需要小心所谓的保留环\(retain cycles\)，这些属性需要被声明为`weak`。
* **3.1.6** 在闭包中直接调用`self`可能会导致保留环，可以使用[capture list](https://developer.apple.com/library/ios/documentation/swift/conceptual/Swift_Programming_Language/Closures.html#//apple_ref/doc/uid/TP40014097-CH11-XID_163) 在这种情况下:

```swift
myFunctionWithClosure() { [weak self] (error) -> Void in
    // you can do this

    self?.doSomething()

    // or you can do this

    guard let strongSelf = self else {
        return
    }

    strongSelf.doSomething()
}
```

* **3.1.7** 不要使用 labeled breaks。
* **3.1.8** 不要在控制流逻辑判断的时候加上圆括号

```swift
// PREFERRED
if x == y {
    /* ... */
}

// NOT PREFERRED
if (x == y) {
    /* ... */
}
```

* **3.1.9** 避免在使用 enum 的时候写出全名

```swift
// PREFERRED
imageView.setImageWithURL(url, type: .person)

// NOT PREFERRED
imageView.setImageWithURL(url, type: AsyncImageView.Type.person)
```

* **3.1.10** 在写类方法的时候不能用简短写法，应该使用类名.方法名，这样能够保证代码的可读性

```swift
// PREFERRED
imageView.backgroundColor = UIColor.whiteColor()

// NOT PREFERRED
imageView.backgroundColor = .whiteColor()
```

* **3.1.11** 在非必要的时候不要写`self.`。
* **3.1.12** 在编写某个方法的时候注意考虑下这个方法是否有可能被复写，如果不可能被复写那么应该使用`final`修饰符。还要注意加上 final 之后也会导致无法在测试的时候进行复写，所以还是需要综合考虑。一般而言，加上`final`修饰符后会提高编译的效率，所以应该尽可能地使用该修饰符。
* **3.1.13** 在使用譬如`else`, `catch`等等类似的语句的时候，将关键字与花括号放在一行，同样遵循[1TBS style](https://en.m.wikipedia.org/wiki/Indent_style#Variant:_1TBS)规范，这边列出了常见的`if`/`else` 以及 `do`/`catch` 示范代码。

```swift
if someBoolean {
    // do something
} else {
    // do something else
}

do {
    let fileContents = try readFile("filename.txt")
} catch {
    print(error)
}
```

### 3.2 Access Modifiers

* **3.2.1** 在需要的时候应该将访问修饰符放在关键字的第一位。

```swift
// PREFERRED
private static let kMyPrivateNumber: Int

// NOT PREFERRED
static private let kMyPrivateNumber: Int
```

* **3.2.2** 访问修饰符不应该单独放一行：

```swift
// PREFERRED
public class Pirate {
    /* ... */
}

// NOT PREFERRED
public
class Pirate {
    /* ... */
}
```

* **3.2.3** 一般来说，不要显式地写默认的 `internal`访问修饰符。
* **3.2.4** 如果某个变量需要在测试的时候被使用到，那么应该标识为`internal`来保证`@testable import ModuleName`。这里需要注意的是，对于某些应该被声明为`private`的变量因为测试用途而声明为了`internal`，那么应该在注释里特别地注明。

```swift
/**
 This variable defines the pirate's name.
 - warning: Not `private` for `@testable`.
 */
let pirateName = "LeChuck"
```

### 3.3 Custom Operators:自定义操作符

尽可能地选用命名函数来代替自定义操作符。如果你打算引入一个自定义的操作符，那么一定要有非常充分的理由来说明为啥要讲一个新的操作符引入到全局作用域，而不是使用其他一些可替代的方式。你也可以选择去复写一些现有的操作符，譬如`==`来适应一些新的类型，不过要保证你添加的用法一定要与语义相符。譬如`==` 应该只能用于表示相等性测试并且返回一个布尔值。

### 3.4 Switch Statements and `enum`s

* **3.4.1** 在使用枚举类型作为 switch 的参数的时候，避免引入`default`关键字，而应该将没有使用的情形放到下面然后使用 break 关键字来避免被执行。
* **3.4.2** Swift 中默认会在每个 case 的结尾进行 break，因此没必要的时候不需要显式地声明`break`关键字。
* **3.4.3** The `case` statements should line up with the `switch` statement itself as per default Swift standards.
* **3.4.4** When defining a case that has an associated value, make sure that this value is appropriately labeled as opposed to just types \(e.g. `case Hunger(hungerLevel: Int)` instead of `case Hunger(Int)`\).

```swift
enum Problem {
    case attitude
    case hair
    case hunger(hungerLevel: Int)
}

func handleProblem(problem: Problem) {
    switch problem {
    case .attitude:
        print("At least I don't have a hair problem.")
    case .hair:
        print("Your barber didn't know when to stop.")
    case .hunger(let hungerLevel):
        print("The hunger level is \(hungerLevel).")
    }
}
```

* **3.4.5** 优先使用譬如`case 1, 2, 3:`这样的列表表达式而不是使用`fallthrough`关键字。
* **3.4.6** 如果你添加了一个默认的 case 并且该 case 不应该被使用，那么应该在 default 情形下抛出异常。

```swift
func handleDigit(digit: Int) throws {
    case 0, 1, 2, 3, 4, 5, 6, 7, 8, 9:
        print("Yes, \(digit) is a digit!")
    default:
        throw Error(message: "The given number was not a digit.")
}
```

### 3.5 Optionals

* **3.5.1** 只应该在 `@IBOutlet`中使用隐式地未包裹的 Options。否则其他情况下就应该使用 Non-Optional 或者正常的 Optional 的变量。虽然有时候你能保证某个变量肯定非`nil`，不过这样用的话还是比较安全并且能保证上下一致性。 The only time you should be using implicitly unwrapped optionals is withs. In every other case, it is better to use a non-optional or regular optional variable. Yes, there are cases in which you can probably "guarantee" that the variable will never be `nil` when used, but it is better to be safe and consistent.
* **3.5.2** 不要使用 `as!` 或者 `try!`.
* **3.5.3** 如果你只是打算判断存放在 Optional 中的值是否为空，那么你应该直接与`nil`进行判断而不是使用`if let`语句将值取出来。

```swift
// PREFERERED
if someOptional != nil {
    // do something
}

// NOT PREFERRED
if let _ = someOptional {
    // do something
}
```

* **3.5.4** 不要使用 `unowned`。你可以将`unowned`当做对于`weak`变量的隐式解包，虽然有时候`unowned`与`weak`相比有小小地性能提升，不过还是不建议进行使用。

```swift
// PREFERRED
weak var parentViewController: UIViewController?

// NOT PREFERRED
weak var parentViewController: UIViewController!
unowned var parentViewController: UIViewController
```

* **3.5.5** 当对 Optionals 进行解包的时候，使用与 Optionals 变量一致的变量名

```text
guard let myVariable = myVariable else {
    return
}
```

### 3.6 Protocols

在实现协议的时候，大体上有两种代码组织方式：

1. 使用 `// MARK:` 来注释你的专门用于实现协议中规定的方法
2. 在你的类或者结构体实现之外使用一个扩展来存放实现代码，不过要保证在一个源文件中

不过需要注意的是，如果你是使用了 Extension 方式，那么定义在 Extension 中的方法是无法被子类复写的，这样可能会无法进行测试。

### 3.7 Properties

* **3.7.1** 如果是定义一个只读的需要经过计算的属性，那么不需要声明 `get {}`

```swift
var computedProperty: String {
    if someBool {
        return "I'm a mighty pirate!"
    }
    return "I'm selling these fine leather jackets."
}
```

* **3.7.2** 在使用 `get {}`, `set {}`, `willSet`, 以及 `didSet`, 注意块的缩进
* **3.7.3** 尽管你可以在`willSet`/`didSet`以及 `set`方法中使用自定义的名称，不过建议还是使用默认的`newValue`/`oldValue` 变量名

```swift
var computedProperty: String {
    get {
        if someBool {
            return "I'm a mighty pirate!"
        }
        return "I'm selling these fine leather jackets."
    }
    set {
        computedProperty = newValue
    }
    willSet {
        print("will set to \(newValue)")
    }
    didSet {
        print("did set from \(oldValue) to \(newValue)")
    }
}
```

* **3.7.4** 将任何类常量设置为`static`

```swift
class MyTableViewCell: UITableViewCell {
    static let kReuseIdentifier = String(MyTableViewCell)
    static let kCellHeight: CGFloat = 80.0
}
```

* **3.7.5** 可以使用如下方式便捷地声明一个单例变量：

```swift
class PirateManager {
    static let sharedInstance = PirateManager()

    /* ... */
}
```

### 3.8 Closures:闭包

* **3.8.1** 如果闭包中的某个参数的类型是显而易见的，那么可以避免声明类型。不过有时候为了保证可读性与一致性，还是会显示声明参数类型。

```swift
// omitting the type
doSomethingWithClosure() { response in
    print(response)
}

// explicit type
doSomethingWithClosure() { response: NSURLResponse in
    print(response)
}

// using shorthand in a map statement
[1, 2, 3].flatMap { String($0) }
```

* **3.8.2** 在参数列表中，如果是使用了捕获变量或者声明了非 Void 的返回值，那么应该将参数列表写在一个圆括号里，其他情况下则可以省略圆括号。

```swift
// parentheses due to capture list
doSomethingWithClosure() { [weak self] (response: NSURLResponse) in
    self?.handleResponse(response)
}

// parentheses due to return type
doSomethingWithClosure() { (response: NSURLResponse) -> String in
    return String(response)
}
```

* **3.8.3** 如果你是将闭包声明为一个类型，那么除非该类型为 Optional 或者该闭包是另一个闭包的参数，否则不需要使用圆括号进行包裹。不过需要用圆括号来标注参数列表，并且使用`Void`来指明没有任何结果返回。

```swift
let completionBlock: (success: Bool) -> Void = {
    print("Success? \(success)")
}

let completionBlock: () -> Void = {
    print("Completed!")
}

let completionBlock: (() -> Void)? = nil
```

* **3.8.4** 尽可能地将参数名与左括号放在一行，不过要避免打破每行最长 160 个字符的限制。

  Keep parameter names on same line as the opening brace for closures when possible without too much horizontal overflow \(i.e. ensure lines are less than 160 characters\).

* **3.8.5** 尽可能地使用 trailing closure 表达式，除非需要显示地声明闭包参数的外部参数名。

```swift
// trailing closure
doSomething(1.0) { parameter1 in
    print("Parameter 1 is \(parameter1)")
}

// no trailing closure
doSomething(1.0, success: { parameter1 in
    print("Success with \(parameter1)")
}, failure: { parameter1 in
    print("Failure with \(parameter1)")
})
```

### 3.9 Arrays

* **3.9.1** 一般来说，避免使用下标直接访问某个数组，而应该使用类似于`.first`、`.last`这样的访问器进行访问。另外，应该优先使用`for item in items`语法来替代`for i in 0..<items.count`。如果你打算用下标遍历数组，那么一定保证不能越界。
* **3.9.2** 永远不要使用`+=` 或者 `+`运算符来增加或者连接数组，应该使用`.append()` 或者 `.appendContentsOf()` 方法。如果你想定义一个从其他数组生成的不可变数组，那么应该使用`let`关键字，即: `let myNewArray = arr1 + arr2`, 或者 `let myNewArray = [arr1, arr2].flatten()`。

### 3.10 Error Handling

假设某个函数 `myFunction` 需要去返回一个`String`类型，不过有可能会在某个点抛出异常，一般来说会将该函数的返回值设置为`String?`： Example:

```swift
func readFile(withFilename filename: String) -> String? {
    guard let file = openFile(filename) else {
        return nil
    }

    let fileContents = file.read()
    file.close()
    return fileContents
}

func printSomeFile() {
    let filename = "somefile.txt"
    guard let fileContents = readFile(filename) else {
        print("Unable to open file \(filename).")
        return
    }
    print(fileContents)
}
```

不过作为异常处理的角度，我们应该使用 Swift 的`try-catch`表达式，这样能显式地知道错误点：

```swift
struct Error: ErrorType {
    public let file: StaticString
    public let function: StaticString
    public let line: UInt
    public let message: String

    public init(message: String, file: StaticString = #file, function: StaticString = #function, line: UInt = #line) {
        self.file = file
        self.function = function
        self.line = line
        self.message = message
    }
}
```

Example usage:

```swift
func readFile(withFilename filename: String) throws -> String {
    guard let file = openFile(filename) else {
        throw Error(message: "Unable to open file named \(filename).")
    }

    let fileContents = file.read()
    file.close()
    return fileContents
}

func printSomeFile() {
    do {
        let fileContents = try readFile(filename)
        print(fileContents)
    } catch {
        print(error)
    }
}
```

总而言之，如果某个函数可能会出错，并且出错的原因不能显式地观测到，那么应该优先抛出异常而不是使用一个 Optional 作为返回值。

### 3.11 Using `guard` Statements

* **3.11.1** 一般来说，我们会优先使用所谓的"early return"策略来避免`if`表达式中的多层嵌套的代码。在这种情况下使用`guard`语句能够有效地提升代码的可读性。

```swift
// PREFERRED
func eatDoughnut(atIndex index: Int) {
    guard index >= 0 && index < doughnuts else {
        // return early because the index is out of bounds
        return
    }

    let doughnut = doughnuts[index]
    eat(doughnut)
}

// NOT PREFERRED
func eatDoughnuts(atIndex index: Int) {
    if index >= 0 && index < donuts.count {
        let doughnut = doughnuts[index]
        eat(doughnut)
    }
}
```

* **3.11.2** 在对 Optional 类型进行解包的时候，优先使用 `guard` 语句来避免`if`语句中较多的缩进。

```swift
// PREFERRED
guard let monkeyIsland = monkeyIsland else {
    return
}
bookVacation(onIsland: monkeyIsland)
bragAboutVacation(onIsland: monkeyIsland)

// NOT PREFERRED
if let monkeyIsland = monkeyIsland {
    bookVacation(onIsland: monkeyIsland)
    bragAboutVacation(onIsland: monkeyIsland)
}

// EVEN LESS PREFERRED
if monkeyIsland == nil {
    return
}
bookVacation(onIsland: monkeyIsland!)
bragAboutVacation(onIsland: monkeyIsland!)
```

* **3.11.3** 在决定是要用`if`表达式还是`guard`表达式进行 Optional 类型解包的时候，最重要的点就是要保证代码的可读性。很多时候要注意因时而变，因地制宜：

```swift
// an `if` statement is readable here
if operationFailed {
    return
}

// a `guard` statement is readable here
guard isSuccessful else {
    return
}

// double negative logic like this can get hard to read - i.e. don't do this
guard !operationFailed else {
    return
}
```

* **3.11.4** 当需要进行多可能性处理的时候，应该优先使用`if`表达式而不是`guard`表达式。

```swift
// PREFERRED
if isFriendly {
    print("Hello, nice to meet you!")
} else {
    print("You have the manners of a beggar.")
}

// NOT PREFERRED
guard isFriendly else {
    print("You have the manners of a beggar.")
    return
}

print("Hello, nice to meet you!")
```

* **3.11.5** 一般来说，`guard`应该被用于需要直接退出当前上下文的情形。而对于下面这种两个条件互不干扰的情况，应该使用两个`if`而不是两个`guard`。

```swift
if let monkeyIsland = monkeyIsland {
    bookVacation(onIsland: monkeyIsland)
}

if let woodchuck = woodchuck where canChuckWood(woodchuck) {
    woodchuck.chuckWood()
}
```

* **3.11.6** 有时候我们会碰到要用`guard`语句进行多个 optionals 解包的情况，一般而言，对于复杂的错误处理的 Optional 类型需要将其拆分到多个单个表达式中。

```swift
// combined because we just return
guard let thingOne = thingOne,
    let thingTwo = thingTwo,
    let thingThree = thingThree else {
    return
}

// separate statements because we handle a specific error in each case
guard let thingOne = thingOne else {
    throw Error(message: "Unwrapping thingOne failed.")
}

guard let thingTwo = thingTwo else {
    throw Error(message: "Unwrapping thingTwo failed.")
}

guard let thingThree = thingThree else {
    throw Error(message: "Unwrapping thingThree failed.")
}
```

* **3.11.7** 不要将`guard`表达式强行缩写到一行内。

```swift
// PREFERRED
guard let thingOne = thingOne else {
    return
}

// NOT PREFERRED
guard let thingOne = thingOne else { return }
```

## 4. Documentation/Comments

### 4.1 Documentation

如果某个函数不是简单地`O(1)`操作，那么最好就是为该函数添加一些注释文档，这样能有效地提高代码的可读性与可维护性。之前有个非常不错的文档工具[VVDocumenter](https://github.com/onevcat/VVDocumenter-Xcode)。推荐阅读 Apple 的官方指南中的描述：[described in Apple's Documentation](https://developer.apple.com/library/tvos/documentation/Xcode/Reference/xcode_markup_formatting_ref/Attention.html#//apple_ref/doc/uid/TP40016497-CH29-SW1).

Guidelines:

* **4.1.1** 每行不应超过 160 个字符
* **4.1.2** 即使某些注释只有一行，也应该使用块注释符: \(`/** */`\).
* **4.1.3** 不用给每行的开头都加上: `*`.
* **4.1.4** 使用新的 `- parameter` 标识符来代替老的`:param:` syntax \(注意这边是小写的 `parameter` 而不是`Parameter`\).
* **4.1.5** 如果你准备对参数/返回值/异常值来写注释，那么注意要一个不落的全局加上，尽管有时候会让文档显得重复冗余。有时候，如果只需要对单个参数进行注释，那么还不如直接放在描述里进行声明，而不需要专门的为参数写一个注释。
* **4.1.6** 对于复杂的使用类，应该添加一些具体的使用用例来描述类的用法。注意 Swift 的注释文档中是支持 MarkDown 语法的，这是一个很好的特性。

```swift
/**
 ## Feature Support

 This class does some awesome things. It supports:

 - Feature 1
 - Feature 2
 - Feature 3

 ## Examples

 Here is an example use case indented by four spaces because that indicates a
 code block:

     let myAwesomeThing = MyAwesomeClass()
     myAwesomeThing.makeMoney()

 ## Warnings:告警

 There are some things you should be careful of:

 1. Thing one
 2. Thing two
 3. Thing three
 */
class MyAwesomeClass {
    /* ... */
}
```

* **4.1.7** 使用 - \` 在注释中著名引用的代码

```swift
/**
 This does something with a `UIViewController`, perchance.
 - warning: Make sure that `someValue` is `true` before running this function.
 */
func myFunction() {
    /* ... */
}
```

* **4.1.8** 保证文档的注释尽可能的简洁

### 4.2 Other Commenting Guidelines:其他的注释规则

* **4.2.1** `//`后面总是要跟上一个空格
* **4.2.2** 注释永远要放在单独的行中
* **4.2.3** 在使用`// MARK: - whatever`的时候，注意 MARK 与代码之间保留一个空行

```swift
class Pirate {

    // MARK: - instance properties

    private let pirateName: String

    // MARK: - initialization

    init() {
        /* ... */
    }

}
```

