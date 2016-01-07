# Swift 2.0 key points
Notes on Swift language features, syntax, library functions, rules, and general computer science theory gathered from playing with Swift, reading books, and following tutorials.
___

## The Basics
* Swift is a multi-functional programming language, with focus on __protocol-__, __generics-__, and __object-oriented__ design, but also with extensive support for __functional programming__

### Variables and constants
* A var/ const gives name to a value
* Variables are defined with the keyword `var`
* Constants are defined with the keyword `let`
* A var/const are declared like so: `var pi: Double = 3.14`
* Swift has type inference, so the type definition can be omitted: `var pi = 3.14`
* The `_` (underscore) is a generic name, and can be used with values you don't want to reference later

### Tuples
* Tuples group values under a single name
* A tuple is declared like so: `let coord: (Double, Double, Double) = (1.1, 2.3, 3.5)`
  * Using type inference: `let coord = (1.1, 2.3, 3.5)`
* A value inside a tuple can be accessed with its index: `let x = coord.0`
* Multiple entries can be accessed at the same time: `let (x, y, _) = coord`
* The individual values inside a tuple can be named: `let coord:(x: Int, y: Int, z:Int) = (1.1, 2.3, 3.5)`
* A named value inside a tuple can be accessed with its name: `let x = coord.x`

### Basic Value Types
* The Swift library comes with a number of basic value types, ranging from numbers to booleans and strings
* Values conforming to the `Equatable` protocol can be compared with `==` and `!=`
* Values conforming to the `Comparable` protocol can be compared with `<`, `>`, `<=`, and `>=`

#### Numbers
* There are three number value types in Swift: `int`, `float`, and `double`
  * `int` is used for whole numbers
  * `float` and `double` is used for floating point numbers, where __float__ is _lower precision_ than __double__
* All numbers conform to these simple operations: `+`, `-`, `*`, `/`, `%`, `++`, `--`
  * __Prefix operators__ (++ value) return the value _after incrementing it_
  * __Postfix operators__ (value ++) return the value _before incrementing it_
* In Swift underscores can be used to make numbers more readable: `1_000_000`
* To convert between number types: `Int(3.14)`

#### Booleans
* The following combine operators apply to bools: `&&` (__and__) and `||` (__or__)

#### Strings
* Swift supports __Unicode__, where a single mapping is called a __code point__
* `Character` stores a single char
* `String` stores a string of _any length_
* Strings can be concatinated: `"String 1 "` + `"String 2"`
* Strings can also be interpolated: `"String \(variable)"`
* Strings conform to the __equatable__ protocol, and can therefore be compared: `"string" == "string"`
  * Canonicalization ensures that only on-screen displayed characters are compared, _not unicode characters_
* Strings also conform to the __comparable__ protocol, and can therefore be compared like so: `"cat" < "dog"`
  * Less-than/ greater-than compares string 1 with string 2 _alphabetically_ (in this case, whether _cat comes before dog_)
* Strings can easily be converted between lower- and uppercase: `s.uppercaseString` and `s.lowercaseString`

### Conditionals
* Swift requires _all blocks_ to be surrounded by brackets (`{}`), _even 1-line `if`-statements_
* Swift supports the ternary operator: `(condition) ? (true value) : (false value)`
* Switch-statements work with _any data type_: `switch (string) {}`
* You can match _more than one case_ in a single `case` statement: `case 0, 1, 2:`
* __Pattern matching__ allows you to match values based on conditions in switch-statements: `case let x where x % 2 == 0:`
* __Partial matching__ allows you to ignore or retreive certain values in switch-statements: `case (0, 0, _):`, `case (0, 0, let z):`
* __Partial pattern matching__ allows you to combine the two: `case (let x, let y, _) where y == x:`

### Iteration
* Closed ranges can be created like so: `0...5` (0, 1, 2, 3, 4, 5)
* Half-open ranges can be created like so: `0..<5` (0, 1, 2, 3, 4)
* For-loops are used like so: `for initial condition; loop condition; iteration code {}`
* For-in-loops are used like so: `for element in range {}`
* Repeat-while-loops are used like so: `repeat {} while condition;`
* While-loops are used like so: `while condition {}`
* You can _break out of a loop entirely_ using `break`
* You can skip _this iteration_ of a loop using `continue`
* Loops can be named and skipped selectively: 

