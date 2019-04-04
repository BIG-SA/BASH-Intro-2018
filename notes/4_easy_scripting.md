* TOC
{:toc}

# Writing Scripts


Before we start this session, let's create a new folder for writing scripts.

```
cd
mkdir -p Bash_Workshop/Session4
cd Bash_Workshop/Session4
```

## Shell Scripts

Sometimes we need to perform repetitive tasks on multiple files, or need to perform complex series of tasks and writing the set of instructions as a script is a very powerful way of performing these tasks.
They are also an excellent way of ensuring the commands you have used in your research *are retained for future reference*.
Keeping copies of all electronic processes to ensure reproducibility is a very important component of any research.

Now that we've been through just some of the concepts and tools we can use when writing scripts, it's time to tackle one of our own where we can bring it all together.

### The `shebang`

Every bash shell script begins with what is known as a *shebang*, which we would commonly recognise as a hash sign followed by an exclamation mark, i.e `#!`.
This is immediately followed by an executable path, in this case `/bin/bash`.
This information is used by the program loader so that it knows which interpreter to start in order to run the script.
The interpreter is then started and given the script file itself to run.
So, if a `/bin/bash` shebang is in an [executable](#Setting-File-Permissions) file called `script.bash`, then invoking `./script.bash` is equivalent to running `/bin/bash ./script.bash`.
As a string this looks like:

```
#!/bin/bash
```

(A similar mechanism is used for starting binary executables; try running `file /bin/bash` and `file /usr/bin/R` and compare the output. `ELF` is the binary equivalent of `#!`.)

The hash symbol generally functions as a comment character in scripts.
Sometimes we can include lines in a script to remind ourselves what we're trying to do, and we can preface these with the hash to ensure the interpreter doesn't try to run them.
It's presence as a comment in the very first position of a script followed by the exclamation mark, is specifically looked for by the interpreter but beyond this specific occurrence, comment lines are generally ignored by scripts and programs.

## An Example Script

First let's look at some simple scripts.
These are really just examples of some useful things you can do and may not really be the best scripts from a technical perspective.
Hopefully they give you some pointers so you can get going

**Don't try to enter the following commands directly in the terminal!!!**
They are designed to be placed in a script which *we will do after we've inspected the contents of the script* (see next section).
Let's start by just looking through the script to make sure we understand what the script is doing.

```
#!/bin/bash

# First we'll declare some variables with some text strings
ME='Put your name here'
MESSAGE='This is your first script'

# Now well place these variables into a command to get some output
echo -e "Hello ${ME}\n${MESSAGE}\nWell Done!"
```

- You may notice some lines that begin with the `#` character.
These are *comments* which have no impact on the execution of the script, but are written so you can understand what you were thinking when you wrote it.
If you look at your code 6 months from now, there is a very strong chance that you won't recall exactly what you were thinking, so these comments can be a good place just to explain something to the future version of yourself.
Programs perform two main functions: 1) make a machine perform a set of defined actions and 2) explain to other humans (or yourself later on) what the machine is doing and why.
The second function is more important, otherwise scripts would be written in machine code rather than something that looks like English.
- Another coding style which can be helpful is the enclosing of each *variable name* in curly braces every time the value is called, e.g. `${ME}`
While not being strictly required, this can make it easy for you to follow in the future when you're looking back and it clarifies where variable labels end and other text begins.
For example what is the difference between `${NAME_FRAG}rest_of_name` and `$NAME_FRAGrest_of_name`?
In the first case, `${NAME_FRAG}` is added to `rest_of_name`, but in the second case, `bash` will try to find to value of the variable `$NAME_FRAGrest_of_name`.
A variable that may not exist!
- Variables have also been named using strictly upper-case letters.
This is another optional coding style, but can also make things clear for you as you look back through your work.
Most command line tools use strictly lower-case names, so this is another reason the upper-case variable names can be helpful.

#### Question
{:.no_toc}
In the above script, there are two variables.
Although we have initially set them each to be one value, they are still variables.
*What are their names?*

### Writing and Executing Our First Script
{:.no_toc}

Let's create an empty file which will become our script.
We'll give it the suffix `.sh` as that is the common convention for bash scripts.
Make sure you're in the `Bash_Workshop/Session4` folder, then enter:

```
touch wellDone.sh
```

Now open this using the using the text editor `nano`:

```
nano wellDone.sh
```

