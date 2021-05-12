# Algorithms

Discrete Math and Algorithms are hard. 

Remember -- all of these algorithms were *discovered* (or refined for computers) over the last 70+ years.  
No one person sat down and conjured them up in a few sessions like this class will.  They are *elementary* or 
*fundamental*, they are not *simple* or *trivial*.

If all of these problems are solved, why are we re-solving or re-analyzing them? Because if you don't learn on
solved problems, you'll never be able to tackle unsolved ones. Also, the general problem solving process sharpens
our process that we'll apply to programming -- think of isolating a single skill, like a tennis serve or basketball
free throw.

## Teaching Tools

* [Deck of Cards](https://deck.of.cards/)

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

* [Algorithms](https://jeffe.cs.illinois.edu/teaching/algorithms/) by Jeff Erickson
* [Algorithms Notes for Professionals](https://books.goalkicker.com/AlgorithmsBook/)
* [Advanced Algorithms and Data Structures](https://www.manning.com/books/advanced-algorithms-and-data-structures) by Marcello La Rocca
* [Data Structures & Algorithm Analysis](https://people.cs.vt.edu/~shaffer/Book/) by Clifford A. Shaffer and the [OpenDSA project](https://opendsa-server.cs.vt.edu/)
* [The Algorithm Design Manual](https://www.algorist.com/) by Steven Skiena
* [Algorithms in a Nutshell](https://learning.oreilly.com/library/view/algorithms-in-a/9781491912973/) by George T. Heineman, Gary Pollice, and Stanley Selkow
* [TheAlgorithms](https://the-algorithms.com/) and [github](https://github.com/TheAlgorithms)
* [Introduction to Algorithms](https://mitpress.mit.edu/books/introduction-algorithms-third-edition) by Cormen, Leiserson, Rivest and Stein *a/k/a* CLR *a/k/a* "The Big White Book That Won't Fit on a Shelf"
* [Daniel G. Graham -- Algorithms](https://danielggraham.com/algorithms/)
  
#### Courses 
* [Algorithms](https://algs4.cs.princeton.edu/home/) by Robert Sedgewick and Kevin Wayne. Also available on Coursera in [Part 1](https://www.coursera.org/learn/algorithms-part1) and [Part 2](https://www.coursera.org/learn/algorithms-part2) 
* [Coursera - Data Structures and Algorithms Specialization](https://www.coursera.org/specializations/data-structures-algorithms)

#### MIT Courses

in prereq order:

* [Mathematics for Computer Science -- MIT 6.042J](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-042j-mathematics-for-computer-science-spring-2015/)
* [Intro to Algorithms -- MIT 6.006](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/)
* [Design and Analysis of Algorithms (MIT 6.046J / 18.410J)](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-046j-design-and-analysis-of-algorithms-spring-2015/)
* [Advanced Data Structures (MIT 6.851)](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-851-advanced-data-structures-spring-2012/)

## Data Structures

structure and semantics

* Bag (multiset)- non-unique; add and iterate
* Set (USet)- unique; add and iterate; unordered
* SSet (sorted set) - supports successor find
* Hash table (map, hash map, associative array, dictionary). A symbol table is
  usually implemented using a hash table, and is sometimes referred to synonymously. Effectively a set of tuples queryable by the 1st element in the tuples.
* Queue - FIFO
* Pushdown stack - LIFO
* Priority Queue - highest priority goes to top. usually implemented using heap.
* Array
* Vector
* List
* Non-empty List 
* Chain - list-like, with constant time prepending and appending. See 
  [Cats Chain](https://typelevel.org/cats/datatypes/chain.html)
* Binary tree
* Heap

## Shuffling

* Fisher-Yates
* https://www.developer.com/tech/article.php/10923_616221_2/How-We-Learned-to-Cheat-at-Online-Poker-A-Study-in-Software-Security.htm
* Sattolo's algorithm https://danluu.com/sattolo/

## Sorts

* Bubble sort (sinking sort) - repeatedly pass through the list and compare adjacent elements, until the list is sorted
* Selection - repeatedly find the next lowest value in the remaining list and swap to head. slow, but, data movement is minimal
* Insertion - repeatedly swap the head of the unsorted partition with the next higher value, then move to the next and repeat. fast for already-sorted data. what most people use for sorting cards.
* Shell - *h*-sorted array -- insertion sort with *h* interleaved sorted sequences, gets items closer to their eventual location with each reduction of *h* . Ciura's gap sequence [701, 301, 132, 57, 23, 10, 4, 1]
* Mergesort - divide-and-conquer -- recursively sort and combine sub-lists
* Quicksort - recursively sort lists into bigger/smaller than pivot (why not called pivot sort)  Tony Hoare
 - scan from right and left to find two elements that are both out of order, then swap
* Introsort - quicksort, then heap sort for small sets
* Heapsort
* Timsort