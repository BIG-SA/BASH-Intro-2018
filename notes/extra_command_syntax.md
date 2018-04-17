# Linux Command Structure

For users who are used to graphical user interface (GUI) environments where double-clicking on files will (usually) launch the associated program for the file (e.g. double-clicking on a Word document will launch MS Office),
one of the most difficult concept is that the Linux/UNIX command line interface (CLI) does not work this way.
Specifically, typing and entering the name of a file at the command prompt in most cases will return a `command not found` error.

The general structure of a Linux/UNIX command line looks like:

`command [-option(s)] [argument(s)]`

The most important thing to note is that the command line *always* starts with a "command", which is an executable file or program (any file with the executable bit set).

## Options and arguments

Options are settings built into the command program (or script), that alter the default behaviour of the program. Some commands can be used without any options or arguments (e.g. `ls` and `pwd`), but some commands usually requires some sort of argument, (`less` and `more`).

In the following examples, we will use two commands to illustrate the various uses and conventions in command options and arguments.

---------------------------------

## **Example 1:** `head`

The default behaviour of `head` is to display the first 10 lines of an input file or stream. Consider the command:

`$ head BDGP6_genes.gtf`

`head` expects an input file or stream, so `BDGP6_genes.gtf` is an argument expected by the program, but is not associated with any options.

<details><summary>**Question**</summary>
What happens if you enter `head` without specifying any files?</details>
<br>

As show in Session 2, we can change the default behaviour of displaying 10 lines by using the option `-n`.

`$ head -n 20 BDGP6_genes.gtf`

### Long and short options

If you look up the `man` page for `head`, you will see that actually `-n` is the shortened version of the option `--lines`. You can check this by trying the long form, which should provide identical output:

`$ head --lines 20 BDGP6_genes.gtf`

Options usually (but not always) have a long and short version, and by convention, the long version is preceded by `--`, while the short version is preceded by `-`.

### Switches/flags

In the command above `-n` is an option, and "`20`" is the argument provided for this option. `-n` always requires an argument, the command will report an error if you use `-n` without providing an integer argument.

Some options don't require an argument, but some can be used without any arguments. As this type of option usually changes between two types of behaviour, they are often called "switches" or "flags".

By default, `head` prints file names only if multiple files are provided as argument. But we can use the option `-v`/`--verbose` to force `head` to always print the input file name.

```
$ head -v BDGP6_genes.gtf
$ head --verbose BDGP6_genes.gtf
```

Some commands have options that can function with or without an argument, basically if no argument is provided, a default value is assumed (e.g. `sed -i`).


### Combining options

Using the short form of the options, we can combine multiple options together. For example, the command `ls` has many different switches:

```
$ ls --all -l -S   # -S and -l have no long forms
```
which we can combine together as:
```
$ ls -laS
```

We have seen two options for `head`: `-n` and `-v`. To combine them in long form, we'd type:

```
head --verbose --lines 20 BDGP6_genes.gtf
```

which can be shortened to:

```
head -vn 20 BDGP6_genes.gtf
```

But if you switch the order of `v` and `n` the command will fail. This is because the option which requires an argument must go last. Therefore you cannot combine multiple options which require arguments.

You can also skip the space between `n` and `20`:
```
$ head -vn20 BDGP6_genes.gtf
```
But this works *ONLY* in the short form.

### Command specific short-hand

`head` has an even shorter form for `-n`. Since the most common option for `head` is to change the number of lines to display, it can accept an even shorter form:

`head -20 BDGP6_genes.gtf`

However, this will only work if no other options are called. This is not a standard short form, most other programs do not support it (except for maybe `tail`).


---------------------------------

## **Example 2:** `tar`

`tar` always expects at least one option (one of `-Acdtrux`), which are function letters (or main operation mode), and usually requires `-f/--file` as well. Since one of the function letters are always expected as the first option, you will sometimes see the `-` omitted. For example, the following two commands work identically:

```
$ tar czf archive.tar.gz [files_to_add]
$ tar -czf archive.tar.gz [files_to_add]
```

However, this is *not* a general behaviour for command options.

First option that are mandatory and are specified without `-` are often **sub-commands**. We will see a lot of them in common bioinformatics tools, such as `samtools` and `bedtools`.

------

## General notes

1. It is important to remember that many of these behaviours are conventions, which are not always strictly enforced. For example, shortened options are not necessarily always shortened to just a single letter (although they usually are single-letter in UNIX/Linux commands).

2. With the exception of `-h`, which is almost always reserved for help (`--help`), short form letters do not usually represent the same function in different commands. For example, `-c` do completely different things in `ls` and `head`. However, `-v` is often used for either *verbose* (`--verbose`) or *version* (`--version`), and `-q` is often used for *quiet* mode.
