---
layout: default
title: Data Structures
description: An overview and reference of common data structures and how they work.
tags: Data-Structures Algorithms Big-O
---
# Data Structures

This post is about the most common data structures, how they work, and some of the different names they go by, which can get confusing quickly with how much overlap there can be in the topic and how different books and programming languages name them.

## Big-O

First, a lightning-quick intro to something called "Big-O" notation. Basically it's the vocabulary people use to describe how well certain data structures and algorithms scale. 

It's not so much concerned with how much time or space something takes. What matters is with how fast the amount of work grows as the problem gets bigger:

  - O(1) is "constant" -- as the input gets bigger the work to solve the problem stays the same and this is very good, though not possible for all problems

  - O(log(n)) is "logarithmic" -- as the input gets bigger the work to solve the problem grows at a logarithmic (slow) rate and this very good, often the best case for many problems

  - O(n) is "linear" -- as the input gets bigger the work to solve the problem increases at a constant rate -- this is usually bad because it is much slower and for many problems you can do better

  - O(n²) is "quadratic" -- as the input gets bigger the work to solve the problem grows rapidly by powers of two and this is very bad in most cases

  - O(2ⁿ) is exponential -- as the input gets bigger the work to solve the problem explodes and your computer might not finish calculating in your lifetime for larger inputs


## Abstract Data Types vs Data Structures

Depending on the context, something could be called an ADT or a data structure. In this post I use data structure to mean an implementation and ADT to mean more of a design or concept.

Take for example a list. A list is a sequential container. But it can be implemented in many ways and with many varying properties by both linked lists and arrays, which themselves can also come in many varieties. Whether you are "using a list" to implement something else or "implementing a list with..." might influence whether you are thinking of the list as an ADT or a data structure.

At the end of this post there is also a short reference for this point, which I mostly find useful for organizing a summary. I just wanted to mention it here to reduce any confusion about what I mean in the rest of the post by ADT or data structure.

# List of Data Structures

### Record / Struct / Structure / Compound Data / Tuple

A record, or struct, is a collection of fields (or elements or members). These fields are given names and are always in the same order, and don't have to store the same types of data as each other, in contrast to regular arrays, which generally store only one type of data per array.

An example might be storing a point in an x-y coordinate as a struct, where the first value is the x value and the second is the y value, and perhaps a third field could be there for the name of the point (perhaps it represents some real work object like a place on a map) or some other piece of information.

They are usually included at language level (though not all languages have them), and can be used in organizing data in a way similar to objects in object oriented languages (though cannot have methods like objects can, only data).

### Array

Arrays can be a bit tricky to talk about because they are ubiquitous, but in the context of different languages they can be quite different. 

In a lower level context an array is literally a block of memory you get a pointer to, and you are only supposed to assign values of one type to the array, and to make it bigger or smaller you have to request a new block of memory and copy everything over. In a higher level context then the array could be a complicated object with loads of useful methods and properties, like the ability to grow and shrink behind the scenes as needed.

In light of that, it's probably best to just leave it at: "a data structure that stores a sequence of values that provides access to values through the use of a numerical index, which is a number indicating the offset from the start of the sequence for the particular value." As you explore any particular language you will quickly encounter them and learn about them more in that context.

![array diagram]({{site.baseurl}}/assets/images/articles/data-structures/array.svg)


### Dynamic Array / Resizable Array / ArrayList / Vector

A dynamic array is an array which keeps track of how many items are in it, and when it gets above or below a certain capacity it grows or shrinks appropriately. The growing or shrinking can be an expensive operation, however they are usually designed so that they don't have to do this often, so you get most of the benefits of a regular array (like speed of access) without having to worry about inserting too many things and overflowing it, or declaring one too big and wasting a ton of memory. 

The implementation of one is pretty straight forward, and is essentially adding the resizing logic to a regular array. Most languages have something similar built in at some level, whether in the language itself (like a list in Python or a regular JavaScript array) or in a standard library (like ArrayList in Java or Vector in C++). These  usually have lots of useful methods built into them, like methods for sorting or iterating through the values to do things with each one, rather than the "dumb" arrays they are built on.

### Linked List

A linked list is a data structure where you gather together nodes (some kind of record or object) where each node has some data and some kind of pointer or reference to another part of the linked list. It's like a chain, and you can follow it one link at a time in order to traverse the whole data structure. Sometimes the linked list has links going both directions, while other times only one direction. Sometimes the list is circular too. It's common to also have a special nodes, for things like keeping track of the head or tail of a list.

![linked list diagram]({{site.baseurl}}/assets/images/articles/data-structures/linked-list.svg)


### List

Most generally, a List is an ADT that is a sequential container for values. It's hard to be more specific since lists come in many varieties, but this gives me the opportunity, especially having now mentioned the above three data structures, to discuss some of the trade offs of basic list implementations, which really gets right to the heart of why people study data structures in the first place.

