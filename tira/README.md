# TIRA — Tietorakenteet ja algoritmit

Coursework for the **Data Structures and Algorithms (Tietorakenteet ja algoritmit, TIRA)** course at the University of Oulu. Completed in Fall 2023.

The project implements fundamental data structures and algorithms from scratch — without using Java's built-in collection classes — through a series of 9 programming tasks integrated into a Swing-based desktop application called **TIRA Coders**.

## Background & Motivation

This repository contains my solutions for the TIRA course, where we built and analyzed classic data structures one by one, integrating them into a shared application framework. The course emphasized both theoretical understanding (time complexity analysis, Big-O notation) and practical implementation skills but sadly not usage and applying them to real world problems. The TIRA Coders application simulates a fictional coder management system where you load, sort, search, and analyze coder data.

Each task builds on the previous ones, forming a complete portfolio of fundamental data structures — from basic sorting to graph algorithms.

## Tasks

### Task 1 — Insertion Sort

- Implemented **insertion sort** using Java's `Comparable<T>` interface
- Implemented a reverse operation to flip array order
- Measured O(n²) time complexity for sorting, O(n) for reverse

### Task 2 — Linear Search and Sorting

- Implemented **linear search algorithms** (`indexOf`, `findIndex`, `find`) with O(n) complexity
- Added `Comparator<T>`-based sorting as an alternative to `Comparable<T>`
- Analyzed performance of the `SimpleContainer.add` method (O(n) due to linear duplicate check)
- Explored why full-name sorting is slower than codename sorting (longer string comparisons)

### Task 3 — Binary Search

- Implemented **binary search** (O(log n)) on sorted data
- Implemented recursive binary search as an optional task
- Compared linear vs. binary search performance: binary search maintains near-constant lookup time even with 50,000+ records, while linear search scales proportionally
- Analyzed the precondition that data must be sorted for binary search to work

### Task 4 — Stack

- Implemented a **stack** data structure with dynamic array resizing
- Operations: `push` (O(1) amortized, O(n) on resize), `pop` (O(1)), `peek` (O(1)), `clear` (O(1))
- Integrated into a **parenthesis checker** that validates code parentheses, brackets, and braces — including handling of string literals
- Learned how stack is used in real-world parsing scenarios

### Task 5 — Queue

- Implemented two queue variants:
  - **Array-based queue** with dynamic resizing
  - **Linked-list-based queue** using nodes
- Operations: `enqueue` (O(1) amortized), `dequeue` (O(1)), `element` (O(1))
- Analyzed trade-offs: arrays use contiguous memory (better cache locality), linked lists allocate exactly the space needed
- Integrated queues into the TIRA Coders app as a call queue feature

### Task 6 — Fast Sorting Algorithms

- Implemented **three** fast sorting algorithms (optional bonus tasks):
  - **Quicksort** (in-place partitioning)
  - **Mergesort** (recursive, stable)
  - **Heapsort** (heap-based)
- All achieve O(n log n) average-case time complexity
- Compared performance: Mergesort was fastest in practice but uses more memory; Quicksort is simple and in-place; Heapsort was the hardest to understand due to its non-intuitive heap structure

### Task 7 — Binary Search Tree (BST)

- Implemented a **binary search tree** with `TreeNode` nodes
- Operations: `add` (O(h)), `search` (O(h) average, O(n) worst-case), `toSortedArray` (O(n))
- Measured tree depth: balanced tree would be ~log₂(n) deep; my implementation achieved depths like 38 for 100,000 nodes (vs. ideal 17)
- Compared BST vs. array performance: BST won on search and add, arrays won on indexed access (O(1) vs. O(n))
- Integrated BST as an alternative data storage backend in TIRA Coders (switchable via menu)

### Task 8 — Hash Table

- Implemented a **hash table** from scratch with:
  - Custom hash function (no `String.hashCode()`)
  - **Linear probing** and **quadratic probing** for collision resolution
  - Dynamic resizing at 75% load factor
- Operations: `insert` (O(1) average), `search` (O(1) average), with probing overhead increasing as load factor approaches threshold
- Integrated into the TIRA Coders app to count **code word frequencies** — analyzing which keywords appear most in coders' source code
- Compared hash table vs. BST: hash table wins on large datasets for key-value lookups; BST wins on smaller datasets and range queries

### Task 9 — Graph

- Implemented a **graph** data structure using adjacency maps
- Created supporting classes: `Vertex`, `Edge`, `Visit`
- Implemented graph algorithms:
  - **Breadth-First Search (BFS)**
  - **Depth-First Search (DFS)**
  - **Dijkstra's shortest path algorithm** (using `PriorityQueue`)
- Optimized vertex lookup from O(n³) to O(1) using a map-based cache
- Analyzed graph density: test graphs were confirmed to be sparse
- This task allowed Java collection classes (`ArrayList`, `Map`, `Queue`, `Stack`, `Set`, `PriorityQueue`)

## Technologies Used

- **Language:** Java 20
- **Build System:** Maven (JUnit 5 for testing, JSON library for data parsing)
- **UI:** Java Swing (desktop application with menus, panels, log views, and graphs)
- **Testing:** JUnit Jupiter 5.9.0
- **Data Format:** JSON
- **Tools:** VS Code, Git, Excel (for performance analysis charts)

## Architecture

The project follows a layered architecture:

- **Model layer** — Data structures (`SimpleContainer`, `PhoneBookArray`, `PhoneBookBST`, coders, etc.)
- **Student layer** — My implementations (algorithms, data structures, comparators)
- **View layer** — Swing UI (main window, search panels, detail panels, log view, graph view, call queue frame)
- **Utility layer** — Interfaces (`StackInterface`, `QueueInterface`, `TIRAContainer`), helpers (`JSONConverter`, `Pair`)
- **Factory layer** — Object creation (`StackFactory`, `QueueFactory`, `BSTFactory`, `HashTableFactory`)

## Key Takeaways

- Implemented **9 fundamental data structures and algorithms** from scratch without using built-in Java collections
- Gained understanding of **time complexity analysis** — from O(1) to O(n²), O(log n), and O(n log n)
- Built and analyzed **three fast sorting algorithms** (quicksort, mergesort, heapsort)
- Implemented a **binary search tree** and measured real-world depth vs. theoretical balanced depth
- Built a **hash table** with two different collision resolution strategies (linear/quadratic probing)
- Implemented **Dijkstra's shortest path** algorithm on a graph structure
- Learned to **optimize from O(n³) to O(1)** by adding appropriate caching
- Worked within strict constraints (no `ArrayList`, no `Arrays.sort`, etc.) to truly understand data structure internals
- Practiced writing clean, efficient code with proper complexity characteristics
- Used **Excel** for performance visualization and report analysis

## Status

- **Completed:** Yes (course assignments finished. Course grade 5)
- **Maintained:** No (archive — coursework reference)
- **Notes:** This repository serves as a reference for fundamental data structures and algorithms implementations. All code in the `student` package is my own work.

## My Contributions

> *Individual coursework — all tasks completed independently*

## Links

- Course: [Data Structures and Algorithms](https://opas.peppi.oulu.fi/fi/opintojakso/811312A/4550), University of Oulu
- Built with [Maven](https://maven.apache.org/)
- Uses [JUnit 5](https://junit.org/junit5/)
- Uses [org.json](https://github.com/stleary/JSON-java)

## Further Notes

This document was created by giving an AI access to the source code. The report was then edited and verified.