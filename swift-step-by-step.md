# SwiftIntro
```
import UIKit

var str = "Hello, playground"
str = 5
// Swift is type safe
```
Error because Swift is a type-safe language, meaning once a variable is declared as a certain type, it must maintain that type

```
var currency: Double = 5
// not using type inference
```
If you don't specify the type of value you need, Swift uses type inference

Yay! no semicolons

```
str = "Something else"
```

Able to overwrite var's

```
let anotherStr = "Hello, playground"
anotherStr = "Something else"
// var is mutable, let is constant/immutable
```
Default should be immutable

**Switch with partner**

Let's talk about control flow: Starting with loops

```
let names = ["John", "Chris", "Lucie"]
for name in names {
    print("Hello " + name)
}
// For each loop
```
```
for index in 0..<5 { //equivalent to for(i = 0; i < 5; i ++)
    print("\(index) times 5 is \(index * 5)")
}
// For loop, using String Interpolation
```
Next we'll use conditionals:
```
let temperature = 40
if temperature <= 32 {
    print("It's cold")
} else {
    print ("It's not that cold")
}
```
Yay! no parenthesis.  Swift syntax is pretty light.

**Switch with partner**

Functions:

```
func greet(name: String) -> String {
    return "Hello \(name)"
}
```
Notice the aruement labels for each parameter

```
print(greet(name: "John"))
```

How everyone is doing? Last two topics slighty more complicated

**Switch with partner**

```
var newStr: String = "hello"
// In java...
if str == nil {
    print("it's nil")
}
```
Look similar to what we do in a language like java to ensure the property has been initialized?
Warning: what does non-optional mean?  It means that str cannot contain nil.

```
var optionalStr: String? = "hello"
```

Optional is just a type in Swift language, nothing fancy. String and String? (optional String) are two different types, if your variable happens to be of type String you can be absolutely sure it will always have an string value, and if your variable is of type String? it will either have a string value or it will have no value at all (in other words, it will be nil).
Think of optional as a wrapper type. It’s like a gift box which wraps the value inside, and like a real-life box, optional can either contain something or be empty.

```
var firstNameElement = names.first
print(firstNameElement)
```
`first` operator is useful to return an optional, in case the array is empty.
But don't want to print optional(..), how do we just get the first name element, if it exists?

```
if let name = firstNameElement {
    print(name)
} else {
    print("array is empty")
}
```

**Switch with partner**

Classes/structs:
```
class Note {
    var content: String?
    let dateCreated = Date()
    
    init() {}
}

var firstNote = Note()
firstNote.content = "first note"
var secondNote = firstNote
secondNote.content = "second note"
print(firstNote.content)
// Pass by reference
```
Change class to struct - pass by copy!
Copying is way safer than having multiple references to the same instance

Subclass/Inheritence:

Similar to an interface in java, doesn't contain any code but rather properties and method stubs

```
class BasicNote {
    var content: String?
    func printNote() {
        print(content!)
    }
}


class Note: BasicNote {
    let dateCreated = Date()
    
    override func printNote() {
        print("create on \(dateCreated): \(content!)")
    }
}
```
