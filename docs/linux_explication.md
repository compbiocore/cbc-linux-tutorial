# Linux Explication


This document constitutes a more elaborate discussion of the principles described in our Linux presentation, presented in a structure more resembling a narrative.  The code within can be copied into a terminal and directly executed (though it is currently undergoing testing and may still have bugs).

*If this page is missing any basic information that you would find helpful, please contact us at cbc-help@brown.edu - when doing so, please specify that your message is regarding the contents of this tutorial*

### Part I: Introduction to Linux


The original way to interact with a computer, dating back to the 1960s - so why use it in this day and age?

The command line offers greater flexibility compared to a graphical user interface (GUI) - many more options can be implented in a command line environment than would be feasible in the limited space of a GUI.  Furthermore, command line utilities are easy to automate using scripts, allowing for more efficient data analysis and saving you from annoying busy work.

At first, the bare-bones ‘retro’ appearance of the CLI is intimidating, especially since you need to memorize a few commands before it becomes usable - eventually, though, it is faster to use than a GUI for many tasks.  For example, you can switch between directories by typing their names rather than scrolling through a folder and searching for the icon you need, or move many files at once without the awkwardness of physically selecting them.

To understand how to use the CLI, it is important to learn a few simple commands that will let you navigate the system - memorizing these will make life much easier and more efficient.  Specifically, the commands worth learning immediately from the outset are those that involve navigating through Linux directories (i.e folders).

*Directories and Navigation*

These commands will be illustrated with examples from Brown’s compute cluster.  For those who are not affiliated with Brown, note that the system in question runs CentOS 6.7.

The first navigation command to remember is::

	pwd

	[aleith@login002 ~]$ pwd
	/users/aleith

It stands for ‘print working directory’, and does exactly that - displays the file path of the directory (folder) you’re currently inside, otherwise known as the 'working directory'.

The next command is::

	ls

	[aleith@login002 ~]$ ls
	4-1                      Figure2c.eps               problem set 1
	5b                       finalgenerate              profile_backup
	anaconda                 helloworld.Rout            qiime2-2018.2-py35-linux-conda.yml


It stands for ‘list’, and also does what it sounds like: lists the contents of the current working directory.

When using ‘ls’, you will note that different colors are used to depict the contents of your working directory.  These vary by the operating system you use; the following information corresponds to a default OSX terminal accessing Brown’s compute cluster (the most common system configuration among our affiliates).  The most commonly-used colors are described below:

Dark blue: a directory / folder

Light blue: a symbolic link (analogous to a shortcut on Windows)

Black: a miscellaneous file

Other colors have niche uses that you are unlikely to encounter at this stage

**Changing Command Behavior**

Many Linux commands have options that change their behavior, and ‘ls’ is no exception.  These options are set using a minus (-), directly followed by the letter associated with that option.

For example, by default, ls only shows non-hidden files.  To show all files, you would type::

	ls -a

	[aleith@login002 ~]$ ls -a
	.                        .fonts.conf                profile_backup
	..                       .gconf                     .pulse
	4-1                      .gconfd                    .pulse-cookie

Below, you can see the radically different output of ls -a relative to ls, specifically the inclusion of the great number of hidden files that exist on any Linux system.  Note that on Linux (and OSX), hidden files and directories always begin with a ‘.’ - all files that begin with a '.' are hidden, and all hidden files begin with a '.'.

The third fundamental navigation command is::

	cd

	[aleith@login002 ~]$ cd anaconda
	[aleith@login002 anaconda]$ pwd
	/users/aleith/anaconda

This command stands for ‘change directory’ and is what allows you to actually move around among the various folders on your system.

**Tab Completion**

Some directories can have long, complicated names, making it difficult to type them out.  Fortunately, the CLI allows so-called ‘tab completion’ - when typing part of a directory’s path (or a file’s path), pressing ‘tab’ will cause the rest of the name to be automatically filled in.

If more than one item matches what you’ve typed, it will autocomplete up to the point of ambiguity and then you will need to type more and type ‘tab’ again.  For example, if you have two directories entitled ``directory1`` and ``directory2`` and type ``dir`` and then press 'tab', Linux will fill in ``ectory`` and then prompt you to complete the directory name, making a noise to inform you that further typing must be done.  Pressing 'tab' several times in rapid succession will display all directories that match what has been filled in so far (sounding the alert noise each time).

