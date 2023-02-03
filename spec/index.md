# Vism

## Introduction

**Vism** (IPA: `/vɪzᵊm/`) is an esoteric programming language mainly inspired from [Assembly](https://en.wikipedia.org/wiki/Assembly_language), [Brainfuck](https://en.wikipedia.org/wiki/Brainfuck) and, in a less linguistic and more technical aspect, the mode system of the text editor [Vim](<https://en.wikipedia.org/wiki/Vim_(text_editor)>).

In this regard, many concepts brought by the language might seem familiar to the reader who has an interest in those. For example, the memory scheme along with the few registers will most certainly delight the average [x86](https://en.wikipedia.org/wiki/X86_assembly_language) enjoyer ; the way to alternate between performing operations and writing values to memory is probably comparable respectively to the Normal and Insert mode of [Moolenaar](https://en.wikipedia.org/wiki/Bram_Moolenaar)'s software.

_- It's interesting, but now begs the question: what does a Vism program look like?_

## Hello World!

Let's start with the classic ["Hello World!" program](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program):

```vism
:0^sHello World!\n^nf!
```

Excuse the lack of syntax highlighting, Vism is not as popular as [Python](<https://en.wikipedia.org/wiki/Python_(programming_language)>) (hopefully, this sentence will be obsolete in the future!).

Here is the equivalent C program:

```c
#include <stdio.h>

int main(void)
{
    fprintf(stdout, "Hello World!\n");
    fflush(stdout);
}
```

Even better, a side-to-side comparison!

![Vism C Hello Comparison](../assets/frames/Vism_C_Hello_Comparison.svg)

> For those who are interested, I made this design and the following ones in [Figma](https://figma.com) 😄

## Modes

As implied earlier, the main idea of Vism is alterning between operations and putting stuff in memory. For this to help, you have (as the time of writing) three modes:

- `^n`: this is the mode by default. It allows performing operations.
- `^s`: this is the string mode. It pushes the given string to a target specified beforehand.
- `^l`: this is the literal mode. Similar to the string mode, but it supports a larger set of data types, such as integers, floats, booleans, [lists](https://en.wikipedia.org/wiki/Dynamic_array), and more!

_- What does "target specified beforehand" mean?_

## Targets

By default, all values you write in `^s` and `^l` go to the [null stream](https://en.wikipedia.org/wiki/Null_device), i.e. get discarded. If you want to save them somewhere, you need to specify a target.

There are three kinds of targets:

- `&X`: represents a memory slot. It can store any type of value, but is [strongly typed](https://en.wikipedia.org/wiki/Strong_and_weak_typing): for example, if a string is assigned to the memory slot `&0`, then putting an integer thereafter will result in a compilation error.

- `$X`: represents an address [register](https://en.wikipedia.org/wiki/Processor_register). The reader might compare it to a [pointer](<https://en.wikipedia.org/wiki/Pointer_(computer_programming)>), and would be correct ; it stores integers only, and these have to be a valid memory address (for Vism virtual machine, not those of the RAM).

- `:X`: represents a stream. By default, `:0` and `:1` are respectively the [`stdout`](<https://en.wikipedia.org/wiki/Standard_streams#Standard_output_(stdout)>) and the [`stderr`](<https://en.wikipedia.org/wiki/Standard_streams#Standard_error_(stderr)>).

> In all three, `X` is an hexadecimal integer.

Alright, that's a lot to digest. Let's recap:

![Vism Detailed Hello World program](../assets/frames/Vism_Detailed_Hello.svg)

## Operations

To better understand the role of registers (see [targets](#targets)), we need to dig into the operations.

As the time of writing, the list of the existing operations in Vism goes as follows:

- `+`: addition/concatenation.
- `-`: substraction/subsitution.
- `×`: multiplication.
- `/`: integer division.
- `%`: modulo.
- `÷`: divmod (a combination of integer division and modulo).
- `p`: for printing a value.
- `f`: to flush a given stream.

The registers are used for the arguments of a chosen instruction.

Let's take the addition as an example. It is a binary operation, that will look at the registers `$0` and `$1`. In those, it will find two addresses (`&0` and `&1` by default), grab the value of these, add them together and put the result in the address corresponding to `$0`.

![Diagram of how addition works](../assets/frames/Vism_Addition_Process.svg)

Here is the code that would lead to the diagram situation:

```vism
$0^l5^n
$1^l13^n
&5^l7^n
&D^l2^n
+!
```

More coming soon!
