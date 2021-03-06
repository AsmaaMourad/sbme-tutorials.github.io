---
layout: page
course: "sbe201"
category: "notes"
year: "2018"
title:  "Week 4 - Part2: More on Structs, Linked Lists, Naming Conventions, Const-correctness, Build Systems, and Git for Teams"
by: "Asem"
---

* TOC
{:toc}

## Assignment of Week 3

### Part 1

* Issue 1: Command Line Arguments.
* Issue 2: Functions and Calling Functions.
* Issue 3: Header Files and Source Files.

### Part 2

* C++ Feature: `struct`s
* C++ Feature: function overloading
* C++ Feature: Type Alias

## C++ Milestones

<img src="/gallery/c++milestones.jpg" style="width:100%">

<img src="/gallery/c++milestones2.png" style="width:100%">

Compiling C++ of 2003

```terminal
g++ source_code.cpp -o output_name
```

Compiling C++ of 2011

```terminal
g++ -std=c++11 source_code.cpp -o output_name
```

### Other Important g++ Compiler Flag

#### `-Wall` flag

* Your `g++` is compiler is your friend.
* Let your compiler not just reports you errors.
* Let him report you all the **warnings**.
* Fixing **warning** avoids many run-time issues.

Compiling C++ of 2003

```terminal
g++ -Wall source_code.cpp -o output_name
```

Compiling C++ of 2011

```terminal
g++ -Wall -std=c++11 source_code.cpp -o output_name
```


## Assignment of Week 4

### General LL: 11 operations

operations:

1. insertion at front.
2. insertion at back.
3. remove front.
4. remove back.
5. return front.
6. return back.
7. return node at arbitrary index.
8. remove the kth-node.
9. remove nodes with given data, i.e filtaration.
10. is empty?
11. printAll.
12. delete the whole list from the heap.

#### A: LL of Integers

<iframe allowfullscreen src="http://www.algomation.com/embeddedplayer?embedded=true&algorithm=593432766bee1c0400365b82" width="900" height="556" seamless="seamless" frameborder="0" style="border:1px solid lightgray" scrolling="no"></iframe>


```c++
struct IntegerNode
{
    int data;
    IntegerNode *next = nullptr;
};

struct IntegerLL
{
    IntegerNode *front;
};

void insertBack( IntegerLL &list , int data )
{
    // Logic
}

void insertFront( IntegerLL &list, int data  )
{
    // Logic
}

int front( IntegerLL &list )
{
    // Logic
}

int back( IntegerLL &list )
{
    // Logic
}

int getAt( IntegerLL &list , int k )
{
    // Logic
}

void removeBack( IntegerLL &list )
{
    // Logic
}

void removeFront( IntegerLL &list )
{
    // Logic
}

void removeAt( IntegerLL &list , int index )
{
    // Logic
}

void filter( IntegerLL &list , int data )
{
    // Logic
}

bool isEmpty( IntegerLL &list )
{
    // Logic
}

void printAll( IntegerLL &list )
{
    // Logic
}

void clear( IntegerLL &list )
{
    // Logic
}
```

#### B: LL of Characters

<img src="/gallery/dna_array.svg" style="width:400">

<img src="/gallery/dna_ll.svg" style="width:400">


```c++
struct CharNode
{
    char data;
    CharNode *next = nullptr;
};

struct CharLL
{
    CharNode *front = nullptr;
};
```

### Stacks using LL

### Node type

```c++
struct CharNode
{
    char data;
    CharNode* next = nullptr;
};
```

### Stack-LL

In case of LL-based stack, we can represent a stack using only **a single pointer**, i.e the front of the stack.

```c++
struct CharStackLL
{
    CharNode *front = nullptr;
};
```

Initially, when stack is empty, the front pointer points to nothing, i.e `nullptr`.

### Push operation

When we push a new element, we add that element to the front. We make sure that we link the new node correctly.

<iframe allowfullscreen src="http://www.algomation.com/embeddedplayer?embedded=true&algorithm=58a0caa54833c1040095d574" width="900" height="556" seamless="seamless" frameborder="0" style="border:1px solid lightgray" scrolling="no"></iframe>


```c++
void push( CharStackLL &stack , char data )
{
    CharNode *newNode = new CharNode;

    newNode->data = data;
    newNode->next = stack.front;

    stack.front = newNode;
}
```

