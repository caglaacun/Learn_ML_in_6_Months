<h1>Week 1: Pointers</h1>



In addition to this course I also went through [C Programming & Data Structures by Neso Academy](https://www.youtube.com/playlist?list=PLBlnK6fEyqRhX6r2uhhlubuF5QextdCSM) to relearn some of the concepts that wasn't clear in this course.


<h2>Pointers Conceptually</h2>


<h3>Pointer Basics</h3>

Pointers are way of referring to the _location_ of a variable. Instead of storing a value like 5 or the character 'c', a pointer's value is the location of another variable. Conceptually, you can think of the location as an arrow that points to another variable.

Just like we can make variables that are integers, doubles, etc., we can make variables that are pointers. Such variables have a size (a number of bytes in memory), a name, and a value. They must be declared and initialized.

__Declaring a pointer:__

In C, "pointer" (by itself) is not a type. It is a _type constructor_—a language construct which, when applied to another type, gives us a new type. In particular, adding a star (__*__) after any type names the type, which is a pointer to _that type_. For example, the code __char * my_char_pointer;__ (pronounced "char star my char pointer") declares a variable with the name __my_char_pointer__ with the type _pointer_ to a _char_ (a variable that points to a character). The declaration of the pointer tells you the name of the variable and type of variable that this pointer will be pointing to.

<img src="../3. Pointers, Arrays, and Recursion/images/firstPointer.png">

__Assigning to a pointer:__

As with all other variables, we can assign to pointers, changing their values. In the case of a pointer, changing its value means changing where it points—what its arrow points at. Just like other variables, we will want to initialize a pointer (giving it something to point at) before we use it for anything else. If we do not initialize a pointer before we use it, we have an arrow pointing at some random location in our program.

Assignment statements involving pointers follow the same rules that we have seen so far. However, we have not yet seen any way to get an arrow pointing at a box—which is what we need to assign to a pointer. To get an arrow pointing at a box (technically speaking, the _address_ of that box in memory), we need a way to name that box, and then we need to use the __&__ operator. Conceptually, the & operator (the symbol is called an "ampersand", and the operator is named the "address-of" operator) gives us an arrow pointing at its operand. The code `xPtr = &x;`, for example, sets the value of the variable __xPtr__ to the _address of **x**_. After it is initialized, __xPtr__ points to the variable __x__.

The address-of operator can be applied to any lvalue and evaluates to an arrow pointing at the box named by the lvalue. Accordingly, for a variable __x__, the expression __&x__ gives an arrow pointing at __x__’s box.  It is important to note that the address of a variable is not itself an lvalue, and thus not something that can be changed by the programmer. The code `&x = 5;` will not compile. A programmer can access the location of a variable, but it is not possible to change the location of a variable.

__Dereferencing a pointer:__

Once we have arrows pointing at boxes, we want to make use of them by "following the arrow" and operating on the box it points at—either reading or changing the value in the box at the end of the arrow. Following the arrow is accomplished using the star symbol __*__, a unary operator that dereferences a pointer. An example of the use of the _derference_ operator is shown in line 7 of the figure above. `*xPtr = 6;` changes the value that xPtr points to (i.e., the value in the box at the end of the arrow—namely, x’s box)—to 6. Notice that the green arrow indicates that this line has not yet been executed, hence x still has the value 5 in the conceptual representation. Once line 7 is executed, however, the value will be 6.

Do not be confused by the two contexts in which you will see the star (_*_) symbol. In a variable declaration, such as `int *p;`), the star is part of the type name, and tells us that we want a pointer to some other type (in our example, __int *__ is the type of p). In an expression, the star is the dereference operator. For example, the code `r = *p;` gives the variable _r_ a new value, namely the value inside the box that _p_ (which is an arrow) points to. The code `*p = r;` changes the value inside the box that p points at to be a new value, namely the value of the variable _r_. As always, remember that you can think about declaration with initialization as two statements: `int *q = &y;` is the same as the two statements `int *q; q = &y;`. Generally when you work with pointers, you will use the star first to declare the variable and then later to dereference it. Only variables that are of a pointer type may be dereferenced.


<h2>Pointers in Hardware</h2>


<h3>Pointers under the Hood</h3>

<img src="../3. Pointers, Arrays, and Recursion/images/addresses_hardware_ptrs.png">

__Addressing:__

