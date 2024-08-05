---
source: https://neetcode.io/problems/find-minimum-in-rotated-sorted-array
difficulty: medium
tags:
---
###### Assumptions:
1. All elements are unique

**Rotated sorted array means:** (eg.)
original sorted array: [1,2,3,4,5,6]
rotated 1 time: [6,1,2,3,4,5]
rotated 4 times: [3,4,5,6,1,2]
rotated 6(=n) times: [1,2,3,4,5,6]

**Question:** Return the minimum element of the array

*O(n) solution is trivial:*
compare 1 element to the next element, when next element is smaller-return that next element.

*Find an O(log n) solution:*

- involves '[[binary search]]' algorithm, since the time taken to search for an element in binary search is `log n`.
