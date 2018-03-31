* TOC
{:toc}


# Grep, Sed and Awk

In this lesson, we will learn about a few of the most frequently used tools that are very useful for extracting and modifying data.

## Command Line Interface (CLI) Text editors (for Small-ish Files)

In general, we rarely work with binary file formats (e.g. MS Office files) in the command line interface (CLI). Instead, we work with ASCII (or plain text) files.

We can use a GUI program like gedit or pluma (Ubuntu), Text Edit (Mac OS X), or Notepad (Windows) to edit these files. However, there are also several command line programs available that you can use to edit files inside the command line console.

<ol>
<li> <b>Nano</b>/<b>Pico</b>:  Nano is an easy to use text editor. On most Linux systems, just type <code>nano</code> to start the program (or <code>pico</code>, the command <code>pico</code> is often soft-linked to <code>nano</code><sup>[1]</sup>). To quit, hold [Ctrl] and press X (^X).
There are a couple important caveats to remember when using Nano:

<ul>
<li> Nano will load the entire file into memory, so it may take a while when working with large files.

<li> Be careful when editing configuration files, as Nano hard-wraps long lines by default. This behaviour can be disabled by <code>-w</code> option.
</ul>


  ![Nano screenshot](../images/3_nano_screenshot.png)

<br><br>

<li> <b>vi</b>/<b>vim</b>: <b>vi</b> is arguably the most popular text editor among Linux users. It was designed to minimise hand movements, thus allowing very fast typing and editing. However, it has very steep learning curve and are usually not recommended for beginners.
To start <b>vi</b>, just enter <code>vi</code>. If you are using a recent Linux distribution, you may notice that it is actually running <b>vim</b>.

  <br>
  <b>To quit:</b> type <code>:q</code>
  <br>

  ![vi screentshot](../images/3_vi_screenshot.png)

<br><br>

<li> <b>Emacs</b>: Emacs is another popular CLI text editor. There are many flame wars on older Internet sites centred on whether <b>vi</b> or <b>Emacs</b> is better. To start Emacs, just enter <code>emacs</code>. However, this will probably bring up a windowed mouse-enabled version. To use the pure CLI version, type <code>emacs -nw</code>.

  <br>
  <b>To quit:</b> type <code>^x^c</code> (i.e. <code>[Ctrl]-X [Ctrl]-C</code>, or in Emacs shorthand: <code>C-x  C-c</code>).<sup>[3]</sup>
  <br>

  ![vi screentshot](../images/3_emacs_screenshot.png)

<br><br>

<li> <b>ne</b> (<b>n</b>ice <b>e</b>ditor) is intended to be easier to use than <b>vi</b>, but more functional than <b>nano</b>. Start by <b>ne</b> by entering <code>ne</code>.<sup>[4]</sup>

  <br>
  If you are stuck in <code>ne</code>, press [Esc] twice to display the program menu.
  <br>

  ![vi screentshot](../images/3_ne_screenshot.png)

</ol>

<br><br><br><br>
<hr>
## Working with large files or many files

CLI text editors are very convenient when you want to quickly edit a small text file (if you just want to read the file, you can use `more`, `less` or `cat`), however, they are less useful when the files are very large. Let's try working on a data file using **nano**.




**1. Prepare the data files (and some revision exercises)**
<details><summary>Instructions</summary>

<table>
<tr><td bgcolor="#DDDDDD">

<ol>
<li>Locate and uncompress the file <code>GRCh38.chr22.ensembl.biomart.txt.gz</code> in the <code>files</code> directory:
<br><br>
  <code>$ gunzip -k GRCh38.chr22.ensembl.biomart.txt.gz</code>
<br><br>
  (Q: What is the function of the option <code>-k</code>?)
<br><br>

<li>Locate and extract the file <code>3_many_files.tar.gz</code> in the <code>files</code> directory:
<br><br>
  <code>$ tar xzvf 3_many_files.tar.gz</code>
<br><br>
  This should create a sub-direction (<code>3_many_files</code>) containing 100 TSV files.
  <br><br>

<li>Use what you have learnt so far and find out:
  <br><br>
  <ul>
    <li> What is the size of the uncompressed file?  <br>
    </ul><br>
    <li> How many characters, words and lines does it contain?  <br>

<li>How are the data organised in the file? i.e. does it look like a tabulated data file? If so, then:
  <br><ul>
  <br><li> What is the column separator?
  <br><li> How many columns does it contain?
  <br><li> What are the column headers?
  </ul><br>

