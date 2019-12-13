

What is a Priority Queue?

   Priority Queues are similar to Queues except, in priority queues, the order in which the
   elements enter the queue may not be the same in which they were processed.
   In priority queue, the item with the smallest key is processed first (Ascending Queue).

A Priority Queue Operations

    1)Insert(key,val) : Inserts val with key in the priority queue.Elements are ordered based on key.
    2)Delete Min : Remove and return element with smallest key.
    3)Get Min : Return the element with the smallest key.

Priority Queue Applications:

    1)Data Compression: Huffman Coding algo
    2)Shortest Path algorithm : Dijkstra's algo
    3)Minimum spanning tree: Prim's algo

Priority Queue Implementations:

    1)Unordered array/ list implementation
        Elements are inserted without bothering about the order. Deletion is performed by searching the element.
        Insertion: O(1) Delete Min: O(n) Find min : O(n)
    2)Ordered Array/List implementation:
        Elements are inserted in the array in sorted order based on the key field. Deletions are performed at only one end.
        Insertion: O(n) Delete Min: O(1) Find Min: O(1)
    3)Binary Search Tree Implementation:
        Both insertion and deletion will take O(logn) on average. Find Min: O(logn) average Worst case could be O(n) for a skewed tree
    4)Balanced Binary Search Implementation:
        The difference between heights of left subtree and right subtree is not more than 1 for every node.
        Both insertion and deletion will take O(logn) on average. Find min: O(logn)
    5)Binary Heap Implementation:
        Insertion: O(logn) Delete Min: O(logn) Find Min: O(1)

Heaps and Binary Heaps
    Heap is an implementation of a priority queue. It will be an array visualized as a nearly complete binary tree.
    A heap is a TREE with some special properties:
        1)Heap Property: The basic requirement is that the value of a node must be greater than the value of its children.(For Max Heap)
        2)All leaves should be at h or h-1 levels (where h is the height of the tree) for some h>0. That essentially
          means that the heaps should form a complete binary trees. [A complete binary tree is a binary tree in which every level except possibly the last , is completely filled. and all nodes are as left as possible]

          This is a (max)heap(each element is larger than its  children and Complete binary tree )
              17
            /    \
           3      6
         /  \    /
        1    2  4

        This is not a heap.
              11
            /    \
           5      2
         /  \    /  \
        6   17  14  13


       In practice Binary Heaps are enough, where each node may have up to two children.


Types of Heaps:
    Min Heap: The value of node must be less than or equal to the values of its children.
                          11
                        /    \
                      15     12
                     /  \    /  \
                    16   17  14  13
    Max Heap: The value of node must be greater than or equal to the value of its children.
                          17
                        /    \
                      13      6
                     /  \    /  \
                    1   4   2    5


Representation of Heaps:
        One possibility to represent heap is using arrays.
                                      17
                                    /    \
                                  13      6
                                 /  \    /  \
                                1   4   2    5

        to array: {17, 13, 6, 1, 4, 2, 5}