**Basic File Structure Overview**

There are two fundamental ways to refer to a file or directory on Linux: a relative file path or an absolute file path.

A relative file path is the location of a file with respect to your current working directory (and will therefore change based on where your working directory is).

An absolute file path, sometimes described as a ‘full file path’, is that file’s true location on the computer and is invariant.

There are several pieces of shorthand notation that are often used in defining a relative file path

\. stands for the current working directory (i.e. the output of pwd)

\.\. stands for the parent directory, i.e. the directory that contains the current working directory

\~ (‘tilde’) stands for the current user’s (i.e. your) home directory, the location of which is dependent on the system configuration

The absolute file path of a file is its location with respect to the 'root directory'; the root directory is the highest level directory on the computer, and is denoted by the absolute path ‘/’.  File paths beginning with ‘/’ are, by definition, absolute paths.

The following two paths are very different:

anaconda/bin/ is the ‘bin’ directory inside the ‘anaconda’ directory inside the *current* directory

/anaconda/bin/ is the ‘bin’ directory inside the ‘anaconda’ directory inside the *root* directory

The only time these two paths will point to the same place is when the root directory is the current working directory.  As a result, to avoid ambiguity, it is generally a good idea to stick to absolute file paths whenever possible - otherwise, it's possible to misplace a file or direct a script to look in the wrong place for data.

**File Manipulation**

We will now cover the basic tenets of file manipulation using the CLI.  First, however, we must briefly address a technical topic that is likely unfamiliar: file permissions.

If you’ve ever had to type your administrator password on Windows or OSX to perform an operation, you’ve encountered file permissions before...these operating systems just make the process more opaque, increasing ease of use at the cost of flexibility as alluded to earlier.

*File Permissions*

There are three fundamental permissions: read (r), write (w), and execute (x).

There are also three *sets* of permissions: those belonging to the user (u), those belonging to the user’s group (g), and those belonging to everyone else (o) on the system.

To view a file’s permissions on Linux, you can type::

	ls -lh

	[aleith@login002 linux_tutorial]$ ls -lh
	total 0
        -rw-r--r-- 1 aleith cbc    0 Mar 19 18:43 randomfile.txt

``-rw-r--r--`` means ‘user has read and write permission’, ‘user’s group has read permission’, and ‘everyone else has read permission’.

``aleith	cbc`` means this file is owned by user ‘aleith’ from group ‘cbc’.

``0`` refers to the file’s rounded file size.

The last-modified date is also included for each file.

A file’s permissions can be changed by the owner using the command::

	chmod [permission to change] [target]

For example, user ‘aleith’ can give himself execute permission (‘u+x’ - add execute permission to user) on ‘randomfile.txt’ using the following command::

	[aleith@login002 linux_tutorial]$ chmod u+x randomfile.txt 
	[aleith@login002 linux_tutorial]$ ls -lh
	total 0
        -rwxr--r-- 1 aleith cbc    0 Mar 19 18:43 randomfile.txt

Note that a file for which you have execute permissions will be colored green within the standard OSX terminal / CentOS 6.7 environment.

Execute permission is largely irrelevant for a text file, but it is vitally important for any script that is to be run; newly-created files often do not start with execute permission even when you create them yourself.  To run a script by typing its name into the terminal, you must first grand that script execute permission, which is a safeguard to protect against the accidental execution of malicious code.

The command::

	chown [new owner] [file]

will transfer ownership of a file to the user specified as ‘new owner’.

The command::

	chgrp [new group] [file]

will transfer the file to another group without changing the user.  As users can each be members of multiple groups, this command is often used to transfer a file from one of your own groups to another.

*Interacting with Files*

To create a new empty directory from the command line, simply type::

	mkdir [directory name]

This command, naturally, means ‘make directory’.::

	[aleith@login002 linux_tutorial]$ mkdir directory1
	[aleith@login002 linux_tutorial]$ ls
	directory1  randomfile.txt

