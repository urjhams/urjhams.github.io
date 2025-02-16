---
layout: post
title:  "The mutating keyword"
date:   2025-01-25 16:50:00 +0200
categories: Swift
---

By default, value types in Swift (struct, enum, and tuple) are immutable. This means that properties within an instance of these types cannot be modified directly. Instead, a new instance with updated property values must be assigned. In such cases, the mutating keyword allows methods to modify the properties of their instance, effectively changing the instance itself.

## Enabling mutation

By marking the method with `mutating`, Swift allows that method to modify the properties of the instance.

```swift
func address(of pointer: UnsafeRawPointer) -> String {
    .init(format: "%p", Int(bitPattern: pointer))
}

struct Foo {
    var a: Int

    mutating func add(_ b: Int) {
        a += b
    }
}

var foo = Foo(a: 1)
withUnsafePointer(to: &foo) {
    print("foo's address:", address(of: $0))  //0x1119a8590
}

// the copy of the instance is allocated at difference memory address
var copy = Foo
withUnsafePointer(to: &copy) {
    print("copy's address:", address(of: $0))  //0x127fb9270
}

// the mutated instance is stay the same at the original memory address
foo.add(3)
withUnsafePointer(to: &foo) {
    print("mutated foo's address:", address(of: $0))  //0x1119a8590
}
```

Additionaly, the instance itself canbe replaced by a completely new instance (but still share the same address)

```swift
struct Bar {
    var a: Int

    mutating func replace(_ c: Int) {
        self = Self(a: c)
    }
}

var bar = Bar(1)
withUnsafePointer(to: &bar) {
    print("original bar:", address(of: $0)) // 0x127f6d5f0
}

bar.replace(9)
withUnsafePointer(to: &bar) {
    print("original bar:", address(of: $0)) // 0x127f6d5f0
}
```

## How mutating work

When a mutating method is called on a value type, Swift first copies the instance, modifies the copy, and then replace the original instance with the modified copy. This mechanism ensures immutability by default but provides controlled mutation, also ensures the developers are explicit about when and how value types can be changed for safer and more predictable code.

## Copy-on-Write mechanisum

In Swift, certain value types like `String`, `Array`, and other `Collection` types use `Copy-on-Write (CoW)` as an optimization technique to manage memory efficiently. The primary goal is to delay data copying until it is necessary—specifically when a mutation actually occurs.

This mechanism is effective when the value type contains multiple properties, which would otherwise be expensive to copy entirely. That is why collection types like Array and String in Swift use `copy-on-write`.

When a value type (e.g., an Array) is assigned to a new variable, Swift does not immediately create a new copy of the data. Instead, both variables reference the same underlying storage. When a modification is triggered in one of the instances via mutating, Swift checks if the storage is shared. If it is, Swift creates a unique copy of the data for the modified instance. This ensures that changes to one variable do not affect the other, preserving value semantics and avoiding unnecessary data copying until a write operation occurs.
