
# Introduction

[Opening Presentation](https://gitpitch.com/BIG-SA/BASH-Intro-2018/)

## General Information

Thank you for your attendance & welcome to the *Introduction to Bash: Using the Terminal For Bioinformatics* Workshop.
This is series of training workshops and materials developed by the Bioinformatics Interest Group - South Australia (BIG-SA), specifically using the skills and resources of the Bioinformatics Hub (University of Adelaide) and the ACRF Cancer Genomics Facility

Some additional resources run by the Bioinformatics Hub which may be of interest beyond today are:

- A [University web-page](http://www.adelaide.edu.au/bioinformatics-hub/)
- To be kept up to date on upcoming events and workshops, please join the internal [Bioinformatics mailing list](http://list.adelaide.edu.au/mailman/listinfo/bioinfo)
- A [Twitter account](https://twitter.com/UofABioinfoHub/)
- An active Slack team for discussing Bioinformatics questions with the local community. Slack teams do require an invitation to join, so please [email the Hub](mailto:bioinf_hub@adelaide.edu.au) to join the community. All are welcome.

## Course Information

### Schedule

The workshop will be run in a weekly series of 2 hour sessions, with sessions for each campus as below

#### North Terrace

All sessions will be held in The Braggs, 313-314

| Date | Time | Main Topics |
| ---------- |---------- | ---------- |
| Wed, April 4th | 2-4pm  | Introduction to `bash` |
| Wed, April 11th | 2-4pm | Working with files in `bash` |
| Wed, April 18th | 2-4pm | The tools `grep`, `sed` and `awk` |
| Wed, April 25th | - | ANZAC DAY |
| Wed, May 2nd | 2-4pm | Bash Scripting |

#### Waite Campus

All sessions will be held in the Biometry Hub Meeting Room, Level 2, Waite Building

| Date | Time | Main Topics |
| ---------- |---------- | ---------- |
| Tues, April 10th | 2-4pm  | Introduction to `bash` |
| Tues, April 17th | 2-4pm | Working with files in `bash` |
| Tues, April 24th | 2-4pm | The tools `grep`, `sed` and `awk` |
| Tues, May 1st | 2-4pm | Bash Scripting |

#### Roseworthy Campus

All sessions will be held in the Davies Conference Room, Davies Building

| Date | Time | Main Topics |
| ---------- |---------- | ---------- |
| Mon, April 16th | 2-4pm  | Introduction to `bash` |
| Mon, April 23rd | 2-4pm | Working with files in `bash` |
| Mon, April 30th | 2-4pm | The tools `grep`, `sed` and `awk` |
| Mon, May 6th | 2-4pm | Bash Scripting |

### Course Outline

There will be a number of components covered during these sessions:

- using the terminal/bash shell
- the linux filesystem and how to navigate around
- working with files/directories and wildcards
- common/standard linux command tools (filters)
- pipes and redirects
- command line arguments and file permissions
- path an executable files
- advanced built-in tools (grep, awk, sed)
- regular expression
- bash scripting

The majority of data handling and analysis required in the field of bioinformatics uses the command line, alternatively known as the terminal or the `bash` shell.
This is a text-based interface in which commands must be typed, as opposed to the Graphical User Interfaces (aka GUIs) that most of us have become accustomed to.
Being able to access these tools enables you to more fully utilise the power & capabilities of your machine, for both Linux & Mac operating systems, and to a lesser extent will even enable you to dig deeper on a Windows system.

Whilst some of the tools we cover may appear trivial, they are used on a daily basis by those working in the field.
These basic tools are essential for writing what are known as shell scripts, which we will work towards throughout the first three sessions and begin to cover in the last.
These are essentially simple programs that utilise the inbuilt functions of the shell, and are used to automate processes such as de-multiplexing read libraries, or aligning reads to the genome.
A knowledge of this simple type of programming and navigation is also essential for accessing the high-performance computing resources such as *phoenix*.

### Course Aims
The long-term goal of this workshop is to enable you to carry out your own data analysis in a **reproducible** manner, using standard bioinformatics tools or write your own programs.
This course serves as a prerequisite for the Introduction to Next Generation Sequencing (NGS) Data workshop coming later in the year.

By the end of both courses, we expect the participants to:
1. Be relatively comfortable working with the command line interface, and familiar with the standard Unix command line tools
2. Understand standard NGS data formats
3. Be able to run several basic NGS data analysis methods (e.g. variant calling)
4. Understand how some simple python scripts work for custom data analysis

### Acknowledgements

This course has been put together by Paul Wang, John Toubia, Stephen Pederson and Jimmy Breen, based on previous material prepared by Stephen Bent, Nathan Watson-Haigh and Dan Kortschak.

Sections of the workshop are also based loosely on material and courses prepared
by Software Carpentary (http://software-carpentry.org/lessons/) and Ryans Tutorials
(http://ryanstutorials.net/)

We hope it will be useful in enabling you to continue and to advance your research.

## Protocols

### Entering Commands

In the following pages, we strongly encourage you to manually type all commands.
The mistakes you will inevitably make will actually *be important learning steps*.
Additionally, in your work beyond today, you will probably not have any instructions to follow.
The experience of typing these commands will equip you for future work far better than if you simply copy & paste.

### Calling For Help

For today’s session, you will also be provided with red post-it notes.
Please use these to signal whether you need help or not **by placing them on your monitors**.
These are easy for instructors to spot so we can make our way over, although do be aware that there will be times when all instructors are busy.
This will be important as we all set our computers up in the following section.


## Computer Setup

Previously we have run these sessions using Virtual Machines, but today we are opting for running all sessions using the tools locally installed on your own machines.
*As there will be a huge variety of laptops in the room, the initial set-up may be a difficult to begin with*, but will settle as the day progresses.
Although this will present numerous challenges from our perspective, we feel this is a better approach as everyone will leave knowing exactly how to get the ball rolling on their own familiar systems.

Instructions for how to set-up everything on your own machine are at the following pages.
Please follow these very carefully, and call an instructor over if you have trouble.
In particular, the Windows set-ups may take a while to get sorted.

- [Mac/OSX](../install/osxInstall.md)
- [Windows](../install/windowsInstall.md)

If you are already running Ubuntu, your computer will already be set-up correctly.


[Home](https://big-sa.github.io/BASH-Intro-2018/)
