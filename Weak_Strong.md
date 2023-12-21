# weak strong 

When a reference or instance of a class is created, it consumes a certain amount of memory. After the compilation of the code or when the instance is not needed anymore, compiler automatically release the allocated memory for further use. This ensures that class instances don’t take up space in memory when they’re no longer needed. This mechanism is known as Automatic Reference Counting (ARC). However, if ARC were to deallocate an instance that was still in use, it would no longer be possible to access that instance’s properties, or call that instance’s methods. Indeed, if you tried to access the instance, your app would most likely crash.

To make sure that instances don’t disappear while they’re still needed, ARC tracks how many properties, constants, and variables are currently referring to each class instance. ARC will not deallocate an instance as long as at least one active reference to that instance still exists. 


To make the reference still exist, a strong reference is created whenever we assign a class instance to property, constant or variable. The name “strong” suggest that there is a firm holding connection to the instance. It doesn’t allow to deallocated the instance as long as the strong reference remain. Still confused? Let’s dive into an example: 




Let consider a Car class with some property. 

```swift
class Car{
    var name: String 
    //constructor
    init(name: String){
        self.name = name
        print("Constructing",self.name," Car")
        
    }
    //destructor called automatically 
    deinit{
        print("Deleting ",self.name," Car")
    }
}
```

Now create some instances of the Car class. 

```swift
var car: Car?
```

> **PIP**
>Why optional Car? not Car. Because the instances are optional and it will automatically be initialized with nil and don’t currently reference a Car instance. 
>


Now create car1 as a new Car instance and assign the name to “BMW”. 

```swift
car = Car(name: “BMW”)
//output will print “Constructing BMW Car” 
```

Here a new Car instance is created and assigned to car variable. So there will be a strong reference between car and new Car instance. If we see it in diagram we can see that. 

<center> <img src="https://github.com/shameem17/iOS-Tutorial/assets/53037559/73eba0b1-401e-48cd-9def-dec977ee3f68"> </center>



If two new Car instances are created and assigns the same instance of car then there will be a strong reference among the three car instances. 

```swift

var car2: Car? = car
var car3: Car? = car
```

As there is a strong relation or reference remains amon the three instances, dealloaction of any single or two of them is not possible.
```swift
car = nil
car2 = nil
```

This will not deallocate the Car instance as there is a strong reference with car3 still exist. 
```swift
car3 = nil
``` 

Now the Car instance will be deallocated. This strong reference is a self class strong reference. 



# Strong Reference Cycles Between Class Instances 

Now again consider of an example of a Car class and a Person class. These two classes can be related strongly in a sense that a Person may have a Car and a Car may have a owner. 

```swift
class Car{
    var name: String 
    var owner: Person? //will refer to a person instance
    //constructor
    init(name: String){
        self.name = name
        print("Constructiong Car")
        
    }
    //destructor called automatically 
    deinit{
        print("Deleting Car")
    }
}

class Person{
    var name: String 
    var car: Car? //will refer to a Car instance
    init(name: String){
        self.name = name
        print("Person Initialized")
    }
    
    deinit{
        print("Person Deleted")
    }
}
```

Now create instances of Person and Car class. 

```swift
var mr: Person? = Person(name: "Mr")
var bmw: Car? = Car(name: "BMW")
```

<center> <img src="https://github.com/shameem17/iOS-Tutorial/assets/53037559/787888d7-80e6-4447-baa3-28982159736b"> </center>

This two instances of the two classes are not connected to each other. But if we assign the car property of Person class with the bmw instance and the owner of Car class with mr instance then there will be a strong reference cycle among the two instance.

```swift
mr!.car = car
car!.owner = mr
```

Thus a strong reference between mr and bmw has been formed. We can’t break the strong reference cycle. 

<center> <img src="https://github.com/shameem17/iOS-Tutorial/assets/53037559/debb29c9-7cf6-44c0-be83-e4d5be6119e6"> </center>

```swift
mr = nil
car = nil 
```

The strong reference between Person and Car remains and can’t be broken. It also known as retention cycle. However it can be broken by breaking the reference cycle as follow:

```swift
mr!.car = nil
```

But there may be thousands of references. To assigning nil to each object reference is quite difficult for us. We need something to automatically deinitialize the instances. 

# Resolving Strong Reference Cycle

Swift provides two ways to resolve strong reference cycles when you work with properties of class type: weak references and unowned references.
Weak and unowned references enable one instance in a reference cycle to refer to the other instance without keeping a strong hold on it. The instances can then refer to each other without creating a strong reference cycle.


