# Interview Prep Repo

This is a repository for my data science interview preparations. The primary item will be `README.md` (this document), which will contain my notes about various topics. 

## 1. General Data Structures & Algorithms

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

#### Recursion

##### Three Laws of Recursion: 

1. Recursive algo must have a base case
2. Recursive algo must change its state and move tword the base case
3. A recursive algo must call itself, recursively. 


#### Big-O notation: 

* Constant - `O(1)`
* Logarithmic - `O(log n)`
* Linear - `O(n)`
* Log Linear - `O(n log n)`
* Quadratic - `O(n^2)`
* Cubic - `O(n^3)`
* Exponential - `O(2^n)`

## 2. Python-specific Stuff

#### General

Assignment statements assign a name to an object.

#### Lists in Python

Python list is an object that contains an ordered collection of objects. 

Elements of the list are accessed using integer-valued indices. 

First element of a list is always index 0. Last element has index -1 and can index things backwards (-1, -2, -, …)

`len` provides number of elements in the list. 

Empty list is `[ ]`

Lists are heterogenous, meaning that the data objects need not all be from the same class 

##### List Methods: 

* `apppend`: adds a new item to the end of list. Modifies the list object. 
*`alist.insert(i, item)`: inserts an item at the ith position in list
*`alist.pop()`: removes and returns the last item in a list
*`alist.pop(i)`: removes and returns the ith element in a list
*`alist.sort()`:: modifies a list to be sorted. 
*`del alist[i]`: deletes the item in the ith position

##### List Performance

* Indexing & index assignment - `O(1)`
* Growing a list: 
	* with `append`: `O(1)`
	* with concatentation operator: `O(k)` where `k` is the size of the list being concatenated.  
* `pop()`: O(1)
* `pop(i)`: O(n)
* `insert(i, item)`: O(n)
* del operator: O(n)
* iteration: O(n)
* contains (`in`): O(n)
* get slice: O(k)
* `reverse`: O(n)
* concatenate: O(k)
* `sort`: O(n log n)

**Note**: Python lists implmeneted such that when an item is taken from the front of the list, all the other elements in the list are shifted one position closer to the beginning (hence O(n) cost of inserting/removing from the front or middle). This behaviour allows index operation to be O(1). 


#### Strings

Sequential collections of zero or more letters, numbers and other symbols. We call these letters, numbers, symbols *characters*

Strings are **immutable** => you cannot change a character in a string by using indexing and assignment (like you can with, e.g. a list)

#### Tuples

Very similar to lists in that they are heterogeneous sequeneces of data. The difference is that a tuple is immutable, like a string. A tuple cannot be changed (e.g. via indexing and assignment)

#### Sets

A set is an unordered collection of zero or more immutable Python data objects. Set do not allow duplicates and are written as comma-delimited values enclosed in curly braces. The empty set is represented by `set()`. Sets are heterogeneous, and the collection can be assigned to a varaible. 

#### Dictionaries

Unordered collection of associated pairs of items where each pair consists of a *key* and a *value*. 

We can manipulate a dictionary by accessing a value via its key or by adding another key-value pair. 

Dictionary is maintained in no particular order with respect ot the keys. 

##### Dict Operators: 

* `myDict[k]`: returns the value associated with `k`, or a `KeyError`
* `key in myDict`: returns `True` if key is in the dictionary, `False` otherwise. 
* `del myDict[k]`: removes the entry from the dict. 

##### Dict Methods: 

* `myDict.keys()`: returns the keys in the dictionary (as a list)
* `myDict.values()`: returns the values in the dictionary (as a list).
* `myDict.items()`: returns the key-value pairs (as a list of tuples)

##### Dictionary Performance

Get item and set item operations on a dictionary are `O(1)`. 

Checking whether a key is in the dictionary is also `O(1)`

Specifics: 
* copy: `O(n)`
* get item: `O(1)`
* set item: `O(1)`
* delete item: `O(1)`
* contains (in): `O(1)`
* iteration: `O(n)`


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

#### Python Resources: 

