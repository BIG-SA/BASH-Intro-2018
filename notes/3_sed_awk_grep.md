* TOC
{:toc}


# Grep, Sed and Awk

In this session, we will learn about a few of the most frequently used tools that are very useful for extracting and modifying data.

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


-----

## Prepare the data files (and some revision exercises)
For this session, we need to prepare some data files for demonstration.

1. Locate and uncompress the file `GRCh38.chr22.ensembl.biomart.txt.gz` in the `files` directory.
   (*Hint: when uncompressing the gzip file, use the `-k` option to keep the original gzip file.*)

2. Locate and extract the file `3_many_files.tar.gz` in the `files` directory.
   This should create a sub-direction (`3_many_files`) containing 100 files, each file contains a subset of the data in `GRCh38.chr22.ensembl.biomart.txt.gz`.

3. Use what you have learnt so far and find out:
   - What is the size of the uncompressed file (`GRCh38.chr22.ensembl.biomart.txt.gz`)? How many characters, words and lines does it contain?
   - How are the data organised in the file?
   - What is the column separator?
   - How many columns does it contain?
   - What are the column headers?
   - Which column is "**Gene name**"? How many unique gene names are there in the file?

4. Can you answer the above questions without uncompressing the original file?

You should now have some idea of the structure of the data file.

We will also need the file `BDGP6_genes.gtf` from previous session.

-----

## Working with large files or many files

Text editors (either CLI or GUI) are very convenient when you want to quickly edit a small text file (if you just want to read the file, you can use `more`, `less` or `cat`), however, they are less useful when the files are very large.

Consider the following questions (try using **nano** to answer them):

For `GRCh38.chr22.ensembl.biomart.txt`:

1. What is the first line that contains "**DNAJB7**"? Give line number.

  <details><summary>Hint:</summary>
  You will need <code>^W</code> (search), and <code>^C</code> (view line number), unless you really enjoy counting and scrolling line by line.</details>

2. How many lines contain "**DNAJB7**"?

  <details><summary>Hint:</summary>Use <code>M-W</code> (<code>[Alt]-W</code>) to repeat search.</details>

3. How many lines contain "**RBX1**"?

4. In how many non-header entries (lines) are the "**Gene name**" and "**HGNC symbol**" values different?

5. Change all instances of "**TBX1**" in "**Gene name**" and "**HGNC symbol**" columns to "**TBX-1**", but not in other columns.

<details>
<summary><b>Answers</b></summary>
<ol>
<li> 55151
<li> 1 line only
<li> Too many to count in <b>nano</b> (but the answer is 5775).
<li> Too hard in <b>nano</b> (answer is 36).
<li> Replacing one or all instances is possible. But replacing only values in specific columns is very tedious.
</ol>
</details>

<br>

Now look into the files in the created sub-directory **`3_many_files`**.

1. Open the file **`datafile1`** in nano. Does it contain an entry for "MAPK1"?

2. Which files in **`3_many_files`** directory contain entries for "MAPK1"?

<details>
<summary><b>Answers</b></summary>

<ol>
<li> There is an entry for MAPK11, but not MAPK1.
<li> It would be too much work to go through each file individually, but the answer is datafile11, datafile26, datafile28,
datafile32, datafile36, datafile38, datafile46, datafile51, datafile63, datafile80, datafile82, datafile83, datafile84, datafile95, datafile98.
</ol>
</details>

<br>

Hopefully by now you can appreciate that using text editors are not the best way to query large data sets.

As an aside, usually when you are working in BASH (or some other Linux/UNIX CLI) and you find yourself doing something repetitively, then there is probably a better way of doing it.

In the rest of this session, we will examine three of the most commonly used CLI tools for working with large text data sets: `grep`, `sed`, and `awk`.

<br>
----


### Primer on Linux Command Structure

Before we introduce these tools, you may find it useful to familiarise yourself
with [the structure (or syntax) of a typical Linux command](extra_command_syntax.md).
However, feel free to continue on if you already understand the topic.

<br>

----


# `grep`

**`grep`**<sup>[5]</sup> is an utility for searching fixed-strings or regular expressions in plaintext files. The basic syntax of a grep command requires two arguments:

`$ grep [PATTERN] [FILE]`

which would search for the string PATTERN (either fixed string or regular expression) in the specified FILE. For example, to search for the string `DNAJB7` in `GRCh38.chr22.ensembl.biomart.txt`, we would enter:

`$ grep DNAJB7 GRCh38.chr22.ensembl.biomart.txt`

By default, **`grep`** prints every line that contains at least one instance of search pattern. (In modern terminal programs, matching strings in each line will be highlighted.)

You can also search for multiple files at the same time, by:

1. Providing multiple file names:

  `$ grep DNAJB7 3_many_files/datafile1 3_many_files/datafile2`

2. Or by using file name wild-cards:

  `$ grep DNAJB7 3_many_files/datafile2?`

  `$ grep DNAJB7 3_many_files/*`