Enter the above code into this file **setting your actual name as the ME variable**,  and save it by using <kbd>Ctrl</kbd>+<kbd>O</kbd> (indicated as `^O`) in the nano screen.
Once you're finished, you can exit the `nano` editor by hitting <kbd>Ctrl</kbd>+<kbd>X</kbd> (written as `^X`).
Assuming that you've entered everything correctly, we can now execute this script by simply entering

```
bash wellDone.sh
```

### Setting File Permissions

Unfortunately, this script cannot be executed without calling `bash` explicitly, but we can also enable execution of the file directly by setting the *execute flag* in the file permissions.
First let's look at what permissions we have:

```
ls -lh *.sh
```

You should see output similar to this:
```
-rw-rw-r-- 1 trainee trainee  247 Aug  14 14:48 wellDone.sh
```

- Note how the first entry is a dash (`-`) indicating this is a file.
- Next come the three Read/Write/Execute triplets which are `rw-` followed by `rw-` and `r--`

#### Question
{:.no_toc}

*Interpret the final triplet? What are these permissions indicating, and for whom?*

As you can see, the `x` flag has not been set in any of the triplets, so this file is yet not executable as a script.
To do this, we simply need to set the `x` flag using the `chmod` command, then we'll look again using long-listing format.

```
chmod +x wellDone.sh
ls -lh *.sh
```

Note how the file now has the `x` flag set for every user, which means every user can execute this script.
Now we can execute the script by calling it using the file path.
One of the settings in `bash` though won't allow you to execute the file from the same folder, so we need to add the `./` prefix to the script.

```
./wellDone.sh
```

We can set each of these flags for all triplets using `+` to turn the flag on, or `-` to turn the flag off.
If we wanted to remove `write` permissions for all users we could simply use the command:

```
chmod -w wellDone.sh
ls -lh *.sh
```

This can be a very useful trick for *write-protecting* files and directories!

These flags are represented by *binary bits* that are either on or off.
Reading from left to right:
1. the first bit is the **r**ead flag, which has the value 4
2. the second bit is the **w**rite flag, which has the value 2
3. the third bit is the e**x**ecute flag, which has value 1

Thus each combination of flags can be represented by a single integer, as shown in the following table:

| Value | Binary | Flags | Meaning |
|:----- | ------ | ----- |:------- |
| 00    | `000`  | `---` | No read, no write, no execute |
| 01    | `001`  | `--x` | No read, no write, execute |
| 02    | `010`  | `-w-` | No read, write, no execute |
| 03    | `011`  | `-wx` | No read, write, execute    |
| 04    | `100`  | `r--` | Read, no write, no execute |
| 05    | `101`  | `r-x` | Read, no write, execute    |
| 06    | `110`  | `rw-` | Read, write, no execute    |
| 07    | `111`  | `rwx` | Read, write, execute	      |

We can now set permissions using a 3-digit code, where 1) the first digit represents the file owner, 2) the second digit represents the group permissions and 3) the third digit represents all remaining users.

To set the permissions for our script to `read-write-execute`for you and any other users in the group you belong to, we could now use
```
chmod 774 wellDone.sh
ls -lh *sh
```

#### Question
{:.no_toc}
*What will the final 4 in the above settings do?*

### Modifying our script to receive input

In the initial script we used two variables `${ME}` and `${MESSAGE}`.
Now let's change the variable `${ME}` in the script to read as `ME=$1`.
First we'll create a copy of the script to edit, and then we'll edit using `nano`

```
cp wellDone.sh wellDone2.sh
nano wellDone2.sh
```

(Change the `ME` variable now...)

```
chmod +x wellDone2.sh
ls -lh *sh
```

This time we have set the script to *receive input from stdin* (i.e. the terminal), and we will need to supply a value, the first of which will be placed in the variable `${ME}`.
Choose whichever random name you want (or just use "Boris" as in the example) and enter the following
```
./wellDone2.sh Boris
```

As you can imagine, this style of scripting can be useful for iterating over multiple objects.
A trivial example, which builds on a now familiar concept would be to try the following.
```
for n in Boris Fred; do (./wellDone2.sh ${n}); done
```

As a good example, this script could summarise key features in a file.
Then we could simply pass the script multiple files using this strategy, and write the output to another file using the `>` symbol.

## Using `for` Loops

Here's an example of a script which uses a `for` loop.