Use a weak reference when the other instance has a shorter lifetime — that is, when the other instance can be deallocated first. In the Person example above, it’s appropriate for a person to be able to have no car at some point in its lifetime, and so a weak reference is an appropriate way to break the reference cycle in this case. In contrast, use an unowned reference when the other instance has the same lifetime or a longer lifetime. 

```swift
class Car{
    var name: String 
    var owner: Person? //will refer to a person instance
    //constructor
    init(name: String){
        self.name = name
        print("Constructiong Car")
        
    }
    //destructor called automatically 
    deinit{
        print("Deleting Car")
    }
}

class Person{
    var name: String 
  weak  var car: Car? //weak reference
// unowned var car: Car?  // weak or unwoned will not create strong reference
    init(name: String){
        self.name = name
        print("Person Initialized")
    }
    
    deinit{
        print("Person Deleted")
    }
}
```

Now there are no strong reference between Person and Car. So there is a question when to use weak and when to use unowned? 

<center> <img src="https://github.com/shameem17/iOS-Tutorial/assets/53037559/14303a29-0238-4b57-a568-7ed673914bef"> </center>

First let’s use see some rules of using a weak reference. 

Rule 1: The variable must be mutuable. That means we can not make a let property as a weak property. 

```swift
weak let variable_name: Variable_Type
//this will create an error
```
Rule 2: The weak property should be optional. 

```swift
 // correct syntax for a weak property be
weak var person: Person?
```

Now let us consider a situation, there is no fixed rule that every Person must have a Car. So for a Person to have a Car is optional. It maintains the rules of using weak property. Thus,

```swift
class person{
    Let name: String
    Weak var car: Car? //optional weak relation
}
```

> **TIPS** 
> Use weak when the relation is optional. 
> Don’t forget to unwrap the optional value before use

Again consider a class BankAccount. Is it possible to open a bank account without a person? Surely no. So the Person for a BankAccount is not optional. In such cases to resolve the strong reference cycle issue we use unowned instead of weak. 

```swift
class BankAccont{
    Let accountNo: Int
    Unowned accountHolder: Person
}
```

> **TIPS** 
> Use unowned when the relation amon the classes is not optional. ARC never assign nil to an unowned reference.
>

In both cases weak and unowned ARC don’t create a strong reference. 


# [weak self] 
Now comes [weak self] or [unowned self]. In Swift, [weak self] is a capture list used in closures to avoid strong reference cycles, also known as retain cycles. A retain cycle occurs when two or more objects hold strong references to each other, which prevents them from being deallocated by the memory management system, even if they are no longer needed.

```swift
class MyClass {
    var closure: (() -> Void)?
    
    func setupClosure() {
        closure = { 
            self.doSomething() //using self class reference 
        }
    }
    
    func doSomething() {
        print("Did something")
    }
    
    deinit {
        print("Delete Instance")
    }
}

var obj: MyClass? = MyClass()
obj?.setupClosure()
obj = nil // Delete Instance will not be printed
```

There is a closure which is using the self class reference. This reference forms a strong reference cycle. So the deallocation is not permitted here. Weak self or unowned self can solve this issue.  

```swift
class MyClass {
   var closure: (() -> Void)?
  
   func setupClosure() {
       closure = { [weak self] in
           self?.doSomething()
       }
   }
  
   func doSomething() {
       print("Did something")
   }
  
   deinit {
       print("Deinitialized")
   }
}

var obj: MyClass? = MyClass()
obj?.setupClosure()
obj = nil // Deinitialized will be printed

```
# IOS Development Scenario 

In iOS development, there are many cases where we use closure to make API calls, download resources from online etc. There we use the self.view instance to make a strong references of the viewcontroller. This causes memory leak. We can avoid this memory leak issue by using [weak self] or [unowned self]. 

> **NOTE**
>I tried to simply mention why and when to use weak and unowned. For more details you can follow the provided references.
>



References:

[Swift Documetation](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/automaticreferencecounting/)

[Weak vs Unowned](https://www.youtube.com/watch?v=HdhJjc7Y6iw&ab_channel=CodeCat )

[Avanderlee](https://www.avanderlee.com/swift/weak-self/)

[Medium](https://medium.com/@almalehdev/you-dont-always-need-weak-self-a778bec505ef)

[Swift With Vincent](https://www.swiftwithvincent.com/newsletter/when-do-you-really-need-to-use-weak-self)


