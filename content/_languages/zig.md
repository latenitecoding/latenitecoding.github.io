+++
title = "Zig"
path = "zig"
in_search_index = true
date = 2024-03-14
[taxonomies]
tags = ["language", "imperative", "systems"]
+++
Zig is a general purpose systems programming language that advertises an emphasis on **robust**, **optimal**, and **reusable** software. Their advertisement isn't as catchy as _blazingly fast_ and often leads developers to ask "Why zig?" when we have Rust, Go, C, and C++. This language undersells itself and should not be underestimated. While it is still in early development (0.12 at the time of this posting), Zig already has a huge value proposition that no other language can boast. Zig is our first programming language since C that can actually be used as an alternative to C while maintaining C interoperability.

Unlike C, it encourages memory safety (without requiring it) via a language implementation that makes memory safety easier to accomplish while making memory unsafety more difficult to implement. This is a consequence of its core tenets of **no hidden control flow** and **no hidden allocations**. In Zig, you have to be explicit about everything your program is doing which means that classic examples of memory unsafe operations have to be explicitly coded and cannot be a matter of "accident". This in turn means that programmers are less likely to make these kinds of mistakes because they can only make these mistakes intentionally.

It is worth mentioning that explicit is not the same as verbose. For example, Java is a verbose language, but it's not an explicit language. Java is verbose because its high-level features have many associated and optional keywords that have to be selected from each time a method or class is defined. In addition, Java includes highly nested namespaces that lead to longer declarations when employing classes or their methods. This leads to verbose code but not explicit code. A great example of this is the `new` keyword. The `new` keyword is used to instantiate an Object. Other than the choice and implementation of the constructor, you the programmer have no control over what happens to your program when you use the `new` keyword. As such, it is not explicit. Zig is an explicit language. It is not a verbose language. It includes many modern and high-level features that make writing code fun and more convenient, significantly more convenient than writing code in C.

Zig, like C, is fast. It also boasts faster compiler times, which should only get faster throughout the year. C/C++ compile times are famously slow (hours for large projects). One of the main reasons why C compile times are slow are due to its use of a preprocessor, which is used to process macros prior to compiling the source code. This preprocessing step also leads to significant problems in program correctness. It is hard to determine how a macro will affect the program, what macros are currently in effect, and what aspects of the program will change due to macros. Zig does not support macros. It instead supports a novel feature called **comptime**. comptime refers to Zig code that is actually executed at compile time rather than runtime. This means that comptime code has a different executing context than non-comptime code, but comptime code can still be reasoned about because it's still the execution of Zig code. This is huge leap forwards for programming languages compared to traditional macros. One of the many benefits that this provides, for example, is the implementation of generics. Zig has one of the nicest implementations of generics across modern languages, and it is completely supported via comptime. This is something that could not otherwise be done in C.

# Entry Point
Zig uses the traditional C style entry point with an explicit main function. All functions in Zig are private by default so the main function needs to be explicitly listed as public. Otherwise, it cannot be used as an entry point and the compiler will complain.

```zig
pub fn main() !void {
    // code goes here
}
```

The `!void` type is likely strange to many developers. Newer programming languages are incorporating what are called _monads_, or _algebraic types_, into their language. The two main algebraic types that are being included are the **Optional** type and the **Result** type. The Optional type is used to specify whether a value can be null. The Result type is used to specify whether an error can occur in the function. Zig uses a shorthand for these two algebraic types. If a value could be null, you'll see a `?` next to the type. For example, the type `?i32` means that the value could be a 32-bit signed integer or null. If the function could error, then you'll see a `!` next to the type. The `!void` type means that the main function could error or terminate without returning a value.

# Printing on Standard Output
As mentioned earlier, Zig is explicit. One great example of this is printing in Zig. In most languages, printing is very concise. In Zig, you need to specify what you're going to use to print. In most cases, you'll just be using the STDOUT handle of your process. The STDOUT handle can be accessed via the standard library, which has to be explicitly imported. From the standard library, you can access the writer associated with STDOUT to print text to console. There is no `println` in Zig, and all printing is formatted by default.

```zig
const std = @import("std");

pub fn main() !void {
    try std.io.getStdOut().writer().print("Hello World!\n", .{});
    std.debug.print("Hello World!\n", .{}); // concise but only for debugging
}
```

