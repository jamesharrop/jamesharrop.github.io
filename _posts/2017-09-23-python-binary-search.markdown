---
layout: post
title:  "Python implementation of a Binary Search"
categories: python
excerpt: ""
---

<pre class="lang:default decode:true " >import math

def binary_search(sorted_list, target_value, verbose):
    """
    Performs a binary search.
    Binary search should be O(log n) for time and O(1) for space.

    Args:
        sorted_list: a sorted list to be searched.
        target_value: the target value to be found.
        verbose: Bool - if true, then print workings of search to console

    Returns:
        The index of the target_value in the sorted_list,
            or None if the target_value is not in the list

    Raises:
        IndexError: if passed an empty list
    """

    index_of_start_of_search_area = 0
    index_of_end_of_search_area = len(sorted_list) - 1

    if verbose:
        print("***** Starting a binary search")

    while True:
        if verbose:
            print("*** Start of loop")
            print("Start index:", index_of_start_of_search_area)
            print("End index", index_of_end_of_search_area)

        middle_index = math.floor(index_of_start_of_search_area + (
            (index_of_end_of_search_area - index_of_start_of_search_area) / 2))

        middle_value = sorted_list[middle_index]

        if verbose:
            print("Middle_index:", middle_index)
            print("Middle_value:", middle_value)

        if middle_value == target_value: 
            if verbose:
                print("Found it")
            return middle_index

        if middle_value &gt; target_value: 
            if verbose:
                print("Target_value may be in the first half of the list")
            index_of_end_of_search_area = middle_index

        if middle_value &lt; target_value: 
            if verbose:
                print("Target_value may be in the second half of the list")
            index_of_start_of_search_area = middle_index + 1

        if index_of_start_of_search_area &gt; index_of_end_of_search_area:
            if verbose:
                print("Target value is not here")
            return None
</pre> 

<h1>Testing:</h1>
 
<pre class="lang:default decode:true " >import unittest
from binary_search import binary_search

class TestBinarySearch(unittest.TestCase):

    def setUp(self):
        self.verbose = False 
        # ^ whether to print interim output to console during binary_search

    def test_simple_for_binary_search(self):
        input_list = [0, 1, 2, 3, 4, 5]
        self.assertTrue(binary_search(input_list, 1, self.verbose) == 1)

    def test_binary_search_with_a_list_of_6_ints(self):
        input_list = [1, 20, 3, 5, 7, 0]
        input_list.sort()

        for n in range(0, len(input_list)):
            output = binary_search(input_list, input_list[n], self.verbose)
            self.assertTrue(output == n)

    def test_binary_search_with_a_list_of_7_ints(self):
        input_list = [1, 20, 3, 5, 7, 0, 21]
        input_list.sort()

        for n in range(0, len(input_list)):
            output = binary_search(input_list, input_list[n], self.verbose)
            self.assertTrue(output == n)

    def test_binary_search_with_target_value_not_in_list(self):
        input_list = [1, 20, 3, 5, 7, 0, 21]
        input_list.sort()

        output = binary_search(input_list, 25, self.verbose)
        self.assertTrue(output == None)

    def test_binary_search_with_empty_list(self):
        input_list = []
        self.assertRaises(IndexError, lambda: binary_search(input_list, 1,
            self.verbose))

suite = unittest.TestLoader().loadTestsFromTestCase(TestBinarySearch)
unittest.TextTestRunner(verbosity=2).run(suite)</pre> 