<li>Which column is "<b>Gene name</b>"? How many unique gene names are there in the file?
<br><br>

<li>Can you derive the answers to questions 2-4 without uncompressing the original file?
</ol>

You should now have some idea of the structure of the data file.

</td></tr>
</table>
</details>

<br>

**2. Exercise 1 (`GRCh38.chr22.ensembl.biomart.txt`)**

1. Open the extracted file in nano:

  <code>$ nano GRCh38.chr22.ensembl.biomart.txt</code>

  This may take a few seconds, depending on your machine.
  Once the file has loaded, try answering the following questions using the functions provided by **nano**.

2. What is the first line that contains "<b>DNAJB7</b>" in  <code>GRCh38.chr22.ensembl.biomart.txt</code>? Give line number.

  <details><summary>Hint:</summary>
  You will need <code>^W</code> (search), and <code>^C</code> (view line number), unless you really enjoy counting and scrolling line by line.</details>

3. How many lines in <code>GRCh38.chr22.ensembl.biomart.txt</code> contain "<b>DNAJB7</b>"?

  <details><summary>Hint:</summary>Use <code>M-W</code> (<code>[Alt]-W</code>) to repeat search.</details>

4. How many lines contain "<b>RBX1</b>"?

5. Ignoring the header, in how many entries (lines) are the "<b>Gene name</b>" and "<b>HGNC symbol</b>" values different in <code>GRCh38.chr22.ensembl.biomart.txt</code>?

6. Change all instances of "<b>TBX1</b>" in "<b>Gene name</b>" and "<b>HGNC symbol</b>" columns to "<b>TBX-1</b>", but not in other columns.

<br>


**3. Exercise 2 (`3_many_files/`)**

  Now look into the files in the created sub-directory <code>3_many_files</code>.

  6. Open the file <b><code>datafile1</code></b> in nano. Does it contain an entry for "MAPK1"?

  7. Which files in <code>3_many_files</code> directory contain entries for "MAPK1"?

<br>

<details>
<summary><b>Answers</b></summary>

<ul>
<li> (2-2) 55151
<li> (2-3) 1 line only
<li> (2-4) Too many to count in <b>nano</b> (but the answer is 5775).
<li> (2-5) Too hard in <b>nano</b> (answer is 36).
<li> (2-6) Replacing one or all instances is possible. But replacing only values in specific columns is very tedious.
<li> (3-1) There is an entry for MAPK11, but not MAPK1.
<li> (3-2) Seems like too much work... (<i>but the answer is datafile11, datafile26, datafile28,
datafile32, datafile36, datafile38, datafile46, datafile51, datafile63, datafile80, datafile82, datafile83, datafile84, datafile95, datafile98</i>)
</ul>

</details>

<br>

Hopefully by now you can appreciate that using text editors are not the best way to query large data sets. The rest of this class, we will examine three of the most commonly used CLI tools for working with large data sets: `grep`, `sed`, and `awk`.

<br><br>
<hr>

### Primer on Linux Command Structure

Before we introduce these tools, you may find it useful to [familiarise](extra_command_syntax.md) yourself
with the structure (or syntax) of a typical Linux command.

However, feel free to continue on if you already understand the topic.


<br><br>
<hr>

# `Grep`

`grep`<sup>[5]</sup> is an utility for search fixed-strings or regular expressions in plaintext files. For example, one of exercises above asks to find "DNAJB7" in the data file. We can do this simply in `grep`:

`$ grep DNAJB7 GRCh38.chr22.ensembl.biomart.txt`

This is `grep` in the simplest form, which requires two arguments: the pattern to look for, `DNAJB7`, and the file in which to look for the pattern, `GRCh38.chr22.ensembl.biomart.txt`.
By default, `grep` prints out the line(s) which contains the query pattern.







<br><br>
<hr>

*Footnotes*

[1] Nano is a opensource GNU clone of the proprietary program, Pico.

[2] Emacs is not installed by default on Ubuntu (16.04). To install, type `sudo apt install emacs` on Ubuntu or `apt`-based Linux systems.

[3] The Control (Ctrl) key is usually denoted as `^`, however, sometimes (e.g. in Emacs) it is denoted as `C-`.

[4] You can install `ne` by entering `sudo apt install ne` on Ubuntu.

[5] `grep` is short for `g/re/p` (Global search for Regular Expression and Print), a command in the original UNIX text editor `ed` (precursor to `vi`/`vim`).