A computer keeps track of all of the data by storing it in memory. The hardware names each location in memory by a numeric address. As each different address refers to one byte of data, this type of storage is called _byte-addressable memory_. A visualization of what this looks like is shown in section (a) of the figure above. Each box represents a byte of memory, and each byte has an address shown immediately to the left of it. For example, the address 208 contains one byte of data, with the value _00000000_.

Section (b) in the figure shows the code, conceptual representation, and hardware representation of the declaration and initialization of one 4-byte integer, four 1-byte characters, and finally two 4-byte pointers. Each variable has a base address. For example, the address of x is 104 and the address of c3 is 101. The variable x is small enough that it can be expressed within the first byte of its allocated space in memory. If it were larger (or simply negative), however, the bytes associated with addresses 105-107 would be non-zero. On a 32-bit machine, addresses are 32 bits in size. Therefore pointers are always 4 bytes in size, regardless of the size of the data they point to.

With this more concrete understanding of memory and addresses, the hardware representation of pointers becomes clear: pointers store the addresses of the variable they point to. The final variables, y and z, in (b) in the above figure show just that. The variable y is an integer pointer initialized to point to x. The conceptual drawing shows this as an arrow pointing to the box labeled x. The hardware drawing shows this as a variable that has the value 104, the base address of x. The variable z is declared as a pointer to a char. Although a character is only 1 byte, an address is 32 bits, and so z is 4 bytes in size and contains the value 101, the location of c3. (If these variables were located in memory locations with higher addresses, the numerical values of the addresses would be larger, and there would be non-zero values across all four bytes of the pointers y and z.)

Pointers are variables whose values are addresses. An important consequence of this fact is that a pointer can only point to _addressable_ data. Expressions that do not correspond to a location in memory cannot be pointed to, and therefore a program cannot attempt to use the ampersand (“address of”) operator for these expressions. For example, neither _3_ nor _(x+y)_ are expressions that represent an addressable “box” in memory. Consequently, lines like these are illegal: `int *ptr = &3;` and `int *ptr = &(x + y);`. Note that _3_ and _(x+y)_ are not lvalues—they do not name boxes, which is why they cannot have an address. The compiler would reject this code.

<h3>A Program's View of Memory</h3>

On a 32-bit machine, where addresses are 32 bits in size, the entire memory space begins at 0x00000000 (in hex, each 0 represents 4 bits of 0) and ends at 0xFFFFFFFF (recall that 0x indicates hexadecimal, and that each F represents four binary 1’s). Every program has this entire address space at its disposal and there is a convention for how a program uses this range of addresses. the figure below depicts where in memory a program’s various components are stored.

<img src="../3. Pointers, Arrays, and Recursion/images/memory.png">

__Code:__

Program itself can be represented by a series of numbers. Producing these numbers is the job of a compiler, which takes a program in a language like C and converts it into a bunch of numbers (called object code) which is readable by the computer. An instruction in C which adds two numbers together, for example, might be encoded as a 32-bit instruction in object code. Some of the 32 bits will tell the machine to perform addition, some of the bits will encode which two numbers to add, and some of the bits will tell the processor where it should store the computed sum. The compiler converts the C code into object code and also assigns each encoded instruction a location in memory. These encoded program instructions live in the Code portion of memory.

__Static Data:__

The static data area contains variables that are accessible for the entire run of the program (e.g. global variables). Unlike a variable that is declared inside a function and is no longer accessible when the function returns, static variables are accessible until the entire program terminates (hence, the term static ). Conceptually, a static variable’s box remains “in the picture” for whole program, whereas other are usable for only a subset of the program’s lifetime. These variables are placed in their own location in memory.

The final two sections of memory are for two different types of program data that are available at specific times during a program’s execution. The __Heap__ stores dynamically allocated data. The __Stack__ stores the local variables declared by each function. The stack is divided into _stack frames_ that are available from starting when the function is called, and last until it returns. The stack is one contiguous piece of memory; each stack frame sits below the stack frame of the function that called it.

<h3>NULL</h3>

At the bottom of the figure below, there is a blank space below the code segment, which is an invalid area of the program’s memory. You might wonder why the code does not start at address 0, rather than wasting this space. The reason is that programs use a pointer with the numeric value of 0—which has the special name, NULL to mean “does not point at anything.” By not having any valid portion of the program placed at (or near) address 0, we can be sure that nothing will be placed at address 0. This means no properly initialized pointer that actually points to something would ever have the value NULL.

