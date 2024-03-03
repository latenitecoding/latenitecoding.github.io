+++
title = "Java"
path = "java"
in_search_index = true
date = 2024-03-03
[taxonomies]
tags = ["language", "imperative"]
+++
Java is a general purpose programming language designed for enterprise scale
software development. It is an object-oriented programming language that offers
memory safety and robust deployment via the Java virtual machine (JVM) and its
tri-generational garbage collector. Java is about 2x slower than C which is
achieved through its multipass just-in-time (JIT) compiler that offers
additional optimizations at runtime over the lifecycle of a program. Although not the most popular language across newer students, Java is still widely used across K-12 programs and at universities. This is in large part due to its staying power
in corporate and enterprise applications both because of the vast amount of
legacy code as well as the wide availability of programmers that can use the
language.

## Entry Point
Java follows the C paradigm of having an explicitly defined `main` method that
can be used as the program entry point.

```java
public static void main(String[] args) {
    // code goes here
}
```

## Printing on Standard Output
Java provides the class `System.out` which is a `java.io.OutputStream` and can
be used to print output on STDOUT. While `.println()` is the most common method,
`System.out` also provides `.print()` and `.printf()` which can be used to print
without a newline character or to use formatted printing, respectively. It
should be noted that the newline character used by `.println()` is platform
dependent.

```java
System.out.println("Hello, World!");
```

## Reading from Standard Input
The preferred approach to reading input from STDIN is to use the
`java.io.BufferedReader` class. It provides a `.readLine()` method that can be
used to read a single line of input from STDIN. It will parse and consume the
next `\n` or `\r\n`. Note that both constructing this class and calling its
methods can result in an `java.io.IOException`.

```java
BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
String input = reader.readLine();
```

If the input contains an array of integers, then we can use a combination of the
`java.io.BufferedReader` and the `java.util.Arrays` classes to parse the input.

```java
int n = Integer.parseInt(reader.readLine());
int[] arr = Arrays
        .stream(reader.readLine().split(" "))
        .mapToInt(Integer::parseInt)
        .toArray();
```