Directories created in this manner cannot contain spaces, as the CLI will interpret any space as the end of the directory’s name.  Likewise, attempting to navigate to a directory with spaces will cause problems, as the navigation commands discussed earlier will interpret spaces in the same manner (to get around this limitation in a pinch, you can force Linux to interpret a space as part of a directory name by prepending a '\' to that space in a process called "[escaping](https://en.wikipedia.org/wiki/Escape_character)" the space; these names should never be used in the first place, though, and if present should immediately be changed to spaceless names using ``mv`` discussed below).

To move a file, one can use the following command::

	mv [target] [destination]

The ‘move’ command moves ‘target’ to the directory specified as ‘destination’::

	[aleith@login002 linux_tutorial]$ mv randomfile.txt ~/linux_tutorial/directory1/
	[aleith@login002 linux_tutorial]$ ls
	directory1
	[aleith@login002 linux_tutorial]$ ls ~/linux_tutorial/directory1/
	randomfile.txt

``mv`` is also the command used to rename files in Linux - if the target and destination are both in the same directory, Linux will interpret that command as a rename request i.e. ``mv old_name new_name``.

Files can be copied by typing::

	cp [original filename] [copy filename]

	[aleith@login002 linux_tutorial]$ cd directory1/
	[aleith@login002 directory1]$ cp randomfile.txt copiedfile.txt
	[aleith@login002 directory1]$ ls
	copiedfile.txt  randomfile.txt

The ‘copy’ command simply duplicates a file in its entirety.

*Wildcard*

The * character is known as the ‘wildcard’, and it is used to mean ‘any number of any characters’.

For example,::

	[aleith@login002 directory1]$ touch script.sh  #create an empty script to serve as a contrast
	[aleith@login002 directory1]$ ls
	copiedfile.txt  script.sh   randomfile.txt
	[aleith@login002 directory1]$ ls *.txt
	copiedfile.txt  randomfile.txt

will run ‘ls’ on every file that ends in ‘.txt’, thus listing every plain textfile.  To illustrate this effect, a new file called ‘script.sh’ has been added to our directory using ``touch``, which just generates an empty file with a designated name, in this case 'script.sh'.  Note that ``ls *.txt`` does not print out this new file because it does not contain the characters '.txt' anywhere.

Next, we show two more examples of how wildcards are interpreted.

First, we list any files beginning with ‘random’ and ending with anything::

	ls random*

	[aleith@login002 directory1]$ ls random*
	randomfile.txt

Then, we list any files that start with anything, have a ‘d’ in the middle, and then end with anything::

	ls *d*
	[aleith@login002 directory1]$ ls *d*
	copiedfile.txt  randomfile.txt

**Viewing Files**

To view the contents of a file, the ``less`` command can be used - it will display the first few lines of the file and allow you to scroll through it.  Pressing ‘q’ will close this browser.  ``less`` is so named because it is a more modern, scrollable version of the old UNIX command ``more`` (which is still available but obsolete)::

	less [file]

The ``cat`` command will print out the entirely of a file::

	cat [file]

The ``echo`` command prints text to the screen::

	echo [text]

e.g.::

	echo "hello world"

**Redirecting Output**

By default, many tools (e.g. ``echo``) will print their output to the terminal.  Naturally, the CLI has operators that can change this behavior by redirecting that output to various places depending on the user's needs.

The::

	[command] > [file]

operator redirects output to ‘file’, overwriting it if it already exists and creating it anew if not.

The related::

	[command] >> [file]

operator appends output to ‘file’.

Here is an example that compares these two operators::

	[aleith@login002 directory1]$ echo "hello world"
	hello world
	[aleith@login002 directory1]$ echo "hello world, redirected" > hello.txt
	[aleith@login002 directory1]$ ls
	copiedfile.txt script.sh
	hello.txt      randomfile.txt
	[aleith@login002 directory1]$ cat hello.txt
	hello world, redirected
	[aleith@login002 directory1]$ echo "hello world, appended" >> hello.txt
	[aleith@login002 directory1]$ cat hello.txt
	hello world, redirected
	hello world, appended


**Downloading Files**

Linux has two primary commands for downloading files.  The simplest and most user-friendly is::

	wget [url]

It downloads a file as-is, with the same filename, and places it in your current working directory.  The more flexibile alternative is::

	curl [url] > [filepath]

Without the ‘> [filepath]’ portion of the command, the contents of the file at 'url' will be printed to the terminal and not saved, a small portion of which is reproduced (the full output being hundreds of lines of HTML)::

	[aleith@login002 directory1]$ curl www.google.com
	<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="en"><head>
	<meta content="Search the world's information, including webpages,

Juducious use of ``>`` allows you to download the page to a file instead, and even invokes a progress meter::

	[aleith@login002 directory1]$ curl www.google.com > google.html
  	% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
	100 11371    0 11371    0     0   187k      0 --:--:-- --:--:-- --:--:--  236k
	[aleith@login002 directory1]$ ls
	copiedfile.txt  script.sh    google.html
	hello.txt      randomfile.txt

**Parsing Files**

For the next few examples, we will use a (highly abridged) dictionary of English words::

	[aleith@login002 directory1]$ mkdir grep_examples
	[aleith@login002 directory1]$ cd grep_examples/
	[aleith@login002 grep_examples]$ curl http://www-personal.umich.edu/~jlawler/wordlist > dictionary.txt
 	 % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
	100  718k  100  718k    0     0  1117k      0 --:--:-- --:--:-- --:--:-- 1240k

The wc -l command uses the ‘word count’ function to count the number of lines this file comprises (69905)::

	[aleith@login002 grep_examples]$ wc -l dictionary.txt 
	69905 dictionary.txt

The grep command allows us to search through a file for any entries that match a specified pattern::

	grep [pattern] [filename]

For example, we can search our dictionary for all words containing the letter sequence ‘aar’.  Note that by default, the pattern supplied to ``grep`` functions as though it has a wildcard on either side of the provided text::

	[aleith@login002 grep_examples]$ grep "aar" dictionary.txt 
	aardvark
	aardwolf
	bazaar
	tumultuaary
	zaar

The previous example included a fairly specific pattern, ‘aar’, intentionally selected so all matches could fit on screen - but what if that weren’t the case?  How can we count the number of matches if there are hundreds or thousands of them?

As we've already seen, the CLI has operators for redirecting the output of commands to files.  As it turns out, it also has an operator that allows us to send the output of one function to *another function* ('>' redirects to files, not other functions)::

	|

This vertical line operator is pronounced ‘pipe’.

If we want to know how many words in our dictionary contain the letter ‘a’ anywhere, we can combine ``grep`` and ``wc -l`` using a pipe, which would be formally stated as "piping the output of ``grep`` to ``wc -l``::

	[aleith@login002 grep_examples]$ grep "a" dictionary.txt | wc -l
	40387

If we tried to do so using the wrong operator, we would end up with weird behavior and a file named ‘wc’, since '>' treats its antecedent as a file name specification::

	[aleith@login002 grep_examples]$ grep "a" dictionary.txt > wc -l
	[aleith@login002 grep_examples]$ ls
	dictionary.txt  wc

**Deleting Files**

This ‘wc’ file is not useful, merely containing “dictionary.txt” as text::

	[aleith@login002 grep_examples]$ cat wc
	dictionary.txt

To delete a file, we use the following command::

	rm [filename]

**!!!!!The ‘remove’ command permanently deletes a file.  Linux has no ‘Trash’ feature like Windows and OSX do - ``rm`` is in fact directly analogous to the ‘Empty Trash’ option on those systems!!!!!**

**Miscellanea**

Sometimes, it will be necessary to stop a command before it completes (e.g. upon making a typo with a wildcard and copying thousands of large files instead of a few).  To interrupt a command, type CTRL+c (this combination is valid on both Windows and OSX - do not substitute COMMAND for CTRL on OSX or it will not work).

One of the most important resources for understanding Linux commands is the manual associated with each command.  On most versions of Linux, you can access the manual for a function by typing::

	[function] --help

e.g.::

	cp --help

to bring up the manual for 'copy'.

Note that this syntax will not necessarily hold for other UNIX-like operating systems such as OSX; when in doubt, Googling the name of the command and the operating system in use will reveal a web-based copy of that version's manual.  Specifying the operating system is important since most OSes based on UNIX have a great many commands that share names but exhibit subtle differences in function or syntax.

*Errors*

The most common error encountered on a Linux system - hands down - is ``command not found``.  This error means that the computer does not know how to process what you’ve typed - it could be due to a simple typo, or there could be a bigger issue.

A slapdash attempt to look at the cp manual will look like this, invoking the nonexistant command ``cp``::

	[aleith@login002 grep_examples]$ cpr --help
	bash: cpr: command not found

Oftentimes, however, you have indeed typed a valid command but Linux still says ‘command not found’ - how is that possible?::

	[aleith@login002 grep_examples]$ samtools --help
	bash: samtools: command not found

It all comes down to how a command line-based system runs software.  Unlike a GUI-based system where every program icon is linked directly to the software, Linux works based on a list of locations to look for program files (‘binaries’) - this list is called the ‘PATH’.

To view your PATH, you can type::

	echo $PATH

	[aleith@login002 grep_examples]$ echo $PATH
	/gpfs/runtime/opt/matlab/R2014a/bin:/gpfs/runtime/opt/perl/5.18.2/bin:/gpfs/runtime/opt/python/2.7.3/bin:/gpfs/runtime/opt/java/7u5/bin:/gpfs/runtime/opt/intel/2013.1.106/bin:/gpfs/runtime/opt/centos-updates/6.3/bin:/usr/lib64/qt-3.3/bin:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin:/opt/ibutils/bin:/gpfs/runtime/bin:/users/aleith/bin

Here, the ‘$’ basically means ‘bash variable’ - PATH is a globally-defined variable on every Linux system.

Any 'binary' i.e. program that is somewhere other than one of the directories listed inside of PATH must be called by its full file path, and on some systems, that is not possible due to permissions and access control.  Thus, it is important to be sure that your PATH contains the necessary directories.

The contents of $PATH can be modified by editing a certain hidden file in your home directory; however, doing so incorrectly will cause severe problems for your account and make it impossible to run even the basic commands like ``cd`` so we will not cover that topic today.  Please contact our office using the contact information on this site's homepage to set up a meeting to discuss the process.

Brown’s compute cluster has functionality that will temporarily modify your path in response to something called a ``module load`` command, which we will cover in the second half of this talk.  Please use that functionality rather than trying to permanently edit your PATH whenever possible.

Another common error is ``Permission denied``, which occurs when you lack the permissions to perform a given operation.  This error usually comes about when you have downloaded a file and it’s been created with weird permissions.  To resolve this issue, simply add the requisite permission for the operation you are attempting.

We will simulate an example of this error below::

	[aleith@login002 grep_examples]$ cd ~/linux_tutorial/directory1/
	[aleith@login002 directory1]$ chmod 000 randomfile.txt 
	[aleith@login002 directory1]$ ls -lh randomfile.txt 
	---------- 1 aleith cbc 0 Mar 19 18:43 randomfile.txt
	[aleith@login002 directory1]$ cat randomfile.txt 
	cat: randomfile.txt: Permission denied
	[aleith@login002 directory1]$ chmod u+r randomfile.txt 
	[aleith@login002 directory1]$ cat randomfile.txt

Linux has an uncountable number of developers publishing software, and most of these packages have their own unique errors.  It is therefore highly likely that you will encounter unintelligible errors in the course of your work, especially if you are using software written in older compiled languages like Java or C.

When encountering such an error, it is advisable to Google the entirety of the error message except for the filename and file path of your data; appending the name of your operating system is also important in finding an applicable solution.  Numerous forums, mailing lists, and blogs discuss how to respond to (almost) any error you might encounter, but these are often specific to a certain OS, so finding the correct one requires care in the specification of your search terms.

### Part II: Brown's Compute Cluster

Brown has a compute cluster operated by CCV, the Center for Computation and Visualization.  CCV offers services to researchers affiliated with Brown.  Chief among their resources is ‘Oscar’, the compute cluster itself.

A compute cluster is a network of computers (‘nodes’), a subset of which can be temporarily allocated to a single job as a group.  These nodes become, for all intents and purposes, a virtual ‘supercomputer’, pooling resources for a designated task.  Users can specify their computational needs, which determines how many nodes are allocated for that task.  Once the task is finished, the nodes are released back into the pool and can be used by other users - that way, resource aren't sitting idle and being wasted.

To connect to Oscar, you must first [contact CCV to obtain an account](https://web1.ccv.brown.edu/start/account).  Once you have one, run the command::

	ssh [user]@ssh.ccv.brown.edu

The ‘secure shell’ command creates a sort of virtual link between your terminal and a terminal on the remote machine, meaning your terminal can interact with the remote machine as though it belonged to that machine.  Once you type the above login command, you will be prompted for your password.

If you plan to use Oscar to print any graphics to the screen, you must use a slightly different version of the 'secure shell' command::

	ssh -Y [user]@ssh.ccv.brown.edu

The ‘-Y’ option will enable graphics forwarding.  That setting is necessary to use any application with a GUI e.g. Firefox, RStudio, or the typical Matlab environment.  Without specifying ‘-Y’, attempts to display graphics will cause errors::

	[aleith@login002 directory1]$ firefox
	Error: no display specified

If left idle, ssh connections will time out.  The time out interval is based on the system.  Attempting to type into a terminal connected to a timed-out session will yield ~10 seconds of lag, and then an error message::

	packet_write_wait: Connection to [ip] port 22: Broken pipe

Batch jobs running in the background will not be affected by this disconnection, though any interactive jobs running in the foreground (e.g. copying files with ``cp``) will be terminated partway though.  This behavior is especially problematic if doing something like downloading a file or moving it with ``mv``, as the resulting file(s) will be corrupted.

CCV also provides a 'VNC client' to connect to Oscar:

https://web1.ccv.brown.edu/technologies/vnc

The VNC client resembles a typical remote desktop application and can be used for graphically-focused applications with no special options.  When logging into the VNC, you specify parameters and are stuck with them until you end that job, so be sure to allocate the maximum amount you think you will need from the outset.

**Transferring Files**

CCV hosts a Samba tutorial that is quite long and can be found here:

https://web1.ccv.brown.edu/doc/cifs

File transfers will be interrupted if the internet connection on either end goes down.

**Batch Jobs**

The question then becomes: how does one actually use a compute cluster to run anything beyond thbe basic commands we have seen thus far?

To accomplish this task, the user must create a special type of script called a ‘batch script’, used for running so-called ‘batch jobs’.  The batch script is submitted to the ‘resource manager’, a utility that allocates nodes to users.

Oscar uses a resource manager called ‘Slurm’.  When a batch script is submitted to Slurm, it will put it in a queue based on the computational requirements requested and the user’s job priority.  The different priority levels and their attributes can be viewed here:

https://web1.ccv.brown.edu/start/account

and are too detailed to reproduce here.

Once a requested job gets to the front of Slurm’s queue, it is initialized.  This job is run in ‘batch mode’, and happens silently in the background, with no output printed to the terminal.  Instead, the batch script is used to specify where the output that would ordinarily appear on the terminal is written to.  This output is provisioned into two files, ‘standard out’ and ‘standard error’ (though these names are not entirely indicative - error messages sometimes go to ‘standard out’ depending on how the software is written).

*Formatting*

All batch scripts require a header that follows a specified format - otherwise, Slurm will be unable to allocate resources to it::

	#!/bin/bash						The typical shebang statement
	#SBATCH --time=144:00:00		The time to request resources for
	#SBATCH --cpus-per-task=8		The CPUs to request
	#SBATCH  --mem=64G				The memory to request
	#SBATCH -J exac					The name under which to list the job
	#SBATCH -o exac.out				The file for ‘standard out’
	#SBATCH -e exac.err				The file for ‘standard error’

Please note that the ``#`` symbols are part of the format and are not an example of commands being 'commented out'.  They must be included for the script to be processed.  Other, optional parameters can be found at the following address:

https://web1.ccv.brown.edu/doc/jobs

To submit a batch script with the aforementioned header, type::

	sbatch [script name]

You will receive a message stating that your job has been submitted.  To monitor its status, type::

	myq

The ‘my queue’ command shows any pending or running jobs associated with your own user account.  It is useful to check if a job has failed - if you submit an alignment you expect to take days, and after 30 seconds no job is running, it broke.

*Modules*

The method for using software is, on the surface, different from that found on a local computer.  To use a given tool, you must first ‘load’ that tool as a ‘module’ as follows::

	module load [software name]

e.g. ``module load samtools``

In truth, though, this command just temporarily prepends that software’s location to the beginning of your PATH (try viewing your PATH before and after loading!)

To see a list of all software available on Oscar, type::

	module avail

To see all of the modules currently loaded, type::

	module list

To unload a loaded module, type::

	module unload [software name]

The terminal you see when first sshing into Oscar is running on a ‘login node’, a shared machine with low capabilities.  Computationally-intensive jobs run on the login node will immediately die.  It is possible, however, to access a node that works like a fully-capable terminal.

To access such a node, type::

	interact

An example of an interact command is::

	interact -n 20 -t 01:00:00 -m 10g

``-n`` refers to the number of cores (20)
``-t`` refers to the time the session will last (1 hour)
``-m`` refers to the memory per node (10 GB)

The full list of options can be found at the following link:

https://web1.ccv.brown.edu/doc/jobs