<img src="../3. Pointers, Arrays, and Recursion/images/null_memory.png">

Knowing that a pointer does not point at an actual thing is useful for a variety of reasons. For one, we may sometimes have algorithms which need to answer “there is no answer”. We may also have functions whose job it is to create something that return NULL if they fail to do so.

The NULL pointer has a special type that we have not seen yet—`void *`. A _void pointer_ indicates a pointer to _any_ type, and is compatible with any other pointer type—we can assign it to an `int *`, a `double*`, or any other pointer type we want. Likewise, if we have a variable of type `void *`, we can assign any other pointer type to it. Because we do not know what a `void *` actually points at, we can neither dereference it (the compiler has no idea what type of thing it should find at the end of the arrow), nor do pointer arithmetic on it.


<h2>Pointers to Sophisticated Types</h2>


<h3>Pointers to Structs</h3>

In this figure note that __*a.p__ dereferences the __b__ field inside of struct _a_. Dereferencing _q_, then selecting field _x_ requires either parenthesis, or the -> operator.

<img src="../3. Pointers, Arrays, and Recursion/images/structs.png">

When we have pointers to __structs__, we _can_ just use the * and . operators that we have seen so far, however, the order of operations means that `.` happens first. If we write `*a.p`, it means `*(a.p)`—_a_ should be a struct, which we look inside to find a field named _p_ (which should be a pointer), and we dereference that pointer. If we have a pointer _q_ to a struct _a_, and we want to refer to the field _x_ in the struct _a_, we would need parenthesis because if we wrote `*q.x`, we would receive a compiler error, as _q_ is not a struct, and the order of operations would say to do `q.x` first so write `(*q).x` or the -> operator `q->x`.

