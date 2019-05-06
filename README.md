# easyaspi314's C Tutorial

[Next](./lesson1.md)

C is a (fairly) simple language, but it can be confusing at first. If you know C++, JavaScript, PHP, or Java, a lot of this will seem familiar.

I will help get you started.

This is designed for people who have some experience with programming concepts, but not C specifically. This mostly covers syntax.

There are many different tutorials, so if you don't understand, try looking at some other tutorials.

But, please, let me know what you think and if this tutorial was helpful.

## Setting Up

So, we are going to be compiling native code to run on your computer. That required a C compiler.

In order to do this, you need either (preferably) Clang (part of LLVM) or GCC. Technically, you can use any C compiler, but trying to juggle the command line options is confusing.

If you are using Clang, substitute `gcc` for `clang`.

On Windows, you should install and use MinGW via [MSYS2](https://www.msys2.org), or use WSL. Visual Studio's C compiler is garbage and the command line options don't match.

On Linux, you *probably* have GCC installed already. If not, install it from your package manager.

On macOS, install [iTerm2](https://www.iterm2.com) (the default terminal is inefficient), and run `xcode-select --install`, and if it doesn't say that the tools are already installed, follow the instructions.

You should be able to open a terminal and type `gcc --version` and see something similar to this:

```
gcc-7 (Homebrew GCC 7.3.0_1) 7.3.0
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

```
Or this (may also say clang instead of Apple LLVM):

```
Apple LLVM version 8.0.0 (clang-800.0.42.1)
Target: x86_64-apple-darwin15.6.0
Thread model: posix
InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin
```

You also want a decent text editor with syntax highlighting. Here are some suggestions:

 * [VSCode](https://code.visualstudio.com) (Windows, Mac, Linux)
 * [Sublime Text](https://www.sublimetext.com) (Windows, Mac, Linux)
 * [Norepad++](http://www.notepad-plus-plus.org) (Windows)
 * [TextMate](https://macromates.com/) (Mac)
 * [Geany](https://www.geany.org/) (Linux, rough ports for Windows and Mac)
 * The various terminal based editors out there (vim, emacs, nano, joe, etc)
 * **DO NOT USE WINDOWS NOTEPAD!!!**

Note that if you prefer watching a video, I have made an unscripted 30 minute video with Lessons 1 and 2, as well as a few later lessons.

It is not a direct transcript, so it is still worth watching. You can watch it here:

<!-- blatant copy from the markdown tutorial -->
<a href="http://www.youtube.com/watch?feature=player_embedded&v=BJK8c9bUEEc" target="_blank">
    <img src="http://img.youtube.com/vi/BJK8c9bUEEc/0.jpg" alt="easyaspi314's C tutorial, part 1" width="240" height="180" border="10" />
</a>

[Lesson 1: Hello, world!](./lesson1.md)

[Lesson 2: Break it up!](./lesson2.md)

Other links:

 * [OOP to C](https://github.com/easyaspi314/oop-to-c), a more advanced tutorial which shows how object oriented programming really works by showing equivalent C code.

Later parts coming soon!

[Next](./lesson1.md)
