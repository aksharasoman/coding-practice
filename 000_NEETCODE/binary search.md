- input : sorted array
- think the scenario: finding a word in a dictionary
	- open the middle of dictionary
	- go to half of either side
	- if overshoot, go to half of appr. side half-side.
- needs 3 indices: lower, higher and middle (mid =floor(l+h)/2)
###### Algorithm:
- loop while (l<h):
	- mid = floor( (l + h) /2 )
	- check if key == arr[mid]
		- return mid
	- else if key < arr[mid]
		- h = mid - 1; (search in left hand side)
	- else:
		- l = mid + 1; (search in right hand side)
- return "key not found" (when l>=h)

**Analysis:** O(log n) :: draw a traversing tree
worst case: takes log n comparisons to find an element. 
best case: O(1) key being the middle element