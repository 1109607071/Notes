# DSAA 算法笔记

[toc]



# DSAA Part



##### [网课地址](http://mooc1.chaoxing.com/course/214395507.html)

leetcode: some questions for you to practise







## 一. 算法





### Recursive Technique

---

#### 1. Iteration (迭代)

* When we encounter a problem that requires repetition, we often use iteration, i.e., some type of loop



#### 2. Recursion

* An alternative approach to problems that require repetition is to solve them using recursion.
* A recursive method is a method that calls *itself*.
* Need a base case to stop the recursion.

```java
public static void printSeries(int n1, int n2){
    if (n1 == n2){
        // base case
        System.out.printl(n2);
    } else {
        System.out.printl(n1 + ",");
        printSeries(n1 + 1, n2);
    }
}
```

* Template of a Recursive Method

``` pseudocode
recursiveMethod(parameters) {
    if (stopping condition) {
        // handle the base case
    } else {
        // recursive case
        // possibly do something here
        recursiveMethod(Modified parameters); // brings us closer to the base case
        // possibly do something here
    }
}
```







### Search Algorithms



#### Binary Search Algorithm

```pseudocode
for an sorted Array[n]
left = 0, right = n-1
repeat
    mid = (left + right)/2
    if (t = A[mid]) then
        return TRUE
    else if (t < A[mid]) then
        right = mid - 1
    else 
        left = mid + 1
until left > right

return FALSE
```

* When the mid pointer approches from the right: 

```pseudocode
for an sorted Array[n]
left = 0, right = n-1
while left < right
    mid = (left + right)/2 // floor of (left + right)/2
    if t > A[mid]
        left = mid + 1
    else // if t <= A[mid]
        right = mid
return right
```







### Sort Algorithms



#### Selection Sort

In the __Selection Sort__, we find the smallest number in every travel then put it in the first place. Pointer `i` marks the first place of this travel, pointer `j` will traverse the right part and compare to the value of register `k`.

> It is like the way you sorted the pokers – find the right one and select it to your hand cards.

```pseudocode
for int i = 0 to n-2 // we don't need to sort the last one
    k = i
    for integer j = i+1 to n-1
        if A[k] > A[j] then
            k = j
        swap A[i] and A[j]
```





#### Insersion Sort

In the __Insersion Sort__, there are _"sorted"_ part and _"not sorted"_ part, divided by pointer `i`. When a new number enters the _"sorted"_ list, it uses `swap()` to bubble-like move to the correct place.

> This is like _"divide but not conqurer"_, divide but only sort one side

```pseudocode
for int i = 0 to n-1
    for int j = i to 1 (j-1 >= 0)
        if A[j-1] > A[j] then
            swap A[j-1] and A[j]
```





#### Merge Sort

> _"Devide and Conquer"_

1. Merge-Sort($A, n$)

In Merge-Sort(), we divide the array until every single subarray only contains 1 element.

```pseudocode
for an unsorted Array[n]
if n > 1
    p = floor of n/2
    B[from 0 to p-1] = A[from 0 to p-1]
    C[from 0 to n-p-1] = A[from p to n-1]
    Merge-Sort(B, p)
    Merge-Sort(C, n-p)
    A[from 0 to n-1] = Merge(B, p, C, n-p)
```



2. Merge($L,\ n_L,\ R,\ n_R$)

In Merge(), we merge 2 sorted arrays to one sorted array.

```pseudocode
for 2 arrays L[nl] and R[nr]
n = nl + nr
let A[n] be a new array
i = 0; j = 0
for k = 0 to n-1
    if i <= nl and (j > nr or L[i] <= R[j])
        A[k] = L[i]; i++
    else
        A[k] = R[j]; j++
return A
```



- However, the algorithem mentioned above needs lots of memory, the stack will easily blow.



- a better way

1. Merge-Sort(Lines[], Lines_[], lo, hi)

```pseudocode
if lo < hi
    mid = floor of (lo+hi)/2
    Merge-Sort(Lines[], Lines_[], count, lo, mid)
    Merge-Sort(Lines[], Lines_[], mid+1, hi)
    Merge(Lines[], Lines_[], lo, mid, hi)
```



2. Merge(Lines[], Lines_[], lo, mid, hi)
    - merge two adjacent part in array A[from p to q] ($p\le q$) and A[from q+1 to r]
    - use A_[] to store the sorted array, this is clerified only once, could save a lot of memory

```pseudocode
n1 = mid - lo + 1 //from lo to mid
n2 = hi - mid     //from mid+1 to hi
i = lo, j = mid+1
for k = lo to hi
    if i <= mid and (Lines[i]->left <= Lines[j]->left or j > hi)
        Lines_[k] = Lines[i]
        i++
    else 
        Lines_[k] = Lines[j]
        j++
        // count += (j - k) if needs to count Bubble-sort times
Lines[lo -> hi] = Lines_[lo -> hi]
```





#### Quick Sort

> _"Let random do the job."_

- Quicksort( A[n], lo = 0, hi = n-1 )

Input an array A[n], divide it into two random part.

```pseudocode
p = partition(A, lo, hi)
Quicksort(A, lo, p-1)
Quicksort(A, p+1, hi)
// pth is sorted
```


- partition( A, lo, hi )

We sort the array in this method.

```pseudocode
p = RANDOM(lo, hi)
pivot = A[p]
L = lo, R = hi
for int i from lo to hi
    if (i != p) 
        if (A[i] < pivot) A_[L++] = A[i]
        else A_[R--] = A[i]
A_[L] = pivot
A[from lo to hi] = A_
return L
```







### Linked List (链表)



#### Definition

A linked list stores a sequence of a elements in separate nodes. Each node contains:

- a single item,
- a "link" to the node containing the next item,
- The last node in the linked list has a link value of "NULL",
- Teh linked list as a whole is reqresented by a variable that hold a regerence to the first node.



#### Features

**advantage**

- It can grow without limit (not fixed length)

- Easy to insert/delete an element

**disadvantage**

- Hard to find a element (it depends), which means it needs to "walk down" the list to access an item

- Do not provide random access

- The links take up additional memory

- Not compact (in terms of Memory)

**comparing to Array**

- advantage

    - **random access** (able to access a location in constant time)

    - very compact (in terms of Memory)

- disadvantage

    - have to specify an initial array size

    - Resize an array is possible, but not easy

    - Difficult to insert/delete elements at arbitrary positions

        needs to move all the elements behind



#### Basic Operators

```pseudocode
q <- p
    q will point to the node p points to

q <- next of p
    q will point to the next node of the node p points to

p <- next of p
    p will point to the next node 

next of q <- p
    (1) p,q are pointing to different linked lists
        q's next, which is a pointer, points to p
    (2) p,q are pointing to the same linked list
        q's next, points to p, jumps through the nodes between

next of q <- next of p

```

#### Linked List in C++

