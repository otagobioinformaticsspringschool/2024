# Introduction to Unix commandline (BASH)
Learning objectives:

- Use the shell to navigate directories
- Perform operations on files in directories outside your working directory
- Interconvert between relative and absolute paths
- View, search within, copy, move, and rename files. Create new directories.
- Construct command pipelines with two or more stages.
- Use for loops to run the same command for several input files.

These introductory notes are drawn from [The Carpentries](https://carpentries.org) **[Introduction to the Command Line for Genomics](https://datacarpentry.org/shell-genomics/)** lessons

---
### Introducing the Shell

A shell is a computer program that presents a command line interface which allows you to control your computer using commands entered with a keyboard instead of controlling graphical user interfaces (GUIs) with a mouse/keyboard combination.

Many bioinformatics tools can only be used through a command line interface, or have extra capabilities in the command line version that are not available in the GUI. This is true, for example, of BLAST, which offers many advanced functions only accessible to users who know how to use a shell.

- The shell makes your work less boring. In bioinformatics you often need to do the same set of tasks with a large number of files. Learning the shell will allow you to automate those repetitive tasks and leave you free to do more exciting things.
- The shell makes your work less error-prone. When humans do the same thing a hundred different times (or even ten times), they’re likely to make a mistake. Your computer can do the same thing a thousand times with no mistakes.
- The shell makes your work more reproducible. When you carry out your work in the command-line (rather than a GUI), your computer keeps a record of every step that you’ve carried out, which you can use to re-do your work when you need to. It also gives you a way to communicate unambiguously what you’ve done, so that others can check your work or apply your process to new data.
- Many bioinformatic tasks require large amounts of computing power and can’t realistically be run on your own machine. These tasks are best performed using remote computers or cloud computing, which can only be accessed through a shell.

In this lesson you will learn how to use the command line interface to move around in your file system.

Open up a terminal

~~~
$
~~~

The dollar sign is a **prompt**, which shows us that the shell is waiting for input;
your shell may use a different character as a prompt and may add information before
the prompt. When typing commands, either from these lessons or from other sources,
do not type the prompt, only the commands that follow it.

Let's find out where we are by running a command called `pwd`
(which stands for "print working directory").
At any moment, our **current working directory**
is our current default directory,
i.e.,
the directory that the computer assumes we want to run commands in,
unless we explicitly specify something else.
Here,
the computer's response is `/home/yourname`,
which is the top level directory within our cloud system:

~~~
$ pwd
/home/yourname
~~~


Let's look at how our file system is organized. We can see what files and subdirectories are in this directory by running `ls`,
which stands for "listing":

~~~
$ ls
R  r_data  shell_data
~~~


`ls` prints the names of the files and directories in the current directory in
alphabetical order,
arranged neatly into columns. 
We'll be working within the `shell_data` subdirectory, and creating new subdirectories, throughout this workshop.  

---
### Navigating Files and Directories

The command to change locations in our file system is `cd`, followed by a
directory name to change our working directory.
`cd` stands for "change directory".

Let's say we want to navigate to the `shell_data` directory we saw above.  We can
use the following command to get there:

~~~
$ cd shell_data
~~~

Let's look at what is in this directory:

We can make the `ls` output from above, more comprehensible by using the **flag** `-F`,
which tells `ls` to add a trailing `/` to the names of directories:

~~~
$ ls -F
sra_metadata/  untrimmed_fastq/
~~~

Anything with a "/" after it is a directory. Things with a "*" after them are programs. If
there are no decorations, it's a file.

We have a special command to tell the computer to move us back or up one directory level. 

~~~
$ cd ..
~~~

### Full vs. Relative Paths

The `cd` command takes an argument which is a directory
name. Directories can be specified using either a *relative* path or a
full *absolute* path. The directories on the computer are arranged into a
hierarchy. The full path tells you where a directory is in that
hierarchy. Navigate to the home directory, then enter the `pwd`
command.

~~~
$ cd  
$ pwd  
~~~

You will see: 

~~~
/home/yourname
~~~

This is the full name of your home directory. This tells you that you
are in a directory called `dcuser`, which sits inside a directory called
`home` which sits inside the very top directory in the hierarchy. The
very top of the hierarchy is a directory called `/` which is usually
referred to as the *root directory*. So, to summarize: `dcuser` is a
directory in `home` which is a directory in `/`. More on `root` and
`home` in the next section.

Now enter the following command:

~~~
$ cd /home/yourname/shell_data/.hidden
~~~

This jumps forward multiple levels to the `.hidden` directory. 
Now go back to the home directory. 

~~~
$ cd
~~~

You can also navigate to the `.hidden` directory using:

~~~
$ cd shell_data/.hidden
~~~

These two commands have the same effect, they both take us to the `.hidden` directory.
The first uses the absolute path, giving the full address from the home directory. The
second uses a relative path, giving only the address from the working directory. A full
path always starts with a `/`. A relative path does not.

A relative path is like getting directions from someone on the street. They tell you to
"go right at the stop sign, and then turn left on Main Street". That works great if
you're standing there together, but not so well if you're trying to tell someone how to
get there from another country. A full path is like GPS coordinates. It tells you exactly
where something is no matter where you are right now.