**`grep`** has many useful options. You can find out all of the available options by reading its help page (`man grep`). We will look at examples of some of the more common options.

### Word-matching

**Example 1 (`-w`)**

If we want to retrieve entries related to **DDT** gene, we may try a command like:

`grep DDT GRCh38.chr22.ensembl.biomart.txt`

However, this will also retrieve entries that match **DDTL**. To retrieve entries which are only related to DDT, we can use the `-w` option:

`grep -w DDT GRCh38.chr22.ensembl.biomart.txt`

This will match lines that contain the string "`DDT`" which are immediately flanked by non-word constituent characters (word constituent characters are alphanumerics and the underscore) and therefore DDTL will no longer match.

**Example 2**

One of the columns in the file is "**GO term name**". If we want to search for entries for which the value in this column is "transport", we may try something like:

`$ grep -w transport GRCh38.chr22.ensembl.biomart.txt`

However, if you examine the output you will see that it also retrieved many lines in which "transport" is simply a word in a longer phrase or sentence, sometimes in the "GO term name" column, and sometimes in other columns.

This is because the `-w` option will allow for any non-alphanumerical, including spaces and commas, whereas what we really want are instances where entire column value is just "transport". In other words, we are looking for the string "`[tab]transport[tab]`":

`$ grep "[tab]transport[tab]" GRCh38.chr22.ensembl.biomart.txt`

To enter an actual [tab] character, you need to use the key sequence: [Ctrl-V][tab]. Just pressing the [tab] key will not work.

While this works when you are entering commands directly in the command line terminal, it will not work in a script (as you will see in the next lesson). So the alternative method is to use the usual tab symbol representation (`\t`), but for this to work, you'll also need to use the `-P` option in **`grep`**:

`$ grep -P "\ttransport\t" GRCh38.chr22.ensembl.biomart.txt`

**Example 3: case sensitivity**

*We will use `BDGP6_genes.gtf` for this example.*

As you should know by now, unlike Windows, in Linux almost everything is case sensitive. If we want to search for the entry for a gene "Zen", we can try:

`$ grep -w Zen BDGP6_genes.gtf`

However, this will return zero results. This is because the gene name used is "zen". So we can perform an case-insensitive search using the option `-i`:

`$ grep -wi Zen BDGP6_genes.gtf`

**Example 4: searching for multiple terms at the same time**

If we want to extract the entries for genes "Ada" and "Zen", we can search for them separately, but this can become tedious quickly.

One method to search for multiple terms at the same time is to use *extended* grep, which is enabled by the option `-E`. (Or simply use the command `egrep`, which is the same as `grep -E`):

`$ grep -Ewi "(Ada|Zen)" BDGP6_genes.gtf`

While this may work fine for just a few terms, if we want to search for many terms  this can still be rather tedious. We can try using the `-f` option instead.

For example if we want to search for these genes: Ace, Ada, Zen, Alc, Alh, Bdp1, Bft, bwa.

First we need to create a file, and enter the genes of interest one per line. Call this file "gene_names". Then we can perform the search by:

`$ grep -wif gene_names BDGP6_genes.gtf`

However, take care that there are no trailing spaces after the gene names, and there are no empty lines.


**Example 5: inverse search**

We can use the `-v` option to perform an inverse search. For example, to extract entries for all protein-coding genes, we can run:

`grep -w protein_coding BDGP6_genes.gtf`

But if we want to get all *other* entries, we can instead run:

`grep -v protein_coding BDGP6_genes.gtf`

**Example 6: simple regular expression**

`$ grep -wi A[dl][ac] BDGP6_genes.gtf`
`$ grep -wi "\"A[dl][ac]\"" BDGP6_genes.gtf`
`$ grep -wi "\"A[^dl][ac]\"" BDGP6_genes.gtf`

Create gene list:
`cut -f 2 -d ";" BDGP6_genes.gtf   | cut -f 2 -d "\"" | sort > fly_genes`

Look for genes:
- starts with z
- starts with a and ends with a numeral


**Exercises**

1. Look up the help page to see which option can provide the line number of search output.

-------------

# `sed` : stream editor
- just the most basic form: 's\search\replace\'
- can search but also edit contents

-------------

# `awk`
- introduce only the simplest mode
- most useful for working with tabulated data
- can search for values in specific columns
- can format output more easily than sed, e.g. can print specific columns

-------------

## get students to decipher some unholy combination of sed, awk and grep command

-------------



*Footnotes*

[1] Nano is a opensource GNU clone of the proprietary program, Pico.

[2] Emacs is not installed by default on Ubuntu (16.04). To install, type `sudo apt install emacs` on Ubuntu or `apt`-based Linux systems.

[3] The Control (Ctrl) key is usually denoted as `^`, however, sometimes (e.g. in Emacs) it is denoted as `C-`.

[4] You can install `ne` by entering `sudo apt install ne` on Ubuntu.

[5] `grep` is short for `g/re/p` (Global search for Regular Expression and Print), a command in the original UNIX text editor `ed` (precursor to `vi`/`vim`).
