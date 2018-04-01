# Linux Command Structure

For users who are used to graphical user interface (GUI) environments where double-clicking on files will (usually) launch the associated program for the file (e.g. double-clicking on a Word document will launch MS Office),
one of the most difficult concept is that the Linux/UNIX command line interface (CLI) does not work this way.
Specifically, typing and entering the name of a file at the command prompt in most cases will return a `command not found` error.

The general structure of a Linux/UNIX command line looks like:

`command [-option(s)] [argument(s)]`

The most important thing to note is that the command line *always* starts with a "command", which is an executable file or program (any file with the executable bit set).

## Options and arguments

Options are settings built into the command program (or script), that alter the default behaviour of the program. Some options are "flags", which simply turns on or off a specific behaviour, while others require some input value, which are called "arguments".

Some commands can be used without any options or arguments, e.g. `ls` and `pwd`.

- commands that expect arguments
- arguments without options
- combining options
- required options (subcommands, e.g. tar)
