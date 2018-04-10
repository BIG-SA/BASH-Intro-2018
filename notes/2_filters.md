* TOC
{:toc}

# More commands and tools
{:.no_toc}

# Introduction

## Recap

Last week we discovered some key bash commands:

- Navigating directories using `cd`, `pwd` and `ls`
- Creating and deleting files or directories using `touch`, `mkdir`, `rmdir` and `rm`
- Copying, moving and renaming files or directories using `cp` and `mv`
- Finding help pages using `man` or the arguments `-h` and `--help`

We also learned some important concepts

- Tab auto-completion and using the [Up]/[Down] arrows
- Relative vs Absolute paths
- The `<command> <argument>` structure of bash commands
- Using wildcards `?`, `*` and `[]` to select subsets of files or directories

## This Session

We will introduce a set of highly useful additional commands, often known as *filters*.
Whilst the first session was mainly focussed on navigation around the file system, this week will be focussed on working with the *contents* of files.

Commands for today:

- `head`, `tail`, `cat`, `less`, `wc`
- `cut`, `sort`, `uniq`
- `|`, `>`, `>>`, `echo`
- `wget`,`curl`
- `zip`, `unzip`, `gzip`, `gunzip`, `zcat`, `tar`

Key concepts for today:

- Understanding text vs binary data
- Understanding `stdout` and introducing `stderr`
- Downloading data
- Compressed data

## Before we start

If you didn't create a folder for last week's files, let's create one and put all of today's work in there.
First navigate to your home folder, which may be one of `/home/<yourname>`, `/Users/<yourname>`, `/u` or `/c/Users/<yourname>`.
The best way to get to this directory is

```
cd ~
```

From here let's create a new folder for today:

```
mkdir BashWk2
cd BashWk2
pwd
```

#### Question
{:.no_toc}

Did we specify a relative or absolute path when we created the directory?  

