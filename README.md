# Interview Prep Repo

This is a repository for my data science interview preparations. The primary item will be `README.md` (this document), which will contain my notes about various topics. 

## 1. General Data Structures & Programming

#### Hash Tables

A hash table is a data structure that maps keys to values for highly efficient lookup. In a very simple implementation of a hash table, the hash table has an underlying array and *hash function*. When you want to insert an object and its key, the hash function maps the key to an integer, which indicates the index in the array. The object is then stored at that index. 

* Hash value of all possible keys must be unique, or we might accidentally overwrite data. The array would have to be extrememly large--the size of all possible keys--to prevent such "hash collisions"
* Instead of making an extremely large array and stroing objects at index `hash(key)` we can make the array much smaller and store objects in a linked list at index `hash(key) % array_length`.
* Can also implement the hash table with a binary search tree. This gives `O(log n)` lookup time since we can keep the tree balanced. 

#### Array

* Simplest type of data structure (linear/one-dimensional array)
* Consists of elements each identified by at least one *array index* or *key*. 
* An array is stored so that the position of each element can be computed from its index tuple by a mathematical formulat. 
* An array of 10 32-bit integer variables, with indices 0 through 9, may be stored as 10 words at memory addresses 2000, 2004, 2008, … , 2036 so that the element with index *i* has the address 2000 + 4i
* **Efficiency**:
	* Both *store* and *select* take *constant time*.
	* No per-element overhead (though maybe some per-array overhead)
	
**Dynamic array**: a random access, variable-size list data strucutre that allows elements to be added or removed. 

#### Linked List

List data structure made up of nodes where each node stores some data and a pointer to the next node in the list. 

* **Benefit of Linked List**:
	* Elements can easily be inserted or removed without reallocation or reorganization of the entire structure because the data items need not be stored contiguously in memory or on disk.
	* Easy to implement linear data structures (stacks & queues)
* **Disadvantages of Linked Lists**:
	* Simple linked lists **do not allow random access to the data or any form of efficient indexing** (inherently sequential access)
		* Thus, many basic operations (finding last node of the list, finding a node with a given data, locating the place where a new node should be inserted) require scanning most or all of the list elements.  
	* Tendency to waste memory due to pointers requiring extra storage space

**Other Notes**:

Recursive solutions are common with linked list problems

Must understand whether you're discussing a linked list or a doubly linked list. 

#### Stacks & Queues

**Stack**: LIFO (last-in first-out) ordering. Like a stack of dinner plates, the most recent item added to the stack is the first item to be removed. 

* Essentially identical to a singly-linked list, except user is not allowed to "peek" below the top item. 

**Queue**: FIFO (first-in first-out) ordering. Like a line or queue at a ticket stand, items are removed from the data structure in the same order that they are added. Can be implemented witha  linked list, with new items added to the tail of the linked list. 

Both implement "pop" and "push" operations.

#### Trees & Graphs

**Things to watch out for**: 

* **Binary Tree vs. Binary Search Tree**: In a binary search tree, impose the condition that, for all nodes, the left children are *less than or equal to the current node* which is less than all the right nodes. 
* **Balanced vs. Unbalanced**: If tree is unbalanced, should describe algorithm in terms of both average and worst case time. Balancing only implies that the depth of subtrees will not vary by more than a certain amount. Does not guarantee that left and right are *identical* sizes. 

**Binary tree traversal**: Most typical is "in-order traversal" where we visit the left side, then the current node, then the right. 

##### Graph Traversal: 

**Depth-First Search (DFS)**: We visit a node `r` and then iterate through each of `r`'s adjacent nodes. When visiting a node `n` that is adjacent to `r`, we visit all of `n`'s adjacent nodes before going on to `r`'s other adjacent nodes. THat is `n` is exhastively searched before `r` moves on to searching its other children. 

*DFS Pseudo-code*: 

```
void search(Node root){
	if (root == null) return;
	visit(root);
	root.visited = true; 
	foreach (Node n in root.adjacent){
		if(n.visited == false){
			search(n);
		}
	}
}
```
[DFS in python](https://github.com/nyghtowl/Interview_Problems/blob/master/python/dfs.py)

**Breadth-First Search (BFS)**: Less intuitive. We visit each of a node `r`'s adjacent nodes before searching any of `r`'s "grandchildren". Iterative implementation with queue: 

```
void search (Node root){
	Queue queue = new Queue();
	root.visited = true;
	visit(root);
	queue.enqueue(root); // add to end of queue
	
	while(!queue.isEmpty()){
		Node r = queue.dequeue(); //Remove from fron t of queue
		foreach (Node n in r.adjacent){
			if (n.visited == false){
				visit(n);
				n.visited = true;
				queue.enqueue(n)
			}
		}
	}
}
```
Key thing to remember if asked to implement BFS: *USE A QUEUE!*

**Which to use:**

DFS is typically the easies if we want to visit every node in the graph, or atleast visit every node until we find whatever we're looking for. However, if we have a very large tree and want to be prepared to quit when we get too far from the original node, DFS can be problematic; we might search thousands of ancestors of the node but never even search all of the node's children. In these cases BFS is typically preferred. 



## 2. Python-specific Stuff

#### Yield & Generators

**yield** keyword: enables a function to come back where it eft off when it is called again. 
	* converts a function into a generator object that stores state. 

**generator**: similar to a function that returns an array, in that a generator has parameters, can be called, and generates a sequence of values. However, instead of building an array containing all the values and returning them all at once, a generator *yields* the values one at a time, which requires less memory and allows the caller to get started processing the first few values immediately. 

In short, a generator *looks like a function but behaves like an iterator*

Python mechanisms:

* **Generator Functions**: Coded as a normal `def` but use `yield` to return results one at a time, suspending and resuming. 

```
>>> def fibonacci(): 
	  Limit = 10
	  count = 0
	  a, b = 0,1
	  while True: 
	  	 yield a
		 a,b = b, a+b
		 if count == Limit: 
		 	 break
		 count+=1
>>> for n in fibonacci(): 
		print n

0 1 1 2 3 5 8 13 21 34 55

```


* **Generator Expressions**: Similar to list comprehensions, but return an object that produces results on demand instead of building a result list. 

`xrange` vs `range`: The `xrange()` function is very similar to `range()` in that it yields the same values as the corresponding list; difference is that it does this without storing them all simultaneously. Advantage of `xrange` is when iterating over a very large range on a memory-starved machine or when all of the range's elements are never used. 