```
#!/bin/bash

FILES=$(ls *sh)

COUNT=0 # Initialise a counter variable at zero
for f in ${FILES};
do
    ((COUNT++)) # This will increment the COUNT variable by one every time
    ln=$(wc -l ${f} | sed -r 's/([0-9]+).+/\1/g')
    echo "File number ${COUNT} (${f}) has ${ln} lines"
done
```

#### Task
{:.no_toc}
Save this as a script in the `Bash_Workshop/Session4` folder called `lineCount.sh`.
**Add comments** where you think you need them to make sure you understand what's happening.

## Using the `%` shortcut

Before we write our net simple script, we'll need to download a pair of `fasta` files.

<!--FIXME(kortschak): This is no longer the locations. Update where is it - currently on the data server, but likely changed to ~/data-->
```
curl "https://universityofadelaide.box.com/s/d4rs2qphctukwxwg2y6ypdii9i4g4bo8" | tar xvz -C ./
```

This will give you two files `SRR5882797_R1.fa` and `SRR5882797_R2.fa` which are paired fastq files.
Let's write a script to change the suffix of both of these from `.fa` to `.fasta`

```
touch changeSuffix.sh
nano changeSuffix.sh
```

Once the `nano` editor has opened, enter the following script before *writing out* (<kbd>Ctrl</kbd>+<kbd>O</kbd>).

```
#!/bin/bash

OLDSUFFIX=fa
NEWSUFFIX=fasta
echo "Change all files of suffix" $OLDSUFFIX "to " $NEWSUFFIX
for f in *.${OLDSUFFIX};
do
    F=${f%.${OLDSUFFIX}}.${NEWSUFFIX}
    echo "Rename file" $f "to" $F
    mv $f $F
done
echo "DONE"
```

While this is essentially a trivial script, note the line `F=${f%.${OLDSUFFIX}}.${NEWSUFFIX}`.
Here we have use the `%` like a pair of scissors and we have *snipped* the `.${OLDSUFFIX}` end off the filename, and replaced it with `.${NEWSUFFIX}`.
In bioinformatics we often have samples which go through multiple steps and this can be very useful for taking from `fastq` to `bam` to `sorted.bam` or other similar data flows.

Let's make the file executable, then run it in our directory.

```
chmod +x changeSuffix.sh
./changeSuffix.sh 
```

This will change the suffic of every `.fa` file to `.fasta`.

#### Task
{:.no_toc}

*Rewrite this script to return these suffixes to `.fa`*

#### Task
{:.no_toc}

Below is the main engine of a script, which we'd like you to complete.
In your script, return a tab-delimited output where each line contains the name of the fasta file in the first column, and the second column contains the number of sequences in the file.

```
for fasta in *.fa
do
    grep -c "^>" ${fasta} >> ${fasta}.count
done
```

