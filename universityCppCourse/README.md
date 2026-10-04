# University C++ Course

Coursework for an **intermediate-level C++ programming course** adapted at the University of Oulu from UIUC's CS 225 — Data Structures and Programming Principles. The course covers the complete C++ landscape from basic syntax through advanced data structures, memory management, inheritance, and graphics programming.

## Background & Motivation

This repository contains my solutions for a comprehensive C++ course that progresses from introductory concepts to advanced data structures and algorithms. The course combines lab exercises (smaller, focused tasks) with machine problems (larger projects), each building on the previous. Almost every assignment produces visual output as PNG images — pixel manipulation is a recurring theme throughout the course.

Compilation uses strict flags (`-Wall -Werror -pedantic`), and all labs include Address Sanitizer builds for memory error detection.

## Labs and Machine Problems

### lab_intro — Introduction to C++ Classes & PNG Manipulation

- Implemented a `brighten()` function to add intensity to all RGB channels
- Implemented `blendImages()` for pixel-by-pixel image blending
- Implemented `drawCrosshairs()` to overlay crosshair lines on an image
- Practiced `const` member functions, class construction, and basic image I/O

### lab_debug — Debugging with GDB

- Debugged a buggy `sketchify` program using GDB
- Fixed uninitialized pointer (`PNG *original`) causing segmentation faults
- Corrected loop condition `0 < y < height` (always true in C++)
- Fixed pixel difference logic for proper edge detection output
- Learned systematic debugging of memory errors and pointer issues

### lab_gdb — Linked Lists & GDB

- Implemented a templated **singly-linked list** (`List<T>`) with:
  - `insertFront`, `insertBack` for adding nodes
  - `reverse` — recursive in-place reversal (initially had a segfault bug when `len <= 1`)
  - `shuffle` — perfect shuffle: splits list in half and interleaves nodes
  - `clear` — deallocates all dynamically allocated memory
- Built a forward iterator (`ListIterator`) for traversal
- Debugged with GDB, comparing output PNGs to reference solutions

### lab_inheritance — Inheritance, Virtual Functions & Polymorphism

Explored the full C++ inheritance model across 6 separate test executables:

- **Class hierarchy:** `Drawable` (abstract) → `Shape` (abstract) → `Rectangle`, `Circle`, `Triangle`, `Flower` (composition), `Truck` (composition)
- **test_virtual** — Added `virtual` to `area()`/`perimeter()` to enable polymorphic dispatch
- **test_pure_virtual** — Made `area()`/`perimeter()` pure virtual (`= 0`)
- **test_destructor** — Added virtual destructor to `Shape` preventing memory leaks via base pointer deletion
- **test_slicing** — Fixed missing virtual destructor in `drawable.h`
- **test_constructor** — Fixed `Circle` constructor to properly chain to `Shape(pcenter, pcolor)`
- **Shapes drawn to PNG output:** rectangles, circles, triangles, flowers, and trucks with proper compositing

### lab_memory — Dynamic Memory Management

Built a **student-to-room allocator** that groups students by last-name initial and assigns them to rooms with capacity limits:

- Implemented **Rule of Three** (copy constructor, assignment operator, destructor) for the `Room` class
- Fixed shallow copy bug: `letters = other.letters` → deep copy with `new Letter[max_letters]`
- Fixed `delete` vs `delete[]` in `Room::clear()`
- Added `Allocator` destructor to properly clean up `rooms` and `alpha` arrays
- Fixed initialization order bug where `roomCount` was used before being set
- **Output:** Successful allocation of 237 students into 9 rooms

### lab_dict — Dictionaries & Algorithms

Five programs exploring `std::map` and algorithmic problem-solving:

- **AnagramDict** — Builds a `map<string, vector<string>>` keyed by sorted letter strings; `get_anagrams()` returns matching words from a dictionary of ~25,000 entries
- **Fibonacci** — Implemented recursive Fibonacci + memoized version using function pointers
- **Cartalk Puzzle** — Uses the CMU pronunciation dictionary to find word triples that are homophones (based on the classic Car Talk radio puzzle)
- **CommonWords** — Finds words appearing >= n times across multiple text files (TF-IDF-style filtering)

### lab_hash — Hash Tables

Two complete hash table implementations:

- **Separate Chaining (SCHashTable)** — Array of `std::list<std::pair<K,V>>` buckets. Implemented `remove()`, `resizeTable()` (double to next prime, rehash all entries), `clear()`, `operator[]`
- **Linear Probing (LPHashTable)** — Array of `pair<K,V>*` with open addressing. Implemented `insert()` (linear probe for empty slot), `remove()`, `resizeTable()`, `findIndex()`
- **Hash function:** Bernstein hash for strings, `key % size` for chars
- **Applications:** Character/word frequency counting, anagram detection, and log file parsing (tracking visited URLs per user session)

### mp_intro — Image Rotation & Art

- Implemented `rotate()` — 180-degree rotation of PNG images (`newX = width - x - 1`, `newY = height - y - 1`)
- Implemented `myArt()` — algorithmic art generation with XOR-like quadrant patterns

### mp_lists — Doubly-Linked List (Machine Problem)

Full implementation of a **templated doubly-linked list** with bidirectional iterators and merge sort:

- **Part 1:** Constructor/destructor, `_destroy()`, `insertFront()`, `insertBack()`, `begin()`/`end()` iterators, `tripleRotate()` (wraps every 3-element block left), `split()` (disconnects list after a split point)
- **Part 2:** `reverse()` — in-place via pointer swaps, `reverseNth()` — reverses in blocks of n, `mergeWith()` — merges two sorted lists, `mergesort()`/`sort()` — divide-and-conquer sorting
- **Tested** with Catch2 framework (~20+ unit tests), comparing actual output PNGs to expected solutions

### mp_collage — Collage & Alpha Blending

Built a **layered canvas compositing system**:

- `Canvas::Add()` — appends items to a fixed-size array
- `Canvas::Remove()` — finds and removes an item by pointer
- `Canvas::Swap()` — swaps positions of two items
- `Canvas::draw()` — iterates items in order, applies position/scale transforms, blends pixels using alpha compositing: `result = (source.RGB × source.A + dest.RGB × (255 − source.A)) / 255`
- `CanvasItem::getBlendedPixel()` — per-channel multiplication by item color
- Fixed `Drawable` virtual destructor to prevent memory leaks

### maketutorial — Makefile Tutorial

A self-contained tutorial covering GNU Make fundamentals:

- **hello/** — Basic compilation and intermediate files (preprocessed `.ii`, assembly `.s`)
- **animals/** — Multi-file compilation with custom Makefile rules
- **macro_intro/** — Makefile variables: `CXX`, `FLAGS`, `$@`, `$<`, `$?`
- **file_meddling/** — Dependency chains and phony targets
- **functional_fun/** — Functional programming in pure Make syntax (Fibonacci via recursive macros)

## Technologies Used

- **Language:** C++ (C++17/20)
- **Compiler:** g++ with strict flags (`-Wall -Werror -pedantic`)
- **Libraries:** libpng, LodePNG, Catch2 (testing)
- **Tooling:** Make, GDB, Address Sanitizer, Doxygen
- **Image Manipulation:** Custom `PNG`/`RGBAPixel` wrapper classes, alpha blending, compositing
- **External Data:** CMU Pronouncing Dictionary

## Key Takeaways

- Learned C++ features: templates, inheritance, virtual functions, pure virtual classes, virtual destructors, operator overloading, and the Rule of Three
- Implemented **four data structures from scratch**: singly-linked list, doubly-linked list, separate chaining hash table, and linear probing hash table
- Built a **merge sort** implementation on a linked list structure
- Debugged real memory bugs using **GDB** and **Address Sanitizer** — uninitialized pointers, shallow copies, array/delete mismatches, object slicing
- Practiced **image composition** with alpha blending, layered canvases, and pixel-level transformations
- Worked with **STL containers** (`vector`, `map`, `list`) in practical applications (anagram finding, word frequency, pronunciation puzzles)
- Learned **Makefile** authoring from basic rules to advanced pattern matching and functional constructs
- Fixed **object slicing** issues by understanding how C++ handles polymorphic types by value vs. by reference/pointer
- Applied **memoization** to optimize recursive algorithms (Fibonacci)

## Status

- **Completed:** Yes (course exercises and machine problems finished)
- **Maintained:** No (archive — coursework reference)
- **Notes:** This repository serves as a reference for intermediate-to-advanced C++ programming. The codebase is adapted from UIUC CS 225 materials for the University of Oulu.

## My Contributions

> *Individual coursework — all tasks completed independently*

## Links

- Original course: [CS 225 — Data Structures and Programming Principles](https://courses.grainger.illinois.edu/cs225/fa2026/), University of Illinois at Urbana-Champaign, Adapted for University of Oulu

## Further Notes

This document was created by giving an AI access to the source code and course materials. The report was then edited and verified.