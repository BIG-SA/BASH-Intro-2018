
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
- Iteration using `for` loops

## Before we start

Let’s create a new folder and put all of this session's work in there. 
First navigate to your home folder, then create a new directory as below.

    cd ~

From here let’s create a new folder for today:

    mkdir -p Bash_Workshop/Session4/files
    cd Bash_Workshop/Session4
    pwd

## Today's Data

To run example scripts in this tutorial, we will need some example data. The Australian government provide a large amount of open data on the website [data.gov.au](https://data.gov.au/), and to create scripts we will use a dataset containing information about particle pollution data for the year 2015 and ongoing for the Adelaide CBD region.

Side note: Public datasets are becoming increasily common in today's society, and many companies and media outlets use this data to investigate everything from the missuse of public funds, to environmental monitoring. A brilliant example of "data journalism" is the US website [fivethirtyeight](http://fivethirtyeight.com/).

To download this data, we will need to go to our "files" directory, run the `wget` command to get a zip file containing all our csv files, and unpack the zip file:

    cd files/
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

    # If you havent already, change into the files directory 
    # The following line will need to be modified accordingly if you saved the files somewhere other than ~/Bash_Workshop/Session4/files
    cd ~/Bash_Workshop/Session4/files

    # Read the input file in my current directory into a variable
    INPUT="ADL07p/ADL07p_1hr201501.csv"

    # Use unix cut to divide the file into columns via the ","
    #   - print each column except 2 and 3
    #   - create a new file
    cut -d',' -f1,4,5 ${INPUT} > ${INPUT}.new.csv

Save this file as "basic_example_1.sh". To run this file on the command-line, we would run:

    $ bash basic_example_1.sh

__Questions__

- What was the output?
- What was contained in the new file and what was the file called?
- Where was this file saved and why was it saved there?


## Using the `#` symbol: Shebang and Comments

The example above displayed a very basic example of a bash shell script.

Every bash shell script begins with what is known as a "shebang", which we would commonly recognise as a hash sign followed by an exclamation mark, i.e #!. This is immediately followed by /bin/bash, which tells the interpreter to run the command `bash` in the directory /bin. This opening sequence is vital & tells the computer how to respond to all of the following commands. As a string this looks like:

    #!/bin/bash

The hash symbol generally functions as a comment character in scripts (as shown above in "Basic example 1"). Sometimes we can include lines in a script to remind ourselves what we’re trying to do, and we can preface these with the hash to ensure the interpreter doesn’t try to run them. It’s presence as a comment here, followed by the exclamation mark, is specifically looked for by the interpreter but beyond this specific occurrence, comment lines are generally ignored by scripts & programs.

Comments are very important in programming because they act as notes or explainations so you can understand what you were thinking when you wrote it. If you look at your code 6 months from now, there is a very strong chance that you won’t recall exactly what you were thinking at the time, so these comments can be a good place just to explain something to the future version of yourself. There is a school of thought which says that you write code primarily for humans to read, not for the computer to understand.

## Adjusting file permissions and executing a script

There are two main ways of executing a script. Firstly, as shown in "Basic Example 1", we can just declare the intepreter of the language on the command-line, followed by the name of the script.

    bash basic_example_1.sh

However, we shouldnt need to call the name of the script, considering that the interpreter is already declared in line 1! To do this, the script needs to be executible, and we need to adjust the read/write/execute file permissions explained above. By adding execute permissions to the file, the script can be run as a program and not just a regular file.

First we’ll look at the files in the folder using `ls -l` and note that unlike the above figure, these triplets should be `rw-` for the user & the group you belong to. To make this script executable, enter the following in your terminal.

    chmod +x ./basic_example_1.sh

If you run `ls -l` again, you'll notice that the third flag in the triplet has now become an x. This indicates that we can now execute the file in the terminal. As a security measure, Linux doesn’t allow you to execute a script from within the same directory so to execute it enter the following:

    ./basic_example_1.sh

Let’s now look at another simple scripts.


## Iteration and `for` loops

So far we've just touched on executing one task at a time. But what if you want to run the same command on multiple files?

For this we can use a `for` loop. A loop is a type of statement that enables the programmer to execute repetitive tasks in one small bit of code, instead of just repeatedly writing the command. The loop has three elements; a) the item, b) the iteratable object or list, and c) command to run on our item.

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

## Basic example 2

In the following example we are going to read some text and make four files

    #!/bin/bash

    # Create a new directory called results where our files will be saved
    mkdir -p results

    # Loop:  1.Iterate over the list of numbers "1 2 3 4 5" and read into NUM
    #   2. Echo some text into a new file
    #   and 3. print file using the command cat
    WORKING_DIR=$(pwd)

    for NUM in 1 2 3 4 5
     do
        echo "The file has the number ${NUM} in it" > ${WORKING_DIR}/results/file_"${NUM}".txt
        cat ${WORKING_DIR}/results/file_"${NUM}".txt
    done

Now save this script and run it:

    bash basic_example_2.sh

---

### EXERCISE