```c++
struct node {
    int data;
    node* next;
};
```



#### Pseudocode

##### Traverse a Linked List

traverse(A)

```pseudocode
if (A = NULL)
    retrun
else 
    print A.value
    traverse(A.next)
```

traverseIteration(A)

```pseudocode
node trav <- A
while (trav != NULL)
    print trav.value
    trav <- trav.next
```

> Using Iteration is **BETTER**, why?



##### Insert an Item

> insert node q in Linked List A at Position i (from 1 on)

insertNode( A, node q, i )

```pseudocode
a <- 0, node p <- A,
while (a < i-1)
    p <- p.next
    a++
tmp <- p.next
p.next <- q
q.next <- tmp
return A
```

- Time Complexity: **O(n)**
- Space Complexity: **O(1)**



##### Deleting an Item

> delete node in Linked List A at Position i

deleteNode( A, i )

```pseudocode
a <- 0, node p <- A
while (a < i-1)
    p <- p.next
    a++
p.next <- p.next.next
return A
```

- Time Complexity: **O(n)**
- Space Complexity: **O(1)**



##### Find an Item

findValue( A, x )

```pseudocode
node p <- A,
while (p != NULL)
    if (x = p.value)
        return p
    p <- p.next
return -1
```

- Time Complexity: **O(n)**
- Space Complexity: **O(1)**



##### Updating an Item

> Update nodes with value x to y in Linked List A

updateNodes( A, x, y )

```pseudocode
node p <- A,
while (p != NULL)
    if (x = p.value)
        p.value <- y
    p <- p.next
return A
```

- Time Complexity: **O(n)**
- Space Complexity: **O(1)**



#### Operators on Polynomials

- **Polynomials**: $p(x) = p_0+p_1x+p_2x^2+p_3x^3+\cdots +p_nx^n$

- < p~i~ , i > : *p~i~ is the coefficient* and *i is the exponent*

- We use linked list to store the < p~i~ , i > pairs of p(x)
- Without loss of generality, we skip all nodes w/p~i~ = 0
- Node representation

``` c++
node polyItem{
    float coef; // record pi
    int expo;   // record exponent
    node next;  // reference to next polyItem
}
```



#### Other variants of Linked List

- Double linked list
    - add a **prev** regerence to each node: regers to the previous node
    - allow us to "back up" from a given node
- Circular linked list
    - empty list: head.next = head?
    - end of list: tmp.next = head?





### Stack (堆)

#### Definition

- First In Last Out (**FILO**)

#### Implementation of Stack

- using an array
```pseudocode
array A[n], Max_Size=n
int i = 0, top = -1

push(item)
    if i < n
        A[i++] = item

pop(j)
    if i >= 0
        // delete A[i] (not necessary)
        // return A[i--]
        i--
```

#### Application

- Making sure the delimiters (parens, brackets, etc.) are balanced:
    - deleting all the numbers and operators, only match the delimiters
    - Read the delimiters from left to right, push all the left delimiter into a stack, then match() when read a right delimiter, delet the left one if it match, return false if it fails
- Evaluating arithmetic expressions:
    - 双目，三目
    - Postfix Expression (后缀操作符):
        - (left, right, operation)
        - for an equation: `a+y*z`, write it as `a y z * +`
        - `5*((9+3)*(4*2)+7)` --> `5 9 3 + 4 2 * * 7 + *`
    - Parse postfix expression is somewhat easier problem than directly parsing infix (cause it has no dilimiters)
    - Postfix Expression Evaluation:
        - Convert from infix to postfix
        - Evaluate a postfix expression
- The runtime stack in memory





### Queue (队列)

#### Definition

- A queue is a sequence in which:
    - items are added a the rear and removed from the front
    - You can only access the item that is currently at the front
- Queue Analogy

- First In First Out (**FIFO**)

#### Operations

- enQueue: add an item at the rear of the queue
- deQueue: remove an item from the front of the queue
- isEmpty
- isFull
- clear
- size

#### Implementation of Queue

- Array based Queue:
    - Max_Size = n
    - front = 0
    - rear = 0 // the current rear
    - Array S with n elements

```pseudocode
enQueue(item)
    if (rear < Max_Size)
        A[rear] = item
        rear++
    else
        print "FULL"

deQueue()
    if (front < rear)
        front++
    else
        print "Empty"
```

$\uparrow$ Waste a lot of space!

- Ring Queue
    - leave a special flag before front,
    - isEmpty: `rear == front`
    - isFull: `(rear+1) %n == front`

- Stack VS. Queue

|                     | **Stack**               | **Queue**    |
| :---|:--:|:--:|
| **In-Out**          | FILO                     | FIFO          |
| **Application**     | function funtime         | OS scheduling  |
| **Operations**      | push  pop                | enQueue, deQueue|
| **Ops Time Complexity** | O(1)                     | O(1)         |
| **Implementation**  | Array-base, Linked-based | Array-based, Linked-based |







### String Matching

#### Concepts

String:
* Sequence of characters over some alphabet
* Binary {0, 1}: S1 = "10000101101010011010"
* English Characters {a...z, A...Z}: S2 = "Hello World"

#### String Operators

* append  : append to string
* assign    : assign content to string
* insert     : insert to string
* erase      : erase characters from string
* replace   : replace portion of string
* swap       : swap string values
* find         : find the specific char in the string

#### String Searching

* Parameter:
    * n: # of characters in text
    * m: # of characters in pattern
    * Typically, **n >> m**
        * e.g., n = 1 Billion, m = 100

#### Brute Force

Check for pattern starting at every text position

```pseudocode
BruteForce(T, P)
    n <- len(T), m <- len(P)
    for i = 0 to n-1
        for j = 0 to m-1
            if P[j] != T[i+j] then
                break;
            if j = m-1
                pattern occurs with shift i
```

* $O(m\cdot n)$
* can be slow when strings repeats themselves
    * Pre-analyze search pattern:
        * If t[0...3] matches p[0...3] then t[1...3] matches p[0...2]
        * No need to check i = 1, j = 0, 1, 2;

Or match $\ge$ 2 different chars from the beginning of the Pattern together.

#### Rabin-Karp Algorithm

* General idea:
    * Convert search pattern to a number p
    * Convert search text to an array of numbers t[0], ..., t[n-m-1]
    * Compare p with t[i], for each i in [0, n-m-1]
    * if p = t[i], pattern p occrus
* How to convert size-m characters to a num?
    * e.g. the alphabet $\sum = \{a,...,z,A,\dots,Z\}$, or even chinese characters
    * ASCII, UTF-8 …
    * Solution: radix-d (d = $|\sum |$) Horner's rule (base)
    * **p = P[m-1] + d( P[m-2] + d( P[m-3] + … + d( P[1] + dP(0) ) ) ) )**
* When `m` is large, `p` may be too large to work
    * modulo a proper prime number `q`
    * **p = P[m-1] + d( P[m-2] + d( P[m-3] + … + d( P[1] + dP(0) ) ) ) ) mod q**