(If you weren't in your home folder when you  exectuted the `mkdir` command, make sure you know where you created this folder.)

It might be a good idea to open your usual file explorer and navigate to this folder.
You'll be able to see any files we create appearing, and that will help you download any files to the correct place.

## Today's Data

Let's get some data for this session.
Save [this file](../files/BDGP6_genes.gtf) to the folder `BashWk2` which you just created.
Just use Firefox, Chrome, Explorer, Safari or whichever browser you prefer.
This file contained genomic information for every known gene in the Drosophila melanogaster genome in a (mostly) tab-separated format.

## File Suffixes and Plain Text Files

This file we've downloaded has the suffix `.gtf`.
**What program do you think you should open this with?**

(If you answered 'none', well done)

We're very used to files with a given suffix being associated with a specific program, such as `.xlsx` with Excel, and `.docx` with MS Word.
These file types are binary files, which means they are not in plain text format, but have been (at least partially) encoded into `0/1` binary numbers that computers see everything as.
We generally can't read files like this with anything but specific software which knows to to decode these patterns of `0` and `1`.

Many other file types are actually just saved as plain text, with the most common of these being a `.csv` file.
We could actually open a `.csv` file with any plain text editor/viewer, even though we're used to opening them with Excel.
Another common file type is a `.tsv` file, which represents *tab-separated values* as opposed to *comma-separated values*.
**If we changed the suffix of a .csv file to .tsv, would we have changed the commas within the file to tabs?**
Clearly, the answer is no.

File suffixes for plain text files like this are really just communicating to other users what the layout of the file is.
In the case of a `.gtf` file there is a set layout, which includes some "header" lines followed by a series of tab-separated columns.
It's really just a plain text file though and we could change the suffix at will.
If we do that though, nobody else will know what the internal structure of the file will be & we'll either be a hindrance or a terrible practical joker.


# Looking Through Large Files

## Printing File Contents to `stdout`

Now we have a file to look through let's try to look at it to see what we mean by a 'plain-text' file and 'header lines'.
The way to dump the entire contents of a file to the terminal (or `stdout`) is to use the command `cat`.
Try this and all the information in the file will whiz past you in the terminal.
If you get sick of watching it, just hit `Ctrl+c` to stop the stream of text.

```
cat BDGP6_genes.gtf
```

This is clearly not a helpful way to look at a large file like this, so an alternate method is just to scan the first few lines using the command `head`.
Note the lines at the start which begin with a `#` symbol.
These are the 'header' lines which are not in the tab-separated format that the rest of the file is, but contain important information about the contents of the file, which here are the genome build, genome build date etc.
This is pretty darn useful information for a bioinformatician.

```
head BDGP6_genes.gtf
```

This shows us the first 10 lines by default, and we can change this to the first 20 by setting the additional argument (or flag) `-n` to the value 20.
We can choose any value we like here, as long as it's a whole number (or integer).

```
head -n 20 BDGP6_genes.gtf
```

In the way the programmers like dumb jokes, to look at the end of the file, we can use the command `tail`.
The flag `-n` can be set in the same way for this command.

```
tail BDGP6_genes.gtf
```

These are 3 ways to look through any plain text file in `bash` by printing a stream of output to `stdout`.

## Using the Pager `less`

If we want to browse through the file in a more interactive way, we can use the pager `less` which many of us saw last week when we discovered `man` pages.
In actual fact, using the `-h` or `--help` flags effectively dumped the whole help page to `stdout`, whilst using the `man` command instead sends the strem of information to `less`.
Let's try this with our `.gtf` file.

```
less BDGP6_genes.gtf
```

Now we can browse the file using the space bar to move down a page, the `b` key to move back a page and the arrows to move up or down a line.
Although we can navigate through the less pager using up & down arrows on our keyboards, some helpful shortcuts are:

| Command |	Action  |
| ------- | ------- |
| `<enter>`	|go down one line |
| `spacebar` | go down one page (i.e. a screenful) |
| `b`	| go backwards one page |
| `<` |	go to the beginning of the document |
| `>` |	go to the end of the document |
| `q` |	quit |

You won't be able to type anything into the file, or into `bash` here so after you've tried navigating around, return to `bash` using the `q` key for `q`uit.

`less` is actually quite powerful and has functions for searching for key pieces of text.
Open the `gtf` file in `less` again and try entering `/ncRNA`.
By typing the `/` we are telling `less` that a search pattern follows, and once we hit the `<Enter>` key, less searches for the pattern and highlights any occurences.
We can search for the same pattern again simply by hitting the `/` key and hitting `<Enter>` again.
We only need to re-enter a pattern if we're searching for something new.
This way we can quickly step through large files and look for any text that we're trying to find.

Try entering a few other patterns and see if you can find anything you're interested in.
Once you've finshed exploring, hit `q` to quit `less`.
To fully expore `less` you can check out the `man` page for `less`, which will ironically be shown to you in `less`.
(Programmers are hilarious).
If you're on `git bash` you'll have to be content with the less hysterical `less --help`



## Word and line counts

We can quickly check for the number of entries in a file by using the command `wc`.
This gives three values by default.

```
wc BDGP6_genes.gtf
```

**To find out what they mean, check the help page.**

A common usage is to quickly perform a line count, using `wc -l`.
Try it out and see how many lines our `gtf` file has.

# Text In the Terminal

## Standard Output
All the information we’ve seen in the terminal so far has been from a data stream known as standard output, or `stdout` for short.
There are two primary data streams in play when we use commands in `bash`.
The first as we’ve seen is `stdout`, with the alternative stream being standard error, or `stderr` for short.
This is where commands and tools send their error messages.
We’ll ignore that for the rest of the day, but it’s good to know it exists.

When a command sends information to us via `stdout`, we refer to this as printing to `stdout`.
This dates back to the days before everyone had printers, when printing to the screen was the main method of interacting with computers.
We can display a line of plain text in `stdout` by using the command `echo`, which we breifly saw last week.
The most simple proram that people learn to write in most languages is called "Hello World" and we’ll do the same thing today.

```
echo 'Hello World'
```


That’s pretty amazing isn’t it & you can make the terminal window say anything you want without meaning it.

```
echo 'This computer will self destruct in 10 seconds!'
```

There are a few subtleties about text which are worth noting.
**Inspect the `man echo` page & note the effects of the -e option.**
This allows you to specify tabs, new lines & other special characters by using the backslash to signify these characters.
This is an important concept & the use of a backslash to escape the normal meaning of a character is very common.
Try the following three commands & see what effects these special characters have.

```
echo 'Hello\tWorld'
echo -e 'Hello\tWorld'
echo -e 'Hello\nWorld'
```

As we’ve seen above, the command echo just repeats any subsequent text. Now enter

```
echo ~
```

We've already tried this last week, so you should now understand why this has happened.
If you don't quite get it, call a helper over or ask someone next to you.

Although this may have seemed trivial, we often include lines like the above in scripts to provide messages about the progress of our tasks.
As we’ll see later, `echo` is actually a *very heavily* used command.

## Redirecting `stdout` To a File

Instead of just sending the output of a command to `stdout`, we can redirect this output into a file using the `>` symbol.
If the file doesn’t exist, bash will simply create the specified file and write the output into it, like using `touch` and including some information all in one step.
If the file does exist, it will *immediately be overwritten* without any warnings.

Let’s see the > symbol in action.
*First, make sure you’re still in the `BashWk2` folder.*

Now we’ll combine the echo command with the redirection:

```
echo 'Hello' > hello.txt
```

Notice that the word Hello was no longer printed to your screen.
Instead a file called `hello.txt` was created and we can simply view the entire contents of the file using the command `cat`

```
cat hello.txt
```

To add any additional information to the file, we can use the `>>` symbol which appends new information to the file without overwriting anything.
If the file doesn’t already exist it will be created, but this time *if the file does exist*, the file will not be overwritten, but rather the new information will be *added to the end of the existing information*.
Let’s see this in action.
(Pay attention to the correct quotation marks here)

```
echo "It's me." >> hello.txt
cat hello.txt
```

(Did you remember to use tab auto-complete in the above commands?)

*For those using phoenix as an HPC, the queueing system will usually generate a file from each job/script which ends in *out.
This is all of the standard output from your job captured into a file for you to inspect later.*

## Redirecting `stdout` to Another Tool

The idea of sending text to a file, like we have just done makes intuitive sense for most of us.
There is another incredibly useful functionality in `bash` which is commely referred to as the *pipe*.
The reasons for this are two-fold.
Firstly, we use the `|` symbol which looks like a drain-pipe, and conceptually this is exactly what we are doing and why we have tried to make you familiar with the term `stdout`.

Apart from `less`, all of the tools we have used have sent their output to `stdout` which has either appeared in the termainl window, or in the files we created.
Most commands are **also able to accept input** from `stdout` instead of referring to a file on the disk, and by using the `|` symbol it is literally like attaching a pipe to `stdout` from one command and redirecting the stream of data to the standard input (i.e. `stdin`) of another command.

Let's try this with a couple of familiar processes first.
**How many lines do you see from this process we tried earlier?**

```
echo -e "Hello\tworld"
```

So if we take this output and pipe it into `wc`, we should know what to expect.

```
echo -e "Hello\tworld" | wc -l
```

So here, we'e taken the output from `echo` and piped it to the input of `wc`.
Let's confirm what we're seeing by changing that `<tab>` separator to a line break.

```
echo -e "Hello\nworld" | wc -l
```

*There is literally no limit to how many commands we can chain together like this.*

We can even do silly (but sometime useful) things like piping the output of a help page into `less` if we're on `git bash`, and don't have any `man` pages.

```
ls --help | less
```

# Manipulating Data From Files

## Selecting a column

Sometimes we want to dig a little deeper through a file, and we can extract one or more columns from any delimited file using the filter `cut`.
By default, it looks for `<tab>` separated columns, but this can be set to any delimiter we can imagine.
Let's try to grab just the first column, except we know that this has thousands of lines, so let's use our amazing skills to just look at the first few lines.

```
cut -f1  BDGP6_genes.gtf | head
```

This will have set the first column to `stdout`, wherever a `<tab>`-separated line was found.
Where no `<tab>` symbol was found, this will have sent the whole line to `stdout`.
By default we're using the `<tab>` symbol as the delimiter, so check the help or man page to *see if you can find a way to stop cut printing any lines that don't contain a delimiter.*

Now, we've found out how to extract the chromosome information from this file, which is what the first column holds, let's try and do something useful.

## Summarising data using `uniq`

Let's see how many chromosomes we have gene information about, by taking this column and just restricting it to unique entries, using the command `uniq`.
Now we're familiar with the pipe, we can send this column to `uniq`.

```
cut -f1 -s BDGP6_genes.gtf | uniq
```

This is a list of all the chromosomes with entries in that column.
We can add the additional flag `-c` to uniq to count these.

```
cut -f1 -s BDGP6_genes.gtf | uniq -c
```

So now we've just counted how many genes there are on each chromosome.
Pretty cool, huh!
If we needed that for another analysis we could even write this to a file using `>`.
**Should we keep the header information if we did that?**

Let's go completely nuts and use a different delimiter inthe `cut` command.
You might have spotted that beyond the 8th column, the `<tab>`delimiters stop and we have a single column, with values separated by a semi-colon.
We can specifiy this as the delimiter using the argument `-d\;`.
Here we've had to escape the semi-colon as it is a special character as we want to use it as a true semi-colon.

**If we wanted to get the gene_biotype column, what number would we need to put after the `-f`argument?**
Try piping this into `uniq -c` and see what happens?
**Was this as you expected?**

## Summarising files using `sort`

The algorithm underlying `uniq -c` checks adjacent lines for equality, and if they're not equal keeps them both.
If they're equal it discards the second entry.
To really make this work, we'd need to sort the column in between the `cut` and `uniq` commands.

```
cut -f4 -s -d\; BDGP6_genes.gtf | sort | uniq -c
```
