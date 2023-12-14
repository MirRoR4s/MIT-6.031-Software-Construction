# [Reading 2: Basic Java](https://web.mit.edu/6.031/www/sp21/classes/02-basic-java/)

**Objectives**

- Learn basic Java syntax and semantics
- Transition from writing Python to writing Java

**Software in 6.031**

| Safe from bugs                                   | Easy to understand                                           | Ready for change                                  |
| :----------------------------------------------- | :----------------------------------------------------------- | :------------------------------------------------ |
| Correct today and correct in the unknown future. | Communicating clearly with future programmers, including future you. | Designed to accommodate change without rewriting. |

## Getting started with Java

Read the first six sections of **[From Python to Java](http://kennethalambert.com/pythontojava/)** (27 short pages):

- Program Structure and Execution
- Data Types and Expressions
- Simple Statements
- Terminal Input and Output
- Control Statements
- Objects and Interfaces

Then check your understanding by answering some questions about how the basics of Java compare to the basics of Python:

---



## Snapshot diagrams

It will be useful for us to draw pictures of what’s happening at runtime, in order to understand subtle questions. **Snapshot diagrams** represent the internal state of a program at runtime – its stack (methods in progress and their local variables) and its heap (objects that currently exist).

Here’s why we use snapshot diagrams in 6.031:

- To talk to each other through pictures (in class and in team meetings)
- To illustrate concepts like primitive types vs. object types, immutable values vs. unreassignable references, pointer aliasing, stack vs. heap, abstractions vs. concrete representations.
- To pave the way for richer design notations in subsequent courses. For example, snapshot diagrams generalize into object models in 6.170.

Although the diagrams in this course use examples from Java, the notation can be applied to any modern programming language, e.g., Python, JavaScript, C++, Ruby.

The simplest kind of snapshot diagram shows a variable name with an arrow pointing to the variable’s value:

```java
int n = 1;
double x = 3.5;
```

![image-20231128201516308](images/image-20231128201516308.png)

When the value is an object value (as opposed to a primitive value), it is denoted by a circle labeled by its type:

```java
BigInteger val = new BigInteger("1234567890");
```

![image-20231128201553049](images/image-20231128201553049.png)

Snapshot diagram syntax is intended to be flexible, not necessarily showing all the details all the time, so that we can draw simple diagrams that focus on particular aspects of the program state that we want to discuss.

For example, the diagrams at the right are all reasonable ways to display a string variable in a snapshot diagram.

```java
String s = "hello";
```

![image-20231128203934512](images/image-20231128203934512.png)

We may use different diagrams in different contexts. Sometimes we care about the particular value of `s`, and sometimes we don’t. Sometimes we want to emphasize that a `String` is an object value (as opposed to a primitive), and sometimes that isn’t relevant.

When we want to show more detail about an object value, we will write field names inside the object circle, with arrows pointing out to their values.

```java
Point pt = new Point(5, -3);
```

![image-20231128204022597](images/image-20231128204022597.png)

For still more detail, variables can include their declared types. Some people prefer to write `x:int` instead of `int x`, but both are fine.

---

## Mutating values vs. reassigning variables

Snapshot diagrams give us a way to visualize the distinction between changing a variable and changing a value:

- When you assign to a variable or a field, you’re changing where the variable’s arrow points. You can point it to a different value.
- When you change the contents of a mutable object – such as an array or list – you’re changing references inside that value.

#### Reassignment and immutable values

For example, if we have a [`String`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/String.html) variable `s`, we can reassign it from a value of `"a"` to `"ab"`.

```java
String s = "a";
s = s + "b";
```

![image-20231128204229576](images/image-20231128204229576.png)

`String` is an example of an *immutable* type, a type whose values can never change once they have been created. Immutability is a major design principle in this course, and we’ll talk much more about it in future readings.

In a snapshot diagram, when we want to emphasize the immutability of an object like `String`, we draw it with a double border, as shown in the diagram here.

#### Mutable values

By contrast, [`StringBuilder`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/StringBuilder.html) (another built-in Java class) is a *mutable* object that represents a string of characters, and it has methods that change the value of the object:

```java
StringBuilder sb = new StringBuilder("a");
sb.append("b");
```

These two snapshot diagrams look very different, which is good: the difference between mutability and immutability will play an important role in making our code *safe from bugs*.

![image-20231128204426866](images/image-20231128204426866.png)

#### Unreassignable references

Java also gives us immutability for references: variables that are assigned once and never reassigned. To make a reference *unreassignable*, declare it with the keyword `final`:

```java
final int n = 5;
```

![image-20231128204602211](images/image-20231128204602211.png)

In this code, `n` can never be reassigned; it will refer to the value 5 for its entire lifetime. If the Java compiler isn’t convinced that your `final` variable will only be assigned once at runtime, then it will produce a compiler error. So `final` gives you static checking for unreassignable references.

In a snapshot diagram, when we want to focus on reassignability, we will use a double-arrow for unreassignable (`final`) references. The diagram at the below shows an object whose `id` never changes (it can’t be reassigned to a different number), but whose `age` can change.

![image-20231128204658764](images/image-20231128204658764.png)

Note that we can have an *unreassignable reference* to a *mutable value* whose value can change even though we’re pointing to the same object:

```java
final StringBuilder sb = new StringBuilder("a");
sb.append("b");
```

![image-20231128204808105](images/image-20231128204808105.png)

We can also have a *reassignable reference* to an **immutable value** where the value of the variable can change because it can be re-pointed to a different object:

```java
String s = "a";
s = "ab";
```

![image-20231128204849076](images/image-20231128204849076.png)

Double lines help to emphasize the unchangeability of parts of a snapshot diagram. But when reassignability or mutability is obvious, or not relevant to the discussion, we will keep the diagram simple, and just use single-line arrows and object borders.

---

## == vs. equals()

Java has two different ways to test equality of values, depending on whether the values are primitives or objects:

- The `==` operator compares the values of primitives. For example, `5 == 5` returns true, as does `'a' == 'a'`.
- The `.equals()` method compares the values of objects. For example, `"abc".equals("abc")` returns true.

In Python, which lacks a distinction between primitive and object values, you use `==` for *both* of these purposes. This can lead to confusion and mistakes in Java – using the wrong kind of equality for the type you’re trying to compare.

Using `equals()` on primitive types is fortunately easy to catch. `5.equals(5)` produces a static error because Java doesn’t allow calling any method on a primitive type.

But the other mistake, using `==` to compare object values for equality, is much more painful, because `==` is overloaded in Java. When used on object types, `==` tests whether the two expressions refer to the same object in memory. In terms of the snapshot diagrams we’ve been drawing, two references are `==` if their arrows point to the same object bubble. In Python, this operator is called `is`.

So if the state of the program looks like the figure on the right, then:

![image-20231128205330860](images/image-20231128205330860.png)

- `x.equals(y)`

  returns true because x and y have the same characters

- `x == y`

  returns true because x and y point to the same object

- `x.equals(z)`

  returns true because x and z have the same characters

- `x == z`

  returns **false** because x and z point to different objects

As a general rule:

- Use `==` for comparing primitive values, like ints, chars, and doubles.
- Use `equals()` for comparing object values, like lists, arrays, strings, and other objects.

And remember that, in Java:

- `char` values are primitives, representing exactly one character. A `char` literal is always single-quoted, like `'a'`.
- `String` values are objects, represent a string of zero or more characters. A `String` literal is double-quoted, like `"abc"` and `""`.

---

## Java Collections

Read the Collections section of **[From Python to Java](http://kennethalambert.com/pythontojava//)** (6 short pages).

### Lists, Sets, and Maps

**A [Java `List`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/List.html) is similar to a [Python list](https://docs.python.org/3/library/stdtypes.html#sequence-types-list-tuple-range).** A `List` contains an ordered collection of zero or more objects, where the same object might appear multiple times. We can add and remove items to and from the `List`, which will grow and shrink to accomodate its contents.

Example `List` operations:

| Java                      | description                       | Python             |
| :------------------------ | :-------------------------------- | :----------------- |
| `int count = lst.size();` | count the number of elements      | `count = len(lst)` |
| `lst.add(e);`             | append an element to the end      | `lst.append(e)`    |
| `if (lst.isEmpty()) ...`  | test if the list is empty         | `if not lst: ...`  |
| `lst.contains(e)`         | test if an element is in the list | `e in lst`         |

In a snapshot diagram, we represent a `List` as an object with indices drawn as fields:

![image-20231128210258247](images/image-20231128210258247.png)

This list of `cities` might represent a trip from Boston to Bogotá to Barcelona.

**A [`Map`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Map.html) is similar to a [Python dictionary](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict).** In Python, the **keys** of a map must be [hashable](https://docs.python.org/3/glossary.html#term-hashable). Java has a similar requirement that we’ll discuss when we confront how equality works between Java objects.

Example `Map` operations:

| Java                   | description                    | Python           |
| :--------------------- | :----------------------------- | :--------------- |
| `map.put(key, val)`    | add the mapping *key → val*    | `map[key] = val` |
| `map.get(key)`         | get the value for a key        | `map[key]`       |
| `map.containsKey(key)` | test whether the map has a key | `key in map`     |
| `map.remove(key)`      | delete a mapping               | `del map[key]`   |

In a snapshot diagram, we represent a `Map` as an object that contains key/value pairs:

![image-20231128210529179](images/image-20231128210529179.png)

This `turtles` map contains `Turtle` objects assigned to `String` keys: Bob, Buckminster, and Buster.

**A [`Set`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Set.html) is an unordered collection of zero or more unique objects.** Like a mathematical *set* or a [Python set](https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset) – and unlike a `List` – an object cannot appear in a set multiple times. Either it’s in or it’s out. Like the keys of a map, the objects in a Python set must be [hashable](https://docs.python.org/3/glossary.html#term-hashable), and Java has a similar requirement.

Example `Set` operations:

| Java                 | description                         | Python                                |
| :------------------- | :---------------------------------- | :------------------------------------ |
| `s1.contains(e)`     | test if the set contains an element | `e in s1`                             |
| `s1.containsAll(s2)` | test whether *s1 ⊇ s2*              | `s1.issuperset(s2)` `s1 >= s2`        |
| `s1.removeAll(s2)`   | remove *s2* from *s1*               | `s1.difference_update(s2)` `s1 -= s2` |

In a snapshot diagram, we represent a `Set` as an object with no-name fields:

![image-20231128210634901](images/image-20231128210634901.png)

Here we have a set of integers, in no particular order: 42, 1024, and -7.

### Literals

Python provides convenient syntax for creating lists:

```python
lst = [ "a", "b", "c" ]
```

And maps:

```python
map = { "apple": 5, "banana": 7 }
```

**Java does not.** It does provide a literal syntax for arrays:

```java
String[] arr = { "a", "b", "c" };
```

But this creates an *array*, not a `List`. We can use [the utility function `List.of`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/List.html) to create a `List` from arguments:

```java
List.of("a", "b", "c");
```

A `List` created with `List.of` comes with an important restriction: it is immutable! So we can’t add, remove, or replace elements once the list has been created.

Java also provides [`Set.of`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Set.html) for creating immutable sets and [`Map.of`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Map.html) for creating immutable maps:

```java
Set.of("a", "b", "c");
Map.of("apple", 5, "banana", 7);
```

### Generics: declaring List, Set, and Map variables

Unlike Python collection types, with Java collections we can restrict the type of objects contained in the collection. When we add an item, the compiler can perform *static checking* to ensure we only add items of the appropriate type. Then, when we pull out an item, we are guaranteed that its type will be what we expect.

Here’s the syntax for declaring some variables to hold collections:

```java
List<String> cities;        // a List of Strings
Set<Integer> numbers;       // a Set of Integers
Map<String,Turtle> turtles; // a Map with String keys and Turtle values
```

Because of the way generics work, we cannot create a collection of primitive types. For example, `Set<int>` does *not* work. However, as we [saw earlier](https://web.mit.edu/6.031/www/sp21/classes/01-static-checking/#Integer_int), `int`s have an `Integer` wrapper we can use (e.g. `Set<Integer> numbers`).

In order to make it easier to use collections of these wrapper types, Java does some automatic conversion. If we have declared `List<Integer> sequence`, this code works:

```java
sequence.add(5);              // wrap 5 as an Integer object, and append it to the sequence
int second = sequence.get(1); // get the second Integer element, and unwrap it into an int
```

### ArrayLists and LinkedLists: creating Lists

As we’ll see soon enough, Java helps us distinguish between the *specification* of a type – what does it do? – and the *implementation* – what is the code?

`List`, `Set`, and `Map` are all *interfaces*: they define how these respective types work, but they don’t provide implementation code. There are several advantages, but one potential advantage is that we, the users of these types, get to choose different implementations in different situations.

Here’s how to create some actual `List`s:

```java
List<String> firstNames = new ArrayList<String>();
List<String> lastNames = new LinkedList<String>();
```

If the generic type parameters are the same on the left and right, Java can infer what’s going on and save us some typing:

```java
List<String> firstNames = new ArrayList<>();
List<String> lastNames = new LinkedList<>();
```

[`ArrayList`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/ArrayList.html) and [`LinkedList`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/LinkedList.html) are two implementations of `List`. Both provide all the operations of `List`, and those operations must work as described in the documentation for `List`. In this example, `firstNames` and `lastNames` will behave the same way; if we swapped which one used `ArrayList` vs. `LinkedList`, our code will not break.

Unfortunately, this ability to choose is also a burden: we didn’t care how Python lists worked, why should we care whether our Java lists are `ArrayLists` or `LinkedLists`? Since the only difference is performance, for 6.031 *we don’t*.

When in doubt, use `ArrayList`.

If you want to initialize an `ArrayList` or `LinkedList`, you can give it another collection as an argument:

```java
List<String> firstNames = new ArrayList<>(List.of("Huey", "Dewey", "Louie"));
List<String> lastNames = new ArrayList<>(Set.of("Duck"));
```

A key difference between `List.of` and `ArrayList` is mutability. Where `List.of("Huey", "Dewey", "Louie")` produces an immutable list of three strings, `new ArrayList<>(List.of("Huey", "Dewey", "Louie"))` produces a mutable list initialized with those strings. If you need an initialized mutable list, this is simpler than making multiple calls to `add()`.

### HashSets and HashMaps: creating Sets and Maps

[`HashSet`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/HashSet.html) is our default choice for `Set`s:

```java
Set<Integer> numbers = new HashSet<>();
```

Java also provides [sorted sets](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/SortedSet.html) with the [`TreeSet`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/TreeSet.html) implementation.

And for a `Map` the default choice is [`HashMap`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/HashMap.html):

```java
Map<String,Turtle> turtles = new HashMap<>();
```

### Iteration

So maybe we have:

```java
List<String> cities        = new ArrayList<>();
Set<Integer> numbers       = new HashSet<>();
Map<String,Turtle> turtles = new HashMap<>();
```

A very common task is iterating through our cities/numbers/turtles/etc.

In Python:

```python
for city in cities:
    print(city)

for num in numbers:
    print(num)

for key in turtles:
    print("%s: %s" % (key, turtles[key]))
```

Java provides a similar syntax for iterating over the items in `List`s and `Set`s.

Here’s the Java:

```java
for (String city : cities) {
    System.out.println(city);
}

for (int num : numbers) { 
    System.out.println(num);
}
```

Note in the second example above that we declare `int num` rather than `Integer num`, even though `numbers` is a `List<Integer>`. This is because Java automatically converts between `int` and `Integer` anyway, and using the simpler primitive `int` type is preferable to the `Integer` object wrapper wherever possible. For example, `int` equality is simpler and less error-prone, as discussed in [== vs. equals](https://web.mit.edu/6.031/www/sp21/classes/02-basic-java/#_vs_equals) above.

We can’t iterate over `Map`s themselves this way, but we can iterate over the keys as we did in Python:

```java
for (String key : turtles.keySet()) {
    System.out.println(key + ": " + turtles.get(key));
}
```

Under the hood this kind of `for` loop uses an [`Iterator`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Iterator.html), a design pattern we’ll see later in the class.

**Warning**: be careful not to mutate a collection while you’re iterating over it. Adding, removing, or replacing elements disrupts the iteration and can even cause your program to crash. We’ll discuss the reason in more detail in a future class. Note that this warning applies to Python as well. The code below does not do what you might expect:

```python
numbers = [100,200,300]
for num in numbers:
    numbers.remove(num) # danger!!! mutates the list we're iterating over
print(numbers) # list should be empty here -- is it?
```

#### Iterating with indices

If you want to, Java provides different `for` loops that we can use to iterate through a list using its indices:

```java
for (int ii = 0; ii < cities.size(); ii++) {
    System.out.println(cities.get(ii));
}
```

Unless we actually need the index value `ii`, this code is verbose and has more places for bugs to hide (starting at 1 instead of 0, using `<=` instead of `<`, using the wrong variable name for one of the occurrences of `ii`, …) Avoid it if you can.

---

## Enumerations

Sometimes a type has a small, finite set of immutable values, such as:

- months of the year: January, February, …, November, December
- days of the week: Monday, Tuesday, …, Saturday, Sunday
- compass points: north, south, east, west
- available colors: black, gray, red, …

When the set of values is small and finite, it makes sense to define all the values as named constants, which Java calls an **enumeration** and expresses with the `enum` construct.

```java
public enum Month { 
    JANUARY, FEBRUARY, MARCH, APRIL, 
    MAY, JUNE, JULY, AUGUST, 
    SEPTEMBER, OCTOBER, NOVEMBER, DECEMBER;
}

public enum PenColor { 
    BLACK, GRAY, RED, PINK, ORANGE, 
    YELLOW, GREEN, CYAN, BLUE, MAGENTA;
}
```

You can use an enumeration type name like `PenColor` in a variable or method declaration:

```java
PenColor drawingColor;
```

Refer to the values of the enumeration as if they were named static constants:

```java
drawingColor = PenColor.RED;
```

Note that an enumeration is a distinct new type. Older languages, like Python 2 and early versions of Java, tend to use numeric constants or string literals to represent a finite set of values like this. But an enumeration is more typesafe, because it can catch mistakes like type mismatches:

```java
int month = TUESDAY;  //  no error if integers are used
Month month = DayOfWeek.TUESDAY; // static error if enumerations are used 
```

or misspellings:

```java
String color = "REd";        // no error, misspelling isn't caught
PenColor drawingColor = PenColor.REd; // static error when enumeration value is misspelled
```

[Python 3 has enumerations](https://docs.python.org/3.4/library/enum.html), similar to Java’s, though not statically type-checked.

---

## Java API documentation

Previous sections in this reading have a number of links to documentation for classes that are part of the [Java platform API](https://docs.oracle.com/en/java/javase/15/docs/api/).

API stands for *application programming interface*. If you want to program an app that talks to Facebook, Facebook publishes an API (more than one, in fact, for different languages and frameworks) you can program against. The Java API is a large set of generally useful tools for programming pretty much anything.

- [**`java.lang.String`**](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/String.html) is the full name for `String`. We can create objects of type `String` just by using `"double quotes"`.
- [**`java.lang.Integer`**](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/Integer.html) and the other primitive wrapper classes. Java automagically converts between primitive and wrapped (or “boxed”) types in most situations.
- [**`java.util.List`**](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/List.html) is like a Python list, but in Python, lists are part of the language. In Java, `List`s are implemented in… Java!
- [**`java.util.Map`**](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Map.html) is like a Python dictionary.
- [**`java.io.File`**](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/io/File.html) represents a file on disk. Take a look at the methods provided by `File`: we can test whether the file is readable, delete the file, see when it was last modified…
- [**`java.io.FileReader`**](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/io/FileReader.html) lets us read text files.
- [**`java.io.BufferedReader`**](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/io/BufferedReader.html) lets us read in text efficiently, and it also provides a very useful feature: reading an entire line at a time.

Let’s take a closer look at the documentation for [`BufferedReader`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/io/BufferedReader.html). There are many things here that relate to features of Java we haven’t discussed! Keep your head, and focus on the **things in bold** below.

![img](images/bufferedreader-1.png)

At the top right corner is the **search box**. You can use it to jump to a class or interface, or straight to a particular method.

Under the *Class BufferedReader* heading is the *class hierarchy* for `BufferedReader` and a list of *implemented interfaces*. A `BufferedReader` object has all of the methods of all those types (plus its own methods) available to use.

Next we see *direct subclasses*, and for an interface, *implementing classes*. This can help us find, for example, that [`HashMap`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/HashMap.html) is an implementation of [`Map`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/Map.html).

Next up: **a description of the class**. Sometimes these descriptions are a little obtuse, but **this is the first place you should go** to understand a class.

![img](images/bufferedreader-2.png)

If you want to make a new `BufferedReader` the **constructor summary** is the first place to look. Constructors aren’t the only way to get a new object in Java, but they are the most common.

![img](images/bufferedreader-3.png)

Next: **the method summary lists all the methods we can call** on a `BufferedReader` object.

Below the summary are detailed descriptions of each method and constructor. **Click a constructor or method to see the detailed description.** This is the first place you should go to understand what a method does.

Each detailed description includes:

- The **method signature**: we see the return type, the method name, and the parameters. We also see *exceptions*. For now, those usually mean errors the method can run into.
- The full **description**.
- **Parameters**: descriptions of the method arguments.
- And a description of what the method **returns**.

### Specifications

These detailed descriptions are **specifications**. They allow us to use tools like `String`, `Map`, or `BufferedReader` *without* having to read or understand the code that implements them.

Reading, writing, understanding, and analyzing specifications will be one of our first major undertakings in 6.031, starting in a few classes.

---

 

 

 

 

 

 

 

 

 

**✓**Numbers & Strings2/2

 

 

 

 

 

 

 

 

 

 

 

 

 

 

## More practice

If you would like to get more practice with the concepts covered in this reading, you can visit the [question bank](https://qable.mit.edu:8001/practice.html#Basic Java). The questions in this bank were written in previous semesters by students and staff, and are provided for review purposes only – doing them will not affect your classwork grades.

Collaboratively authored with contributions from: Saman Amarasinghe, Adam Chlipala, Srini Devadas, Michael Ernst, Max Goldman, John Guttag, Daniel Jackson, Rob Miller, Martin Rinard, and Armando Solar-Lezama. This work is licensed under [CC BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/).

MIT EECS

  spring 2021 course site archive  |  latest site at [mit.edu/6.031](http://web.mit.edu/6.031/)  |  [accessibility](http://accessibility.mit.edu/)