# Matias Pequeno's Swift Style Guide

This style guide is overall goals are conciseness, readability, and simplicity.

Objective-C style guide: [here](https://github.com/matias-pequeno/Guidelines/blob/master/ObjCStyleGuide.md).

API Design Guidelines: [here](
https://swift.org/documentation/api-design-guidelines).

**Table of Contents**

  - [Naming](#naming)
    - [Class Prefixes](#class-prefixes)
  - [Spacing and Formatting](#spacing-and-formatting)
    - [Ternary operator](#ternary-operator)
    - [nil-coalescing operator](#nil-coalescing-operator)
  - [Comments](#comments)
  - [Classes and Structures](#classes-and-structures)
    - [Which one to use?](#which-one-to-use)
    - [Example definition](#example-definition)
    - [Use of self](#use-of-self)
    - [Protocol Conformance](#protocol-conformance)
    - [Vertical whitespaces](#vertical-whitespaces)
  - [Function Declarations](#function-declarations)
  - [Closure Expressions](#closure-expressions)
    - [Positional Arguments](#positional-arguments)
  - [Unnecessary parentheses](#unnecessary-parentheses)
  - [Types](#types)
    - [Constants](#constants)
    - [Optionals](#optionals)
    - [Struct Initializers](#struct-initializers)
    - [Type Inference](#type-inference)
    - [Syntactic Sugar](#syntactic-sugar)
  - [Semicolons](#semicolons)
  - [Language](#language)
  - [Exposing functions to Objective-C](#exposing-functions-to-objective-c)


## Naming

Use descriptive names with camel case for classes, methods, variables, etc. Class names should be capitalized, while method names and variables should start with a lower case letter. Constants should be private to the class scope and they should start with a lower case "k".

**Right:**

```swift
private let kMaximumWidgetCount = 100

class WidgetContainer {
    var widgetButton: UIButton
    let widgetHeightPercentage = 0.85
}
```

**Wrong:**

```swift
let MAX_WIDGET_COUNT = 100

class app_widgetContainer {
    var wBut: UIButton
    let wHeightPct = 0.85
}
```

Apple has created a [list](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/APIAbbreviations.html) of acceptable abbreviations and acronyms. Using the abbreviations on that list is acceptable, other abbreviations are not.

**Right:**

```swift
let navigationController = UINavigationController(rootViewController: viewController)
```

**Wrong:**

```swift
let navCon = UINavigationController(rootViewController: vc)
```
For booleans it is good practice to avoid negations and prefix the name with "is" or "has" if it provides more clarity. Adjectives such as "disabled" or "allowed" are fine:

**Right:**

```swift
let allowed = true
let isCurrentUser = true
let hasPermission = true
```

**Wrong:**

```swift
let notAllowed = true
let notCurrentUser = true
let permission = true
```

### Class Prefixes

Swift types are all automatically namespaced by the module that contains them. As a result, prefixes are not required in order to minimize naming collisions. If two names from different modules collide you can disambiguate by prefixing the type name with the module name:

```swift
import MyModule

var myClass = MyModule.MyClass()
```

You **should not** add prefixes to your Swift types.

## Spacing and Formatting

* Indent using 4 spaces rather than tabs to conserve space and help prevent line wrapping. Be sure to set this preference in Xcode.
* The line length for Swift files is 110 columns. With *no allowance*. You should enable the column guide in Xcode in the preferences (Text Editing > Page guide at columns: 110).
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.

**Right:**
```swift
if user.isHappy {
    // Do something
} else {
    // Do something else
}
```

**Wrong:**
```swift
if user.isHappy
{
    // Do something
}
else {
    // Do something else
}
```

* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but having too many sections in a method often means you should refactor into several methods.

### Ternary operator
 - When the ternary operator can fit on one line, it is preferred over the long expression
 - Add spaces before and after the `?` and `:`
 - Don't nest ternary operators

```swift
let someVariable = someCondition ? someValue : someOtherValue
let someVariable = someInteger == 4 ? someValue : someOtherValue
```

### nil-coalescing operator
 - Use the nil-coalescing operator where appropriate

```swift
label.text = self.someText ?? "Default text"
```

## Comments

When they are needed, use comments to explain **why** a particular piece of code does something. Comments must be kept up-to-date or deleted.

All public methods should contain [Swift's markup styling](https://developer.apple.com/library/ios/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html) formats but the code should be as self-documenting as possible. Example of Swift markup styling:

```swift
/// Cancels all tasks in the queue whose URLs match the specified HTTP method and URL string.
///
/// - parameter url:    The URL string used to create the request URL.
/// - parameter method: The HTTP method to match for the cancelled requests, such as GET, POST, PUT, or DELETE.
///                     If you need to continue on the next line align it.
///
/// - returns: The description of what it returns.
```

You should use `- parameter <name>: <description>` for params, `- returns: <what it returns>` for return types and `- throws <description of the ErrorType>` for errors.

## Classes and Structures

### Which one to use?

Remember, structs have [value semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144). Use structs for things that do not have an identity. An array that contains [a, b, c] is really the same as another array that contains [a, b, c] and they are completely interchangeable. It doesn't matter whether you use the first array or the second, because they represent the exact same thing. That's why arrays are structs.

Classes have [reference semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145). Use classes for things that do have an identity or a specific life cycle. You would model a person as a class because two person objects are two different things. Just because two people have the same name and birthdate, doesn't mean they are the same person. But the person's birthdate would be a struct because a date of 3 March 1950 is the same as any other date object for 3 March 1950. The date itself doesn't have an identity.

Sometimes, things should be structs but need to conform to `AnyObject` or are historically modeled as classes already (`NSDate`, `NSSet`). Try to follow these guidelines as closely as possible.

### Example definition

Here's an example of a well-styled class definition:

```swift
class Circle: Shape {
    /// Outlets grouped at the top
    @IBOutlet private var myView: UIView!

    var x: Int, y: Int
    var radius: Double
    var diameter: Double {
        get {
            return radius * 2
        }
        set {
            radius = newValue / 2
        }
    }

    init(x: Int, y: Int, radius: Double) {
        self.x = x
        self.y = y
        self.radius = radius
    }

    convenience init(x: Int, y: Int, diameter: Double) {
        self.init(x: x, y: y, radius: diameter / 2)
    }

    func describe() -> String {
        return "I am a circle at \(centerString()) with an area of \(computeArea())"
    }

    override func computeArea() -> Double {
        return M_PI * radius * radius
    }

    private func centerString() -> String {
        return "(\(x),\(y))"
    }
}
```

The example above demonstrates the following style guidelines:

 + Specify types for properties, variables, constants, argument declarations and other statements with a space after the colon but not before, e.g. `x: Int`, and `Circle: Shape`.
 + Define multiple variables and structures on a single line if they share a common purpose / context.
 + Indent getter and setter definitions and property observers.
 + Don't add modifiers such as `internal` when they're already the default. Similarly, don't repeat the access modifier when overriding a method.


### Use of self

For conciseness, *always* use `self`. This way makes clear that the variable is an instance property. Also helps avoiding retain cycles when using closures since those errors are hard to catch when not explicitly using self.

```swift
class BoardLocation {
    let row: Int, column: Int

    init(row: Int,column: Int) {
        self.row = row
        self.column = column

        let closure = { () -> () in
            print(self.row)
        }
    }
}
```

### Protocol Conformance

When adding protocol conformance to a class, prefer adding a separate class extension for the protocol methods. This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

**Right:**
```swift
class MyViewController: UIViewController {
    // class stuff here
}

extension MyViewController: UITableViewDataSource {
    // table view data source methods
}

extension MyViewController: UIScrollViewDelegate {
    // scroll view delegate methods
}
```

**Wrong:**
```swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
    // all methods
}
```

## Function Declarations

Keep short function declarations on one line including the opening brace:

```swift
func reticulateSplines(spline: [Double]) -> Bool {
    // reticulate code goes here
}
```

For functions with long signatures, add line breaks at appropriate points and follow Xcode's indentation suggestions.

```swift
func reticulateSplines(spline: [Double], adjustmentFactor: Double,
                       translateConstant: Int, comment: String) -> Bool
{
    // reticulate code goes here
}
```

Note that this is the only place where adding the bracket on a new line is allowed (required).


## Closure Expressions

Use trailing closure syntax wherever possible. In all cases, give the closure parameters descriptive names:

```swift
return SKAction.customActionWithDuration(effect.duration) { node, elapsedTime in
    // more code goes here
}
```

If wrapping is required, wrap closure parameters together with the starting curly brace:

```swift
return self.menuView.addMenuOption(title: I18N.Menu.AboutLine, icon: Asset.MenuAboutLine)
{ [weak self] _ in
    // more code goes here
}
```

Avoiding the trailing closure in this case is permissible. Indent ending curly brace to the closure parameter:

```swift
return self.menuView.addMenuOption(title: I18N.Menu.AboutLine, icon: Asset.MenuAboutLine,
    completion: { [weak self] _ in
        // more code goes here
    })
```

For single-expression closures where the context is clear, use implicit returns:

```swift
attendeeList.sort { > }
```

### Positional Arguments

Positional arguments can either make things very expressive or very cryptic. As a general guideline, only use positional arguments on one-liners.

**Right:**

```swift
sum = numbers.reduce(0) { $0 + $1 }
sorted = contacts.sort { $0.name < $1.name }
firstNames = passengers.map { $0.firstName }
```

```swift
MyAPI.someCall(ignoreError: true) { documents, user in
    someManager.documents = documents
    someFunction(user, documents.expired)
}
```

**Wrong:**
```swift
MyAPI.someCall(ignoreError: true) {
    someManager.documents = $0
    someFunction($1, $0.expired)
}
```

## Unnecessary parentheses

Don't add extra parentheses to statements where they are parentheses.

**Right:**

```swift
if true {
  ...
}

switch foo {
  ...
}
```

**Wrong:**

```swift
if (true && bar) {

}

switch (foo) {

}
```

The one exception to this is when parentheses are required for precedence.

**Right:**

```swift
if (foo && bar) || baz {
  ...
}
```

## Types

Always use Swift's native types when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.

**Right:**
```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```

**Wrong:**
```swift
let width: NSNumber = 120.0                                 // NSNumber
let widthString: NSString = width.stringValue               // NSString
```


### Optionals

Declare variables and function return types as optional with `?` where a nil value is acceptable.

Use implicitly unwrapped types declared with `!` only for instance variables that you know will be initialized later before use, such as subviews that will be set up in `viewDidLoad`.

When accessing an optional value, use optional chaining if the value is only accessed once or if there are many optionals in the chain:

```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

Use optional binding when it's more convenient to unwrap once and perform multiple operations:

```swift
if let textContainer = self.textContainer {
    // do many things with textContainer
}
```

When naming optional variables and properties, avoid naming them like `optionalString` or `maybeView` since their optional-ness is already in the type declaration.

For optional binding, shadow the original name when appropriate rather than using names like `unwrappedView` or `actualLabel`.

**Right:**
```swift
var subview: UIView?

// later on...
if let subview = subview {
    // do something with unwrapped subview
}
```

**Wrong:**
```swift
var optionalSubview: UIView?

if let unwrappedSubview = optionalSubview {
    // do something with unwrappedSubview
}
```

### Struct Initializers

Use the native Swift struct initializers rather than the legacy CGGeometry constructors.

**Right:**
```swift
let bounds = CGRect(x: 40, y: 20, width: 120, height: 80)
var centerPoint = CGPoint(x: 96, y: 42)
```

**Wrong:**
```swift
let bounds = CGRectMake(40, 20, 120, 80)
var centerPoint = CGPointMake(96, 42)
```

Prefer the struct-scope constants `CGRect.infiniteRect`, `CGRect.nullRect`, etc. over global constants `CGRectInfinite`, `CGRectNull`, etc. For existing variables, you can use the shorter `.zeroRect`.

### Type Inference

The Swift compiler is able to infer the type of variables and constants. You can provide an explicit type via a type alias (which is indicated by the type after the colon), but in the majority of cases this is not necessary.

Prefer compact code and let the compiler infer the type for a constant or variable.

**Right:**
```swift
let message = "Click the button"
var currentBounds = computeViewBounds()
```

**Wrong:**
```swift
let message: String = "Click the button"
var currentBounds: CGRect = computeViewBounds()
```

When you need to initialize a variable with a different type than what's implied, use an explicit type (not a type initializer):

**Right:**
```swift
let width: CGFloat = 25
```

**Wrong:**
```swift
let width = CGFloat(25)
```

**Exception:** An exception to this rule are the API models (in the `Models` and `MyModels` frameworks), which should always have explicit types:

>  It's usually better to express what we want to achieve (filter a from array b) rather than how we want to achieve it (`for (int i; i++; i < count) ...`). Type mapping is what we want to achieve in our models since type should not be defined by the "business logic" of our app, but by a strong mapping between the API and the models.

**NOTE**: Following this guideline means picking descriptive names is even more important than before.


### Syntactic Sugar

Prefer the shortcut versions of type declarations over the full generics syntax.

**Right:**
```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**Wrong:**
```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

## Semicolons

Swift does not require a semicolon after each statement in your code. They are only required if you wish to combine multiple statements on a single line.

Do not write multiple statements on a single line separated with semicolons.

The only exception to this rule is the `for-conditional-increment` construct, which requires semicolons. However, alternative `for-in` constructs should be used where possible.

**Right:**
```swift
var swift = "not a scripting language"
```

**Wrong:**
```swift
var swift = "not a scripting language";
```

## Language

Use US English spelling to match Apple's API.

**Right:**
```swift
var color = "red"
```

**Wrong:**
```swift
var colour = "red"
```

## Exposing functions to Objective-C

If you need to expose a function to Objective-C, for a gesture recognizer for example, make sure the `@objc` specifier is on a line by itself.

**Right:**

```swift
@objc
private func handlePan(...) {
  ...
}
```

**Wrong:**

```swift
@objc private func handlePan(...) {
  ...
}
```

## Use of `guard`

Only `guard` if unwrapping an optional or matching a pattern for later use.

**Right:**

```swift
guard let location = somePlace.location else {
  return
}
```

**Right:**

```swift
guard case .Success(let item) = result else {
  return
}
```

**Right:**

```swift
if locationOne != locationTwo {
  return
}
```

**Wrong:**

```swift
guard locationOne == locationTwo else {
  return
}
```