* Compute t[0], t[1], …, t[n-m-1] in time $O(n-m)$
    * Compute t[i+1] by using t[i] in $O(1)$ time
    * **t[i+1] = d( t[i] - d^m-1^T[i] ) + T[i+m]**
    * **t[i+1] = d( ( t[i] - hT[i] ) + T[i+m] ) mod q**, where $h\equiv d^{m-1}\ (mod\ q)$
    * **t[0] -> t[1] -> t[2] -> t[3] -> … -> t[n-m-1] in $O(n-m)$**
* Correctness analysis
    * Additional test to check
        * **P[0, …, m-1] = T[i, …, i+m-1]**

```pseudocode
Rabin-Karp(T, P, d, q):
    n = lengthof(T), m = lengthof(P)
    h = d^(m-1) mod q, p = 0, t[n-m]
    for j = 0 to m-1
        p = (d * p + P[j]) mod q,        // P[j]: jth element of P
        t[0] = (d * t[0] + T[j]) mod q,  // T[j]: jth element of T
    for i = 0 to n-m-1
        if p != t[i] then
            t[i+1] = (d * (t[i] - T[i]*h) + T[i+m]) mod q
        else
            if P[0, ..., m-1] = T[i, ..., i+m-1]
                pattern occurs with shift I
            else
                t[i+1] = (d * (ti - T[i]*h) + T[i+m]) mod q
```



#### Finite State Automata (FSA)

* A finite State automaton is defined by:
    * $\mathbb{Q}$ , a set of states
    * $q_0\in Q$ , the start state
    * $A \in Q$ , the accepting states
    * $\sum$ , the input alphabet
    * $ $, the transition function, from $Q \mul \sum$ to Q

|       | 0     | 1     |
| :---: | :---: | :---: |
| **a** | 1     | 2     |
| **b** | 0     | 0     |



##### FSA idea for String Matching

* Start in state q~0~
* Perform a trasition from q~0~ to q~1~ if next character of T = P[1]
* State q~i~ means first `i` characters of P match.
* Transition from q~i~ to q~i+1~ if the next character of T = P[i+1]

* Search Pattern
* i^th^ situation: the pattern has passes `i` tests

| #               | 1^th^ | 2^th^ | 3^th^ | 4^th^ | 5^th^ | 6^th^ |
| --------------- | ----- | ----- | ----- | ----- | ----- | ----- |
| Search Pattern: | a     | a     | b     | a     | a     | a     |
* map (6 is return value)
|       |  0   |  1   |  2   |  3   |  4   |  5   |
| :---: | :--: | :--: | :--: | :--: | :--: | :--: |
| **a** |  1   |  2   | *?*  |  4   |  5   |  6   |
| **b** | *?*  | *?*  |  3   | *?*  | *?*  | *?*  |

* How to fill these ???
    * `?` == 0 --> brute force
    * It actually depends on the Search Pattern itself ...
    * '?' represents the maximum public length at `i`

* FSA construction
    * FSA builds itself
* Example: Build FSA for `aabaaabb`

|       | 0    | 1    | 2    | 3    | 4    | 5    | 6    | 7         |
| :---: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--:      |
| **a** | *1*  | *2*  | 2    | *4*  | *5*  | *6*  | 2    | 4         |
| **b** | 0    | 0    | *3*  | 0    | 0    | 3    | *7*  | *8* wins! |

##### Pseudocode

* **NextArray(P):**

```pseudocode
m <- len(P)
Let next[0, ..., m-1] be a new array
next[0] = -1, k <- 0, j <- 1
while j < m
    if P[j] = P[k]       // char match
        k <- k + 1
        next[j] = k
        j <- j + 1
    else if k = 0        // the first mismatch
        next[j] <- 0, j <- j + 1
    else                 // char mismatch
        k <- next[k-1]
return next
```

* **KMP(T, P):**

```pseudocode
n <- len(T), m <- len(P)
next <- NextArray(P)
i <- 0, j <- 0
while (i < n && j < m)
    if (P[j] = T[i] && j = m-1)
        pattern occurs with shift i-m
    else if (P[j] = T[i])
        i <- i + 1, j <- j + 1
    else
        j <- next[j-1]
        if j = 0
            i <- i + 1
```







### Knuth-Morris-Pratt (KMP)

* Resolved theoretical and practical problem

* In general, the FSA is constructed so that the state number tells us how much of a prefix of P has been matched.
* FSA trasition function:
    * 1) Find the longest prefix of P is also a suffix of T[..., i], denote as k, i.e., P[1, ..., k] = T[i-k+1, ..., i]
    * 2) Read the next character at "k+1", there are two kinds of transitions:
        * P[k+1] = T[i+1], it is matched, continues.
        * Otherwise, it is mismatched, go to (k, T[m+1])







### Tree

This lecture provides a formal definition of ***trees***, which constitute an important approach to organize data in computer science. We will also prove some basic properties of trees that will be useful in computer sci  ence.

#### General

##### Motivation

* Data Dictionary: maintain a sorted collection of data
    * search for an item (with possibly delete it)
    * insert a new item
* A list implemented using an array
    * Searching for an item, $O(\log n)$ *(binary search)*
    * Inserting an item, $O(n)$
* A list implemented using an linked list.
    * Searching for an item, $O(n)$
    * Inserting an item, $O(n)$
* In practice, Inserting an item using a linked list is much faster than array. Since the copy operation is slow.
In the next few lectures, we will look at data structures (trees, hash tables) that can be used for a more efficient data dictionary.


##### What is a tree?
* A tree consist of:
    * A set of nodes,
    * A set of edges, each of which connects a pair of nodes.
* Each node may have one or more data items:
    * **Key field**(键值) = the field used when searching for a data item
    * Multiple data items with the same key are referred to as **duplicates**.
* The node at the "top" of the tree is the "root" of the tree.
* We only talk about "rooted tree" in this course.


##### Tree Property 1
* A tree with `n` nodes has `n-1` edges.
* Proof:
    + Each tree node has a parent, and an edge which links it to its parent, except for the *root node*.

##### Relationship between nodes

* **Parent**
    * Consider a tree T, let u and v be two nodes in T. We say that `u` is the *parent* of `v` if `v` is the node directly below `u`.
    * Accordingly we say that `v` is a child of `u`.
    * Each node is the child of at most one parent.
    * Node with the same parent are `siblings`.

* **Ancestor** and **descendant node**
    * We say that `u` is an ancestor of `v` if one of the following holds:
        + u = v
        + u is the parent of v, or
        + u is the parent of an ancestor of v.
    * Accordingly, we say that `v` is a **descendant** of u. (including `u` itself!)
    * In particular, if `u != v`, `u` is a proper ancestor of `v`, and likewise, `v` is a proper descendant of u.