Pointers to structs are incredibly common, and the above syntax gets quite cumbersome, especially with pointers to structs which have pointers to structs, and so on. For `(*q).x`, it may not be so bad, but if we have `(*(*(*q).r).s).t` it becomes incredibly ugly, and confusing. Instead, we should use the `->` operator, which is shorthand for dereferencing a pointer to a struct and selecting a field—that is, we could write `q->x` (which means exactly the same thing as `(*q).x)`. For our more complex example, we could instead write `q->r->s->t`.

<h3>Pointers to Pointers</h3>

We can have pointers to pointers (or pointers to pointers to pointers…etc ). For example, an __int**__ is a pointer to a pointer to an int. We can have as many levels of “pointer” as we want (or need), however, the usefulness drops of quite quickly. The rules for pointers to pointers are no different from anything else we have seen so far: the * operator dereferences a pointer (follows an arrow), and the & operator takes the address of something (gives an arrow pointing at that thing).

<img src="../3. Pointers, Arrays, and Recursion/images/ptr2ptr.png">

You may wonder why we might want pointers to pointers. One answer to this question is “for all the same reasons we want pointers”—a pointer gives us the ability to refer to the location of a thing, rather than to have a copy of that thing. Anytime we have a variable that tells us the location of a thing, we can change the original thing through the pointer.

Notice how the types work out ( i.e. match up so that the compiler type check the program). Whenever we take the address of an lvalue of type _T_, we end up with a pointer of type _T*_.

<h3>const</h3>

As we discuss pointers to various types, it is good time to introduce the notion of const data—data which we tell the compiler we are not allowed to change. When we declare a variable, we can add __const__ to its type to specify that the compiler should not allow us to change the data:

```c
const int x = 3; // assigning to x is illegal
```

If we try to change the value of x, the compiler will produce an error. Declaring data as __const__ can be useful, as it removes a certain class of mistakes that we can make in our program: changing things that we should not.

When we have pointers, there are two different things that can be const: the data that the pointer points at (what is in the box at the end of the arrow), or the pointer itself (where it points). If we write:

```c
const int *p = &x;
```

We have declared p as a pointer to a __const int__—that is, p points at a int, and we are not allowed to change that int. We can change where _p_ points (e.g., _p = &y;_ is legal—if _y_ is an int). However, changing the value in the box that _p_ points at (e.g., _*p = 4;_) is illegal—we have said that the int which _p_ points at is __const__. If we do try to write something like _*p = 4;_, we will get a compiler error like this:

```
assignment of read-only location '*p'
```

We can achieve exactly the same effect by writing:

```c
int const *p = &x; // same as const int * p
```

If we want to specify that we can change *p, but not p itself, we would write

```c
int *const p = &x;
```

This declaration says that _p_ is a __const__ pointer to a (modifiable) __int__. Writing _*p=4;_ would be legal, but writing _p =&y;_ would be illegal. If we so desire, we can combine both to prevent changing either where the pointer points, or the value it points at:

```c
const int *const p = &x;
```

The same principle applies to pointers to pointers (to pointers to pointers…). For example, with an int **, we have the following combinations:

<img src="../3. Pointers, Arrays, and Recursion/images/ptr_const.png">

Note that a declaration of ___const__ tells the compiler to give us an error only if we try to change the data through the variable declared as __const__, or perform an operation where the __const__-ness gets dropped. For example, the following is legal:

```c
int x = 3;
const int *p = &x;
x = 4;
```

Here, we are not allowed to change _*p_, however, the value we find at _*p_ can still be changed by assigning to _x_ (since _x_ is not __const__, it is not an error to assign to it). However, if we write:

```c
const int y = 3;
int *q = &y;
*q = 4;
```

then we will receive a compiler warning (which we should treat as an error):

The error is on line 2, in which we assign _&y_ (which has type __const int *__) to q (which has type __int *__)—discarding the __const__ _qualifier_ (const is called a qualifier because it modifies a type). This snippet of code is an error because `*q=4;` (on line 3) would be perfectly legal (q is not declared with the __const__ qualifier on the type it points to), but would do something we have explicitly said we do not want to do: modify the value of y.


<h2>Aliasing and Arithmetic</h2>


<h3>Aliasing</h3>

In the figure below, we have four names for a’s box: a, _*p_, _**q_, and _***r_. Whenever we have more than one name for a box, we say that the names alias each other—that is, we might say _*p_ aliases _**q_.

<img src="../3. Pointers, Arrays, and Recursion/images/ptr2ptr.png">

<h3>Pointer Arithmetic</h3>

Like all types in C, pointers are variables with numerical values and it can be manipulated the way one can manipulate the value of integers and floating point numbers, for example:

```c
int x = 4;
float y = 4.0;
int *ptr = &x;

x = x + 1;
y = y + 1;
ptr = ptr + 1;
```

Lines 1–3 initialize the values of 3 variables of various types (integer, floating point number, and pointer-to-an-integer). Lines 5–7 add 1 to each of these variables. For each type, adding 1 has a different meaning. For _x_, adding 1 performs integer addition, resulting in the value 5. For _y_, adding 1 requires converting the 1 into a floating point number (1.0) and then performing floating point addition, resulting in 5.0. For both integers and floating point numbers, adding 1 has the basic semantics of “one larger”. For the integer pointer _ptr_ (which initially points to x), adding 1 has the semantics of “one integer later in memory”. Incrementing the pointer should have it point to the “next” integer in memory.

In order to do this, the compiler emits instructions which actually add the number of bytes that an integer occupies in memory (e.g., +1 means to change the numerical value of the pointer by +4). Likewise, when adding N to a pointer to any type T, the compiler generates instructions which add (N * the number of bytes for values of type T) to the numeric value of the pointer—causing it to point N Ts further in memory.

The code we have written here is legal as far as the compiler is concerned, however, our use of pointer arithmetic is rather nonsensical in this context. In particular, we have no idea what box ptr actually points at when this snippet of code finishes. If we had another line of code that did `*ptr = 3;`, the code would still compile, but would have undefined behavior — we could not execute it by hand and say with certainty what happens. Specifically, when `ptr = &x`, it is pointing at one box (for an integer) which is all by itself—it is not part of some sequence of multiple integer boxes. Incrementing the pointer will point it at some location in memory, we just do not know what. It could be the box for y, the return address of the function, or even the box for ptr itself.

We will consider all code with undefined behavior, such as this, to be erroneous. Note that simply performing arithmetic on pointers such that they do not point to a valid box is not, by itself, an error—only dereferencing the pointer while we do not know what it points at is the problem.



<h1>Week 2: Arrays</h1>



<h2>Array Basics</h2>


<h3>Array Declaration and Initialization</h3>

An _array_ is a sequence of items of the same type (e.g., an array of ints is a sequence of ints). When an array is created, the programmer specifies its size (i.e., the number of items in the sequence). If we wanted to declare an array of 4 ints called _myArray_, we would write:

```c
int myArray[4];
```

The variable name _myArray_ is a pointer to the 4 boxes that make up the array.

<img src="../3. Pointers, Arrays, and Recursion/images/array1.png">

Notice that there are 4 boxes (which are not yet initialized). As with other variables, we can initialize an array in the same line that we declare it. However, as the array holds multiple values, we write the initialization data inside of curly braces. For example, we might write:

```c
int myArray[4] = {4, 5, 6, 7};
```

If you write too few elements in the array, the compiler will fill the remaining ones in with 0. This behavior can be a useful feature for zero-initialing the entire array—we can write just a single 0 in the curly braces and the compiler will fill in as many zeros as there are elements of the array:

```c
int myArray[4] = {0};
```

Note that most compilers will also accept `int myArray [4] = {};`, however, _gcc_ will warn you for it if you compile with _-pedantic_, as its not strictly allowed by the language standard.

If we provide an initializer, we can also omit the array size, and instead write empty square brackets—the compiler will count the elements of the initializer, and fill in the array size:

```c
int myArray[] = {4, 5, 6, 7};
```

<h3>Accessing an Array</h3>

Accessing an array element using pointer arithmetic works fine, and sometimes is the natural way to access elements. However, sometimes we just want the _nth_ element of an array, and it would be cumbersome to declare a pointer, add _n_ to it, then dereference it. We can accomplish this goal more succinctly by indexing the array. When we index an array, we write the name of the array, followed by square brackets containing the number of the element we want to refer to: e.g., `myArray[3]`.

We will note that accessing an array out of bounds (at any element that does not exist) is an error that the compiler cannot detect. If you write such code, your program will access some box, but you do not know what box it actually is.


<h2>Arrays in Action</h2>


<h3>Passing Arrays as Parameters</h3>

In general, when we want to pass an array as a parameter, we will want to pass a pointer to the array, as well as an integer specifying how many elements are in the array, such as this:

```c
int myFunction(int *myArray, int size) {
    // whatever code ...
}
```

There is no way to get the size of an array in C, other than passing along that information explicitly, and we often want to make functions which are generic in the size of the array they can operate on (i.e., we do not want to hard code a function to only work on an array of a particular size). If we wanted, we could make a struct which puts the array and its size together, as one piece of data—then pass that struct around.

When we pass a pointer that actually points at an array, we can index it like an array (because it is an array—remember the name of an array variable is just a pointer), and perform pointer arithmetic on it.

We can also pass an array as a parameter with the square bracket syntax:

```c
int myFunction(int myArray[], int size) {
    // whatever code ...
}
```


<h2>Array Caveats</h2>


<h3>Dangling Pointers</h3>

When you write code with arrays, you may be tempted to return an array from a function (after all, it is natural to solve problems where an array is your answer). However, we must be careful, because the storage space for the arrays we have created in this chapter live in the stack frame, and thus are deallocated after the function returns. The value of the expression that names the array is just a pointer to that space, so all that gets copied to the calling function is an arrow pointing at something that no longer exists. Whenever you have a pointer to something whose memory has been deallocated, it is called a dangling pointer. Dereferencing a _dangling pointer_ results in undefined behavior (and thus represents a serious problem with your code) because you have no idea what values are at the end of the arrow.

<h3>Array Size</h3>

In C, the number of bits we would need to describe the size of the array and/or index varies from one platform to another—on a 32-bit platform (meaning memory addresses are 32 bits), we would want a 32-bit unsigned int; on a 64-bit platform we would want a 64 bit unsigned int. Fortunately, the designers of C realized this possibility, and decided to make a type name for “unsigned integers that describe the size of things”—*size_t*.

Instead of writing a numerical constant that represents the size on one platform, you should let the C compiler calculate the size of a type for you with the __sizeof__ operator. The __sizeof__ operator takes one operand, which can either be a type name (e.g., __sizeof(double)__) or an expression (e.g., __sizeof(*p)__). If the operand is an expression, the compiler figures out the type of that expression (remember expressions have types), and evaluates the size of that type. In either case, __sizeof__ evaluates the number of bytes which the type requires. The type of this number of bytes is _size_t_.



<h1>Week 3: Uses of Pointers</h1>








<h1>Week 4: Recursion</h1>
