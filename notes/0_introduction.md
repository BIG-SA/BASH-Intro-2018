# Introduction

## General Information
# Should we add BIG-SA details (if we have any by then?)

Thank you for your attendance & welcome to the *Introduction to Bash: Using the Terminal For Bioinformatics* Workshop.
This is an offering by the University of Adelaide, Bioinformatics Hub and the Bioinformatics Interest Group - South Australia (BIG-SA).

The Bioinformatics Hub is a centrally funded initiative from the Department of Vice-Chancellor (Research), with the aim of assisting & enabling researchers in their work.
Training workshops & seminars such as this one are an important part of this initiative.

Some additional resources run by the Bioinformatics Hub which may be of interest beyond today are:

- A University web-page at http://www.adelaide.edu.au/bioinformatics-hub/
- To be kept up to date on upcoming events and workshops, please join the internal Bioinformatics mailing list on http://list.adelaide.edu.au/mailman/listinfo/ bioinfo
- A Twitter account https://twitter.com/UofABioinfoHub/
- An active Slack team for discussing Bioinformatics questions with the local community. Slack teams do require an invitation to join, so please email the Hub on bioinf_hub@adelaide.edu.au to join the community. All are welcome.

Today’s workshop has been put together based on previous material and courses prepared by Dr Stephen Bent (University of Queensland), with generous technical support & advice provided by Dr Nathan Watson-Haigh and Dr Dan Kortschak.

Sections of the workshop are also based loosely on material and courses prepared
by Software Carpentary (http://software-carpentry.org/lessons/) and Ryans Tutorials
(http://ryanstutorials.net/)

We hope it will be useful in enabling you to continue and to advance your research.

## Course Summary

The workshop will be run in 2 hour sessions, weekly for four weeks starting from the 4th of April as follows;

| Date | Time | location | Room | Details |
| ---------- | ---------- | ---------- | ---------- | ---------- |
| Wed, April 4th | 2-4pm | Nth Terrace | TBA | Intro to Bash |
| Wed, April 11th | 2-4pm | Nth Terrace | TBA | Bash Filters |
| Wed, April 18th | 2-4pm | Nth Terrace | TBA | Sed, Awk and Grep |
| Wed, April 25th | 2-4pm | Nth Terrace | TBA | Bash Scripting |

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

## Course Aims
The long-term goal of this workshop is to enable you to carry out your own
data analysis using standard bioinformatics tools or write your own programs. This course serves as a prerequisite for the Introduction to Next Generation Sequencing (NGS) Data workshop coming later in the year.

We expect the participants, by the end of both workshops, to:
1. Be relatively comfortable working with the command line interface, and familiar with the standard Unix command line tools
2. Understand standard NGS data formats
3. Be able to run several basic NGS data analysis methods (e.g. variant calling)
4. Understand how some simple python scripts work for custom data analysis

## Workshop Instructors

Your helpers will be

- Steve Pederson, Hien To, Alastair Ludington (Bioinformatics Hub)
- Jimmy Breen (Robinson Research Institute & Bioinformatics Hub)
- Paul Wang, John Toubia (Centre for Cancer Biology)

## Recommendations For Working

In the following pages, we strongly encourage you to manually type all commands.
The mistakes you will inevitably make will actually be important learning steps.
Additionally, in your work beyond today, you will probably not have any instructions to follow.
The experience of typing these commands will equip you for future work far better than if you simply copy & paste.

For today’s session, you will also be provided with red post-it notes.
**Please use these to signal whether you need help or not by placing them on your monitors**.
These are easy for instructors to spot so we can make our way over, although do be aware that there will be times when all instructors are busy.
This will be important as we all set our computers up in the following section.

# Do we need a section here on feedback?

## Computer Setup
# Should we require that attendees do this before the session and ask us via Slack if they need assistance? If so we need to ammend this section.

Previously we have run these sessions using Virtual Machines, but today we are opting for running all sessions using the tools locally installed on your own machines.
*As there will be a huge variety of laptops in the room, the initial set-up may be a difficult to begin with*, but will settle as the day progresses.
Although this will present numerous challenges from our perspective, we feel this is a better approach as everyone will leave knowing exactly how to get the ball rolling on their own familiar systems.

Instructions for how to set-up everything on your own machine are at the following pages.
Please follow these very carefully, and call an instructor over if you have trouble.
In particular, the Windows set-ups may take a while to get sorted.

- [Mac/OSX](../install/osxInstall)
- [Windows](../install/windowsInstall)

If you are already running Ubuntu, your computer will already be set-up correctly.

[Home](../)