* **leaf node** and **Internal node**
    * A *leaf* node is a node without children.
    * An internal node is a node with one or more children. (including root)

##### Tree Property 2

* Let T be a tree where every internal node has at least 2 child nodes. If `m` is the number of leaf nodes, then the number of internal nodes is at most `m-1`
* Proof:
    + 


##### Tree: a Recursive Data Structure
* Each node in the tree is the root of a smaller tree!
    * Refer to such trees as **subtrees** to distinguish them from the tree as a whole
* Tree algorithms often lend themselves to recursive *implementations*.



##### Path, Depth, Level, and Height
* There is exactly one path (one sequence of edges) connecting each node to the root.
* **depth** of a node = # of edges on the path from it to the root. (# of parents - 1)
* Nodes with the same depth form a level of the tree
* The height of a tree is the maximum depth of its nodes.

#### k-ary and Binary Tree

* A **k-ary (k叉树)** is a rooted where every internal node has at most `k` child nodes.
* A 2-ary trees is called a binary tree
* In a binary tree, nodes have at most two children:
    + Distinguish between them using the direction left and right.


#### Binary Tree Definition
* Binary tree recursive definition:
* A binary tree is either:
    * 1) empty or
    * 2) a node (the root of the tree) that has:
        + one or more pieces of data (the key, and possibly others)
        + a *left subtree*, which is itself a binary tree
        + a *right subtree*, which is it self a binary tree
* A binary tree implies an ordering among the nodes at the same level.


##### Binary Tree: Full Level(满层)
* Consider a binary tree with height `h`, its level `l`($0\le l\le h$) is full if it contains 2^l^ nodes.


##### Binary Tree: Complete Binary Tree
* A binary tree of height `h` is complete if:
    * Level 0, 1, ... , h-1 are full
    * At level h, the leaf nodes are "*as far left as possible*"
        + This means that if you want to add a leaf node `v` at level `h`, `v` would need to be on the right of all the existing left nodes.

#### Tree Property 3
* A complete binary tree with $n\ge 2$ nodes has height $O(\log n)$
* Proof:
    * level 0 to h-1 is full:
        * level i: 2^i^
    * level h: x nodes;
    * thus, $2^0 + 2^1 + 2^2 + ... + 2^{h-1} + x = n \\ \rightarrow (1-2^{h-1})/(1-2) = n-x \rightarrow 2^{h-1} \ge n$
    * thus, $h = O(\log n)$

#### Binary Tree Implementation
```cpp
struct treeNode {
    int key;
    treeNode left;
    treeNode right;
};
```

```cpp
void BTreeCreate() {
    int lastParentIndex = (N+1 >> 1) - 1;     // N+1 ceil
    for (int i = 0; i < lastParentIndex; i++) {
        nodelist[i]->left = nodelist[i*2 + 1];
        nodelist[i]->right = nodelist[i*2 + 2];
    }
    nodelist[lastParentIndex]->left = nodelist[lastParentIndex*2 + 1];
    if (N % 2 == 1)
        nodelist[lastParentIndex]->right = nodelist[lastParentIndex*2 + 2];
}

void ascendingPrint(node *root) {
    printf("%d ", root->val);
    if (root->left != NULL) {
        if (root->right != NULL) {
            if (root->left->val < root->right->val) {
                ascendingPrint(root->left);
                ascendingPrint(root->right);
            } else {
                ascendingPrint(root->right);
                ascendingPrint(root->left);
            }
        } else
            ascendingPrint(root->left);
    } else if (root->right != NULL)
            ascendingPrint(root->right);
}
```



##### Traversing a Binary Tree
* Traversing a tree involves visiting all of the nodes in the tree:
    * Visiting a node = processing its data in some way, e.g., print the key val
* We will look at four types of traversals. Each of them visits the nodes in a different order.
* To understand traversals, ti helps to remember the recursive definition of a binary tree, in which every node is the root of a subtree.

* Preorder Traversal(前序遍历)
    + visit the root N
    + Recursively perform a preorder traversal of N's left subtree
    + Recursively perform a preorder traversla of N's right subtree

```pseudocode
preorderprint(treeNode root):
    print(root)
    if (root->left != NULL)
        preorderprint(root->left)
    if (root->right != NULL)
        preorderprint(root->right)
```

* Postorder Traversal(后序遍历)
    + Recursively perform a preorder traversal of N's left subtree
    + Recursively perform a preorder traversla of N's right subtree
    + visit the root N

* Inorder Traversal(中序遍历)
    + Recursively perform a preorder traversal of N's left subtree
    + visit the root N
    + Recursively perform a preorder traversla of N's right subtree

* Binary Tree & postfix
    * root -> operator
    * leaf -> numbers
    * Use the Postorder Traversal to construct the postfix. (Interesting)



#### Character encoding (字符编码)
Unicode(16 bits) / ASCII(8 bits) / UTF / UTF64
a character -> a number, (fixed length)

**Huffman Encoding**
* Huffman encoding is a type of variable-length encoding that is based on the actual character frequencies in a given document.
* Huffman encoding uses a binary tree:
    * to determine the encoding of each character
    * to decode an encoded file
* the root is the total frequency
* left and right doesn't matter

* Building a Huffman tree
    * 1) Begin by reading through the text to determine the frequencies
    * 2) Create a list of nodes that contain (character, frequency) pairs for each character that appears in text
    * 3) Remove and “merge” the nodes with the two lowest frequencies, forming a new node that is their parent, that the root is an add of their frequencies.
    * 4) Add the parent to the list of nodes
    * 5) Repeat steps 3) and 4) until there is only a single node in the list, which will be the root of the Huffman tree.