```
loop1: for x in 0...5 {
	loop2: for y in 0...5 {
		if ... {
			continue loop1
		}
	}
}
```


### Optionals
* Swift vars/ consts _cannot hold nil_, except for __optionals__
* An optional type is allowed to reference either a value or nil
* They are declared using a question-mark: `var errorCode: Int?`
* Optionals must be unwrapped before they can be used
  * Force unwrapping means the app _will crash_ if it is nil: `errorCode!`
  * Safely unwrapping _will not crash_ if value is nil: `if let unwrappedErrorCode = errorCode {}`
    * Multiple values can be unwrapped at the same time: `if let a = aOpt, b = bOpt, ... {}`
* You can give variables default values in case an optional is nil: `var result = optionalInt ?? 0`

## Blocks
* Blocks must _always_ be surrounded by brackets

### Functions
* Function parameters comes after the function name in parentheses: `functionName(param1: type, param2: type)`
* Functions can be called like so: `functionName(1, 2)`
* You can label parameters to make the function call more readable: `functionName(param1: type, and param2: type)`
* Labeled functions are called like so: `functionName(1, and: 2)`
* You can give parameters a default value in case they are omitted: `functionName(param1: Int = 2)`
* The return value comes after the function parameters followed by `->`: `functionName() -> ReturnType`
* All parameters passed to a function are constants
  * Using `var` before the parameter makes it mutable
* All values passed to a function are copied (pass-by-value)
  * Using `inout` keyword before the parameter makes it pass-by-reference
  * Remember to use `&value` in function call to pass reference instead
* Functions are values, and can be saved in variables: `var add: (Int, Int) -> Int = functionName`

