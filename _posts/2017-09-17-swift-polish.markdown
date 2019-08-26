---
title:  "Simple implementation of prefix notation calculator in Swift 3"
categories: swift
---
A Prefix (Polish) notation calculator.

Explanation of Polish notation: [Wikipedia](https://en.m.wikipedia.org/wiki/Polish_notation)

{% highlight swift %}
import UIKit

struct InputText {
    var input: String
    var currentIndex: Int
    var inputArray: [String]
    
    init(_ input: String) {
        self.input = input
        self.inputArray = input.components(separatedBy: " ")
        self.currentIndex = inputArray.count - 1
    }
    
    mutating func getNextWord() -> String? {
        if self.currentIndex == -1 { return nil }
        let word = inputArray[currentIndex]
        self.currentIndex -= 1
        return word
    }
}

struct Stack {
    var doubleValues: [Double] = []
    
    mutating func push(_ value: Double) {
        doubleValues.append(value)
    }
    
    mutating func pop() -> Double? {
        return doubleValues.removeLast()
    }
}

var input = InputText("+ 1 / 20 10")
// 1 + (20 / 10) = 3

var stack = Stack()

func processInput() -> Double? {
    while true {
        guard let nextSymbol = input.getNextWord() else {
            let output = stack.pop()
            return output
        }
//        print(stack.doubleValues, nextSymbol)
        if let operand = Double(nextSymbol) {
            stack.push(operand)
        } else {
            let operand1 = stack.pop()
            let operand2 = stack.pop()
            
            switch nextSymbol {
            case "+":
                stack.push(operand1! + operand2!)
            case "-":
                stack.push(operand1! - operand2!)
            case "/":
                stack.push(operand1! / operand2!)
            case "*":
                stack.push(operand1! * operand2!)
            default:
                print("Invalid symbol:", nextSymbol)
                abort()
            }
        }
    }
}

print(processInput()!)
{% endhighlight %}