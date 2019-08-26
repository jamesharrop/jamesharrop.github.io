---
layout: post
title: "The Reduce function in Swift"
categories: swift
---
Let's look at the ```reduce``` function working on an Array.

It can be thought of as working through the Array from start to finish, taking each element in turn.

You give it three things:

1. The Array which it will work on
2. An initial value
3. A closure which takes a "value so far" and the current element of the array and then returns an updated "value so far" for the next iteration. How the closure combines the initial "value so far" with the current element is up to us.

We will start by creating an array of Doubles:
<pre>
let a: [Double] = [2, 6, 3, 1, 2]
</pre>

Here is an example of the reduce function being used to add all the elements of the array:

<pre>
let result = a.reduce(0, combine: { (soFar, thisElement) -> Double in
    let sum = soFar + thisElement
    return sum
})
</pre>

The first parameter of the reduce function is the initial value - here it is ```0```.
The second parameter is the closure to combine the "value so far" with each element in turn.

So ```result``` is 2 + 6 + 3 + 1 + 2 = 14

This is easier to understand if we include some print statements in the closure so we can see the output of each iteration (obviously we don't need these in our final code!):

<pre>
let result = a.reduce(0, combine: { (soFar, thisElement) -> Double in
    let sum = soFar + thisElement
    print("So far we have the following value: \(soFar).")
    print("We are adding the current element of the array which is \(thisElement).")
    print("\(soFar) + \(thisElement) = \(sum) so we pass \(sum) on to the next iteration.")
    print("----------------")
    return sum
})
</pre>

Which produces this output to the terminal:

<pre crayon="false">
    So far we have the following value: 0.0.
    We are adding the current element of the array which is 2.0.
    0.0 + 2.0 = 2.0 so we pass 2.0 on to the next iteration.
    ----------------
    So far we have the following value: 2.0.
    We are adding the current element of the array which is 6.0.
    2.0 + 6.0 = 8.0 so we pass 8.0 on to the next iteration.
    ----------------
    So far we have the following value: 8.0.
    We are adding the current element of the array which is 3.0.
    8.0 + 3.0 = 11.0 so we pass 11.0 on to the next iteration.
    ----------------
    So far we have the following value: 11.0.
    We are adding the current element of the array which is 1.0.
    11.0 + 1.0 = 12.0 so we pass 12.0 on to the next iteration.
    ----------------
    So far we have the following value: 12.0.
    We are adding the current element of the array which is 2.0.
    12.0 + 2.0 = 14.0 so we pass 14.0 on to the next iteration.
    ----------------
</pre>
It's possible to write this function in more concise ways. However we should take care that this is not at the expense of readability. Here are some examples. All these formulations produce the same output (```14```) as the more verbose form above.

We don't really need to use the ```sum``` variable at all, we can do the sum as part of the ```return``` statement:
<pre>
a.reduce(0, combine: { (soFar, thisElement) -> Double in return soFar + thisElement })
</pre>
Swift can infer the output type is a ```Double```:
<pre>
a.reduce(0, combine: { (soFar, thisElement) in return soFar + thisElement })
</pre>
We don't need to include ```return``` in a one line closure:
<pre> 
a.reduce(0, combine: { (soFar, thisElement) in soFar + thisElement })
</pre>
We can use ```$0``` and ```$1``` as shorthand argument names. However this can make it more difficult to read the code unless we remember that ```$0``` is the value so far and ```$1``` is the current element. In this example it doesn't matter anyway as ```$0 + $1``` of course produces the same result as ```$1 + $0```
<pre>
a.reduce(0, combine: { $0 + $1 })
</pre>
Or even:
<pre>
a.reduce(0, combine: +)
</pre>