
Tutorial 3 : Compiling and Running C Programs
==========================================================================

The first few programming assignments for this course will use the C
programming language. This tutorial begins by briefly reviewing the
basics of C functions, conditional statements, and iteration statements.
The tutorial then discusses the C preprocessor before describing how to
compile and execute both single-file and multi-file C programs using the
command line. All of the tools are installed and available on the
`ecelinux` machines. This tutorial assumes that students have completed
the Linux and Git tutorials. We strongly recommend students also read
Chapters 1-6 in the course text book, _``All of Programming,''_ by A.
Hilton and A. Bracy (2015). Chapters 5-6 are particularly relevant since
they discuss the general process of compiling, testing, and debugging C
programs.

To follow along with the tutorial, access the course computing resources,
and type the commands without the `%` character (for the `bash` prompt).
In addition to working through the commands in the tutorial, you should
also try the more open-ended tasks marked **To-Do On Your Own**.

Before you begin, make sure that you have **sourced the setup-ece2400.sh
script** or that you have added it to your `.bashrc` script, which will
then source the script every time you login. Sourcing the setup script
sets up the environment required for this class.

You should start by forking the tutorial repository on GitHub. Go to the
GitHub page for the tutorial repository located here:
[https://github.com/cornell-ece2400/ece2400-tut3-c](). Click on _Fork_ in
the upper right-hand corner. If asked where to fork this repository,
choose your personal GitHub account. After a few seconds, you should have
a new repository in your account:
`https://github.com/<githubid>/ece2400-tut3-c` Where `<githubid>` is your
GitHub username on [github.com](). Now access an `ecelinux` machine and
clone your copy of the tutorial repository as follows:

```bash
 % source setup-ece2400.sh
 % mkdir -p ${HOME}/ece2400
 % cd ${HOME}/ece2400
 % git clone https://github.com/<githubid>/ece2400-tut3-c.git tut3
 % cd tut3
 % TUTROOT=${PWD}
```

**NOTE:** It should be possible to experiment with this tutorial even if
you are not enrolled in the course and/or do not have access to the
course computing resources. All of the code for the tutorial is located
on GitHub. You will not use the `setup-ece2400.sh` script, and your
specific environment may be different from what is assumed in this
tutorial.

Section 1: C Basics
--------------------------------------------------------------------------

C is a programming language that has a long history of use in _computer
systems programming_. Computer systems programming involves developing
software to connect the low-level computer hardware to high-level,
user-facing application software and usually requires careful
consideration of performance and resource constraints. Examples of
computer systems software include compilers, operating systems,
databases, numerical libraries, and embedded controllers. C was developed
in the early 1970s at Bell Laboratories by Ken Thompson, Dennis Ritchie,
and many others. C was originally developed for the purposes of writing
an early version of the Unix operating system. The Linux operating system
which you learned about in the first tutorial was inspired by these
original versions of Unix, and the Linux kernel is also written in C. C
provides a nice balance between high-level abstractions for productive
software development and low-level control over the hardware, and thus it
remains one of the primary languages chosen by computer systems
programmers. In this section, we will briefly review some of the basic
syntax and semantics of C.

### Using Compiler Explorer and Repl.it to Experiment with C Programs

Compiling a C program can be challenging, since it usually involves
multiple command line tools and steps. So to get started, we will be
using two online tools which will enable us to quickly experiment with
small C programs. Later in the tutorial, we will see how to compile,
build, test, debug, and evaluate both small and large C programs on the
`ecelinux` machines.

The first online tool is called Compiler Explorer, and it is accessed
through the following link: [https://godbolt.org](). You can enter simple
C functions in the left text box, and then Compiler Explorer will display
the corresponding machine instructions (i.e., assembly code) in the right
text box. This is a great way to quickly use your browser to see the
connection between C programs and the low-level computer hardware.
Compiler Explorer color codes the C program and the machine instructions,
so it is possible to see which C statements compile into which machine
instructions. There is a drop-down menu to choose different compilers.
Choosing _x86-64 gcc 7.2_ roughly corresponds to the compiler we will be
using on the `ecelinux` machines which is based on the Intel x86-64
instruction set. Choosing _MIPS gcc 5.4 (el)_ roughly corresponds to the
compiler used in ECE 2300 which is based on the MIPS instruction set.
There is also a text box where you can enter various compiler command
line options.

The second online tool is called Repl.it, and it is accessed through the
following link: [http://repl.it](). You can create a new "repl" by
choosing the C programming language. You can enter simple C programs in
the left text box. Clicking on the _run_ button will first compile the C
program into an executable binary, and then run this executable binary.
This is a great way to quickly experiment with C programs in your
browser. The output of running the program is shown in the right text
box.

### C Functions

The definition for a simple function to calculate the average of two
integers is shown below.

```c
1. int avg( int x, int y )
2. {
3.   int sum = x + y;
4.   return sum / 2;
5. }
```

A C function definition specifies a named parameterized sequence of
statements. C function definitions have four parts: a _function name_
(e.g., `avg` on Line 1; a _function parameter list_ (e.g., `int x, int y`
on Line 1); a _function return type_ (e.g., `int` at the beginning of
Line 1); and the `function body` (e.g. Lines 2-5). In this tutorial, we
will only use the `int` type for variables, but the C language supports a
rich selection of different types for representing integer numbers, real
numbers, characters, pointers, and composite structures. A function
creates a new block and all variable declarations within the function are
local to that scope. Line 3 is a variable initialization statement which
first creates a new variable of type `int` named `sum` and then
initializes this variable with the value of the expression `x+y`.
Remember that all C statements end in a semicolon (`;`). Line 4 is a
return statement which causes the function to return the value of the
corresponding expression (e.g., `sum/2`). Calling a C function involves
setting the parameters in the parameter list, executing the sequence of
statements in the function body, and then returning the return value. A C
function call is just another kind of expression which evaluates to the
function's return value.

Enter the `avg` function into Compiler Explorer and take a look at the
corresponding machine instructions for the Intel x86-64 instruction set.
The `avg` function compiles to 15 machine instructions. In this course,
you are not responsible for understanding these machine instructions, but
it is still very important to recognize that C programs usually map
relatively directly to machine instructions. Notice that the statement on
Line 3 maps to four machine instructions (the C statement and the machine
instructions are all colored yellow) including an `add` instruction. If
you right click on one of the Intel x86-64 machine instructions and
choose _View Asm Doc_, Compiler Explorer will display a detailed
description of that machine instruction. Try finding out more about the
`add` instruction. This instruction directly corresponds to the `+`
operator in the `avg` function; this direct mapping from high-level
syntax to low-level machine instructions is one of the key features of C.
The `+` operator in a language such as Python or MATLAB would eventually
correspond to hundreds of machine instructions! Enter `-O3` into the
_Compiler options..._ text box and press return. This option tells the
compiler to apply various optimizations to improve the performance of the
compiled machine instructions. The `avg` function now compiles to six
machine instructions, and we can see that Line 3 now maps to a single
machine instruction (the C statement and the machine instruction are both
are colored green). Even without understanding the details of these
machine instructions, it should be obvious that reducing the number of
machine instructions from 15 to six should improve performance.

The definition for a simple main function to call the avg function and
then print its result is shown below.

```c
#include <stdio.h>

int avg( int x, int y )
{
  int sum = x + y;
  return sum / 2;
}

int main()
{
  int a = 10;
  int b = 20;
  int c = avg( a, b );
  printf( "average of %d and %d is %d\n", a, b, c, );
  return  0;
}
```

Recall that the `main` function is special. The `main` function is always
where program execution starts; in other words, the `main` function is
always the first function to be called in any C program. Lines 11-13 are
variable initialization statements. Line 13 calls the `avg` function with
the values in variables `a` and `b`, and then the return value is used to
initialize variable `c`. Line 14 calls the `printf` function to display
text on the screen. The `printf` function is defined in the _C standard
library_ which is usually included along with every C compiler. More
specifically, the `printf` function declaration is in a separate _header
file_ which is included on Line 1 using the C preprocessor. We will learn
more about the C preprocessor later in this tutorial. The `printf`
function always takes a format string as the first parameter, and then
takes a variable number of parameters that are evaluated and substituted
into the format string. The format string includes _format specifiers_
(i.e., special codes) to indicate where to substitute these values. In
this example, we are using the `%d` format specifier to indicate that we
would like to substitute an integer into the format string. The format
string can also include other special codes; for example, this format
string includes a newline special code (`\n`) which results in a line
break. The return value of the `main` function is used as the _exit
status_ of the program; this is a way for a C program to tell the
operating system whether or not the program ended successfully (by
returning zero) or something went wrong (by returning a non-zero value).

Enter the `avg` and `main` functions into Repl.it and then use the _run_
button to compile and execute this C program. Try initializing `a` to 10
and `b` to 15. The average of these two integers is 12.5, but our program
currently does not support arithmetic on non-integer values. In this
case, the division operator (i.e., `/`) truncates the result by always
rounding towards zero. Quickly experimenting with small C programs can
help illustrate subtle issues in C syntax and semantics.

**To-Do On Your Own:** Write a simple function named `avg3` to average
three integers. Use Compiler Explorer to examine the corresponding
machine instructions and identify how the `+` operators in the C program
maps to `add` machine instructions. Use Repl.it to compile and execute a
program with the `avg3` function and a `main` function to call the
average function and print its result. Experiment with averaging negative
numbers, especially negative numbers where ideally the result would not
be an integer (e.g., `-7`, `-5`, and `-4`).

### subsection 1.2

Section 2
--------------------------------------------------------------------------

Section 3
--------------------------------------------------------------------------
