---
layout: post
title:  "Recursion in Swift"
categories: swift
---
Recursion is essentially when a function calls itself as one of its steps.

I've been reading about recursion recently (<a href="https://en.wikipedia.org/wiki/Recursion">wikipedia</a>). 

This doesn't sound very useful - wouldn't we just get a program that runs for ever? This is avoided by having a "base case" at which the function terminates.

I tried out some examples in Swift. First, a function to find the length of an array:

```swift
import UIKit

func lengthOfArray(_ input: Array<Any>) -> Int {
    var a = input
    if a.isEmpty {
        return 0
    } else {
        a.removeLast()
        return 1 + lengthOfArray(a)
    }
}

let a = [1,2,3,4]

print(lengthOfArray(a))
```

This could be done a lot easier in other ways, but it demonstrates the principles.

Here's another one, to reverse an array:

```swift
func reverseOfArray(_ input: Array<Any>) -> Array<Any> {
    var a = input
    if a.isEmpty {
        return a
    } else {
        let b = a.removeFirst()
        var c = reverseOfArray(a)
        c.append(b)
        return c
    }
}

print(reverseOfArray(a))
```