# Optional

<a href="https://github.com/shameem17/Swift/tree/master/Optional/optional.playground"> ![code](https://img.shields.io/badge/Code-Playground-1769DE?style=for-the-badge&logo=codeium&labelColor=grey)</a>  <a href="https://github.com/shameem17/Swift/blob/master/Optional/optional.swift"> ![code](https://img.shields.io/badge/Swift-Code-red?style=for-the-badge&logo=swift)</a>

Type safety is a cornerstone of the Swift language. As a result, variables and constants need to be initialized before they can be accessed. 

>  **Definition <br>
>Optional is a representation of variable and constant that may or may not have a value. 
>

When we are accessing a variable or constant, we know that the variable or constant we are accessing has a value or may not have a value. But Swift doesn't allow us to use a variable before initialization. 
```swift
var message: String
print(message) //will be a cause of errer because message is not initialized
```

There in most senarios we don't know if the variable will hold a value or not. If we fetch data from remote backend (i.e: API), for example, we don't know whether we are going to receive a valid response until the network request has completed. In such scenarios, we need the ablity to represent a different state, the absence of a value. And that is the concept of optionals.

> **Benefits <br>
>Optional helps to prevent runtime error
>


In the previous code snipt, we don't assign a value to the variable ```message``` which caused a error. We can declare the ```message``` variable as an optional variable such that it may or may not have a value.

```swift
var message: String? //optional variable
print(message) //nil
```

The ```message``` variable has no value initialized and it's an optional variable. So the ```message``` will contain ```nil ``` value. Whatever the data type of an optional variable, the default value will always be ```nil ```  unless it is defined. That means ```var age: Int? ```  age will also contain ```nil ```  value. 

## Declaring Optionals

To declare optional, we use a question mark(?). In the following example an optional of type ```String?``` or optional String is declared. Example:

```swift
var firstName: String?
```
Now printing the ```firstName``` variable will print ```nil``` value. One important thing to remember that, here the firstname is a variable. So we can change the variable anytime. But if we declare the variable as a constant like ```let firstName: String?``` and don't initialize the value then we can never use it as we can't change the value of a constant. Using a optional value is in the example:

```swift
var firstName: String?
firstName = "Shameem"
print(firstName)
```
The output will be something like ```Optional("Shameem")```. 

## Unwrapping Optionals 

Because the value of an optional is wrapped in a container, we cannot access the value of an optional like we access the value of a variable or constant. 

```swift
let finalName: String = firstName
```
This will create an error with message ```Value of optional type String? not unwrapped....```. So we need to unwrapped the optional value before use. There are several ways to unwrap optionals. They are:
- Conditional unwrap
- Forced unwrap
- Optional Chaining
- Nil-coalescing


### 1. Conditional Unwrapping (Optional Binding)

Unwrap the optional value if there exists any value. If, guard and switch can be used for Optional Binding. Example:

```swift 
if let name = firstName{
	print("Name is \(name)")
}else{
	print("Name is nil")
}
```

```guard``` is used inside function because guard returns something. 

```swift

func showName(){
	guard let name = firstName else{
		return
	}
	print("Name is \(name")
}                                                                                                
showName()
```


### 2. Optional Chaining 

Optional chaining is used to safely access properties and methods of a wrapped instance. For example ```count``` is an optional property of ```String```. In the above example, ```firstName``` is an optional String. ```count``` is an optional property of ```String```. 

```swift
let numberOfLetters = firstName?.count
//numberOfLetters is now an optional constant
print(numberOfLetters) //output = Optional(7)

//unwrap with conditional unwrapping
if numbers = firstName?.count{
	print("There are \(numbers) letter in firstName variable")
}

```
Some other optional properties are: hasSuffix, hasPrefix, isMultiple .....


### 3. Nil-Coalescing Operator

Use nil-coalescing operator ```(??)``` to set a default value in case the optional instance is nil. 

```swift
var country: String?
let my_country = country ?? "Default Country"
print(my_country) //output: - Default Country

```

### 4. Forced Unwrapping

When we are sure that the optional variable certainly contains a value then we can use forced unwrapping. Exclamatory ```(!)``` sign is used to forcly unwrap the optional variable.

```swift

var day: String? 
day = "Sunday"

let today = day! //forced unwrap
```

Forced unwrapping may cause runtime error if the optional variable doesn't contains any value i.e. contains ```nil``` value

> **IMPORTANT :x: <br>
> Optionals should never be forced unwrapped unless you are absolutely certain it contains a value
>


<a href="https://github.com/shameem17/Swift/tree/master/Optional/optional.playground"> ![code](https://img.shields.io/badge/Code-Playground-1769DE?style=for-the-badge&logo=codeium&labelColor=grey)</a>  <a href="https://github.com/shameem17/Swift/blob/master/Optional/optional.swift"> ![code](https://img.shields.io/badge/Swift-Code-red?style=for-the-badge&logo=swift)</a>


## References 

<a href="https://developer.apple.com/documentation/swift/optional#Using-the-Nil-Coalescing-Operator"> Swift Documentation </a>  

<a href="https://www.hackingwithswift.com/quick-start/beginners/how-to-handle-missing-data-with-optionals"> Hack With Swift</a>

