
Avito ProTools team Swift style guide

v 0.6.1

#1. Basics

##1.1 Naming

### 1.1.1 General

Use descriptive names with camel case for classes, methods, variables, etc. Type names (classes, structures, enumerations and protocols) should be capitalized, while method names and variables should start with a lower case letter. Avoid using "_" in names.

For functions and init methods, prefer named parameters for all arguments unless the context is very clear. Include external parameter names if it makes function calls more readable.

```swift
func date(fromString dateString: String) -> Date
func convertPointAt(column column: Int, row: Int) -> CGPoint
func timedAction(afterDelay delay: TimeInterval, perform action: Action) -> SKAction
```

For methods, follow the standard Apple convention of referring to the first parameter in the method name:

```swift
class Counter {

	// MARK: - Methods -
    func combine(with otherCounter: Counter, options: [String: Any]?) { 
        ... 
    }
    
    func increment(by amount: Int) { 
        ... 
    }
}
```
### 1.1.2 Protocols

Following Apple's API Design Guidelines, protocols names that describe what something is should be a noun. Examples: `Collection`, `WidgetFactory`. Protocols names that describe an ability should end in -ing, -able, or -ible. Examples: `Equatable`, `Resizing`.

### 1.1.3 Enumerations

Following Apple's API Design Guidelines for Swift 3, use lowerCamelCase for enumeration values.

```swift
enum Shape {

    case rectangle
    case square
    case rightTriangle
    case equilateralTriangle
}
```

## 1.2 Spacing

Indent using 4 spaces

## 1.3 Braces

Method braces and other braces (if/else/switch/while etc.) always open on the same line as the statement but close on a new line.

```swift
if user.isReady {
    ...
} else {
    ...
}
```

## 1.4 Blank Lines

There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but having too many sections in a method often means you should refactor into several methods.

Blank line should be placed after braces if there are more than one code line:

```swift
if user.isReady {
    user.Run()
}
```

```swift
if user.isReady {

	user.prepare()
    user.Run()
}
```

```swift
final class MyClass {
	
	private(set) var property1: String?
	private(set) var property2: String?
}
```

Same for methods, properties and initializers: 

```swift
func doWork() {
	user.doWork()
}
```

```swift
func doWork() {
	
	user.prepare()
	user.doWork()
}
```

Exceptions: 

Single guard:

```swift
func doWork() {
	guard let user = user else { return }

	user.prepare()
	user.doWork()
}
```

Calling super init:

```swift
init(user: User) {
	super.init()

	self.user = user 
}
```

## 1.5 Colons

Colons always have no space on the left and one space on the right. Exceptions are the ternary operator ? :, empty dictionary [:] and #selector syntax for unnamed parameters (_:).

```swift
final class MyClass: NSObject {

    var property1: [String: Any] = [:]
    var property2: [String: Any] = ["A": 1.2, "B": 3.2]
}
```

## 1.6 Use of Self

For conciseness, avoid using self since Swift does not require it to access an object's properties or invoke its methods

## 1.7 Computed Properties

For conciseness, if a computed property is read-only, omit the get clause. The get clause is required only when a set clause is provided.

```swift
var diameter: Double {
    return radius * 2
}
```

```swift
var diameter: Double {
    
    get {
    	radius * 2
    } 
    set {
    	radius = newValue / 2
    }
}
```

But not:

```swift
var diameter: Double {
    get { radius * 2 } 
    set { radius = newValue / 2 }
}
```

## 1.8 Final

Mark classes final when inheritance is not intended. Example:

```swift
final class MyClass {
    ....
}
```

## 1.9 Function Declarations

Keep short function declarations on one line including the opening brace:

```swift
func update(with item: Item) -> Bool {
    ...
}
```

For functions with long signatures, add line breaks like:

```swift
func update(with item: Item,
            info userInfo: [String: Any]? = nil,
            options: [String: Any]? = nil,
            user: User) -> Bool 
{
    ...
}
```

## 1.10 Types

Always use Swift's native types when available.

## 1.11 Constants

Constants are defined using the let keyword, and variables with the var keyword. Always use let instead of var if the value of the variable will not change.

## 1.12 Type Inference

Prefer compact code and let the compiler infer the type for local variables, but not for class/struct properties:

```swift
final class MyClass {
	
	// MARK: - Properties -
	var property1: Bool = false
	var property2: [String: Any] = SomeClass.calculateProperty2()
	var property3: String = ""
	var property4: [String] = []


	// MARK: - Mathods -
	func method1() {

		var p1 = false
		var p2: [String: Any] = SomeClass.calculateProperty2()
		var p3 = ""
		var p4 = [String]()
	}
}
```

But not:

```swift
final class MyClass {
	
	// MARK: - Properties -
	var property1 = false
	var property2 = SomeClass.calculateProperty2()
	var property3 = ""
}
```

# 2 Best Practice

## 2.1 Classes and Structures

Use structs only for short data structures with no reference properties. Like:

```swift
struct Point {
    
    var x: Int = 0
    var y: Int = 0
}
```

```swift
final class NamedPoint {
    
    var x: Int = 0
    var y: Int = 0
    var name: String?
}
```

### 2.1.1 Protocol Conformance

In particular, when adding protocol conformance to a model, prefer adding it in class declaration rather than to a separate extension:

```swift
final class SomeViewController: UIViewController, UITableViewDataSource {
    
    // MARK: - UITableViewDataSource -
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        ...
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        ...
    }
}
```

Avoid using extensions in same file, use // MARK: - <NAME> -  instead.

## 2.2 Functional Programming

### 2.2.1 Basics

Use functional programming where it fit well.

### 2.2.2 Loops

Use forEach when you need to iterate through all elements of collection:

```swift
collection.forEach {
	$0.doSomething()
}
```

Use map or flatMap to transform one collection to another:

```swift
let b = a.map { 
	...
}
```
### 2.2.3 Optionals

Consider using map or flatMap to unwrap optionals:

```swift
let b = a.map { 
	B($0)
}
```
```swift
a.map { 
	$0.doSomething()
}
```

## 2.3 Aggregating protocols

Aggregate protocols through typealias:

```swift
typealias CollectionViewProtocol = UICollectionViewDataSource & UICollectionViewDelegate
```

But not: 

```swift
protocol CollectionViewProtocol: UICollectionViewDataSource, UICollectionViewDelegate {
}
```