So when might you want to implement a list with an array vs maybe a linked list?

  - Say you want to search a sorted list for one item. Linked lists must be traversed one link at a time while arrays can access any element by index using a bit of arithmetic. This lets you jump right into the middle and move up or down based on where you're at. The difference in speed is huge, being linear vs logarithmic

  - Let's say that instead of searching, you just want to put them in or take them out in an order based on when they went in, like a stack of plates where you just grab the top one, or a queue of people where you serve the one who was there first. In this case a linked list is much better because you just have to change a few pointers or references to add a new item, while an array would require you to shift all the items by one -- a constant time operation vs a linear time one

### Stack

A stack is an ADT that is basically a List where you can only add and remove items from the top, also called LIFO for last-in, first-out. When you add an item it's called "pushing" an item onto the stack, and when you remove an item it's called "popping" an item off the stack.

Since a stack is basically a restricted list you could implement it in many ways. As mentioned above a linked list can be particularly nice since you can have a reference to the head, and since you only ever interact with the head then all inserts and removals are constant time operations (i.e. it doesn't matter how big the stack is, insert and remove will always take the same amount of time).

![stack diagram]({{site.baseurl}}/assets/images/articles/data-structures/stack.svg)


### Queue

 A Queue is an ADT that is also like a restricted List, except that it's first-in, first-out (FIFO), like standing in line and waiting your turn. The first item added will be the first item taken out. The operations for a queue are called "enqueue" and "dequeue" respectively, and just like a stack you can achieve constant time insertions and removals by keeping a reference to the head and tail of a linked list.

![queue diagram]({{site.baseurl}}/assets/images/articles/data-structures/queue.svg)


### Hash Table

A hash table is a data structure which attempts to allow for constant time insertion, removal, and lookup of stored items, and it does very well at that. It starts with an array for storing items, and then, rather than adding elements to it in order like with a list, it uses a hashing function to determine which item goes in which spot. Then when it needs to look it up, it calculates where it should be instead of searching the whole thing like with a list. This is excellent when order doesn't matter because it's fast like a sorted array but adding and removing items is also a constant time operation since you don't have to shift items around to maintain order like with a sorted array.

Let's take a list of employees as an example. Say you have 25 employees and each one has a name, a job, and a salary. Rather than storing each employee in a list, you might use their name as a key and use a hash function to calculate what slot to put it into. This gives you constant time insertion, removal, and lookup on anything you store since all you have to do to do any of those things is run the name through the hash function again and then access the resulting data at that array index.

But what if the hash function gives collisions? What if, say, Dave and Bill both hash to array index 4, or there are two employees named Emily? Ideally your hash function will spread everything out really well, but there are ways to deal with this.

The simplest to visualize and one of the more common ways is to use linked lists as buckets. Instead of index 4 having a single entry, it would have a linked list where the first entry would be Dave and then the second would be Bill. If the buckets are relatively evenly filled then it will still be close to constant time (this is called separate chaining if you are curious).

Another way to deal with this is called linear probing. Basically, if the slot is already taken, then search the next slot in the array, and go up by one until you either find an empty slot (for insertion) or the item you are looking for (other operations). This could of course be bad in the worst cases but assuming you have a really good hash function with few collisions it works very well. Generally it will be faster than chaining up until the hash table storage is about 80% full, and then it starts getting worse. Separate chaining starts out a little slower but doesn't really get much worse as the slots fill up.

![hash table diagram]({{site.baseurl}}/assets/images/articles/data-structures/hash-table.svg)


### Binary Search Tree

Binary search trees are data structures that are made up of nodes like a linked list, however instead of forming a linear chain they branch out and form a tree shape. Each node in a BST can have two children (which is where the binary in BST comes from). They are set up such that everything to the left of a node is always less than the node, and everything to the right is greater than the node.

Because of the way they are set up, they allow for a lot of really useful properties. To find a certain item, you just need to explore the left or right side of any particular node over and over until you find your value or you reach the end. Assuming your tree is somewhat balanced then this is a logarithmic operation, that is, each time you repeat you cut the number of things left to look through in half, kind of like starting in the middle of the phone book and then searching the left or right half until you narrow down to where you need to be.

Another useful property is that you can visit every node in sorted order, using what is called an in-order depth-first search. This in turn opens up a large array of operations like finding items in ranges, finding the min, or finding the max.

If you're thinking this sounds similar to a sorted array then you're right! Just like a sorted array, a binary search tree does an excellent job for many operations. So what are some considerations if deciding between a BST and a sorted array?

- To start, everything in an array is stored in consecutive order in memory while that isn't usually the case for a BST. Because of this, if you plan to iterate through all the items, in practice an array will almost always be faster, even though both operations are of a linear time complexity

- Arrays are also faster when adding an item to the end, since it will be a constant time operation vs a logarithmic one for the tree. This might be useful when you expect to receive data that is already sorted, especially since sorted data can lead to an unbalanced BST (basically everything on one side and it turns into a list if you don't use a self-balancing variant)

- BSTs really shine when you plan to add items to the middle of the structure. For an array you would need to shift part of the data (on average half, so a linear operation) to make space if you wanted to keep it sorted, but for a BST finding the right spot and making space is usually a much faster logarithmic operation

- They both have excellent searching times, being logatithmic

So in general, you want to be thinking about what sorts of things you want to do with the data you will be putting into the data structure. Arrays, linked lists, BSTs, and hash tables all have different properties and are good at different things so understanding what you need is key. Do you need to keep the data sorted? Do you plan to add or remove any data? What about finding data? Do you care about finding arbitrary data or just data that should be at a certain spot, like the first or last item? By asking these kinds of questions (or at worst googling "X vs Y for Z purpose") will help you make more informed decisions that can really speed up or save space in an application.

![binary search tree diagram]({{site.baseurl}}/assets/images/articles/data-structures/binary-search-tree.svg)


### Balanced Binary Search Tree

As hinted at above, a balanced BST essentially solves the problem of making sure the data in the tree is always spread out evenly. Think about it like this: if you have a BST and you input the numbers 1, 2, 3, 4, and 5, in sorted order, each one gets added to the right of the previous, and the tree essentially devolves into a linked list. If that happens many of its super fast logarithmic operations become much slower linear operations. In many cases a regular BST is just fine, but if you want to guarantee good performance then you can go one step further by implementing a balanced search tree.

The two most popular variants of the search tree that aim to maintain balance are the AVL Tree and the Red-Black Tree. They are both relatively complicated but essentially they each set up some basic patterns the tree must follow, and when you insert or remove a node that breaks one of those patterns, they perform a series of rotations on nodes that restores those patterns, and keeps the trees balanced. If you would like to know more about them then I would recommend [this article](http://www.drdobbs.com/cpp/stls-red-black-trees/184410531) about Red-Black trees, and [this article](https://benpfaff.org/papers/libavl.pdf) comparing BSTs.

### Map / Dictionary / Associative Array / Symbol Table

So just like the List entry above, now that we have discussed a few more data structures we can look at another ADT, which like the List also has more than one option for implementation.

The essence of a map is that it associates a key with a value. This might sound a lot like hash tables, and to be sure hash tables are one of the most common ways of implementing this ADT. But hash tables aren't perfect for everything. For instance, they have no semblance of order, so if you wanted to, say, print out all the items in order you would have to essentially build a new sorted list one value at a time, or even worse, find the lowest/highest value n number of times, which means you search all n items n times. This operation would be quadratic and would take a long long time on a large dataset. Thankfully, in a case like this, we have other options, like trees.

You can just as easily store all of your keys in a tree as a hash table, and then you would be able to do any operations that required the keys to be sorted, at the cost of slightly slower access, insert, and delete operations. So while a hash table has constant time access for lookups, insertions, and deletions, it is stuck with linear time for operations that require order. On the other hand, a search tree handles pretty much everything in logarithmic time, which is still extremely fast, so that makes it a viable option if you want to do a little more with your data. This is just another example of where you can implement an ADT in multiple ways based on your needs.

### Multimap

A multimap is basically a map / dictionary / associative Array / symbol table) that can have more than one value attached to a key. This could be useful for when you want to express one-to-many relationships, like one teacher with many courses (each which has its own info), or one person who could have multiple vehicles registered under their name (each with its own info).

### Set

A set is an ADT that is similar to the mathematical idea of a set, and rather than being concerned with the value itself is more interested in whether a value is in the set or not. They support basic operations like checking the size of a set and whether it is empty, along with testing for membership of a value. Some sets also allow you to add or remove items, while others are static. To implement one you can use any a map / dictionary / associative array / symbol table implementation and just ignore any values, instead treating the key as the value itself.

### Bag / Multiset

This is simply a set that allows for multiple copies of items. The items can either be counted (if they are exactly equal) or they can be kept as duplicates (if they are equivalent). In some cases a bag might be defined more specifically to include the idea that you can't remove an item from the set once it is added (Sedgewick and Wayne, p.124) so if working with one you will want to make sure to check out exactly what operations are supported.

### Binary Heap

This is one of the more unique data structures, and it is very useful for many things. It is probably best visualized as a tree structure, as pictured below, even though you rarely implement it as a tree, though more on that shortly. It basically follows one rule: every element is greater (or less than) than its children. This is called the heap property. Because of the heap property, one really useful trait emerges: the root of the heap is always the greatest (or smallest) value in the heap. This is great for when you want to keep track of a max (or min) and can also be used directly to implement a priority queue (below). They are also used in a variety of other algorithms, like heapsort and certain graph algorithms.

In order to implement a min or max heap, the critical thing to do is that every time you insert or remove an item you must make sure the heap property still holds. When you insert an item, you simply insert it at the end, and bubble it up by comparing it to its parent, until the heap is "fixed". Removal is similar, where you grab the last element, make it the new root (you usually only remove from the root because you usually only care about the min/max you are keeping track of), and let it sink down to its correct position by comparing it to its children.

One more thing, which I mentioned earlier, is that while it's easy to visualize as a tree you don't usually implement it as one (though you could if you wanted to). Because of the way it is constructed, the tree visualization will always be what is called a complete tree, i.e. it fills up each level before moving on to the next. Because of this people are able to store the heap in an array. Starting at [0], you have the root. Then [1] and [2] are the second level. [3], [4], [5], [6] are the third level, and so on. When doing the bubbling up or sinking down, you are able to do simple math with the indices to find the relevant children or parent indices, so it is easy to compare them over and over until you are in the right place. If you are curious the math is simply that the parent of any child is 2/k, where k is the level of the item, and to go the other way to find the children it is simply 2*k and 2*k + 1.

![binary heap diagram]({{site.baseurl}}/assets/images/articles/data-structures/binary-heap.svg)


### Priority Queue

Given that a priority queue is so often implemented as a heap which can keep a min or max as the root, there isn't too much left to say about this ADT. When you are implementing a priority queue, you want to be able to insert an item with any priority at any time, and to always extract the highest priority item in constant time (rather than FIFO, it is highest priority out and then FIFO in case of equal priorities). Since with a heap that highest priority is always the root, the heap is a fantastic and commonly used option for implementing a priority queue.

### Graph

A graph is an ADT made up of vertices and edges. They are very widely used because they can model so many things, like a map of a region where vertices might be cities and edges might be roads. Just like the real-world things they model, there is a lot of variety in the types of graphs. Some graphs have directed edges (like one-way roads) while others are undirected. Some graphs are cyclical while others are acyclic. Some edges have weights, or costs associated with them and sometimes graphs aren't even fully connected. Below we look at two common graph implementations.

### Adjacency Matrix

This data structure is pretty straight forward. Every vertex is accounted for on each side of the matrix, and a 0 or 1 is used where they intersect to indicate that there is an edge from one to the other. This particular data structure has the advantage that adding or removing an edge is a very fast operation (constant) in that you only have to swap a 0 or 1. It's also very quick to check if an edge exists from one vertex to another (constant also) and it has the advantage of being relatively simple to understand.

If you have to add or remove a vertex and reconstruct the graph however it is expensive and a quadratic operation. Also it can take up a lot of space even when there aren't many edges in the graph, because you will always have O(V^2) slots.

![adjacency matrix diagram]({{site.baseurl}}/assets/images/articles/data-structures/adjacency-matrix.svg)


### Adjacency List

Another implementation, and probably the most common, is the adjacency list. It's made up of an array of linked lists, where each array slot is the vertex and the linked list is a list of other vertices that are connected to it. Some advantages include that it can save space, only representing the vertices and edges that exist, and it is also easier to add a vertex (you just need another array entry).

Some downsides might be that removing an edge or vertex is more complicated and slower, because you have to hunt down all the right vertices and edges. Also checking whether two vertices are adjacent is slower, since it has to search the linked list in order (linear operation) to see if one is connected to the other. This isn't very bad in a sparse graph, where the linked lists have relatively few entries, but in a dense graph where almost everything is connected to almost everything else it can be a much slower operation.

In general it is recommended to use adjacency lists for sparse graphs and to use adjacency matrices for dense graphs or for when you really want to prioritize the speed of checking for an edge between two vertices.

![adjacency list diagram]({{site.baseurl}}/assets/images/articles/data-structures/adjacency-list.svg)


### Further reading


  - [Introduction to Algorithms](https://en.wikipedia.org/wiki/Introduction_to_Algorithms) by Cormen, Leiserson, Rivest, and Stein (known as CLRS)

  - [Algorithms, 4th Edition](https://algs4.cs.princeton.edu/home/) by Sedgewick and Wayne

  - [The Algorithm Design Manual](https://www.amazon.com/exec/obidos/ASIN/1848000693/thealgorithmrepo) by Skiena


### Implementations Cheat Sheet

  - **Lists:** dynamic arrays, linked lists

  - **Stacks, Queues:** linked lists

  - **Maps, Multimaps:** hash tables, BSTs, balanced BSTs

  - **Sets, Multisets:** hash tables, BSTs, balanced BSTs

  - **Priority Queues:** binary heaps

  - **Graphs:** adjacency lists, adjacency matrices

