* TOC
{:toc}


# Grep, Sed and Awk

In this lesson, we will learn about a few of the most frequently used tools that are very useful for extracting and modifying data.

## CLI Text editors

In general, we rarely work with binary file formats (e.g. MS Office files) in the command line interface. Instead, we work with ASCII (or plain text) files.

We can use a GUI program like gedit or pluma (Ubuntu), Text Edit (Mac OS X), or Notepad (Windows) to edit these files. However, there are also several command line programs available that you can use to edit files inside the command line console.


<ol>
<li> **Nano**/**Pico**:  Nano is an easy to use text editor. On most Linux systems, just type `nano` (or `pico`, the command `pico` is often soft-linked to `nano`<sup>[1]</sup>) to invoke the program. To quit, hold [Ctrl] and press X (^X).
There are a couple important caveats to remember when using Nano:

  - Nano will load entire file into memory, so it may take a while when working with large files.
  - Be careful when editing configuration files, as Nano hard-wraps long lines by default. This behaviour can be disabled by `-w` option.

![Nano screenshot](../images/3_nano_screenshot.png)

<li> **vi**/**vim**: **vi** is arguably the most popular text editor among Linux users. It was designed to minimise hand movements, thus allowing very fast typing and editing. However, it has very steep learning curve and are usually not recommended for beginners.
To start **vi**, just enter `vi`. If you are using a recent Linux distribution, you may notice that it is actually running **vim**.

  **To quit:** type `:q`

![vi screentshot](../images/3_vi_screenshot.png)


<li> **Emacs**:


<li> **ne**:



</ol>
<hr>

*Footnotes*

[1] Nano is a opensource GNU clone of the proprietary program, Pico.