or, equivalently

```c++
void push( CharStackLL &stack , char data )
{
    CharNode *newNode = new CharNode{ data , stack.front };
    stack.front = newNode;
}
```

or, a DRY solution (optional):

```c++
void push( CharStackLL &stack , char data )
{
    // 1. Make list interface
    CharLL list{ stack.front };

    // 2. DRY
    lists::pushFront( list , data );

    // 3. Update Stack front
    stack.front = list.front; // update the front of the stack
}
```

### Pop operation

When popping an element from the front,

<iframe allowfullscreen src="http://www.algomation.com/embeddedplayer?embedded=true&algorithm=58a0d1144833c1040095d586" width="900" height="556" seamless="seamless" frameborder="0" style="border:1px solid lightgray" scrolling="no"></iframe>


```c++
char pop( CharStackLL &stack )
{
    if( stack.front )
    {
        // Save the value of the front->data, before we delete it from the stack
        char lifo = stack.front->data;
        
        // Save the pointer of the front, so we delete it later
        CharNode *oldFront = stack.front;

        // Update the front of the stack
        stack.front = stack.front->next;

        // Now delete the old pointer
        delete oldFront;

        // Return the lifo
        return lifo;
    }
    else
    {
        // If the stack is empty, make the program to terminate (crash)!
        // The user of this function should have checked if the stack is not empty.
        exit( 1 );
    }
}
```

or DRY solution,

```c++
char pop( CharStack &stack )
{
    // 1. Make list interface
    CharLL list{ stack.front };

    // 2. DRY
    char toPop = lists::front( list );
    lists::removeFront( list );
    
    // 3. Update Stack front
    stack.front = list.front;
    return toPop;
}
```

### Asking a Stack if it is Empty

```c++
bool isEmpty( CharacterStackLL &stack )
{
    if( stack.front == nullptr )
    {
        return true;
    }
    else
    {
        return false;
    }
}
```

Again, could be done that way:

```c++
bool isEmpty( CharacterStackLL &stack )
{
    return stack.front == nullptr;
}
```

### Home Demo

