
# Bash Scripting

2018-04-04

Jimmy Breen (jimmymbreen@gmail.com)
Stephen Pederson
Paul Wang
John Toubia

## Recap

## This session

This session we will be tying together many of the concepts that you have learnt over the last few weeks. Today we'll be learning about:

- Executing `bash` Scripts
- Path orientation within scripts
- Variables
- Control statements
- Iteration using `for` loops

## Before we start

Much like we have bgun all other weeks, if you didn’t create a folder for last week’s files, let’s create one and put all of today’s work in there. First navigate to your home folder, which may be one of /home/<yourname>, /Users/<yourname>, /u or /c/Users/<yourname>. The best way to get to this directory is

    cd ~

From here let’s create a new folder for today:

    mkdir BashWk4
    cd BashWk4
    pwd

## Today's Data

To run example scripts in this tutorial, we will need some example data. The Australian government provide a large amount of open data on the website [data.gov.au](https://data.gov.au/), and to create scripts we will use a dataset containing information about particle pollution data for the year 2015 and ongoing for the Adelaide CBD region.

Side note: Public datasets are becoming increasily common in today's society, and many companies and media outlets use this data to investigate everything from the missuse of public funds, to environmental monitoring. A brilliant example of "data journalism" is the US website [fivethirtyeight](http://fivethirtyeight.com/).

To download this data, we will need to go to our "files" directory, run the `wget` command to get a zip file containing all our csv files, and unpack the zip file:

    cd ./BASH-Intro-2018/files/
    wget -c https://data.sa.gov.au/data/dataset/9fd65c8d-a3bc-474e-9cf2-03a58a837fc0/resource/a0fa35fb-fedf-4db6-8bbb-668f9959fe42/download/adl07p.zip
    unzip adl07p.zip


---

## Introduction

Sometimes we need to perform repetitive tasks on multiple files, or need to perform complex series of tasks and writing the set of instructions as a script is a very powerful way of performing these tasks. They are also an excellent way of ensuring the commands you have used in your research are retained for future reference. Keeping copies of all electronic processes to ensure reproducibility is a very important component of any research.

### Scripts are just basic "to-do" lists

Many people can get freaked out by the complexity of a program "script", but its really quite simple. Think of a script as a to-do list that you are giving the computer to run a task. The script/code is sent to a interpreter (in our case, we mean the program `bash`), which then converts the to-do list into a language that the computer can understand and execute.

Programmers will often start a project with a basic outline of the tasks that we need to do. Often we call it "pseudo-code".

### Basic example 1

Have a look at our Adelaide CBD dataset by using the command `less` on the first month file of 2015 ("./ADL07p/ADL07p_1hr201501.csv"). Theres a lot of data in there, but we want to simplify it by only grabbing the 1st column ("Date/Time") and the 4th and 5th columns ("Temperature Deg C" and "Barometric Pressure atm"). To do this I would write the following pseudo-code:

    Step 1: Change into our data directory
    Step 2: Open input file
    Step 3: Cut the file into columns
    Step 3: Print 1st, 4th and 5th column
    Step 4: Output the result into a new file

This simple example can be written in bash below (Don't worry that you don't understand all of it yet. That will come later):

    #!/bin/bash

    # If you havent already, change into the files directory in the "BASH-Intro-2018" diectory
    cd ./BASH-Intro-2018/files

    # Read the input file in my current directory into a variable
    INPUT="ADL07p/ADL07p_1hr201501.csv"

    # Use unix cut to divide the file into columns via the ","
    #   - print each column except 2 and 3
    #   - create a new file
    cut -d',' -f1,4,5 ${INPUT} > ${INPUT}.new.csv

Save this file as "basic_example_1.sh". To run this file on the command-line, we would run:

    $ bash basic_example_1.sh

What was the output? What was contained in the new file and what was the file called?


## Using the `#` symbol: Shebang and Comments

The example above displayed a very basic example of a bash shell script.

Every bash shell script begins with what is known as a "shebang", which we would commonly recognise as a hash sign followed by an exclamation mark, i.e #!. This is immediately followed by /bin/bash, which tells the interpreter to run the command `bash` in the directory /bin. This opening sequence is vital & tells the computer how to respond to all of the following commands. As a string this looks like:

    #!/bin/bash

The hash symbol generally functions as a comment character in scripts (as shown above in "Basic example 1"). Sometimes we can include lines in a script to remind ourselves what we’re trying to do, and we can preface these with the hash to ensure the interpreter doesn’t try to run them. It’s presence as a comment here, followed by the exclamation mark, is specifically looked for by the interpreter but beyond this specific occurrence, comment lines are generally ignored by scripts & programs.

Comments are very important in programming because they act as notes or explainations so you can understand what you were thinking when you wrote it. If you look at your code 6 months from now, there is a very strong chance that you won’t recall exactly what you were thinking at the time, so these comments can be a good place just to explain something to the future version of yourself. There is a school of thought which says that you write code primarily for humans to read, not for the computer to understand.


## File permissions

An important concept in UNIX computing is file permissions. There are files in which you look at and interact with when you are running analysis on a UNIX command line, and there are files in which are incredibily important to the system that we shouldnt be able to touch. And of course, if you are creating and interacting with files in your home directory, you want to make those files only available to you, and not other users.

A file can have there are three types of attributes:

1. Ownership permissions: what actions the owner of the file can perform on the file
2. Group permissions: what actions a user, who is a member of the group that a file belongs to, can perform on the file, and
3. Other's permissions: what action all other users can perform on the file

To demonstrate what this means, I'm going to run a simple `ls` command on the data directory that we downloaded and unpacked for this tutorial:

    ls -l ./ADL07p

The result on my computer is:

    total 2192
    -rw-r--r--  1 jbreen  staff  26826 23 Jun  2016 ADL07p_1hr201501.csv
    -rw-r--r--  1 jbreen  staff  24288 23 Jun  2016 ADL07p_1hr201502.csv
    -rw-r--r--  1 jbreen  staff  26977 23 Jun  2016 ADL07p_1hr201503.csv
    -rw-r--r--  1 jbreen  staff  25593 23 Jun  2016 ADL07p_1hr201504.csv
    ...

For each file there is a number of blocks of information. For now, we don't need to know what everything means, but you initially you might be able to guess what each means. The third and fourth block of information (i.e. `jbreen` and `staff`) are the owner and group permissions. After that there is the size of the file (in bytes), the date that the file was last edited and then the name of the file.

The first block contains the important information and that is regarding file access permissions. These permissions follow the format of read(r), write(w) and execute(x). The block contains the following information:

| Directory | Owner | Group | Other |
|-----------|-------|-------|-------|
| - or "d"  |  rwx  |  rwx  |  rwx  |

So in my directory, `ADL07p_1hr201501.csv` has the permissions of `-rw-r--r--` which means that its a file (not a directory), where `jbreen` (the owner) has read and write access but can't execute it `rw-`, members of the group (`staff`) has read access only and everyone else has read access only.



## Adjusting file permissions and executing a script

There are two main ways of executing a script. Firstly, as shown in "Basic Example 1", we can just declare the intepreter of the language on the command-line, followed by the name of the script.

    $ bash basic_example_1.sh

However, we shouldnt need to call the name of the script, considering that the interpreter is already declared in line 1! To do this, the script needs to be executible, and we need to adjust the read/write/execute file permissions explained above. By adding execute permissions to the file, the script can be run as a program and not just a regular file.

First we’ll look at the files in the folder using `ls -l` and note these triplets should be `rw-` for the user & the group you belong to. To make this script executable, enter the following in your terminal.

    $ chmod +x ./basic_example_1.sh

If you run `ls -l` again, you'll notice that the third flag in the triplet has now become an x. This indicates that we can now execute the file in the terminal. As a security measure, Linux doesn’t allow you to execute a script from within the same directory so to execute it enter the following:

    $ ./basic_example_1.sh

Let’s now look at another simple scripts.

## Path orientation



## Variables

As you've probably noticed above, a variable is essentially a holding place for information that the program needs to run its code. In "Basic example 1", we read our input file into the variable INPUT and in "Basic example 2", we read text into the variable ME and MESSAGE. You'll notice that when you declare the variable you use the equals sign to assign the information to that variable name (e.g. VARIABLE_NAME="THIS IS THE INFORMATION"), while when you actually use the variable in your code, we put a `$` in front to declare that this is in fact a variable.

Additionally, notice the use of the curly brackets around the variable name in "Basic example 1" (e.g. `${INPUT}`). Whilst not being strictly required, this can make it easy for you to follow in the future when you’re looking back. Its also helpful to type variables using strictly upper-case letters. This is another optional coding style, but can also make things clear for you as you look back through your work. Most command line tools use strictly lower-case names, so this is another reason the upper-case variable names can be helpful.

### What can be a variable?

[TLDP.org](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html#toc1) defines variable as:

    "in bash (a variable) can contain a number, a character, a string of characters. You have no need to declare a variable, just assigning a value to its reference will create it."

This can be anything from the name of a file (as we've seen in "Basic Example 1", where we read our file into the variable `${INPUT}`), a number of string or even the output of unix command (more of those later)

---

__NOTE: Special Variables__

There are a number of special variables that can be used when writing bash scripts, and these have certain behaviours:

- `$0` - The name of the Bash script.
- `$1` -> `$9` - The first 9 arguments to the Bash script.
- `$#` - How many arguments were passed to the Bash script.
- `$@` - All the arguments supplied to the Bash script.
- `$?` - The exit status of the most recently run process.
- `$$` - The process ID of the current script.
- `$USER` - The username of the user running the script.
- `$HOSTNAME` - The hostname of the machine the script is running on.
- `$SECONDS` - The number of seconds since the script was started.
- `$RANDOM` - Returns a different random number each time is it referred to.
- `$LINENO` - Returns the current line number in the Bash script.

---

#### Basic Example 2

Using the text editor gedit, enter the above code into a file setting your actual name as the ME variable, and save it as wellDone.sh in your home folder.

    #!/bin/bash

    # First we'll declare some variables with some text strings
    ME='Put your name here'
    MESSAGE='This is your first script'

    # Now well place these variables into a command to get some output
    echo -e 'Hello ${ME}\n"${MESSAGE}"\nWell Done!'

Now change permissions and execute the script, and see what the output is

### Command line arguments

Consider the `ME` variable in the script above. Let’s change the variable to read a special variable `$1`:

    ME=$1

Now save this as `wellDone2.sh`. You’ll now need to set the execute permission again.

    chmod +x wellDone2.sh

This time we have set the script to receive input from a command-line argument, and we will need to supply a value, which will then be placed in the variable ME. Choose whichever random name you want and enter the following

    $ ./wellDone2.sh Boris

### Command outputs as variables

So its clear that you can assign a number or a string to a variable (e.g. 'Put your name here' and 'This is your first script' was assigned to the variables ME and MESSAGE in Basic Example 2), but you can also capture output of a unix command and assign it to a variable.

#### Basic Example 3

Type the following script into a text file and save it as "count_lines.sh":

    #!/bin/bash

    # Read in my input file into INPUT
    INPUT=$1

    # Count the number of lines using wc and read into LINES
    LINES=$(wc -l ${INPUT})

    # print the variable to see the result
    echo "The number of months contained in the data is ${LINES}"

Set the permissions and execute the file by declaring the name of the script and one of our Adelaide CBD csv files:

    $ ./count_lines.sh ADL07p/ADL07p_1hr201704.csv

---

## EXERCISE

The Adelaide CBD Particule data is measured by a Beta Attenuation Monitor (BAM) where the two individual measurements are PM10 BAM µg/m3 (Particulate Matter 10 microns or less) and PM2.5 BAM µg/m3 (Particulate Matter 2.5 microns or less). Across every month in 2015, during what time of the day was the PM10 and PM2.5 measurements at their highest?

Using the skills in this tutorial, as well as your knowledge of commands such as `echo`, `awk`, `grep`, write a script that reads the 2015

---

### Control statements

When we write scripts, we generally do it for a specific purpose, and therefore we are generally in control of the input data and the execution of the code that we have created. However, if I was to create a program for others to use, we often need to make sure the script is robust to any issue that the user defined inputs might throw at it.



## Iteration and `for` loops

So far we've just touched on executing one file at a time. But what if you want to run the same command on multiple files?

For this we can use a `for` loop. A loop is a type of statement that enables the programmer to execute repetitive tasks in one small bit of code, instead of just repeatedly writing the command. The loop has three elements, a) the item, b) the iteratable object or list, and c) command to run on our item.