### _Buffered Writer_
A slightly better way to print is actually using a buffered writer, which can buffer the output before executing any system calls to print the text to console. The output is buffered into an array. Zig provides a factory method to produce a buffered writer with a 4KB buffer.

```zig
const std = @import("std");

pub fn main() !void {
    var stdout = std.io.bufferedWriter(std.io.getStdOut().writer());
    try stdout.writer().print("Hello World!\n", .{});
    try stdout.flush();
}
```

# Reading from Standard Input
Reading from STDIN requires a similar approach to printing on STDOUT. You'll first need to acquire a reader for the STDIN handle, which comes from the standard library. You can use that reader to parse input up to a specific delimiter of your choice. The most common choice is the newline character for parsing by lines. The reader is used to parse input from STDIN into a byte buffer. Similar to C, you'll need to declare this buffer yourself. The most paralyzing aspect of this for new programmers is choosing the size of the buffer because you could choose anything and you don't typically know exactly how much content you're going to receive. If you're not sure, you can always choose 4096 (4KB) since that's the default size that Zig uses for its buffers anyway.

```zig
const std = @import("std");

pub fn main() !void {
    var buffer: [4096]u8 = undefined;
    var line = try std.io.getStdIn().reader().readUntilDelimiterOrEof(&buffer, '\n');
}
```

The `buffer = undefined` likely looks strange. This kind of syntax isn't common and other languages that use this syntax, like JavaScript, mean something different by `undefined`. In Zig, when a variable is declared as undefined, you're explicitly declaring that the variable's data is undefined, not that the variable itself is undefined. In this case, `var buffer: [4096]u8 = undefined;` means that the buffer exists and all 4096 indices of the byte buffer have data, but you don't know what that data is. The buffer does have data. It just hasn't been defined.

Most programmers are used to having default values associated with their data types. For example, a new byte buffer would be allocated as an array of \\(0\\)'s. Default values are very convenient, but they can also be misleading. The language isn't able to magically allocate an array and have every element be defined as \\(0\\). It first allocates the array and then sets each element to the default value by iterating over the array. Languages that provide default values are typically optimized to do this very quickly, but the process is still the same nonetheless.

In practice, memory that's been used isn't typically "cleaned up". Imagine that you have an array of primes. That array of primes lives somewhere in memory. When the program terminates, it doesn't zero out that array by replacing each element with a \\(0\\). The array of primes is simply just left there. It will continue to just sit in memory until overwritten by some other program that uses that memory. It's possible that you might execute the program again and the program allocate an array for primes exactly over the previous region that was used to hold the primes. In which case, this new array would already contain its primes without the program having to compute any of them again. However, this would be happy coincidence. A program has no way to guarantee that kind of behavior. The underlying data that the array sits on top of could just as easily be some other array of integers, the instructions for a "Hello, World!" program, or a letter to your mom. The point is that every allocated array sits on top of _some_ underlying data. But the program doesn't know what that data is. That data is _undefined_.

To drive that point home, **it's not that the data is undefined because you marked it as undefined in Zig. Zig has you mark it as undefined because the data is actually undefined.** Zig is having you be explicit about what the data is so that you acknowledge what it is that you're working with. Operating on undefined data accidentally is a common source of bugs especially for programmers that are used to having default values.

### _Buffered Reader_
Just as there is a buffered writer for writing, there is a buffered reader for reading. You'll still need to allocate a byte buffer yourself to hold the input content. The purpose of the buffered reader isn't to give you a default buffer for the message but to reduce system calls associated with reading from STDIN, which are typically slower than copying from one array to the other. The buffered reader will reuse its buffer for consecutive reads whereas you may want to store the content from each read in a separate buffer. Hence why you still need to allocate your own buffer to hold the input content.

```zig
const std = @import("std");

pub fn main() !void {
    var stdin = std.io.bufferedReader(std.io.getStdIn().reader());
    var buffer: [4096]u8 = undefined;
    try msg = try stdin.reader().readUntilDelimiterOrEof(&buffer, '\n');
}
```

The `msg` in this case is a _slice_ into the provided buffer. A slice is like a subarray. It points to a location in an array and has a length, which includes some set of contiguous elements from the array. Slices are a much more convenient way of dealing with portions of arrays. The slice `msg` includes the content that was read from STDIN but not the delimiter that was used. The input content and the delimiter are both in the provided buffer though.
