# CS 1501 – Lab #5 (Airline System)[^1]

## Table of Contents

- [Overview](#overview)
- [Directed Graphs](#directed-graphs)
- [AirlineSystem.java](#airlinesystemjava)
- [Testing](#testing)
- [Notes](#notes)

## Overview

* __Purpose__: In this lab, you will complete the implementation of __`AirlineSystem.java`__,
a program for simple airline route management using directed graphs.

A starter project has been provided in this repository with the following
directory contents.

```bash
├── AirlineSystem.java
├── README.md
├── docs/
├── img/
├── data1.txt
├── data2.txt
├── output1.txt
└── output2.txt
```

__`AirlineSystem.java`__ reads in an airline route data file (ex. `data1.txt`), constructs a directed graph of cities and routes, and prints the graph's adjacency matrix similar to the sample output files (ex. `output1.txt`).


 In addition to the project files in the root directory, you can refer to the
 __`javadoc`__ documentation of the __`AirlineSystem`__ class inside
 __`docs/index.html`__ (you may safely disregard any other file within the
 __`docs/`__ subdirectory). It is an auto-generated class description webpage
 intended to provide neatly structured API documentation for developers using the
 contents of __`@`__-tagged comment blocks within the source code. This is strictly
 optional, however, and sometimes it's easier to just read the comment blocks
 directly.

There are a total of __two__ code implementation tasks, each defined inside
numbered __`@TODO`__ objective comment blocks.

__`AirlineSystem.java`__ will compile into an executable class file that provides a simple filename prompt. From this prompt, you can test the correctness of your implementation against `data1.txt` and `data2.txt`, which should each match `output1.txt` and `output2.txt`, respectively.

## Directed Graphs

A directed graph (Digraph) can be represented as a set of vertices and edges (just like undirected graphs), but each edge will be an ordered pair __(u,v)__ where an edge from __u__ to __v__ exists. Unless explicitly stated otherwise, the opposite direction cannot be assumed.

A Digraph can be stored through either an adjacency matrix or an adjacency list.
We will use the following Digraph with unweighted edges to demonstrate the two methods.

<center>

![digraph1 visualization](./img/digraph1.png "digraph1")

</center>

<p align = "center">
Visualization of Digraph 1
</p>

Our sample Digraph can be defined as __$G = (V,E)$__, where $V = \{1,2,3,4,5\}$ and $E = \{(1,2),(1,3),(2,3),(3,4),(4,5),(5,4)\}$. When storing a directed graph in computer memory, we can combine the two definitions into a single data structure that maps the neighbors (connected by an edge) of each vertex in the graph.

An __Adjacency Matrix__ achieves this by creating an __N x N__ matrix where __N = |V|__. Each item in the matrix will denote whether there exists an edge from vertex __row__ to vertex __col__, with __1__ meaning edge exists and __0__ otherwise.

<center>

|   | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| 1 | 0 | 1 | 1 | 0 | 0 |
| 2 | 0 | 0 | 1 | 0 | 0 |
| 3 | 0 | 0 | 0 | 1 | 0 |
| 4 | 0 | 0 | 0 | 0 | 1 |
| 5 | 0 | 0 | 0 | 1 | 0 |

</center>

<p align = "center">
Adjacency Matrix for Digraph 1
</p>

Having an Adjacency Matrix allows for O(1) lookup for whether an edge is present from one vertex to another. However, it will always take up O($V^2$) space in memory, which might not be optimal for storing sparse graphs, where the number of vertices far outweighs the number of directed edges. Also, to enumerate all neighbors of a vertex, it takes $\Theta(V)$ time.

An __Adjacency List__ is a length-$V$ array with each element at index __`i`__ storing a pointer to a list of vertex __`i`__'s neighbors.

<center>

![adjlist1 visualization](./img/adjlist1.jpg "adjlist1")

</center>

<p align = "center">
Adjacency List for Digraph 1
</p>

Unlike an Adjacency Matrix, an Adjacency List is space-efficient since it only stores information on existing edges. Enumerating the neighbors of a vertex __$u$_ is also more efficient; it takes at most $O(deg(u))$, where $deg(u)$ is the outward degree of vertex __$u$__.

The downside of an Adjacency List is that simply checking whether edge __(u,v)__ exists is $O(deg(u))$, which could end up being $O(V)$ for dense graphs.



## AirlineSystem.java

In this lab, we will use an Adjacency List to build and store our Digraph object. This is better suited for later printing out all available destinations from each city (which is just print-walking the Adjacency List).

The __`Digraph`__ and __`DirectedEdge`__ inner classes are completed for you. You will use these to parse an airline route data file into a complete Digraph and finally print the Digraph out to the console.


## Testing

Compile the completed Java file from your `lab5` directory. Once compiled, you can run the class file without additional command-line arguments.

```bash
$ cd #YOUR_LAB5_DIRECTORY
$ javac AirlineSystem.java
$ java AirlineSystem

```
Sample execution using `data1.txt`

```
Please enter graph filename:
data1.txt
Pittsburgh: Erie  Johnstown  Harrisburg  Philadelphia
Erie:
Altoona: Johnstown  Harrisburg
Johnstown:
Harrisburg: Philadelphia  Reading
Philadelphia: Scranton  Allentown
Scranton:
Reading: Allentown  Pittsburgh
Allentown:
```
To run the program directly from VS Code, [you'll need to configure the launch.json file to take command-line arguments](https://code.visualstudio.com/docs/editor/debugging).

## Notes

When reading in the number of vertices from a data file (first integer on the first line) using __`fileScan.nextInt()`__, you may want to call a __`fileScan.nextLine()`__ before extracting city names. This is because __`Scanner.nextInt()`__ does not read the newline character of the input line. You must manually call __`Scanner.nextLine()`__ to read the newline character and move on to the next line.

```java
int count = fileScan.nextInt();
System.out.println("Count:" + count);

fileScan.nextLine();

String city1 = fileScan.nextLine();
System.out.println("city1:" + city1);
```

Alternatively, you can simply call __`fileScan.nextLine()`__ to get the numeric string of the first line and parse the string into an integer.

```java
String countstr = fileScan.nextLine();
System.out.println("countstr:" + countstr);

int count = Integer.parseInt(countstr);
System.out.println("Count:" + count);

String city1 = fileScan.nextLine();
System.out.println("city1:" + city1);
```

## Submission

- The only file you can modify is `AirlineSystem.java`.
- `commit` and `push` the finished code to your GitHub repository. You can check the following [page](https://code.visualstudio.com/docs/sourcecontrol/github) to set up GitHub access directly from VS Code. If you think the commit command hangs in VS Code, it probably asks for a commit message in an open file. You would then need to enter a message and close the file before it can commit to GitHub.
- Submit your Github repository to GradeScope for automatic grading.
  
Note: If you use an IDE, such as NetBeans, Eclipse, or IntelliJ, to develop your programs, make sure the programs will compile and run on the command line before submitting – this may require some modifications to your program (e.g., removing package information).
[^1]: Contributed by [Alex Zhou](https://github.com/yuz727)