Take our to-do list analogy for example. Instead of writing:

    From the grocery store, buy milk
    From the grocery store, buy cheese
    From the grocery store, buy coffee
    From the grocery store, buy onions
    From the grocery store, buy bread
    From the grocery store, buy chocloate
    From the grocery store, buy toilet paper
    From the grocery store, buy tomatoes
    From the grocery store, buy bacon

We could write something like:

    for item in grocery_store
     do
        buy item
    done

In this context, the word "item" ends up being the variable, and so we run the command "buy" on each "item" in the list (or iterable object) "grocery_store". Each loop requires you to start the for loop with the command `for`, indicate the command by adding the command `do` and the closing the loop by adding `done` at the end. We generally separate out this loop structure on each line, with the command indented by one tab character. This is an established style throughout most programming languages, but the loop can easily be run on one line using semi-colons as separators:

    for item in grocery_store; do buy item; done

Lets write a script that shows you how a for loop works

## Basic example 3

In the following example we are going to read some text and make four files

    #!/bin/bash

    # Read the path of the current directory into the variable WORKING_DIR
    WORKING_DIR=$(pwd)

    # Create a new directory called results where our files will be saved
    mkdir -p ${WORKING_DIRECTORY}/results

    # Loop:  1.Iterate over the list of numbers "1 2 3 4 5" and read into NUM
    #   2. Echo some text into a new file
    #   and 3. print file using the command cat
    for NUM in 1 2 3 4 5
     do
        echo "The file has the number ${NUM} in it" > ${WORKING_DIR}/results/file_"${NUM}".txt
        cat ${WORKING_DIR}/results/file_"${NUM}".txt
    done

Now save this script and run it:

    bash basic_example_3.sh
