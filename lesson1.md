# easyaspi314's C tutorial, continued

[Home](https://github.com/easyaspi314/c-tutorial/blob/master/README.md) | [Previous](https://github.com/easyaspi314/c-tutorial/blob/master/README.md) | [Next](https://github.com/easyaspi314/c-tutorial/blob/master/lesson2.md)

## Lesson 1: "Hello world!"

First, you want to make a directory for these examples. I suggest something you can find without any spaces, such as `~/c-tutorials` on Mac/Linux or `C:\Users\username\Documents\c-tutorials\` on Windows.

Now, open a terminal and navigate to that folder.

Do this by typing cd, space, then the file path.

On Windows, you need to change the path name from eg `C:\Users\username\Documents\c-tutorials` to `/c/Users/username/Documents/c-tutorials`

Ok, now create a new text file in that folder called "hello.c" with the text editor of your choice.

Paste the following lines into it and save:

<details><summary>Click to expand</summary>


```c
/*
 * Tutorial 1: Hello, world!
 * Filename: hello.c
 * Run as:
 * gcc -Wall hello.c -o hello && ./hello
 */

/**
 * Include the system header stdio.h
 * for the puts function below.
 */
#include <stdio.h>

/**
 * Declare and implement a function called main,
 * which takes no arguments and returns int.
 *
 * main is the function that is called at the very start
 * of the program.
 */
int main(void)
// brace opens the main function block
{
    // Print "Hello, world!" to the console
    puts("Hello, world!");
    // Set the return value of main to 0, meaning the program ended OK
    return 0;
// Close the brace above
} // end function main
```

</details>

Now, go back to your terminal, and type this:
`gcc -Wall hello.c -o hello && ./hello`

If all things go out right, you should see this:

```
Hello, world!
```

Ok, now let's talk about what happened.

First of all, let's talk about comments. Comments are notes that you can put in a file that are ignored by the compiler. There are two types of comments in C, **block comments** and **line comments**.

**Line comments** start with `//` and end at the end of the line.

**Block comments** start with `/*` and end with `*/`. Anything between these two marks is a comment, whether it is in the middle of the line, the end of the line, or across multiple lines. Note that when you are trying to make a paragraph to document something, it is a good idea to format it like this:

```c
/**
 * This is a comment.
 * It spans multiple lines.
 * It documents the line below.
 * Note the two stars at the top.
 */
code_line_to_comment
```

Another thing to know is that unlike, say, Python, formatting doesn't matter in C, with the exception of any lines that start with `#`. The only thing that matters is that the words are in tact. But, please, keep it consistent.

Anyways, so after that comment is this:

```c
#include <stdio.h>
```

This **includes** all of the functions from the system header **stdio.h**. We specifically need the puts function. More on that later.

```c
int main(void)
```

This is a **function declaration**. You can identify the **function signature** like so:

![main is a function that has no arguments (is void of arguments) and has a return type of int](https://raw.githubusercontent.com/easyaspi314/c-tutorial/master/images/main.png)

A **function** is a set of instructions that can be reused. Think of it like a recipe. It can take in values and spit out up to one value.

__`main` is a special function__. When you start a program, it calls the function `main` and everything happens, and eventually ends (unless there is a problem) in `main`,

__**Note:**__ __In C, all functions need a unique name__. Unlike C++ or Java, C doesn't have **overloading**, which is when you have two functions with the same name and different arguments.

```c
{
...
}
```

This is a **block of code**. Because it comes directly after the signature of `main`, it becomes the **function implementation**. It is marked by **curly braces**, or `{}`. Every open brace needs a close braces, and what happens in the braces stays in the braces.

Let's go into the block.

```c
puts("Hello, world!");
```

Here is a **function call.** It is one type of **statement.** It tells the compiler to go to that function and run the function `puts` with the **argument** being a **string** containing `Hello, world!`. Strings are in double quotes.

A function call is similar to a function declaration, but instead of type names like `int` and `void`, you use actual values.

`puts` is a function that prints a string the console. Other people use `printf`, but printf deserves a lesson of its own, it is slow, and prone to insecure code.

So, what we do is print "Hello, world!" to the console.

All statements in C end in a semicolon. You can't leave it out like JavaScript, Python, or Ruby. Think about it like a period at the end of a sentence.

For example, if we leave out the semicolons (bad code below)…

```c
#include <stdio.h>

int main(void)
{
    puts("Hello, world!") // no semicolon
    return 0 // no semicolon
}
```

If you use GCC, it isn't the greatest at showing errors and will say something like this:

```
hello.c: In function 'main':
hello.c:6:5: error: expected ';' before 'return'
     return 0
     ^~~~~~
```

but Clang is a bit more helpful:

```
hello.c:5:26: error: expected ';' after expression
    puts("Hello, world!")
                         ^
                             ;
hello.c:6:13: error: expected ';' after return statement
    return 0
            ^
            ;
2 errors generated.
```

This is why I recommend Clang if possible, as it just has better errors.

Anyways, back on topic, the next line:

```c
return 0;
```

`return` will jump to the end of a function, going back to where the function was called.

If the return type of the function is not void, you have to put a value after it and you can't leave it out (the compiler will complain with the message "control reaches end of non-void function" — memorize it, it is a terrible error message). If it is void, you have to **not** return a value and you can leave it out. If you return early from a function, make sure you wrap it in a conditional or the compiler will complain about unreachable code.

To return or not to return? That is the rather complicated question.

<details><summary>Click to expand</summary>


```c
int returning_int(void)
{
    return 2; // okay, returns an int
}

void not_returning_anything(void)
{
    return; // okay, but not required because it is at the end of the void function
}

void dont_need_to_return(void)
{
    // okay, the end of void functions don't need a return statement
}

int should_return(void)
{
    // not okay, this needs a return statement because it isn't void.
    // this errors as "control reaches end of non-void function".
}

void should_not_be_returning_anything(void)
{
    return 3; // not okay, this return statement returns int, but the void function should not return a value
}

int should_return_int(void)
{
    return; // not okay, this return statement expects a value because the function returns int
}

int should_also_return_int(void)
{
    return "totally an int and not a string"; // not okay, this is a string, not an int!
}

void shouldnt_be_leaving_early(void)
{
    return; // not okay, the stuff after this is not used
    do_something();
}

void could_be_leaving_early(void)
{
    if (condition) { // more on these later!
        return; // okay because this only might happen
    }
    do_something();
}

int could_not_return_a_value(void)
{
    if (condition) {
        return 4;
    }
    // not okay, if the condtion is false, we reach the end of the function and do not return a value.
    // this errors as "control may reach end of non-void function".
}

int else_return(void)
{
    if (condition) {
        return 4;
    } else {
        return 2;
    }
    // okay, because the else catches all possible conditions.
}
```

</details>

`main`, being a special function, lets you set the exit code. If you programmed in Java, it is the same as calling `System.exit(x)`. Whatever number you put after a return statement in main is the exit code.

To check this, in bash, run `./hello; echo $?` (note the semicolon, not the double ampersand), mess with the `return 0` and watch the number change.

This all ends in that closing brace. Congratulations, you have created a "Hello, world!" program and completely beaten the concept to death (don't worry, the later lessons are much shorter)!

[Home](https://github.com/easyaspi314/c-tutorial/blob/master/README.md) | [Previous](https://github.com/easyaspi314/c-tutorial/blob/master/README.md) | [Next](https://github.com/easyaspi314/c-tutorial/blob/master/lesson2.md)
