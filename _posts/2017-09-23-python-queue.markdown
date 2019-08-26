---
layout: post
title:  "Python implementation of a Queue"
categories: python
---
A Queue is an ordered collection of items of data.

<code>Enqueue</code> adds an item to the collection.

<code>Dequeue</code> removes the item which was added first. 

It is a FIFO (first in, first out) structure.

 
<pre class="lang:default decode:true " >class Queue(object):
    """
    Implementation of a queue in Python
    A FIFO (first in, first out data structure)

    Example usage:

    &gt;&gt;&gt; queue = Queue()
    &gt;&gt;&gt; queue.enqueue(1)
    &gt;&gt;&gt; queue.enqueue(2)
    &gt;&gt;&gt; print(queue.dequeue())
    2

    """

    def __init__(self):
        self.queue_store = []

    def enqueue(self, new_item):
        """ Add a new item to the back of the queue """
        self.queue_store.append(new_item)

    def dequeue(self):
        """ Remove an item from the front of the queue """
        return self.queue_store.pop(0)</pre> 

<h1>Testing</h1>
 
<pre class="lang:default decode:true " >import unittest
from queue import Queue

class TestQueue(unittest.TestCase):
    """
    Tests for queue.py
    """

    def setUp(self):
        self.queue = Queue()

    def test_queue_with_one_item(self):
        self.queue.enqueue(1)
        out = self.queue.dequeue()
        self.assertTrue(out == 1)

    def test_queue_with_multiple_items(self):
        test_list = [1, 2, 3, 4, 5]
        for item in test_list:
            self.queue.enqueue(item)
        for item in test_list:
            self.assertTrue(self.queue.dequeue() == item)

    def test_queue_with_empty_queue(self):
        """ Check that an IndexError is raised by trying to remove an item from an empty queue """
        self.assertRaises(IndexError, lambda: self.queue.dequeue())

suite = unittest.TestLoader().loadTestsFromTestCase(TestQueue)
unittest.TextTestRunner(verbosity=2).run(suite)</pre> 