### Closures
* _All variables and constants_ within the closure body are captured by the closure
* Closures are values, and can be assigned to a variable or constant like so: `var multiply: (Int, Int) -> Int = {}`
* Closures are declared like so: `{(parameters) -> returnValue in body}`
  * Example: `{(a: Int, b: Int) -> Int in return a * b}`
  * For simple closures, the `return` keyword can be omitted: `{(a: Int, b: Int) -> Int in a * b`
  * Because of type inference, the return type can be omitted: `{(a: Int, b: Int) in a * b}`
  * Because of type inference, the parameter types do not have to be specified: `{(a, b) in a * b}`
  * You do not need to list parameters: `{$0 * $1}` ($n refers to the n'th parameter)
* If closure is final parameter of function call we can move the closure outside parentheses: `operateOnNumbers(4, 2, operation: {$0 + $1})` can be written: `operateOnNumbers(4, 2) {$0 + $1}`

## Collections
* Swift supports three basic collection types: `arrays`, `dictionaries`, and `sets`
* All collection types conform to a number of protocols, some are shared by all collection types
* All standard collection types support __sequence operations__, which are functional-style functions that work on the content of the collection:
  * `map` takes a _collection of values_, transforms them by applying the _transform block_, and returns an _collection of new values_: `array.map(transform block)`
  * `filter` takes a _collection of values_, runs the _filter block_ on each element, and returns a _collection of new values_ with all elements where the block returned true: `array.filter(filter block)`
  * `reduce` takes a _collection of values_, combines them based on the _combine operation_ passed, and returns the _final value_: `array.reduce(initial value, combine: +)`
  * In general `$0` refers to the current element, in dictionaries `$0.0` refers to the key and `$0.1` to the value
  * In `reduce` `$0` refers to the _partially combined result_, and `$1` the current element
* You can check the content of any collection using `.isEmpty`
* You can check the length/ size of any collection using: `.count`
* You can get the first/ last element of any collection using: `.first`/ `.last`
* You can get the smallest/ largest element in any collection using: `.minElement()`/ `.maxElement()`
* You can check if a collection contains an element using: `.contains(element)`

### Arrays
* Specifically declare an array like so: `let numbers: Array<Int>`
* Initialize an empty array like so: `let numbers = Array<Int>()`, `let numbers = [Int]()`
* Initialize an array with elements like so: `let numbers = [1, 2, 3]`, `let zeroes = [Int](count: 3, repeatedValue: 0)`
* You access elements using subscripting: `array[index]`
* You can access multiple elements at the same time: `array[1...2]` (returns a new array)
* You can append a new element to the array using: `array.append(element)`, `array += [element]`
* You can insert an element into an array using: `array.insert(element, atIndex: idx)`
* You can remove the last element in an array using: `array.removeLast()` (returns the element)
* You can remove any element in an array using: `array.removeAtIndex(index)` (returns the element)
* Arrays can be sorted using: `array.sort()`, `array.sortInPlace()`
* You can iterate through the array using `for ... in ... {}` loops: `for element in array {}`, `for (index, element) in array.enumerate () {}`

### Dictionaries
* Specifically declare a dictionary like so: `let pairs: Dictionary<String, Int>`
* Initialize an empty dictionary like so: `let pairs = Dictionary<String, Int>()`, `let pairs = [String: Int]()`
* Initialize a dictionary with elements like so: `let pairs = ["Key1": 1, "Key2": 2, ...]`
* All dictionaries can be emptied using: `dictionary = [:]`
* You access an element using its key: `dictionary[key]`
* You can create an array of a dictionary's keys/ values like so: `dictionary.keys`/ `dictionary.values`
* You can update or add a new value to a dictionary like so: `dictionary.updateValue(value, forKey: key)`, `dictionary[key] = value`
* You can remove a value from a dictionary like so: `dictionary.removeValueForKey(key)`, `dictionary[key] = nil`
* You can iterate through a dictionary using a `for ... in ... {}` loop: `for (key, value) in dictionary {}`
* You can get the hash value for any value/ object conforming to the `Hashable` protocol: `object.hashValue`

### Sets
* Specifically declare a set like so: `let set: Set<Int>`
* Initialize an empty set like so: `let set = Set<Int>()`
* Initialize a set with elements like so: `let set: Set<Int> = [1, 2, 3, 1]`, `let set: Set = [1, 2, 3, 1]`
* You can insert an element into a set using: `set.insert(element)`
* You can remove an element from a set using: `set.remove(element)`
* You can iterate through the elements of a set using a `for ... in ... {}` loop: `for element in set {}`
* Sets support _four basic_ __set operations__: `union`, `intersect`, `subtract`, and `exclusiveOr`
  * __Union__ creates a new set with all values from both sets: `set1.union(set2)`
  * __Intersect__ creates a new set with only values found in both sets: `set1.intersect(set2)`
  * __Subtract__ creates a new set by removing values from first set that also appears in second set: `set1.subtract(set2)`
  * __ExclusiveOr__ creates a new set with values that appear in one, but not both sets: `set1.exclusiveOr(set2)`
  
## Named types
* Swift supports four kinds of __named types__: `structs`, `classes`, `enums`, and `protocols`
* All named types support both properties and methods
* Properties and methods of a type can be referenced using __dot-notation__: `type.property`, `type.method()`
* All types can be extended with more properties and methods: `extension type {}`
* Methods and properties can be marked as `static` to be available on type-level
* Types allow you to specify __access control__ on their methods or properties
  * __Public__ makes entities available to the code inside the module it is defined in, as well as modules importing the module
  * __Internal__ makes entities available to code inside the module it is defined in, but not modules importing the module
  * __Private__ makes entities available only to the same file they're defined in


### Initializers
* Initializers can be marked as `required` to work as a __designated initializer__, which means all subclasses are forced to `override` and use it when calling `super`
* Initializers can be marked as `convenience` to work as a __convenience initializer__, which are forced to call the designated initializer from within

### Properties
* Stored properties hold a value
* Stored properties can have __default values__
* Computed properties performs a calculation before returning
  * Computed properties are defined like so: `var name: type {calculations}`
  * _Only a getter_ is created for you by default
  * Computed properties can also _set related properties_, but you have to specify both getter and setter like so: `var name: type {get {calculations} set {calculations using newValue}}`
* Properties can be declared as `weak` to avoid reference cycles to other objects
* You can use __property observers__ `willSet` and `didSet` by specifying their implementation: `var name: type {willSet {} didSet{}}` (property observers are _not called in the init-stage_)
  * You can use property observers to limit the value of a property using `currentValue`/`newValue` and `oldValue`/`currentValue` constants
* You can specify a stored-computed property as `lazy` to make sure it isn't calculated before being used: `lazy var name = {calculations}()`
* Using __access control__ you can limit properties, or certain methods on the property, from being visible to other modules: `private(set) var...`

### Structures
* A struct encapsulated related properties and methods
* Structs are __value types__, which means they are copied on assignment instead of referenced by a pointer
* You can create a struct like so: `struct name {properties/ methods}`
* An initializer is _defined automatically_ by swift, using all properties
  * If you define a custom initializer, _the default is removed_
* You can assign values to a struct member using: `struct.property = value`
* Structs, by definition, _cannot modify themselves_. A method can be marked as `mutating` if you want it to change properties on the struct, but this makes a new copy of the struct which is different from previous copies

### Classes
* Classes are __reference types__, which means a pointer to the object are copied on assignment
* You can create a class like so: `class name {properties/ methods}`
* You can use `===` to check for identical objects
* Classes can inherit from other classes like so: `class name: inheritedClass {}`
* You can use the keyword `final` for classes/ methods that don't allow subclassing/ overriding
* Shared (singleton) classes uses a private initializer `private init() {}` to make sure no other class can initialize a copy, and a static property `static let defaultClass = ThisClass()` to give a global access point to the singleton class

### Enumerations
* Enums are lists of related values that define a common type
* You can create an enum like so: `enum name {case name, \ncase name, ...}`, `enum name {case name, name, ...}`
* By default the cases are _not backed by integers_, but the name itself is the value
* You can specify custom raw values like so: `enum name: type {case name = value, case name, ...}`
* You can also give enums __associated values__ like so: `enum Result {case Success(Int), \ncase Error(String)}`

### Protocols
* Protocols are interfaces/ templates for a type
* You can create a protocol like so: `protocol Name {property/ method signatures}`
* Types adopt protocols like so: `type Name: Protocol, ... {}`
* Properties and method parameters defined in a protocol _cannot have default values_
* Methods defined in a protocol _cannot have implementations_
* Properties defined in a protocol must specify the property methods like so: `var name: type {get set}`
* Protocols can inherit from other protocols
* Protocols can define `typealias`'es, which allows it to use arbitrary names in place of specifying a type when it is not known: 

```	
PROTOCOL:
typealias WeightType
func calculateWeight() -> WeightType

IMPLEMENTATION:
typealias WeightType = Int
func calculateWeight() -> Int {}	
```

* Variables can be typed as a protocol, which allows it to hold classes, structs, or enums given that they all conform to the protocol
* Swift standard library comes with a number of protocols that can be implemented by your own types (such as `Equatable`, `Comparable`, etc.)
* Protocols can be extended, and their methods can be implemented/ properties filled such that they don't have to be repeated in every type that implements them: `extension protocol {implementation}`
* When extending a protocol you can also specify what other protocols a class must conform to before the implementation goes into effect: `extension Protocol where Self: AnotherProtocol {implementation}`

### Generics
* Generics provide a mechanism for using one set of types to define a new set of types
* Works with any named type (`enum`, `struct`, `class`)
* All collections are examples of generics (`Array<Element>`)
* You can make a generic like so: `struct Keeper<T> {}`
* You use a generic like so: `catKeeper = Keeper<Cat>()`
* You can use `T` inside the type body to define properties, methods, etc.
* If your generic has properties that define the type (`T`), you can omit the `Keeper<Cat>` declaration: `var catKeeper = Keeper(Cat(name:"Pip"))`
* A generic can use multiple types separated by commas: `Dictionary<Int, String>`
* A type can be constrained by a protocol the type must implement, like so: `Dictionary<Key: Hashable, Value>`
* Generics can also be used in standalone functions: `func swap<T, U>(x:T, _ y:U) -> (U, T) {}`

## Error Handling
* The Swift Standard Library Protocol `ErrorType`, can be conformed to and works with other parts of the error handling architecture
* A method with the keyword `throws` can throw an error: `func method() throws {}`
* Functions that handles errors should use the `do {try ...} catch {...} ...` block
* Failable initializers are allowed to fail without crashing your program, simply returning nil: `init? (param) {guard param correct else return nil}`