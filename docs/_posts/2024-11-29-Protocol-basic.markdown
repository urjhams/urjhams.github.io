---
layout: post
title:  "Protocol Basic"
date:   2024-11-29 16:50:00 +0200
categories: Swift
---
Protocol in Swift, similar to Interface in other langauge, defines a blueprint of required methods and properties for a particular task or feature. When a class, structure, or enumeration conforms to a protocol, it must provide the actual implementation of the protocol’s requirements.

### Protocol syntax

Using the protocol keyword, we could define a protocol, similar to classes, structres or enumerations.

```swift
protocol Foo {
    // protocol definitions
}
```

A class, struct or enumeration can conform one or multiple protocols

```swift
struct aStruct: ProtocolA, ProtocolB, ProtocolC {
    ...
}
```

If a class inherits from a super class, the super class is listed before the protocol(s)

```swift
class aClass: ParrentClass, ProtocolA, ProtocolB {
    ...
}
```

When the type conforms a protocol, it must declare all the requirements from the protocol.

```swift
protocol Foo {
    var protocolProperty: Int { get set }
    func protocolFunction(variable: String)
}

struct Bar: Foo {
    var protocolProperty: Int

    func protocolFunction(variable: String) {
        ...
    }
}
```

### Property requirements

The protocol can require any conforming type to provide property with a specific name and type, as instance property or type property, be gettable only or both gettable or settable. However, it doesn’t specify the property to be a stored property or computed property.

The property requirements of the protocol are always declared as variable, following with `{ get }` for the gettable variable or `{ get set }` for gettable and settable variable.

```swift
protocol Foo1 {
    var cannotBeSet: Int { get }
    var canBeSet: Int { get set } 
}
```

A type property requirements always has `static` keyword as prefix.

```swift
protocol Foo2 {
    static var typeProperty: Int { get set }
}
```

### Method requirements

Protocols can define required methods—both type and instance methods—that conforming types must implement. These methods are declared without curly braces or a body, similar to method signatures in interfaces.

```swift
protocol SomeProtocol {
    func someMethod() -> Bool 
}

struct SomeStruct: SomeProtocol {
    func someMethod() -> Bool {
        return true
    }
}
```

The body of the required methods can also be defined with default implementation in the protocol’s extension. With a default implementation, conforming types are not required to implement the method unless they need a specific or customized behavior.

```swift
extension SomeProtocol {
    func someMethod() -> Bool {
        return false
    }
}
```

### Initializer requirements

Protocols can define required initializer for the conforming types. Similar to methods, these initializers is written the exact same way as normal initializers, without curly braces or body.

```swift
protocol SomeProtocol {
    init(parameter: Int)
}
```

### Conclusion

Protocols offer high flexibility for writing reusable code in Swift. Furthermore, the **Protocol-Oriented Programming** paradigm enhances polymorphism by supporting multiple protocol conformances. Defining interfaces through protocols helps prevent the complexities of deep inheritance hierarchies and reduces boilerplate or unnecessary code. Instead, it encourages composing small, reusable, and specific requirements, leading to cleaner and more maintainable codebases.
