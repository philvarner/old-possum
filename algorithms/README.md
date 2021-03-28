# Algorithms

## Resources

### Functional-oriented

* https://github.com/vkostyukov/scalacaster
* https://github.com/TheAlgorithms/Scala
* https://www.scala-algorithms.com/
* Advanced Functional Data Structures and Algorithms by Atul S. Khot and Raju Kumar Mishra

### Visualizations

* [Toptal - Sorting Algorithms Animations](https://www.toptal.com/developers/sorting-algorithms)

### Less academic/technical

* [Grokking Algorithms](https://www.manning.com/books/grokking-algorithms) by Aditya Y. Bhargava
* [Dive Into Algorithms]() by Bradford Tuckfield 

### More academic/technical

* [Algorithms](https://algs4.cs.princeton.edu/home/) by Robert Sedgewick and Kevin Wayne. Also available on Coursera in [Part 1](https://www.coursera.org/learn/algorithms-part1) and [Part 2](https://www.coursera.org/learn/algorithms-part2)
* [Introduction to Algorithms](https://mitpress.mit.edu/books/introduction-algorithms-third-edition) by Cormen, Leiserson, Rivest and Stein *a/k/a* CLR *a/k/a* "The Big White Book That Won't Fit on a Shelf"
* [Algorithms](https://jeffe.cs.illinois.edu/teaching/algorithms/) by Jeff Erickson
* [Algorithms Notes for Professionals](https://books.goalkicker.com/AlgorithmsBook/)
* [Advanced Algorithms and Data Structures](https://www.manning.com/books/advanced-algorithms-and-data-structures) by Marcello La Rocca
* [Data Structures & Algorithm Analysis](https://people.cs.vt.edu/~shaffer/Book/) by Clifford A. Shaffer and the [OpenDSA project](https://opendsa-server.cs.vt.edu/)
* [The Algorithm Design Manual](https://www.algorist.com/) by Steven Skiena
* [Algorithms in a Nutshell](https://learning.oreilly.com/library/view/algorithms-in-a/9781491912973/) by George T. Heineman, Gary Pollice, and Stanley Selkow
* [TheAlgorithms](https://the-algorithms.com/) and [github](https://github.com/TheAlgorithms)
* Coursera [Data Structures and Algorithms Specialization](https://www.coursera.org/specializations/data-structures-algorithms)
* [Introduction to Algorithms (MIT SMA 5503)](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-046j-introduction-to-algorithms-sma-5503-fall-2005/)
* [Design and Analysis of Algorithms (MIT 6.046J / 18.410J)](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-046j-design-and-analysis-of-algorithms-spring-2015/)
* [Advanced Data Structures (MIT 6.851)](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-851-advanced-data-structures-spring-2012/)


## Data Structures

* Hash table, hash map, associative array, and dictionary refer to the same data structure. A symbol table is
  usually implemented using a hash table, and is sometimes referred to synonymously. 
* Bag - add and iterate, non-unique
* Set - add and iterate, unique
* Queue - FIFO
* Pushdown stack - LIFO
* List
* Non-empty List 
* Array
* Vector
* Chain - list-like, with constant time prepending and appending. See 
  [Cats Chain](https://typelevel.org/cats/datatypes/chain.html)
* Binary tree

## Shuffling

* Fisher-Yates
* https://www.developer.com/tech/article.php/10923_616221_2/How-We-Learned-to-Cheat-at-Online-Poker-A-Study-in-Software-Security.htm
* Sattolo's algorithm https://danluu.com/sattolo/

## Sorts

* Selection - repeatedly find the next lowest value in the remaining list and swap to head. slow, but, data movement is minimal
* Insertion - repeatedly swap the value that was at the head of the unsorted partition with the next higher value, then move to the next and repeat. fast for already-sorted data.
* Shell - *h*-sorted array -- insertion sort with *h* interleaved sorted sequences
* Bubble sort (sinking sort) - repeatedly compare adjacent elements until the list is sorted
* Merge sort
* Quicksort
* Heap
* Timsort



Insertion sort - insert each element in the right place n^2 - 2n memory
selection sort - find the next lowest element and swap n^2 - in place
Shellsort sort - (related to insertion sort) in-place - progressive k-sort, moves longer distances than insertion sort Ciura's gap sequence [701, 301, 132, 57, 23, 10, 4, 1]

bubble sort - continually swap items until they appear in the correct place

merge sort - swap elements in increasingly larger lists

introsort - quicksort then heap sort for small sets
Timsort

quicksort - recursively sort lists into bigger/smaller than pivot (why not called pivot sort)  Tony Hoare
 - scan from right and left to find two elements that are both out of order, then swap


in place quicksort - swap
non-in-place quicksort - find all less, find all greater, and recurse