* Correctness of Huffman tree
    + Given a Huffman tree, it includes at least 2 nodes, assume node u and node v have the top-2 lowest frequencies, then
        + 1) node u and v have the same parent node
        + 2) depth(u) and depth(v) >= depth(x), where node x is any leaf node in the Huffman tree.
            + proof?
    + Huffman encoding is the optimum prefix code, i.e., the space cost is minimized.
        + Proof.
        + [Reading Materials](http://home.cse.ust.hk/faculty/golin/COMP271Sp03/Notes/MyL17.pdf)





### Advanced Binary Tree

#### Priority Queue (binary heap)

* A priority queue stores a set S of n integers and supports the following operations:
    * `Insert(e)`: adds a new integer to S
    * `Delete-min`: removes the smallest integer in S, and returns it.
>Applications:

	* Artificial intelligence (A* algorithm)
	* Operating systems (load balancing)
	* Graph searching (Shortest path algorithms)

- elements always leave in ascending order(min-heap) or descending order(max-heap). 



**Implementation**

* Use "binary heap" to achieve the following **Time & Space Complexity**:
  * $O(n)$ **space Complexity**                         –> array
  * $O(\log n)$ **insertion time complexity**
  * $O(\log n)$ **delete-min time complexity**
* The binary heap data structure is an array object that we can view as *a complete binary tree*.



**Binary Heap**

* Let S be a set of n integers. A binary heap on S is a binary tree T satisfying:
    + (1) T is complete
    + (2) Every node u in T corresponds to a distinct integer in S, the integer is called the key of u (and is stored at u)
    + (3) If u is an internal node, the key of u is smaller than those of its child nodes
* Note that:
    + Condition 2 implies that T has n nodes
    + Condition 3 implies that the key of u is the smallest in the subtree of u

**Binary Heap Insertion**

* We perform `insert(e)` on a binary heap T as follows:
    * Step 1: Create a leaf `node z` with `key e`, while ensuring that T is a complete binary tree, it means there is only one place where z could be added.
    * Step 2: Set u <- z
    * Step 3: If u is the root, return.
    * Step 4: If the key of u > the key of its parent p, return
    * Step 5: Otherwise, swap the keys of u and p. Set u <- p, and repeat from Step 3.
```pseudocode
insert(e):
    new node z, z->key = e;
    Tree T.add(z)            // complete binary tree
    u = z
    while (u->key < u->parent->key)
        swap(u->key, u->parent->key)
        u = u->parent
        if (u == root) return;
```






**Binary Heap Delete-min**

* We perform delete-min on a binary heap T as follows:
    + Step 1: Report the key of the root
    + Step 2: Identify the rightmost leaf z at the bottom level of T
    + Step 3: Delete z, and store the key of z at the root
    + Step 4: Set u <- the root
    + Step 5: If u is leaf, return
    + Step 6: If the key of u < the keys of the children of u, return
    + Step 7: Otherwise, let v be the child of u with a smaller key Swap the keys of u and v. Set u <- v, and repeat from Step 5
```pseudocode
delete_min():
    print root->key
    node z = rightmost leaf
    swap(z->key, root->key)
    Tree T.delete(z)
    u = root
    while (u->key > u->leftchild->key || u->key > u->rightchild->key)
        if (u->key > u->leftchild->key)
            swap(u->key, u->leftchild->key)
            u = u->leftchild
        else
            swap(u->key, u->rightchild->key)
            u = u->rightchild
        if (u->leftchild == NULL && u->rightchild == NULL) return
```
if we can find the rightmost leaf in $O(1)$, then the algorithm complexity is $O(\log n)$

* Find the rightmost Leaf Example
    * Here n = 9, binary form: 1001
    * Skip the first bit ‘1’ 
    * We scan the remaining bits
    * Start from root node 1.
    * The 2nd leftmost bit is 0, so we visit left, and go to node 15
    * The 3rd leftmost bit is 0, so we visit left, and go to node 39
    * The 4th leftmost bit is 1, so we turn right, and go to node 79 (done).



#### Binary Heaps in Dynamic Arrays

* a way to implement  **O(n)** build a n-integer Heap.
* a "pointerless" way to implement a binary heap, which in practive achieves much lower space consumption.



* Sorting a complete binary tree using an array
* Let us store the linearized sequence of nodes in an array A of length n.

$$
\vdots \\ parent\ {A[n>>1]} \\ \downarrow \\ A[n] \\ \downarrow \qquad \downarrow \\ child \quad A[n*2] \ \ A[n*2+1] \\ \vdots \qquad \qquad \vdots
$$

* Suppose that node u of T is stored at A[i]. Then, the left child of u is stored at A[2i], and the right child at A[2i+1]. t node u of T is stored at A[i]. Then, the parent of u is stored at A[i>>1].



##### Root-fix operator

* We are given a complete binary tree T with root r. It guaranteed that:
    * The left subtree of r is binary heap
    * The right subtree of r is a binary heap
    * However, the key of r may not be smaller than the keys of its children. 
* We define the root-fix operation, it fixes the issue and makes T a binary heap.
* Root-fix can be done in **O(log n)** time — since the tree hight h = log n.

The way to do this:
```pseudocode
For each i=n downto 1:
    Perform root-fix on the subtree of T rooted at A[i]
```

* Time Complexity: **O(n)**
* Proof as follows:
    * view `A` as a complete binary tree
    * The hight of `T` is `h = log n`
    * Without loss of generality, assume that all the levels of `T` are full, i.e., $n = 2^{h+1} - 1$.
    * total running time:

        $C(n)=\displaystyle\sum_{h=0}^{h=\log n}(node\ num)(hight-current\ hight) \\ \qquad =\displaystyle\sum ^h_{i=0}i\cdot 2^{h-i} \\ \qquad =2^0\cdot h+2^1\cdot (h-1)+2^2\cdot (h-2)+...+2^{h-2}\cdot 2+2^{h-1}\cdot 1+2^h\cdot 0 \\ \qquad = $
    * Proof $C(n) = O(n)$ with $n=2^{h+1}-1$





#### Binary nary Search Tree (BST)

Binary Search Tree (especially, balanced BST) is the most powerful data structure of this course. This is without a doubt one of the most important data structures in computer science.

In extreme case, BST is equivalent to a linked list, thus, we guarantee the operations performance of BST by studying AVL-tree.



##### Dynamic Predecessor Search

Let S be a set of integers. We want to store S in a data structure to support the following operations: 
+ The predecessor query( ): give an integer q, find its predecessor in S, which is the largest integer in S that does not exceed q.
+ Insertion( ): adds a new integer to S
+ Deletion( ): removes a integer from S



What is a predecessor? 

* Suppose that S={3,10,15,20,30,40,60,73,80}
    + The predecessor of 23 is 20
    + The predecessor of 15 is 15
    + The predecessor of 2 does not exist




A following BST garenteed:

* **O(n)** space consumption

* **O(h)** time per predecessor query

* **O(h)** time per insertion

* **O(h)** time per deletion

    where n = |S|, h is the height of BST

Note that all the above complexities hold in the worst case. that a predecessor query is more general than a “dictionary look-up”.



A BST on a set S of n integers in a binary tree T satisfying all the following requirements:
+ T has n nodes
+ Each node u in T stores a distinct integer in S, which is called the key of u
+ For every internal u, it holds that:
    + The key of u is larger than all the keys in the left subtree of u.
    + The key of u is smaller than all the keys in the right subtree of u.



###### Predecessor Query

Suppose that we have created a BST T on a set S of n integers. A predecessor query with search value q can be answered by descending a single root-to-leaf path:
+ (1) Set p $\leftarrow$ −∞ (p will contain the final answer at the end)
+ (2) Set u $\leftarrow$ the root of T
+ (3) If u = nil, then return p
+ (4) If key of u = q, the set p to q, and return p
+ (5) If key of u > q, then set u to the left child (now u = nil if there is no left child), and repeat from Step (3)

* (6) Otherwise, set p to the key of u and u to the right child, and repeat from Step (3)



###### BST Insertion

Suppose that we need to insert a new integer e. First create a new leaf z storing the key e. This can be done by descending a root-to-leaf path:
+ 1. Set u $\leftarrow$ the root of T
+ 2. If e < the key of u
    + 2.1 If u has a left child, then set u to the left child
    + 2.2 Otherwise, make z the left child of u, and done
+ 3. Otherwise:
    + 3.1 If u has a right child, then set u to the right child
    + 3.2 Otherwise, make z the right child of u, and done.
+ Repeat from Step 2.

The total cost is proportional to the height of T, i.e., O(h)



###### BST Deletion

* Suppose that we want to delete an integer e. Frist, find the node u whose key equals to e in **O(h)** time (through a predecessor query). Then:
    * Case 1: if u is a leaf node, simply remove it from T
    * Case 2: if u has a right subtree:
        + Find the node v storing the successor s of e.
        + Set the key of u to s
    * Case 2.1: if v is a leaf node, then remove it from T
    * Case 2.2: otherwise, it must hold that v has a right child w, but not left child. Replace node v by subtree which rooted at w.
    * Case 3: if u has no right subtree:
        + It must hold that u has a left child v, Replace node u by the subtree rooted at v.

C++ code: [AVL tree](https://www.luogu.com.cn/problem/solution/P3369?page=3)





### Graph

#### Graph Concepts

* Undirected Graph:

    * An undirected graph is a pair of (V, E) where:
        * V is a set of elements, each of which called a node
        * E is a set of **unordered** pairs {u, v} such that u and v are nodes

    * A node may alse be called a vertex(顶点). We will refer to V as the vertex set or the node set of graph, and E the edge set


* Differeces between graph, image, figure: [CSDN](https://blog.csdn.net/u010584319/article/details/82704889)



TODO:

- [ ] learn the other basic concepts like 最大流 最小流.




* Directed Graph

    + An directed graph is a pair of (V, E) where:
        + V is a set of elements, each of which called a node
        + E is a set of unordered pairs {u, v} where u and v are nodes, we say there is a directed edge from u to v.
* A directed edge (u,v), i.e. `u->v`, is said to be an outgoing edge of u, and incoming edge of v. Accordingly, v is an *out-neighbor* of u, and u is *in-neighbor* of v.
* Note that every edge has a direction.



Definitions in Graph:
* Let G = (V, E) be a graph. A path in G is a sequence of nodes (v1, v2, ..., vk) such that:
    * For every i $\in [i, k]$, there is an edge between v~i~ and v~i+1~.
* A **cycle** in G is a **path** (v1, v2, ..., vk) such that $k\le 4$ and v~1~ = v~k~
* trail: a path that has unique nodes.
* In an undirected graph, the degree of vertex u is the number of edges of u
* In a directed graph, the out-degree of a vertex u is the number of outgoing edges of u, and its in-degree is the number of its incoming edges.



Connected Graph


* Graph vs. Tree vs. Forest
    * A tree is a connected undirected graph contains no cycles.
    * Forest is a set of disjoint trees.




* Adjacency list: Undirected G
    * Each vertex $u\in V$ is associated with a linked list that enumerates all the vertices that are connected to u.
```
v1 -> v2 -> v3 -> v4 -> v5
v2 -> v1 -> v4
v3 -> v1
v4 -> v1 -> v2
v5 -> v1
```
Space = $O(|V|+|E|)$



* Adjacency list: Directed G
    * Each vertex $u\in V$ is associated with a linked list that enumerates all the vertices $v\in V$ that there is an edge from u to v.
```
v1 -> v2 -> v3
v2 -> v4
v3
v4 -> v1 -> v2
v5 -> v1 -> v4
```
Space = $O(|V|+|E|)$



- 邻接表

```cpp
int head[MAXN];		// head[i] store the next store_node in the nodelist of the ith node;
					// head of the linked list
int vis[MAXN];		// vis[i] represent the visit status of the ith node
int cnt = 0;		// a pointer in the nodelist

struct node {		// each node store one adjacent node of this node(check it in the head[])
    int to;			// the adjacent_node stored inside this store_node
    int nxt;		// next store_node, a linked list of one actual node
} nodelist[MAXN*2];

// link two nodes together
inline void link(int x, int y) {
    cnt++;
    nodelist[cnt].to = y;
    nodelist[cnt].nxt = head[x];
    head[x] = cnt;
}

void dfs(int x, int pre, int d) {
	for (int i = head[x]; i > 0; i = nodelist[i].nxt) {
        int v = nodelist[i].to;
        if (v != pre)
            dfs(v, x, d + 1);
    }
}
```









#### Breadth First Search (BFS)

At the beginning, color all vertices in graph white. And create an empty `BFS tree T`.

* Create a `queue Q`. Insert the source vertex s into Q, and color it yellow.

* Make s the root of T

* Repeat the following until Q is empty

    * De-queue from Q the first vertex v

    * For every out-neighbor u of v that is still white

        * 2.1 Enqueue u into Q, and color u yellow

        * 2.2 Make u a child of v in the BFS tree T.

    * Color v red

```pseudocode
Q.push(s);
s.color <- yellow;
root of T <- node s;
while (Q is not empty)
	v <- Q.pop();
	for every out-neighbor u of v
		if (u.color == white)
			Q.push(u);
			u.color <- yellow;
			T.insert(u);
	v.color <- red
```

(white: “haven’t visited”; yellow: “in the queue”; red: “is visited”)



Complexity:

- Time Complexity: 
    $$
    O\left(\displaystyle\sum_{u\in V}(1+deg(v))\right)=O(|V|+|E|)
    $$

When a vertex v is dequeued from Q(line 5), we spend $O(1+deg(v))$ time precessing it. Additionally, every vertex enter Q only once.

> The Hand-shaking Theorem: 
> $$
> \displaystyle\sum_{all\ v\in V}{deg(v)}=2\cdot e
> $$



#### Depth First Search (DFS)

At the beginning, color all vertices in the graph white, and create an empty `DFS tree T`.

- Create a `stack S`. Pick an arbitrary vertex v. Push v into S, and color it yellow

- Make v the root of T

- Repeat the following until S is empty

    - Let v be the vertex that currently tops the stack S (do not remove v from S)

    - Does v still have a white out-neighbor
        - 2.1 If yes: let it be u.
            - Push u into S, and color u yellow
            - Make u a child of v in the DFS-tree T
        - 2.2 If no, pop v from S, and color v red

- If there are still white vertices, repeat the above by restarting from an arbitrary white vertex v’, creating a new DFS tree rooted at v’.

```pseudocode
S.push(v);
v.color <- yellow;
root of T <- vertex v;
while (S is not empty)
	v <- S.top();
	for (every child u of v)
		if (u.color == white)
			S.push(u);
			u.color <- yellow;
			v.child.add(u);
		else
			S.pop();
			v.color <- red;
```

(white: “haven’t visited”; yellow: “in the stack”; red: “is visited”)

```c++
void DFS(int root, int prev) {
    visited[root] = 1;
    node v = nodelist[root];
    while (!v.child.empty())
        for (int i = 0; i < v.child.size(); i++) {
            node v = u.child.at(i);
        	if (v.color == 0 && v != prev) {
                
            }
        }
}
```

(0: “not visited”; 1: “is visiting”; 2: “is visited”)



Difference: BFS → Queue, DFS → Stack



Applications:

- Find cycle



#### Shrotest Path

Let G = (V, E) be a directed graph. A path in G is a sequence of nodes (𝑣~1~, 𝑣~2~, … , 𝑣~𝑘~) such that

* 𝑣~1~ → 𝑣~2~ → ⋯ → 𝑣~𝑘~ 

* The path is said to be from 𝑣~1~ to 𝑣~𝑘~, the length of the path is 𝑘 − 1. 

* Given two vertices 𝑢, 𝑣 ∈ 𝑉, a shortest path from 𝑢 to 𝑣 is *a path from 𝑢 to 𝑣 that has the minimum length among all the paths from 𝑢 to 𝑣* . 
    * can have multiple shortest path.
* If there is no path from 𝑢 to 𝑣, then 𝑣 is said to be *unreachable* from 𝑢.







#### Minimum Spanning Tree

##### Undirected Weighted Graphs

##### Spanning Trees

The minimum spanning tree problem
* Goal: find a spanning tree of the smallest cost.
* denoted as: an MST of (G, w)
* *not unique*

###### Prim's Algorithm:

1. Let `{u, v}` be an edge with the smallest weight among all edges.

2. Set `S = {u, v}`. Initialize a tree `Tmst `with only one edge `{u, v}`.

3. Enforce the lightest extension principle: 

    - build a min-heap `best-ext()` to store the best-extension edge of vertex `z`

    - For every neighbor `z` of `S`

        - If z is a neighbor of `u`, but not of `v`:

            ​    best-ext(z) = edge {z, u}              // best-ext(z) means that z is connected to set S by edge {z, u};

        - If z is a neighbor of `v`, but not of `u`:

            ​    best-ext(z) = edge {z, v}

        - If z is a neighbor of both `u` and `v`:

            ​    best-ext(z) = min( edge {z, u}  ,  edge {z, v} )

4. Repeat until S == V:

    - Get the smallest edge among all best-ext().
    - Add `v` to `S`, and add edge `{u, v}` into `Tmst`
    - For every edge `{v, z}` of v:             // restore the lightest extension principle again
        - If $z \notin S$ then:
            - If best-ext(z) is heavier than edge {v, z} then
                - Set best-ext(z) = edge {v, z}

```pseudocode
MST-PRIM(G, w, r)
    for each u belongs to V[G]
        do key[u] <- INT_MAX
            pie[u] <- NIL
    key[r] <- 0
    Q <- V[G]
    while Q != empty
    	do u <- EXTRACT-MIN(Q)
    		for each v belongs to Adj[u]
    			do if v belongs to Q and w(u,v) < key[v]
    				then pie[v] <- u
    					key[v] < w(u,v)
```













---






## 二、理论







### RAM Computation Model

**Hard Disk (TB Level)** —[I/O cost]—> **RAM (GB Level)** —[CPU cost]—> **CPU (ALU)**

- We focus on the **CPU cost**
    * In CPU We have *Registers* which load data form the memory and *ALU* which do the computation.

#### Memory 

- A finite sequence of cells, each cell has the same number of bits.

- Every cell has an address: the first cell of memory has address 0, the second cell 1, and so on.

- Store information for immediate use in a computer

- Computer hardware devices 



#### Center Process Unit (CPU)

- Contains a fixed number of registers
- Basic (atomic) operations
    - Initialization
        - Set a register to a fixed values (e.g., 100, 1000, etc.)
    - Arithmetic(ALU)
        - Take integers a, b stored in two registers, calculate one of {+, -, *, /} and store the result in a register
    - Comparison / Branching
        - Take integers a, b stored in two registers, compare them, and learn which of {a<b, a=b, a>b} is true.
    - Memory Access
        - Take a memory address A currently stored in a register, Do the READ (i.e., load data from memory) or WRITE  (i.e., flush data to memory)operator





### Algorithm Analysis

- **Algorithm**
    - A sequence of basic operations
- **Algorithm Analysis**
    - **Cost analysis**
        - Algorithm cost (running time) is the length of the sequences, i.e., the number of basic operations
    - Is your algorithm fast?
        - Focus on the order of growth (how the running time grows for large n)
    - Unless otherwise stated, we refer algorithm analysis as cos analysis in CS203

- Cost:
    - initialization cost: 1
    - ALU {$+,-,\times,\div$} cost: 1
    - loops(n times) cost: $\times n$ 



#### Worst-Case Running Time

>  The worst-case running time (or worst case cost) of an algorithm under a problem size n, is defined to be the largest running time of the algorithm on all the inputs of the same size n.

| Complexity   | time          | Algorithm                                                    |
| ------------ | ------------- | ------------------------------------------------------------ |
| $O(1)$       | Constant time | Compare two numbers                                          |
| $O(\log n)$  | Logarithmic   | **Binary Search** (on a sorted array)                        |
| $O(n)$       | Linear time   | Search (on a unsorted array)                                 |
| $O(n\log n)$ |               | **Merge Sort** <br/>**E(Quick Sort)**                        |
| $O(n^2)$     | Quadratic     | **Selection Sort** <br/>**Insertion Sort** <br/>**Quick Sort** |
| $O(n^3)$     | Cubic         | Matrix multiplication                                        |
| $O(2^n)$     | Exponential   | Brute-force search <br/>on boolean satisfiability            |
| $O(n!)$      | Factorial     | Burte-force search <br/>on traveling salesman                |



#### Big-O notation

- We say that $f(n)$ *grows asymptotically no faster than* $g(n)$ if there is a constant $c_1>0$ such that:

    - $f(n)≤c_1∙g(n)$

        holds for all $n≥c2$. 
    
- We denote this by $f(n)\le O(g(n))$

- Examples: 

    - $10n = O(5n)$
    - $5n = O(10n)$
    - $10000\log_2n= O(n)$
        - c~1~ = 1, c~2~ = 220
    - $(5n^2+3n)=O(n^2)=O(n^3)$
    - $(\log_2n)^3=O(\sqrt{n})$
    - $\log_an= O(\log_bn)$ , $\forall$a>1, b>1







---



### Master Theorem （主定理）

- Recurrence equation: T(n) = a T(n/b) + f(n)

- Let T(n) be a function that return a positive value for every integer n>0. We know that:
  
    - $T(1) = O(1)$
    
    - $T(n) =\displaystyle \textcolor{red}\alpha T \left( \left\lceil \frac{n}{\textcolor{red}\beta} \right\rceil \right) + O(n ^ \textcolor{red}\gamma) \qquad \left( n\ge 2 \right)$
    
    where $\alpha \ge 1,\ \beta > 1,\ andy \ge 0$. Then: 
    
    - If $\log_\beta^\ \alpha < \gamma$, then $T(n) = O(n^\gamma)$
    - If $\log_\beta^\ \alpha = \gamma$, then $T(n) = O(n^\gamma \log n)$
    - If $\log_\beta^\ \alpha < \gamma$, then $T(n) = O(n^{\log_\beta \alpha})$

> Example:
>
> Consider the recurrence of __merge sort:__
>
> - $T(1) \le c_1$
> - $T(n) = 2T\left(\dfrac{n}{2} \right) + O(n) = 2T\left(\dfrac{n}{2} \right) + c_{2n} \quad (for\ n \ge 2)$
>
> Hence, $α = 2, β = 2, γ = 1$. 
>
> Since $\log_𝛽 𝛼 = \log_2 2 = 1 = γ$, we know that $𝑇(𝑛) = 𝑂(𝑛^1\log 𝑛) = 𝑂(n\log 𝑛)













# 算法设计与分析





### Problem1.1 Stable Matching



#### Stable Matching Problem

Goal: Given n men and n women, find a "suitable" matching.

- Participants rate members of opposite sex.
- Each man lists women in order of preference from best to worst. 
- Each woman lists men in order of preference from best to worst.

Example :

| Man    | 1^st^  | 2^nd^  | 3^rd^ |
| ------ | ------ | ------ | ----- |
| Xavier | Amy    | Bertha | Clare |
| Yancey | Bertha | Amy    | Clare |
| Zeus   | Amy    | Bertha | Clare |



| Woman  | 1^st^  | 2^nd^  | 3^rd^ |
| ------ | ------ | ------ | ----- |
| Amy    | Yancey | Xavier | Zeus  |
| Bertha | Xavier | Yancey | Zeus  |
| Clare  | Xavier | Yancey | Zeus  |

- X-C, Y-B, Z-A    —> unstable			Bertha & Xavier, improve both matching
- X-A, Y-B, Z-C    —> stable





- **Perfect matching**: everyone is matched monogamously(一夫一妻的).
- Each man gets exactly one woman. 
    - Each woman gets exactly one man.
- **Stability**: no incentive(刺激) for some pair of participants to undermine(侵蚀) assignment by joint action.
    - *In matching M, an unmatched pair m-w is unstable if man m and woman w prefer each other to current partners.*
    - **Proof** :  $\uparrow$ find one unstable match 
- Unstable pair m-w could each improve by eloping.
- **Stable matching**: perfect matching with no unstable pairs.
- **Stable matching problem**: Given the preference lists of n men and n women, find a stable matching *if one exists*.





==A Stable Matching does not always exist, not always unique either==

#### Stable Roommate Problem

- 2n people; each person ranks others from 1 to 2n-1.
- Assign roommate pairs so that no unstable pairs.



#### Propose-And-Reject Algorithm

[Gale-Shapley 1962] Garantee to find a stable matching

```pseudocode
Initialize each person to be free.
while (some man is free and hasn't proposed to every woman) {
    Choose such a man m
	w = 1st woman on m's list to whom m has not yet proposed 
	if (w is free)
        assign m and w to be engaged
    else if (w prefers m to her fiancé m_)
	    assign m and w to be engaged, and m_ to be free
    else
    	w rejects m
}
```

- Complexity: $O(n^2)$

- Proof:
    - Perfection: Everyone get matched.
    - Stability: No unstable pairs.



#### Efficient Implementation

- Efficient implementation. We describe $O(n^2)$ time implementation.
- Representing men and women.
    - Assume men are named 1, ..., n.
    - Assume women are named 1', ..., n'.



- Engagements.
	- Maintain a list of free men, e.g., in a queue.
    - Maintain two arrays `wife[m]`, and `husband[w]`.
        - set entry to 0 if unmatched
        - if m matched to w then `wife[m]=w` and `husband[w]=m`



- Men proposing.
    - For each man, maintain a list of women, ordered by preference.
    - Maintain an array count[m] that counts the number of proposals made by man m.



- Women rejecting/accepting.
	- Does woman w prefer man m to man m'?
	- For each woman, create inverse of preference list of men.
	    - Use `inverse[]` to find preference of wooer in $O(1)$ time.
	- Constant time access for each query after $O(n)$ preprocessing.

| Amy      | 1^st^ | 2^nd^ | 3^rd^ | 4^th^ | 5^th^ | 6^th^ | 7^th^ | 8^th^ |
| -------- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| `Pref[]` | 8     | 3     | 7     | 1     | 4     | 5     | 6     | 2     |

| Amy         | 1     | 2     | 3     | 4     | 5     | 6     | 7     | 8     |
| ----------- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| `Inverse[]` | 4^th^ | 8^th^ | 2^nd^ | 5^th^ | 6^th^ | 7^th^ | 3^rd^ | 1^st^ |

```pseudocode
for i = 1 to n
	inverse[pref[i]] = i
```



#### Man Optimality

- Man-optimal assignment for Gale-Shapley Algorithm:
    -  Each man receives best valid partner.



#### Woman Pessimality

- Woman-pessimal assignment for Gale-Shapley Algorithm:
    -  Each woman receives worst valid partner.







### Problem1.2 Five Representative Problems

- **Interval scheduling**: 
    - $n\cdot logn$ greedy algorithm.
- **Weighted interval scheduling**: 
    - $n\cdot logn$ dynamic programming algorithm.
- **Bipartite matching**: 
    - $n^k$ max-flow based algorithm.
- **Independent set**: 
    - NP-complete.
- **Competitive facility location**: 
    - PSPACE-complete.

[计算复杂性分类](https://www.sohu.com/a/271557851_741733)



#### Interval Scheduling

- **Input:**
    - Set of jobs with **start times** and **finish times**.
- **Goal:**
    - Find maximum cardinality subset of mutually compatible jobs.
        - Jobs don’t overlap





#### Weighted Interval Scheduling

- **Input:**
    - Set of jobs with **start times**, **finish times**, and **weights**. 
- **Goal:**
    - Find maximum weight subset of mutually compatible jobs.
        - Jobs don’t overlap





#### Bipartite Matching

- **Input:**
    - Bipartite graph.
- **Goal:**
    - Find maximum cardinality matching.





#### Independent Set

- **Input:**
    - Graph.
- **Goal:**
    - Find maximum cardinality independent set.
        - Independent: subset of nodes such that no two joined by an edge





#### Competitive Facility Location

- **Input:**
    - Graph with weight on each node.
- **Game:**
    - Two competing players alternate in selecting nodes.
    - Not allowed to select a node if any of its neighbors have been selected.
- **Goal:**
    - Select a maximum weight subset of nodes.
        - Subset: independent set







