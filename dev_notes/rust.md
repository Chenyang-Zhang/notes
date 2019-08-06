...menustart

 - [Rust](#f5e265d607cb720058fc166e00083fe8)
 - [3. Common Programming Concepts](#33aeb9ff32fc92be604650a5753b4688)
     - [3.3 Variables and Mutability](#e2799508d28e790ecf602efff80d8525)
         - [Difference Between Variables and Constants](#afc26594fed3ec8c6e3e666642c9f62c)
         - [Shadowing](#fd170458f72897a30012683701ae874d)
     - [3.2 Data Types](#979c67baedaf7f34e1c39f5c91294bbe)
     - [3.3 How Functions Work](#818a10091ae4990a5982cd785351aad7)
     - [3.4 Control Flow](#f47fa56fa78316a8b2ce7abd9a7228b1)
         - [if expression](#4ef066d4aafd6c98284f5140e0daa65d)
             - [Using if in a let statement](#fa5a5a88a5c1b046ee672a674d71abdf)
         - [Loop](#89d7b10cb4238977d2b523dfd9ea7745)
 - [4. Understanding Ownership](#703a0f194d8018e9bd046f1d66eb8a0b)
     - [4.1 What is Ownership ?](#c8483d2e6e2023448ea8f4e9853fddf5)
         - [Memory and Allocation](#e4dbc79a1b693c3c9b0a60c0a844e2ca)
         - [Ways Variables and Data Interact: Move](#cdf87cffc74af0e837714390ee553425)
         - [Ways Variables and Data Interact: Clone](#116fd707a848a34e7bf63f1d0c4d0cef)
         - [Stack-Only Data: Copy](#a7f9c89fa8462ca2bf517b37cc846534)
         - [Ownership and Functions](#5dba9b59a6ac8da65f7589c266c2616a)
         - [Return Values and Scope](#078919842dd64fe430dfde470a4c29a5)
     - [References and Borrowing](#7a94ffa954e34e50e159527e51367810)
         - [Mutable References](#85e175e3ace6bb081700c58322910831)
         - [Dangling References](#5ab994e9e78665eb0c26226dd2e695de)
         - [The Rules of References](#b172e4bff02b5a9a4db1943ce0c725d0)
     - [4.3 Slices](#9b0c5c583b42825a78e22070349106d4)
         - [String Slices](#4802b309597c38a0e315a41088f5e7ce)
         - [String Literals Are Slices](#0326ddde0b60afd7cabed1dfa526d3b2)
         - [String Slices as Parameters](#33d042b567d55b78fd644d49a29c1e9f)
         - [Other Slices](#0d42b1fd2a5b5b7309665776c9f05990)
         - [Summary](#290612199861c31d1036b185b4e69b75)

...menuend


<h2 id="f5e265d607cb720058fc166e00083fe8"></h2>

-----
-----

# Rust

https://doc.rust-lang.org/book/second-edition/ch03-03-how-functions-work.html


```rust
$ cargo new hello_cargo --bin
$ cargo build [--release]
$ cargo run
```


```rust
let mut guess = String::new();
io::stdin().read_line(&mut guess).expect(...);
println!("You guessed: {}", guess);
```

 - The & indicates that this argument is a reference, which gives you a way to let multiple parts of your code access one piece of data without needing to copy that data into memory multiple times.
 - references are immutable by default.  Hence, we need to write &mut guess rather than &guess to make it mutable.


 - Handling Invalid Input

```rust
let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,
};
```

<h2 id="33aeb9ff32fc92be604650a5753b4688"></h2>

-----
-----

# 3. Common Programming Concepts

<h2 id="e2799508d28e790ecf602efff80d8525"></h2>

-----

## 3.3 Variables and Mutability

```rust
let foo = 5; // immutable
let mut bar = 5; // mutable
```

<h2 id="afc26594fed3ec8c6e3e666642c9f62c"></h2>

-----

### Difference Between Variables and Constants

 - use `const` keyword
 - the type of the value must be annotated. 
 - Constants can be declared in any scope, including the **global** scope, 
    - which makes them useful for values that many parts of code need to know about.
 - constants may only be set to a constant expression 
    - not the result of a function call or any other value that could only be computed at runtime.

```rust
const MAX_POINTS: u32 = 100_000;
```

<h2 id="fd170458f72897a30012683701ae874d"></h2>

-----

### Shadowing 

 - rust support variable shadow 


```rust
let mut guess = String::new(); 
...
let guess: u32 = guess.trim().parse() ...
```
 
<h2 id="979c67baedaf7f34e1c39f5c91294bbe"></h2>

-----

## 3.2 Data Types

 - scalar types
    1. integers, 
        - `(i|u)(8|16|32|64|size)`
            - i32 is rust default 
            - isize/usize similar to size_t in c++? 
        - 0xFF , 0o77 , 0b1111_0000 , b'A' (u8 only)
    2. floating-point numbers, 
        - f32
        - f64 , default
    3. booleans, 
        - bool:  true/false
    4. and characters.
        - char type represents a Unicode Scalar Value
        - which means it can represent a lot more than just ASCII.
        - Unicode Scalar Values range from U+0000 to U+D7FF and U+E000 to U+10FFFF inclusive.

```rust
let c = 'z' ;
let cc = '☺' ;
```

 - Compound types 
    - tuples
        - element can be different type
    - arrays.
        - every element of an array must have the same type.
        - have a fixed length: once declared, they cannot grow or shrink in size.

```rust
// Grouping values into tuples
let tup: (i32, f64, u8) = (500, 6.4, 1);
let tup = (500, 6.4, 1);
// destructuring
let (x, y, z) = tup;
// access a tuple element directly 
let six_point_four = x.1;
```

```rust
// Arrays
let a = [1, 2, 3, 4, 5];
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
let second = a[1];
```

<h2 id="818a10091ae4990a5982cd785351aad7"></h2>

-----

## 3.3 How Functions Work 


```rust
fn another_function(x: i32, y: i32) {
    ...
}
```


 - In function signatures, you must declare the type of each parameter
 - statement does not return a value
    - you can write such like `x = y = 6` in C
 - The block that we use to create new scopes, {}, is an expression

```rust
fn main() {
    let x = 5;

    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {}", y);  // 4
}
```

 - Note the `x + 1` line without a semicolon at the end
    - Expressions do not include ending semicolons 
    - in C you should add `;` , and surround entrire `{  ... }` with `( )` 
 - Function with return value 
    - the return value of the function is synonymous with the value of the final expression
    - You can return early from a function by using the *return* keyword and specifying a value
    - but most functions return the last expression implicitly.

```rust
fn plus_one(x: i32) -> i32 {
    // Implicit return (no semicolon)
    x + 1
}
```

<h2 id="f47fa56fa78316a8b2ce7abd9a7228b1"></h2>

-----

## 3.4 Control Flow

<h2 id="4ef066d4aafd6c98284f5140e0daa65d"></h2>

-----

### if expression 

 - yes, it is an expression

```rust
    if number < 5 {
        ...
    } else if number > < 10 {
        ...
    } else {
        ...
    }
```

 - close to golang 

<h2 id="fa5a5a88a5c1b046ee672a674d71abdf"></h2>

-----

#### Using if in a let statement

 - Because if is an expression, we can use it on the right side of a let statement
    - must be the same type

```rust
let number = if condition {
    5
} else {
    6
};
```

<h2 id="89d7b10cb4238977d2b523dfd9ea7745"></h2>

-----

### Loop 

```rust
loop {
    println!("again!");
}

while number != 0 {
    println!("{}!", number);

    number = number - 1;
}

let a = [10, 20, 30, 40, 50];
for element in a.iter() {
    println!("the value is: {}", element);
}

for i in 0u32..10 {
    print!("{} ", i); // 0 1 2 3 4 5 6 7 8 9
}

for number in (1..4).rev() {
    println!("{}!", number); // 3!  2!  1!
}

```

<h2 id="703a0f194d8018e9bd046f1d66eb8a0b"></h2>

-----
-----

# 4. Understanding Ownership

 - most unique feature
 - enables Rust to make memory safety guarantees without needing a garbage collector.
 - several related features : borrowing, slices, and how Rust lays data out in memory.

<h2 id="c8483d2e6e2023448ea8f4e9853fddf5"></h2>

-----

## 4.1 What is Ownership ?

 - All programs have to manage the way they use a computer’s memory
    - Some languages have garbage collection
    - other languages explicitly allocate and free the memory
 - Rust uses a third approach: 
    - memory is managed through a system of ownership , with a set of rules that the compiler checks at compile time. 
    - No run-time costs are incurred for any of the ownership features.

<h2 id="e4dbc79a1b693c3c9b0a60c0a844e2ca"></h2>

-----

### Memory and Allocation

 - In the case of a string literal ( "hello") , we know the contents at compile time so the text is hardcoded directly into the final executable
    - making string literals fast and efficient
 - With the *String* type ( String::from("hello") ), in order to support a mutable, growable piece of text, we need to allocate an amount of memory on the heap to hold the contents. 
    - unknown at compile time
    - The memory must be requested from the operating system at runtime.
        - done by `String::from`
    - We need a way of returning this memory to the operating system when we’re done with our *String*.
        - Rust takes a different path: the memory is automatically returned once the variable that owns it goes out of scope.

```rust
{
    let s = String::from("hello"); 
    // s is valid from this point forward

    // do stuff with s
} // this scope is now over, and s is no
  // longer valid
```

 - Note: In C++, this pattern of deallocating resources at the end of an item’s lifetime is sometimes called Resource Acquisition Is Initialization (RAII). 
 - The `drop` function in Rust will be familiar to you if you’ve used RAII patterns.
    - when a variable goes out of scope, Rust automatically calls the drop function and cleans up the heap memory for that variable
 - The example above is simple. But the behavior of code can be unexpected in more complicated situations when we want to have multiple variables use the data we’ve allocated on the heap. 

<h2 id="cdf87cffc74af0e837714390ee553425"></h2>

-----

### Ways Variables and Data Interact: Move

```rust
let x = 5;
let y = x;
```

 - based on our experience with other languages:
    - “Bind the value 5 to x; then make a copy of the value in x and bind it to y.”
 - This is indeed what is happening because integers are simple values with a known, fixed size, and these two 5 values are pushed onto the stack.

```rust
let s1 = String::from("hello");
let s2 = s1;
```

![](../imgs/rust_string_copy.png)

 - only the *String* data copied , meaning we copy the pointer, the length, and the capacity that are on the stack. 
 - We do not copy the data on the heap that the pointer refers to.
 - if Rust instead copied the heap data as well , the operation s2 = s1 could potentially be very expensive in terms of runtime performance if the data on the heap was large.
 - Earlier, we said that when a variable goes out of scope, Rust automatically calls the drop function and cleans up the heap memory for that variable.
    - This is a problem: when s2 and s1 go out of scope, they will both try to free the same memory. This is known as a double free error and is one of the memory safety bugs we mentioned previously. 
 - In Rust. Instead of trying to copy the allocated memory, Rust considers s1 to no longer be valid and therefore, Rust doesn’t need to free anything when s1 goes out of scope. 

```rust
let s1 = String::from("hello");
let s2 = s1;

// You’ll get an error like this because 
// Rust prevents you from using the invalidated reference:
println!("{}, world!", s1); 
```

 - So , String copy is like a "shallow copy" , not a “deep copy” .  
 - But because Rust also invalidates the first variable, instead of calling this a shallow copy, it’s known as a **move**.
 - Here we would read this by saying that s1 was moved into s2. So what actually happens is :

![](../imgs/rust_string_move.png)

 - That solves our problem! With only s2 valid, when it goes out of scope, it alone will free the memory, and we’re done.
 - In addition, there’s a design choice that’s implied by this: 
    - Rust will never automatically create “deep” copies of your data. 
    - Therefore, any *automatic* copying can be assumed to be inexpensive in terms of runtime performance.


<h2 id="116fd707a848a34e7bf63f1d0c4d0cef"></h2>

-----

### Ways Variables and Data Interact: Clone 

 - If we do want to deeply copy the heap data of the *String*, not just the stack data, we can use a common method called *clone*.

```rust
let s1 = String::from("hello");
let s2 = s1.clone();
println!("s1 = {}, s2 = {}", s1, s2);
```

<h2 id="a7f9c89fa8462ca2bf517b37cc846534"></h2>

-----

### Stack-Only Data: Copy

```rust
let x = 5;
let y = x;

println!("x = {}, y = {}", x, y);
```

 - this code seems to contradict what we just learned
 - we don’t have a call to clone, but x is still valid and wasn’t moved into y.
 - The reason is that types like integers that have a known size at compile time are stored entirely on the stack.
    - so copies of the actual values are quick to make. 
    - That means there’s no reason we would want to prevent x from being valid after we create the variable y. 
    - In other words, there’s no difference between deep and shallow copying here, so calling *clone* wouldn’t do anything differently from the usual shallow copying and we can leave it out.

 - Rust has a special annotation called the *Copy* trait that we can place on types like integers that are stored on the stack 
    - If a type has the *Copy* trait, an older variable is still usable after assignment. 
 - Rust won’t let us annotate a type with the *Copy* trait if the type, or any of its parts, has implemented the *Drop* trait.
 - So what types are *Copy*? 
    - You can check the documentation for the given type to be sure
    - general rule, any group of simple scalar values can be *Copy*
    - and nothing that requires allocation or is some form of resource is *Copy*
 - Here are some of the types that are *Copy*:
    - All the integer types, like u32.
    - The boolean type, bool, with values true and false.
    - All the floating point types, like f64.
    - Tuples, but only if they contain types that are also Copy. 
        - (i32, i32) is Copy
        - but (i32, String) is not.
    - what about list ?

<h2 id="5dba9b59a6ac8da65f7589c266c2616a"></h2>

-----

### Ownership and Functions 

 - The semantics for passing a value to a function are similar to assigning a value to a variable
 - Passing a variable to a function will move or copy , just like assignment. 

<h2 id="078919842dd64fe430dfde470a4c29a5"></h2>

-----

### Return Values and Scope

 - Returning values can also transfer ownership

```rust
fn gives_ownership() -> String { 
    // gives_ownership will move its
    // return value into the function
    // that calls it.
    let some_string = String::from("hello");
    // some_string is returned and
    // moves out to the calling function
    some_string 
}

// takes_and_gives_back will take a String and return one.
fn takes_and_gives_back(a_string: String) -> String  {
    // a_string is returned and moves out to the calling function.
    a_string
}

fn main() {
    // // gives_ownership moves its return value into s1
    let s1 = gives_ownership();   
    let s2 = String::from("hello");
    // s2 is moved into takes_and_gives_back, which also
    // moves its return value into s3.
    let s3 = takes_and_gives_back(s2);  
}
// Here, s3 goes out of scope and is dropped.
// s2 goes out of scope but was moved
// s1 goes out of scope and is dropped.
```

 - Taking ownership and then returning ownership with every function is a bit tedious
 - What if we want to let a function use a value but not take ownership?
 - It’s quite annoying that anything we pass in also needs to be passed back if we want to use it again, in addition to any data resulting from the body of the function that we might want to return as well.
 - It’s possible to return multiple values using a tuple, like this:

```rust
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() returns the length of a String.

    (s, length)
}
```

 - But this is too much ceremony and a lot of work for a concept that should be common.
 - ust has a feature for this concept, and it’s called references.

<h2 id="7a94ffa954e34e50e159527e51367810"></h2>

-----

## References and Borrowing 

 - 前面返回 tuple的例子的问题是， we have to return the String to the calling function so we can still use the String after the call to calculate_length， because the String was moved into calculate_length.
 - Here is how you would define and use a calculate_length function that has a *reference* to an object as a parameter instead of taking ownership of the value:

```rust
fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1);
    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()
}
```

 - `&` are *references*, and they allow you to refer to some value without taking ownership of it.

![](../imgs/rust_references.png)

 - We call having references as function parameters *borrowing*.
    - As in real life, if a person owns something, you can borrow it from them. When you’re done, you have to give it back.
 - So what happens if we try to modify something we’re borrowing?

```rust
fn main() {
    let s = String::from("hello");
    change(&s);
}

fn change(some_string: &String) {
    // compile error !!!
    some_string.push_str(", world");
}
```

 - Just as variables are immutable by default, so are references. We’re not allowed to modify something we have a reference to.

<h2 id="85e175e3ace6bb081700c58322910831"></h2>

-----

### Mutable References

```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

 - Now it works
 - But mutable references have one big restriction: 
    - you can only have one mutable reference to a particular piece of data in a particular scope. 
    - This code will fail:

```rust
let mut s = String::from("hello");

let r1 = &mut s;
let r2 = &mut s;
// cannot borrow `s` as mutable more than once at a time
```

 - This restriction allows for mutation but in a very controlled fashion.
 - The benefit of having this restriction is that Rust can prevent data races at compile time.
 - A *data race* is a particular type of race condition in which these three behaviors occur:
    1. Two or more pointers access the same data at the same time.
    2. At least one of the pointers is being used to write to the data.
    3. There’s no mechanism being used to synchronize access to the data
 - As always, we can use curly brackets to create a new scope, allowing for multiple mutable references, just not simultaneous ones:

```rust
let mut s = String::from("hello");
{
    let r1 = &mut s;
} // r1 goes out of scope here, so we can make a new reference with no problems.

let r2 = &mut s;
```

 - A similar rule exists for combining mutable and immutable references. 

```rust
let mut s = String::from("hello");

let r1 = &s; // no problem
let r2 = &s; // no problem
let r3 = &mut s; // BIG PROBLEM
// cannot borrow `s` as mutable because it is also borrowed as immutable
```

<h2 id="5ab994e9e78665eb0c26226dd2e695de"></h2>

-----

### Dangling References

 - In languages with pointers, it’s easy to erroneously create a *dangling pointer*
    - a pointer that references a location in memory that may have been given to someone else
    - by freeing some memory while preserving a pointer to that memory. 内存释放了，指针仍然保留着
 - In Rust, by contrast, the compiler guarantees that references will never be dangling references:
    - if we have a reference to some data, the compiler will ensure that the data will not go out of scope before the reference to the data does.
 - Let’s try to create a dangling reference:

```rust
fn main() {
    let reference_to_nothing = dangle();
}

// error[E0106]: missing lifetime specifier
fn dangle() -> &String {
    let s = String::from("hello");
    &s
    //Here, s goes out of scope, and is dropped. Its memory goes away.
    // Danger
}
```

 - This error message refers to a feature we haven’t covered yet: *lifetimes*.
    - 'this function's return type contains a borrowed value, but there is no value for it to be borrowed from.'

 - The solution here is to return the String directly:
    - The following code works without any problems. Ownership is moved out, and nothing is deallocated.

```rust
fn no_dangle() -> String {
    let s = String::from("hello");
    s
}
```

<h2 id="b172e4bff02b5a9a4db1943ce0c725d0"></h2>

-----

### The Rules of References

 1. At any given time, you can have either but not both of:
    - One mutable reference.
    - Any number of immutable references.
 2. References must always be valid.


<h2 id="9b0c5c583b42825a78e22070349106d4"></h2>

-----

## 4.3 Slices

 - a different kind of reference 
 - will not have ownership as well
 - Slices let you reference a contiguous sequence of elements in a collection rather than the whole collection.
 - Here’s a small programming problem: 
    - write a function that takes a string and returns the first word it finds in that string. 
    - `fn first_word(s: &String) -> ?`
 - what should we return ?  
    - we can return the length of the first word

```rust
fn first_word(s: &String) -> usize {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return i;
        }
    }
    s.len()
}
```

 - 这个方法只能 返回 usize, 并不完美

<h2 id="4802b309597c38a0e315a41088f5e7ce"></h2>

-----

### String Slices

 - A string slice is a reference to part of a String, and looks like this:

```rust
let s = String::from("hello world");

let hello = &s[0..5];
let world = &s[6..11];
```

 - This is similar to taking a reference to the whole String but with the extra `[0..5)` bit.
 - Rather than a reference to the entire String, it’s a reference to a portion of the String.

![](../imgs/rust_string_slice.png)

```rust
let slice = &s[0..2];
let slice = &s[..2];  // same

let slice = &s[3..len];
let slice = &s[3..];  // same 

let slice = &s[0..len];
let slice = &s[..];   // same
```

 - the new method

```rust
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[..i];
        }
    }
    &s[..]
}
```

 - following code will throw a compile error if we use the slice version `first_word` 
    - but it is valid if we use the  usize version 
    - That usize version  was logically incorrect but didn’t show any immediate errors  
    - Slices make this bug impossible and let us know we have a problem with our code much sooner. 

```rust
fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s);

    s.clear(); // Error!
}
```

 - *clear* needs to truncate the String, it tries to take a mutable reference
    - but we already have a immutable borrow . 

<h2 id="0326ddde0b60afd7cabed1dfa526d3b2"></h2>

-----

### String Literals Are Slices 

```rust
let s = "Hello, world!";
```

 - The type of s here is `&str`
    - it’s a slice pointing to that specific point of the binary.
    - `&str` is an immutable reference
    - 注意： 不是之前的 `&String`

<h2 id="33d042b567d55b78fd644d49a29c1e9f"></h2>

-----

### String Slices as Parameters

```rust
// 更好的写法
fn first_word(s: &str) -> &str {
```


<h2 id="0d42b1fd2a5b5b7309665776c9f05990"></h2>

-----

### Other Slices

```rust
let a = [1, 2, 3, 4, 5];
// type &[i32]
let slice = &a[1..3];
```

<h2 id="290612199861c31d1036b185b4e69b75"></h2>

-----

### Summary

 - The concepts of ownership, borrowing, and slices are what ensure memory safety in Rust programs at compile time. 


