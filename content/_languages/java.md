+++
title = "Java"
path = "java"
in_search_index = true
date = 2024-03-03
[taxonomies]
tags = ["language", "imperative"]
+++
Java is a general purpose programming language designed for enterprise scale software development. It is an object-oriented programming language that offers memory safety and robust deployment via the Java virtual machine (JVM) and its tri-generational garbage collector. Java is about 2x slower than C which is achieved through its multipass just-in-time (JIT) compiler that offers additional optimizations at runtime over the lifecycle of a program. Although not the most popular language across newer students, Java is still widely used across K-12 programs and at universities. This is in large part due to its staying power in corporate and enterprise applications both because of the vast amount of legacy code as well as the wide availability of programmers that can use the language. 

# Entry Point
Java follows the C paradigm of having an explicitly defined `main` method that can be used as the program entry point.

```java
public static void main(String[] args) {
    // code goes here
}
```

# Printing on Standard Output
Java provides the class `System.out` which is a `java.io.OutputStream` and can be used to print output on STDOUT. While `.println()` is the most common method, `System.out` also provides `.print()` and `.printf()` which can be used to print without a newline character or to use formatted printing, respectively. It should be noted that the newline character used by `.println()` is platform dependent.

```java
System.out.println("Hello, World!");
```

# Reading from Standard Input
The preferred approach to reading input from STDIN is to use the `java.io.BufferedReader` class. It provides a `.readLine()` method that can be used to read a single line of input from STDIN. It will parse and consume the next `\n` or `\r\n`. Note that both constructing this class and calling its methods can result in an `java.io.IOException`.

```java
BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
String input = reader.readLine();
```

If the input contains an array of integers, then we can use a combination of the `java.io.BufferedReader` and the `java.util.Arrays` classes to parse the input.

```java
int n = Integer.parseInt(reader.readLine());
int[] arr = Arrays
        .stream(reader.readLine().split(" "))
        .mapToInt(Integer::parseInt)
        .toArray();
```

# Generics
Java, like many languages of its kind, permits the use of _generics_ to subtype a class when the underlying subtype can be treated as opaque (i.e., the structure of the subtype is irrelevant). This is extremely useful for defining various kinds of abstract data types (ADTs), such as Collections. Lists, Queues, Sets, Trees, Maps, etc. rarely care about the underlying representation of the data they contain. They simply care about the structure that they employ to organize the data. As computer scientists and engineers, that's what we care about too.

The following is an example of a generic subtype in Java and its use within methods and when defining variables.

```java
public class MyList<E> {

    public void add(E elt) { ... }

    public E first() { ... }
}

MyList<Integer> list = new MyList<>();
```

The primary intention of generics is to reduce the number of subtypes that need to be implemented for some class when the underlying subtype is opaque. Instead of defining a List for handling Integers and a List for handling Strings, we can define a single List that handles a generic subtype.

The downside of generics--specifically within Java--is that they're implemented extremely poorly. Generic support was not a part of the original language specification and had to be added into the language later, which has been true of most older statically typed languages. In short, the Java engineers at the time could not find an adequate design to include generics into the language and opted for a more shallow design called _type erasure_. After a Java class is compiled, its generic subtype is erased and replaced with the more general _Object_ type. This has two very serious consequences.

First, **Java's static type enforcement is undermined and can no longer guarantee program correctness in all cases.** Classes could already be subtyped with the Object type. This would permit a Collection to have a mixture of different kinds of objects and would place the responsibility on the developer to ensure that only permissible types were used, if such constraints needed to be enforced. The inclusion of a generic type offers the possibility that a Collection could be defined over any type but only include members of the same type. Since Java employs type erasure, this constraint can only be enforced at compile time. Types are typically checked at compile time, so this should be fine. Except the pre-existing language semantics permit subtypes to be ignored when the Object subtype is being used. This leads to a blind spot in Java's type enforcement when subtypes are omitted although it does result in a warning.

Normally, if you attempt to assign a `Collection<A>` to a variable whose type is `Collection<B>`, then the operation should only be permissible if A is a subtype of B. This is permissible because a variable whose type is `Collection<B>` could reasonably hold members of type A, since they are also a subtype of B. However, a generic always indicates a type, such as String or Integer, that is a subtype of Object. Therefore, we shouldn't be able to assign a `Collection<Object>` to a variable whose type is `Collection<String>`, since a Collection of Strings wouldn't reasonably hold members that are other types of Objects, such as Integers. Due to type erasure, this is permissible in Java since a `Collection<String>`is actually a `Collection<Object>` and assigning a `Collection<Object>` to a variable whose type is `Collection<Object>` appears perfectly reasonable. This leads to some extremely strange results that permit a Collection to be both a `Collection<String>` and `Collection<Integer>` simultaneously.

```java
List strings = new ArrayList();
strings.add("hello");
List<Integer> ints = new ArrayList<>();
ints.add(1);
// ints.add("abc"); // not permitted
strings.add("world");
System.out.println(ints); // ["hello", 1, "world"]
```

Second, **Java's performance dramatically declines when using classes that include the generic subtype.** This specifically occurs when using the Object representation of primitives (e.g., Integer, Long, Float, Double, etc.). Since generics are erased into Objects and primitives are not Objects, generics cannot currently support primitives. As such, all primitive values must be _autoboxed_ into an Object when added to a generic Collection and _unboxed_ back into a primitive when removed. This leads to a significant GC load due to the massive population of unnecessary Objects stored in the Collection and the large amount of garbage created through constant boxing and unboxing. In most cases, this performance degradation can be ignored. However, in performance critical software, it cannot.

Furthermore, in competitive programming, transforming an `int[]` into an `ArrayList<Integer>` can be the singular reason why test cases result in a time limit exceeded (TLE). The mem allocations associated with each `int` being autoboxed into an `Integer` plus the GC load to manage those Objects results in a significantly larger running time. Competitive programmers are therefore strongly advised to ignore all Collections provided by the Java standard library as well as any other classes that employ generics until primitives receive generic support. Arrays and non-generic implementations of a Node class should be employed instead.

```java
List<Integer> list = new ArrayList<>(n); // bad
int[] arr = new int[n]; // good
```