(Note that for simple loops like the one above, there is an excellent tool called `parallel` that does this work for you and distributes it across all available cores. The loop here would be written as "`parallel 'grep -c "^>" {} >> {}.count' ::: *.fa`". See [here](https://www.gnu.org/software/parallel/) for more details.)

## A Final Script

Let's get one final file containing all the `ncRNA` sequences from *Drosophila melanogaster*.

```
curl -O ftp://ftp.ensembl.org/pub/release-93/fasta/drosophila_melanogaster/ncrna/Drosophila_melanogaster.BDGP6.ncrna.fa.gz
```

After you've downloaded it, unzip the file using `gunzip`

Briefly inspect the file before checking the script.
We're going to extract some key information from those sequence header lines.

```
head Drosophila_melanogaster.BDGP6.ncrna.fa
```

Now let's look through the following script before we run it.
There is a lot of extra information here!

```
#!/bin/bash

INFILE=$1

# Check the file has the suffix .fa or .fasta
SUFFIX=$(echo ${INFILE} | sed -r 's/.+(fasta|fa)$/\1/')
if [ ${SUFFIX} == "fa" ] || [ ${SUFFIX} == "fasta" ]; then
    echo File has the suffix ${SUFFIX}
else
    echo File does not have the suffix 'fa' or 'fasta'. Exiting with error.
    exit 1
fi

# Define the output file by changing the suffix to .locations
OUTFILE=${INFILE%.${SUFFIX}}.locations
echo Output will be written to $OUTFILE

# Get the header lines which correspond to chromosomes, then collect the
# gene id, chromosome, start and end and write to the output file
egrep '^>.+chromosome' ${INFILE} | \
  sed -r 's/.+BDGP6:([^:]*):([0-9]+):([0-9]+).+gene:([^ ]+).+/\4\t\1\t\2\t\3/g' \
  > ${OUTFILE}

echo Done
```

- After the `shebang`, the first line takes a filename as input.
- The next set of lines contains two processes:
    + The suffix `fasta` or `fa` is pulled from the filename and passed to the variable `${SUFFIX}`. *What will this return if the suffix is not either of these?*
    + Next, the value of the new variable `${SUFFIX}` is checked to make sure it contains only `fa` or `fasta`
        + If this condition is met a message will print to `stdout`
        + If one of these conditions is not satisfied, the script will exit giving an error message (`exit 1`)
    + The output file (`OUTFILE`) is defined by changing the suffix from whichever is provided to `.locations`. The use of the `%` to *snip* the filename and replace with another is a very useful trick
    + Finally the header lines containing the word "chromosome" are piped into `sed`
        + `sed` then captures the **chromosome** (`[^:]*`), **start** (`[0-9]+`), **end** (`[0-9]+`) and **gene id** (`[^ ]+`)
        + These are returned in the order **gene id**, **chromosome**, **start**, **end**
        + All information is written to the file specified in `${OUTFILE}`


Save this as the file `getLocations.sh` and make it executable using `chmod +x`
Now run it passing the `.fa` file as the first argument.

```
./getLocations.sh Drosophila_melanogaster.BDGP6.ncrna.fa
```

# Using an HPC environment

Much of the time we'll be using an HPC environment such as `phoenix` or `tango` which rely on a queueing system and each script being submitted as a job in the queue.
On `phoenix` the queuing system is known as `slurm`.

Additionally, due to the large number of avaiable tools for the large number of users, the exact tools we need won't be automatically available like they are on your VM.
Instead, they are available as 'modules' which we load at the start of our script.

## Connecting to an HPC

The most simple way to work on an HPC is to open your terminal and connect using `ssh` in the same way as we have been connecting to our VMs.
If you have a phoenix account, this would look like:

```
ssh axxxxxxx@phoenix.adelaide.edu.au
```

This would then ask you for your usual ITS password and will take you to your home folder `~`.

## Modules

If you're on `phoenix` or another HPC, the most common way of accessing the list of available modules is to use the command `module avail`.
Alternatively, you may choose to search explicitly for a tool using `module spider samtools`, which would look for all modules that matched 'samtools'.

## Slurm Parameters

At the start of a script written to run on `slurm` you'll commonly see a set of specific comments, which the queuing system is able to interpret.
An example may be:
```
#!/bin/bash
#SBATCH -p batch
#SBATCH -N 1
#SBATCH -n 6
#SBATCH --time=2:00:00
#SBATCH --mem=32GB
#SBATCH -o /home/axxxxxxx/AnalysisName/scriptName_%j.out
#SBATCH -e /home/axxxxxxx/AnalysisName/scriptName_%j..err
#SBATCH --mail-type=END
#SBATCH --mail-type=FAIL
#SBATCH --mail-user=stephen.pederson@adelaide.edu.au
```

Let's look at these on at a time:

```
#SBATCH -p batch
```
This is requesting a specific section of the HPC. You can request sections such as `himem` here depending on your job, but choosing `batch` just puts it into the general pool able to run wherever appropriate.

```
#SBATCH -N 1
#SBATCH -n 6
```

An HPC like phoenix consists of a series of nodes, with 16/32 cores in each node. Here we are selecting one node (`-N 1`) and using 6 cores from that node (`-n 6`).
You can change this depending on your requirements.

```
#SBATCH --time=2:00:00
#SBATCH --mem=32GB
```

The next two lines just specify how long you expect the job to run and how much memory to allocate.
If you underestimate, the job will fail as it reaches the limit.
If you overestimate, you may just wait a little longer in the queue.
Either way, it's nothing to become too anxious about.

```
#SBATCH -o /home/axxxxxxx/AnalysisName/scriptName_%j.out
#SBATCH -e /home/axxxxxxx/AnalysisName/scriptName_%j.err
```

These two lines specify files which will take the output generated by `stdout` and `stderr`.
Any `echo` messages you include will go to `stdout` so this will be a useful fill for you to keep.
`%j` will be your job ID and you'll find that you often submit the same job a few times as the first attempt will often fail.


The final three lines just specify when you will receive an email and on what account.


[Home](../)
