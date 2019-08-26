---
layout: post
title:  "Python implementation of an Insertion Sort"
categories: python
---
Insertion sort is a classic sorting algorithm.

(<a href="https://en.wikipedia.org/wiki/Insertion_sort">wikipedia link</a>).

 
<pre class="lang:default decode:true " >def insertion_sort(my_list):
    """
    Performs an insertion sort.

    Args:
        my_list: a list to be sorted.

    Returns:
        A sorted list

    Raises:
        TypeError: There are mixed types in the list which are not orderable
    """

    for location in range(1, len(my_list)): # Skip the first item - so range starts at 1

        current_item = my_list[location] # this is the first item which has not been sorted yet

        working_location = location

        # now work backwards through the sorted items to find the correct location
        # for the current_item

        for sorted_location in range(location - 1, -1, -1):

            compare_with = my_list[sorted_location]

            if current_item &lt; compare_with:
                # then swap items at sorted_location and working_location
                my_list[sorted_location] = current_item
                my_list[working_location] = compare_with
                working_location -= 1
                # here we could print the list to show the progress so far
                # print(my_list)

    return my_list
</pre> 

<h1>Testing</h1>
 
<pre class="lang:default decode:true " >import unittest
from insertion_sort import insertion_sort
import random

class TestInsertionSort(unittest.TestCase):
    """ Tests for insertion_sort """

    def test_insertion_sort_with_a_small_list_of_ints(self):
        input_list = [1, 20, 3, 5, 7, 0]
        expected_list = [0, 1, 3, 5, 7, 20]
        self.assertTrue(insertion_sort(input_list) == expected_list)

    def test_insertion_sort_with_a_small_list_of_mixed_items(self):
        '''  '''
        input_list = [1.2, 20, "a", -5.23, 7, 0]
        self.assertRaises(TypeError, lambda: insertion_sort(input_list))

    def test_insertion_sort_with_a_random_list_of_ints(self):
        list_length = random.randrange(20,100)
        input_list = []
        for n in range(0, list_length):
            input_list.append(random.randrange(-1000,1000))
        self.assertTrue(insertion_sort(input_list) == sorted(input_list))

suite = unittest.TestLoader().loadTestsFromTestCase(TestInsertionSort)
unittest.TextTestRunner(verbosity=2).run(suite)
</pre> 
