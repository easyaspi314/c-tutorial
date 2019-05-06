# easyaspi314's C tutorial, continued

[Home](https://github.com/easyaspi314/c-tutorial/blob/master/README.md) | [Previous](https://github.com/easyaspi314/c-tutorial/blob/master/lesson1.md)

## Lesson 2: Break it up!

Now, you can put all your code in `main`. But your code will become so disorganized and ugly that it isn't even funny.

So, we break things up into separate functions.

Copy hello.c to hello2.c and let's make a new function, called `print_hello_world`.
Note that I removed the extra comments that you would never see outside of a tutorial.

```c
/*
 * Tutorial 2: Break it up!
 * Filename: hello2.c
 * Run as:
 * gcc -Wall hello2.c -o hello2 && ./hello2
 */

#include <stdio.h> // for puts

/**
 * main is the function that is called at the very start
 * of the program.
 */
int main(void)
{
    print_hello_world(); // Note that we just use empty parentheses despite the void
    return 0;
} // end function main

/**
 * Prints "Hello, world!" to the console.
 */
void print_hello_world(void)
{
    puts("Hello, world!");
    // Because this is a void statement, we don't need a return value.
} // end function print_hello_world
```

The same thing happens, but it is broken up into a separate function.

Cool, huh?

Well, I lied.

```
hello2.c: In function 'main':
hello2.c:16:5: warning: implicit declaration of function 'print_hello_world' [-Wimplicit-function-declaration]
     print_hello_world(); // Note that we just use empty parentheses despite the void
     ^~~~~~~~~~~~~~~~~
hello2.c: At top level:
hello2.c:23:6: warning: conflicting types for 'print_hello_world'
void print_hello_world(void)
      ^~~~~~~~~~~~~~~~~
hello2.c:16:5: note: previous implicit declaration of 'print_hello_world' was here
     print_hello_world(); // Note that we just use empty parentheses despite the void
     ^~~~~~~~~~~~~~~~~
```

The compiler will complain.

Why is this? I'll tell you, but first, let's talk about ~~parallel universes~~ the compiler.

The C compiler is quirky. First of all, **it reads from top to bottom**.

So, unlike Java, you can't use a function that isn't declared first without a complaint (GCC) or an error (Clang). At the point where we call `print_hello_world()` in `main`, the compiler doesn't know what `print_hello_world` is. We are trying to make an **implicit declaration**, which is bad.

So, there are two ways to fix this.

The first is to reorder how we declare the function so that we put it above main, like this:

```c
/*
 * Tutorial 2: Break it up!
 * Filename: hello2.c
 * Run as:
 * gcc -Wall hello2.c -o hello2 && ./hello2
 */

#include <stdio.h> // for puts

/**
 * Prints "Hello, world!" to the console.
 */
void print_hello_world(void)
{
    puts("Hello, world!");
    // Because this is a void function, we don't need a return value.
} // end function print_hello_world

/**
 * main is the function that is called at the very start
 * of the program.
 */
int main(void)
{
    print_hello_world(); // Note that we just use empty parentheses despite the void
    return 0;
} // end function main
```

Compile and run, and it works fine.

But, things can get complicated when you do that.

The other way of doing this is a **forward declaration**.

Forward declarations look like this:

```c
void print_hello_world(void);
```

So, let's take our example again and use a forward declaration:

```c
/*
 * Tutorial 2a: Break it up!
 * Filename: hello2.c
 * Run as:
 * gcc -Wall hello2.c -o hello2 && ./hello2
 */

#include <stdio.h> // for puts

// Forward declare print_hello_world
void print_hello_world(void);

/**
 * main is the function that is called at the very start
 * of the program.
 */
int main(void)
{
    print_hello_world(); // Note that we just use empty parentheses despite the void
    return 0;
} // end function main

/**
 * Prints "Hello, world!" to the console.
 */
void print_hello_world(void)
{
    puts("Hello, world!");
    // Because this is a void statement, we don't need a return value.
} // end function print_hello_world
```

But, we can break this up even more. Because even this can get [messy](https://github.com/pret/pokeruby/blob/1f60ac0f857da06bd8a678f7150c42cc94940f3f/src/battle/battle_4.c).

Back to the ~~parallel universes~~ compiler.

C is a compiled language. Unlike, say, a Bash or Python script (an interpreted language) which is interpreted by another program, C code needs to be compiled to a program.

C was designed to be portable. Cross platform, cross processor, fast, simple, yet still powerful. In order to do that, it needs to do a few steps:

 1. **Preprocessing** (cpp/gcc -E): The compiler strips away comments, expands macros, isolates specific code, and replaces include statements with the content of the file. This creates a preprocessed C file (.i).  
 2. **Compiling** (cc1/gcc -S): The compiler takes the C code, does its optimizations and translates it to native assembly. While it isn't important, Clang does two steps here. This creates an assembly file (.s). 
 3. **Assembling** (as/gcc -c): The compiler takes assembly files and converts it to a binary. This creates an object file (.o).  
 4. **Linking** (ld): The compiler takes all of the .o files and mashes them into a single file. This either makes an executable that you can run (.exe on Windows, no extension or .out on Mac/Linux) or a library (.dll on Windows, .so on Linux, .dylib on Mac) that you can use in other programs.


We're going to look at the **preprocessor**.

Preprocessor statements start with a `#` and are the only things in C that have to be on one line and only one line.

The preprocessor statement we are looking at is `#include`, called an **include statement**.

Include statements are very basic. They simply take a file and dump its contents in the file that includes it.

We almost always use this to include a **header file**, which contains a bunch of forward declarations, but you can *technically* use it for any file.

There are two variations of them: Angled brackets (`<` and `>`), and quotes (I think you know what quotes are).

Angled brackets are to include a system or library header. In this case, I am including stdio.h from the C Standard Library, also known as libc.

These headers are found in /usr/include on Linux, *gasp* /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include *gasp* on Mac (/usr/include has unused GCC headers), or on Windows, I believe it is in the include folder in your MSYS2 installation. You can add more paths to this with the `-I` option.

The include statement we were using was `#include <stdio.h>`.

The other version, quotes, includes a file relative to the directory, or, alternatively, relative to a path that you specify with `-iquote`. We use these for our own headers.

If we run the preprocessor on hello2.c like this: `gcc -E -P hello2.c`, your console will be bombarded by a bunch of code, then the code in hello2.c with comments stripped out. You will find out that this is exactly what I said, the preprocessed contents of stdio.h in that include directory. The `-E` tells GCC to only use the preprocessor, and the `-P` tells GCC's preprocessor to not add a bunch of debug info. It is useful normally, but not when we are to analyze it.

But, for now, let's work on making our own header.

Make a new file, called hello2.h, in the same folder as hello2.c. Header files in C always end in .h.

Cut and paste that forward declaration from hello2.c and put it in hello2.h.

```c
// Forward declare print_hello_world
void print_hello_world(void);
```

And replace those lines in hello2.c with this:

```c
#include "hello2.c"
```

And comment out (put two slashes before) the

```c
#include <stdio.h>
```

temporarily so we can better see what is happening.

Run `gcc -E -P hello2.c` again and tada! (note that Clang likes to put a bunch of blank lines in the output):

```c
void print_hello_world(void);

int main(void)
{
    print_hello_world();
    return 0;
}

void print_hello_world(void)
{
    puts("Hello, world!");
}
```

Uncomment (remove the two slashes before) the I told you to comment out and run GCC normally, `gcc -Wall hello2.c -o hello2 && ./hello2`. Done.

However. I lied again. There is a terrible mistake waiting to happen.

Make three new files, hello_goodbye.c, hello.h, and goodbye.h with the following contents, respectively:

```c
/*
 * Tutorial 2a: Hello, Goodbye!
 * Filename: hello_goodbye.c
 * Run as:
 * gcc -Wall hello_goodbye.c -o hello_goodbye && ./hello_goodbye
 */
#include <stdio.h> // for puts
#include "hello.h" // for print_hello_world
#include "goodbye.h" // for print_goodbye_world

int main(void)
{
    print_hello_world();
    print_goodbye_world();
    return 0;
}

/**
 * Prints "Hello, world!" to the console
 */
void print_hello_world(void)
{
    puts("Hello, world!");
}

/**
 * Prints "Goodbye, world!" to console.
 *
 * This s*** got dark.
 */
void print_goodbye_world(void)
{
    puts("Goodbye, world!");
}
```

```c
/* Filename: hello.h */
#include "goodbye.h"

void print_hello_world(void);
```

```c
/* Filename: goodbye.h */
#include "hello.h"

void print_goodbye_world(void);
```

```
~/c-tutorials $ gcc -Wall hello_goodbye.c -o hello_goodbye && ./hello_goodbye
In file included from goodbye.h:2:0,
                 from hello.h:2,
                 from goodbye.h:2,
                 from hello.h:2,
                 from goodbye.h:2,
                 from hello.h:2,
                 from goodbye.h:2,
                 from hello.h:2,
                 from goodbye.h:2,
                 from hello.h:2,
                 ... snip, this goes on for a while
                 from hello_goodbye.c:9:
hello.h:2:21: error: #include nested too deeply
#include "goodbye.h"
                     ^
In file included from hello.h:2:0,
                 from goodbye.h:2,
                 from hello.h:2,
                 from goodbye.h:2,
                 from hello.h:2,
                 from goodbye.h:2,
                 from hello.h:2,
                 from goodbye.h:2,
                 from hello.h:2,
                 from goodbye.h:2,
                 from hello.h:2,
                 from goodbye.h:2,
                 from hello.h:2,
                 ... snip
                 from goodbye.h:2,
                 from hello.h:2,
                 from hello_goodbye.c:10:
goodbye.h:2:19: error: #include nested too deeply
#include "hello.h"
                   ^
```
AAAAAAHHHHHH!!!!!! PANIC! PANIC! PANIC!
![PANIC!](http://2.bp.blogspot.com/-9EiMPIvWUs0/Ti7OtK5ViAI/AAAAAAAAAKU/lx8rXif7teM/s320/fun145_panic_button_300hand.jpg)

We got ourselves a recursive include. hello.h and goodbye.h both included each other. The C preprocessor didn't know what to do!

Now, we can be supercautious about what files we include, or we can make an **include guard**.

Just like they told you in health class, **always use protection!**

To make an include guard, we wrap our header files somewhat like what's below (I personally use the naming scheme below, but other people use stuff like `GUARD_FILE_NAME`). **Make sure the name is unique, though.** You can't just call them all `GUARD` or whatever.

This tells the preprocessor to only include this file once.

```c
/* Filename: hello.h */

/**
 * If the guard for this header, in this case, HELLO_H, is not defined (set),
 * include the contents below as it hasn't been included already, until
 * the #endif marking the end of this include-once header.
 *
 * Otherwise, skip it.
 */
#ifndef HELLO_H
/**
 * Declare that the file was included by setting a preprocessor flag named HELLO_H,
 * to serve as the guard for this header.
 */
#define HELLO_H

#include "goodbye.h"

void print_hello_world(void);

/**
 * Mark that this is the end of the HELLO_H preprocessor statement,
 * and as a result of this, the end of the code to include once.
 */
#endif // HELLO_H
```

For goodbye.h, I am not going to comment it, as *nobody* comments include guards.

```c
/* Filename: goodbye.h */

#ifndef GOODBYE_H
#define GOODBYE_H

#include "hello.h"

void print_goodbye_world(void);
#endif // GOODBYE_H
```

Try compiling it again, and it's all good!

For good measure, do the same thing for hello2.h.

[Home](https://github.com/easyaspi314/c-tutorial/blob/master/README.md) | [Previous](https://github.com/easyaspi314/c-tutorial/blob/master/lesson1.md)