* [Interactive Python](http://interactivepython.org/)
	* [specifically](http://interactivepython.org/courselib/static/pythonds/index.html#) 


### 3. SQL 

#### SQL Notes: 

**UNION**:  List a number of SELECT statements separated by the UNION key word. 

#### Practice Problems

#### Question 1:

Three tables (schemas): 

```
Salesperson: ID | Name | Age | Salary

Customer: ID | Name | City | Industry_Type

Orders: Number | order_date | cust_id | salesperson_id | Amount
```

Find the following: 

a. The names of all salespeople that have an order with Samsonic. 

```
SELECT DISTINCT(Name) FROM
Salesperson JOIN Orders 
	ON (Salesperson.ID = Orders.salesperson_id)
JOIN Customer 
	ON (Orders.cust_id=Customer.ID)
WHERE Customer.Name = 'Samsonic' 
```

b. The names of all salespeople that do not have any order with Samsonic. 

```
SELECT Name FROM Salesperson
WHERE Name NOT IN (SELECT Name FROM
Salesperson JOIN Orders
	ON (Salesperson.ID = Orders.salesperson_id)
JOIN Customer
	ON (Orders.cust_id = Customer.ID)
WHERE Customer.Name = 'Samsonic)
```

c. The names of salespeople that have 2 or more orders. 

```
SELECT MAX(Name) FROM 
Salesperson LEFT JOIN Orders
	ON (Salesperson.ID = Orders.salesperson_id) 	AS ID
GROUP BY ID
HAVING COUNT(Orders.Number) >= 2
```

#### Question 2:

Two tables: 

**User** : user_id | name | phone_num
**UserHistory**: user_id | date | action

1.Write a SQL query that returns the name, phone number and most recent date for any user that has logged in over the last 30 days (you can tell a user has logged in if the action field in UserHistory is set to "logged_on").

Every time a user logs in a new row is inserted into the UserHistory table with user_id, current date and action (where action = "logged_on").

**Answer**: 

Users who have logged in over the last 30 days: 

```
SELECT user_id FROM UserHistory
WHERE action = "logged_on"
GROUP BY user_id
HAVING MAX(date) > TODAY - 30
```

Full Answer: 

```
SELECT name, phone_num, MAX(date) FROM 
User JOIN UserHistory ON (User.user_id=UserHistory.user_id)
WHERE user_id IN 

(SELECT user_id FROM UserHistory
WHERE action = "logged_on"
GROUP BY user_id
HAVING MAX(date) > TODAY - 30)

GROUP BY name, phone_num 
```

Better way: 

```
SELECT name, phone_num, MAX(date) 
FROM User JOIN UserHistory 	ON User.user_id=UserHistory.user_id)
WHERE UserHistory.action = "logged_on"
	AND UserHistory.date > (currdate() - 30)
GROUP BY name, phone_num
```

2.Write a SQL query to determine which user_ids in the User table are not contained in the UserHistory table (assume the UserHistory table has a subset of the user_ids in User table). Do not use the SQL MINUS statement. Note: the UserHistory table can have multiple entries for each user_id.

Note that your SQL should be compatible with MySQL 5.0, and avoid using subqueries.

With subqueries:

```
SELECT user_id FROM User 
WHERE user_id NOT IN (SELECT user_id FROM UserHistory)
```

Without subqueries: 

```
SELECT DISTINCT(user_id)
FROM User LEFT JOIN UserHistory ON (user_id = user_id)
GROUP BY user_id
HAVING COUNT(action) > 0
```

Or: 

```
SELECT DISTINCT(user_id)
FROM User LEFT JOIN UserHistory ON (user_id = user_id)
WHERE action is null
```

Following refer to this DB schema: 

**Apartments**: AptID | UnitNumber

**Buildings**: BuildingID | ComplexID

**Tenants**: TenantID | TenantName

**AptTenants**: TenantID | AptID

**Requests**: RequestID | Status | AptID | Description

Write a SQL query to get a list of tenants who are renting more than one apartment. 

```
SELECT MAX(TenantName)
FROM Tenants 
JOIN AptTenants
ON Tenants.TenantID = AptTenants.TenantID
GROUP BY Tenants.TenantID
HAVING COUNT(AptID) > 1
```