Open [StackLL](https://www.cs.usfca.edu/~galles/visualization/StackLL.html) and play with the stack to realize its behaviour. This demo shows a **Stack** implemented with **linked list**.

## LL-Based Queues

The same node `struct`.

```c++
struct DoubleNode
{
    double data;
    DoubleNode *next;
};
```

And the whole **Queue** is represented by two elements: **Front** (or **Head**) and **Back** (or **Rear**). We can wrap these element in the following `struct`:

```c++
struct DoublesQueueLL
{
    DoubleNode *front = nullptr;
    DoubleNode *back = nullptr;
};
```

### Asking if the LL is Empty

```c++
bool isEmpty( NumbersQueueLL queue )
{
    if( queue.back == nullptr )
    {
        return true;
    }
    else
    {
        return false;
    }
}
```

Or equivalently,

```c++
bool isEmpty( NumbersQueueLL queue )
{
    return queue.back == nullptr;
}
```

### Enqueuing a New Element

As you guessed, new elements will be added to the **back**.

```c++
void enqueue( NumbersQueueLL &queue , double newSample )
{
    if( isEmpty( queue ))
    {
        queue.back = new DoubleNode{ newSample , nullptr };
        queue.front = queue.back;
    }
    else
    {
        DoubleNode *oldBack = queue.back;
        DoubleNode *newBack = new DoubleNode{ newSample , oldBack };
        queue.back = newBack;

        // Short solution: queue.back = new DoubleNode{ newSample , queue.back };
    }
}
```

### Dequeueing an Element

Left for the assignment.

### Bonus: Array-Based Queues

* Trick: Circular Buffer.

## Regular Functions and Methods

```c++
CharStackLL cstack;
```

#### Which is more elegant?

```c++
push( cstack , 'A' );
```

What if we can do

```c++
cstack.push('A');
```

* First version => regular function.
* Second version => method.

#### Procedural Paradigm

```c++
struct CharStackLL
{
    CharNode *front = nullptr;
};

void push( CharStackLL &stack , char data )
{
    CharNode *newNode = new CharNode{ data , stack.front };
    stack.front = newNode;
}
```

#### Object Oriented Paradigm (OOP)

```c++
struct CharStackLL
{
    CharNode *front = nullptr;
    
    void push( char newElement )
    {
        CharNode *newNode = new CharNode{ data , this->front };
        this->front = newNode;
    }
};
```

* A method inside a `struct` has an access to a very special pointer called `this`.
* `this` pointer gives a method access to all the `struct` members.

## Preprocessing

![Compilation](/gallery/compile.gif)

[Preprocessor directives](http://www.cplusplus.com/doc/tutorial/preprocessor/)

* Includes.
* Include guards.
* Macros.
* Macro functions.

```c++
#ifndef MATHEMATICS_HPP
#define MATHEMATICS_HPP

// includes of external headers here

// Your functions here

#endif // MATHEMATICS_HPP
```

[Include guards](https://en.wikipedia.org/wiki/Include_guard)

## Naming Conventions

You should agree with your teammates on naming conventions. This makes your whole project consistent and easier to read.

* No matter what name convention you settle with your team. **Consistency** what matters.
* Descriptive names.

### Variables naming

```c++
int adenine_counter = 0; // all lower_case
```

```c++
int adenineCounter = 0; // camelCase
```

* It is optional for the team.
* But afterwards, the team should be consistent.

### Types naming

`struct` (or equivalently `class`) naming.

```c++
struct IntegerArray // PascalCase
{
    // some data
}
```

* *PascalCase* is usually recommended for `struct` and `class` names.
* `struct` and `class` are the same. They both define new types.

### Functions naming

```c++
int countChar( CharArray &array , char query ) // camelCase
{
    // Logic
}
```

```c++
int count_char( CharArray &array , char query ) // lower_case
{
    // Logic
}
```

### Namespace naming

* recommended to be all *lower\_case*

### Popular Naming Conventions

* [Google](https://google.github.io/styleguide/cppguide.html#Naming)
* [Geosoft](http://geosoft.no/development/cppstyle.html)


## Const Correctness

When passing a *pointer* or *reference* that **should not be modified**, It is very recommended to add `const` qualifier, so you guarantee the used function **won't modify its contents**.

```c++
struct IntegerNode
{
    int data;
    IntegerNode *next = nullptr;
};

struct IntegerLL
{
    IntegerNode *front;
};

void insertBack( IntegerLL &list )
{
    // Logic
}

void insertFront( IntegerLL &list )
{
    // Logic
}

int front( const IntegerLL &list )
{
    // Logic
}

int back( const IntegerLL &list )
{
    // Logic
}

void removeBack( IntegerLL &list )
{
    // Logic
}

void removeFront( IntegerLL &list )
{
    // Logic
}

void removeNode( IntegerLL &list , IntegerNode *node )
{
    // Logic
}

void removeData( IntegerLL &list , int data )
{
    // Logic
}

bool isEmpty( const IntegerLL &list )
{
    // Logic
}

void printAll( const IntegerLL &list )
{
    // Logic
}

void clear( IntegerLL &list )
{
    // Logic
}
```

## Advanced: Intro to Build Systems

[An introduction to build systems by *Marwan Abdellah, Ph.D, Blue Brain Project, EPFL*](http://bme-ws.yolasite.com/resources/Software%20Build%20Systems.pdf)

To access the workshop pages and materials: [The Second Biomedical Engineering Workshop](http://bme-workshop.weebly.com/material.html).

In depth:

<iframe src="https://player.vimeo.com/video/32212195" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
<p><a href="https://vimeo.com/32212195">Introduction to CMake</a> from <a href="https://vimeo.com/kitware">Kitware</a> on <a href="https://vimeo.com">Vimeo</a>.</p>


### Installation

```terminal
sudo apt-get install cmake
```

### Demo

## Git for Teams

<img src="/gallery/distributed.png" style="width:70%">

## Teaser: C++ Awesome GUI

### Summer and Next Year Expectations

#### Agenda

* Object Oriented Programming in C++.
* STL and Qt in more depth.
* GUI using Qt.

<img src="/gallery/Autodesk-Maya-Tips-and-Tricks-7.jpg" style="width:70%">


<img src="/gallery/google-earth-22.jpg" style="width:70%">


#### Patience

<img src="/gallery/knowledge-is-power.jpeg" style="width:90%">

## Quiz Next Tuesday

Prepare for a quiz on **Tuesday of 13/3**. Solving week 4 task qualifies you to solve the quiz.
