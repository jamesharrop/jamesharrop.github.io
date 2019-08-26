---
layout: post
title:  "Python implementation of a Stack"
categories: python
---
A Stack is an ordered collection of items of data (<a href="https://en.wikipedia.org/wiki/Stack_(abstract_data_type)">wikipedia</a>).

<code>Push</code> adds an item to the collection.

<code>Pop</code> removes the most recently added item. 

<code>Peek</code> gives access to the most recently added item.

It is therefore a LIFO (last in, first out) structure.

<pre class="lang:default decode:true " >class Stack(object):
    """
    Implementation of a stack in Python
    """

    def __init__(self):
        self.stack_store = []

    def push(self, new_item):
        self.stack_store.append(new_item)

    def pop(self):
        try:
            last_element = self.stack_store.pop()
            return last_element
        except IndexError:
            print("Error - pop() called - but no items in stack")

    def peek(self):
        try:
            return self.stack_store[-1]
        except IndexError:
            print("Error - peek() called - but no items in stack")</pre>

Unit Testing:

<pre class="lang:default decode:true " >import unittest
from stack import Stack

class TestStack(unittest.TestCase):
    """
    Tests for stack.py
    """
    def setUp(self):
        self.stack = Stack()

    def test_stack_with_one_item(self):
        self.stack.push(1)
        out = self.stack.pop()
        self.assertTrue(out == 1)

    def test_stack_with_multiple_items(self):
        self.stack.push(1)
        self.stack.push(2)
        self.stack.push(3)
        out = self.stack.pop()
        self.assertTrue(out == 3)
        out = self.stack.pop()
        self.assertTrue(out == 2)
        out = self.stack.pop()
        self.assertTrue(out == 1)

    def test_stack_with_empty_stack(self):
        out = self.stack.pop()

    def test_stack_peek(self):
        self.stack.push(1)
        self.assertTrue(self.stack.peek() == 1)
        # Then peek again to check the item is still on the stack
        self.assertTrue(self.stack.peek() == 1)

suite = unittest.TestLoader().loadTestsFromTestCase(TestStack)
unittest.TextTestRunner(verbosity=2).run(suite)</pre> 

Example usage:
<pre class="lang:default decode:true " >
>>> stack = Stack()

>>> stack.push("apple")
>>> stack.push("pear")
>>> stack.push("banana")

>>> print(stack.peek())
banana

>>> print(stack.pop())
banana

>>> print(stack.pop())
pear

>>> print(stack.pop())
apple
</pre> 