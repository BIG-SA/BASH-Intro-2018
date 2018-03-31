
# Bash Scripting

Sometimes we need to perform repetitive tasks on multiple files, or need to perform complex series of tasks and writing the set of instructions as a script is a very powerful way of performing these tasks. They are also an excellent way of ensuring the commands you have used in your research are retained for future reference. Keeping copies of all electronic processes to ensure reproducibility is a very important component of any research.

## Scripts are just basic "to-do" lists

Many people can get freaked out by the complexity of a program "script", but its really quite simple. Think of a script as a to-do list that you are giving the computer to run a task. The script/code is sent to a interpreter (in our case, we mean the program `bash`), which then converts the to-do list into a language that the computer can understand and execute.

Programmers will often start a project with a basic outline of the tasks that we need to do.

### Basic example 1

For example, I want to take my comma seprated file (csv file) and remove the 2nd column of the table. To do this I would write the following pseudo-code:

    Step 1: Open input file
    Step 2: Cut the file into columns (there are 5 columns)
    Step 3: Print every column except column 2
    Step 4: Output the result into a new file

This simple example can be written in bash below (Don't worry that you don't understand all of it yet. That will come later):

    #!/bin/bash

    # Read the input file in my current directory into a variable
    INPUT="./MyInputFile.csv"

    # Use unix cut to divide the file into columns via the ","
    #   - print each column except 2
    #   - create a new file
    cut -d',' -f1,3,4,5 ${INPUT} > ${INPUT}.newfile.csv

Save this file as "basic_example_1.sh". To run this file on the command-line, we would run:

    $ bash basic_example_1.sh

## Description of a shell script

Every bash shell script begins with what is known as a shebang, which we would commonly recognise as a hash sign followed by an exclamation mark, i.e #!. This is immediately followed by /bin/bash, which tells the interpreter to run the command `bash` in the directory /bin. This opening sequence is vital & tells the computer how to respond to all of the following commands. As a string this looks like:

    #!/bin/bash

The hash symbol generally functions as a comment character in scripts (as shown above in "Basic example 1"). Sometimes we can include lines in a script to remind ourselves what we’re trying to do, and we can preface these with the hash to ensure the interpreter doesn’t try to run them. It’s presence as a comment here, followed by the exclamation mark, is specifically looked for by the interpreter but beyond this specific occurrence, comment lines are generally ignored by scripts & programs.

Let’s now look at another simple scripts. These are really just examples of some useful things you can do & may not really be the best scripts from a technical perspective. Hopefully they give you some pointers so you can get going

### Basic Example 2

Don’t try to enter these commands directly in the terminal!!! They are designed to be placed in a script which we will do after we’ve inspected the contents of the script. First, just have a look through the script & make sure you understand what the script is doing.

    #!/bin/bash

    # First we'll declare some variables with some text strings
    ME='Put your name here'
    MESSAGE='This is your first script'

    # Now well place these variables into a command to get some output
    echo -e "Hello ${ME}\n${MESSAGE}\nWell Done!"


Firstly, you may notice some lines that begin with the # character. These are comments which have no impact on the execution of the script, but are written so you can understand what you were thinking when you wrote it. If you look at your code 6 months from now, there is a very strong chance that you won’t recall exactly what you were thinking, so these comments can be a good place just to explain something to the future version of yourself. There is a school of thought which says that you write code primarily for humans to read, not for the computer to understand.

Using the text editor gedit, enter the above code into a file setting your actual name as the ME variable, and save it as wellDone.sh in your home folder.

### Executing the script

Unfortunately, this script cannot be executed yet but we can easily enable execution of the code inside the script. If you recall the flags from earlier which denoted the read/write/execute permissions of a file, all we need to do is set the execute permission for this file. First we’ll look at the files in the folder using `ls -l` and note these triplets should be `rw-` for the user & the group you belong to. To make this script executable, enter the following in your terminal.

Notice that the third flag in the triplet has now become an x. This indicates that we can now execute the file in the terminal. As a security measure, Linux doesn’t allow you to execute a script from within the same directory so to execute it enter the following:

    ./basic_example_2.sh


### Making a Small Change

Notice that the third flag in the triplet has now become an x. This indicates that we can now execute the file in the terminal. As a security measure, Linux doesn’t allow you to execute a script from within the same directory so to execute it enter the following:

    ./wellDone.sh

 Now let’s change the variable ME in the script to read as

    ME=$1

and save this as wellDone2.sh. You’ll now need to set the execute permission again.

    chmod +x wellDone2.sh

This time we have set the script to receive input from a command-line argument, and we will need to supply a value, which will then be placed in the variable ME. Choose whichever random name you want and enter the following

    ./wellDone2.sh Boris


## Variables

As you've probably noticed above, a variable is essentially a holding place for information that the program needs to run its code. In "Basic example 1", we read our input file into the variable INPUT and in "Basic example 2", we read text into the variable ME and MESSAGE. You'll notice that when you declare the variable you use the equals sign to assign the information to that variable name (e.g. VARIABLE_NAME="THIS IS THE INFORMATION"), while when you actually use the variable in your code, we put a `$` in front to declare that this is in fact a variable.

Additionally, notice the use of the curly brackets around the variable name in "Basic example 1" (e.g. `${INPUT}`). Whilst not being strictly required, this can make it easy for you to follow in the future when you’re looking back. Its also helpful to type variables using strictly upper-case letters. This is another optional coding style, but can also make things clear for you as you look back through your work. Most command line tools use strictly lower-case names, so this is another reason the upper-case variable names can be helpful.

### Special variables

As we've seen in "Basic Example 2", the variable `$1` is special in the sense that it demarks the first argument at the command-line. So if we execute the command `./wellDone2.sh Boris`, "Boris" will be the value assigned to $1. There are a number of special variables that can be used when writing bash scripts, and these have certain behaviours:

- `$0` - The name of the Bash script.
- `$1` -> `$9` - The first 9 arguments to the Bash script. (As mentioned above.)
- `$#` - How many arguments were passed to the Bash script.
- `$@` - All the arguments supplied to the Bash script.
- `$?` - The exit status of the most recently run process.
- `$$` - The process ID of the current script.
- `$USER` - The username of the user running the script.
- `$HOSTNAME` - The hostname of the machine the script is running on.
- `$SECONDS` - The number of seconds since the script was started.
- `$RANDOM` - Returns a different random number each time is it referred to.
- `$LINENO` - Returns the current line number in the Bash script.

Within a script we can also read the results of commands into variables. For example, I want to read the total number of lines in a file into the variable "LINES". We could write something like this script:

    #!/bin/bash

    # Read in my input file into INPUT
    INPUT="my_file.txt"

    # Count the number of lines using wc and read into LINES
    LINES=$(wc -l ${INPUT})

    # print the variable to see the result
    echo "${LINES}"


EXERCISE:

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

### Control statements
