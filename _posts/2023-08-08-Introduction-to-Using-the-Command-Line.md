---
title: Introduction to Using the Command Line
author: doctorfree
date: 2023-08-08 16:20:00 +0800
tags: [intro, command, command line, tutorial, history]
pin: true
img_path: "/posts/20230808"
---

## Table of Contents
1. [What is the Command Line](#what-is-the-command-line)
1. [Why use the Command Line](#why-use-the-command-line)
    1. [Resize images example](#resize-images-example)
    1. [MagicMirror timed menu display example](#magicmirror-timed-menu-display-example)
1. [History](#history)
1. [Opening the Command Line](#opening-the-command-line)
    1. [Mac OS](#mac-os)
    1. [Linux](#linux)
    1. [Microsoft Windows](#microsoft-windows)
1. [The Command Line Prompt](#the-command-line-prompt)
1. [Getting started with basic commands](#getting-started-with-basic-commands)
    1. [Moving files](#moving-files)
    1. [Copying files and folders](#copying-files-and-folders)
    1. [Wildcards](#wildcards)
    1. [Deleting files and folders](#deleting-files-and-folders)
    1. [Listing files](#listing-files)
    1. [Changing the current working directory](#changing-the-current-working-directory)
    1. [Making a new folder](#making-a-new-folder)
    1. [Getting help for a command](#getting-help-for-a-command)
    1. [Common Commands](#common-commands)
1. [Running scripts](#running-scripts)
1. [Accessing another system with the Command Line](#accessing-another-system-with-the-command-line)
1. [Permissions and ownership](#permissions-and-ownership)

## What is the Command Line

The command line or command-line interface, is a text-based application for
viewing, handling, and manipulating files on your computer. Other names for
the command line are: cmd, CLI, prompt, shell prompt, console, or terminal.

A command line interface (CLI) processes commands to a computer program in the
form of lines of text. The program which handles the interface is called a command
line interpreter or command line processor. Operating systems implement a command
line interface in a shell for interactive access to operating system functions or
services. Such access was primarily provided to users by computer terminals starting
in the mid-1960s, and continued to be used throughout the 1970s and 1980s on VAX/VMS,
Unix systems and personal computer systems including DOS, CP/M and Apple DOS.

Today, many users rely upon graphical user interfaces and menu-driven interactions.
However, some programming and maintenance tasks may not have a graphical user
interface and may still use a command line.

Alternatives to the command line interface include text-based user interface menus
(for example, IBM AIX SMIT), keyboard shortcuts, and various desktop metaphors
centered on the pointer (usually controlled with a mouse). Examples of this include
the Microsoft Windows, DOS Shell, and Mouse Systems PowerPanel. Command line
interfaces are often implemented in terminal devices that are also capable of
screen-oriented text-based user interfaces that use cursor addressing to place
symbols on a display screen.

Programs with command line interfaces are generally easier to automate via scripting.

Many software systems implement command line interfaces for control and operation.
This includes programming environments and utility programs.

## Why use the Command Line
As noted above, the command line interface can be used to easily automate computer
actions. This is the primary motivation for command line usage. In addition,
there are many tasks that can only be performed at the command line. That is,
a graphical user interface may not yet exist for some actions you wish to perform.
The graphical application you are using may be sufficient for most use cases
and satisfy the requirements for most users but all functionality behind the
application's normal use may not be exposed in the graphical user interface.

### Resize images example
For example, say you need to resize all the images in a folder. This can be
accomplished in a graphical user interface through a series of tedious mouse
clicks and text entries into forms followed by more mouse clicks and confirmation
at prompts. Each image file will require multiple (many many) mouse clicks and/or
keyboard entries. For a large collection of images this might take hours or even
days to perform in a graphical user interface. The poor worker whose job it is
to resize several thousand images will undoubtedly tire, make mistakes, and
possibly develop carpal tunnel syndrome. It's beyond tedious, it's error prone
and dangerous!

A command line approach to automate resizing of all images in a folder can
drastically reduce the time required, risk of error, and potential harm to
humans performing the task. In this example, assume there are thousands of
images files in a directory named 'images'. These image files all need to be
resized to 800x600 for display on the company's website. To further complicate
matters, only the image files in the JPEG format are to be resized while leaving
those in the PNG format at full size.

The task can be performed in a few minutes by writing a simple shell script which
invokes the ImageMagick command line utility 'convert':

```bash
#!/bin/bash
#
# Simple script to resize all JPEG image files in a folder to 800x600

inst_convert=`type -p convert`
[ "${inst_convert}" ] || {
    echo "Could not locate the convert command in the execution PATH."
    echo "Install ImageMagick and execute this script again."
    echo "Exiting."
    exit 1
}

cd /path/to/images/folder/parent
[ -d resized_images ] || mkdir resized_images
cd images
count=0
for image in *.jpg
do
    convert ${image}  -resize 800x600\! ../resized_images/${image}
    ((count++))
done

echo "Successfully resized ${count} JPEG images in ${IMGDIR}"
```

### MagicMirror timed menu display example
There are many many uses of the command line in automation. Another example,
specific to the MirrorCommand project, might be display of a menu on a
restaurant bathroom mirror that has been setup as a MagicMirror. The restaurant
owner wants the bathroom mirror to display the breakfast menu during morning
hours, the lunch menu at midday, and the dinner menu in the evening. Before
and after business hours she just wants the mirror to be a mirror.

This can be accomplished by a worker manually configuring the MagicMirror in each
of the bathrooms but again is prone to error, delays, and can be tedious. Let's
do it in the command line utilizing the Cron facility which enables commands to
be executed at specified times of day, specified days/weeks/months/years, and so on.

After installing the MirrorCommand package and configuring MagicMirror config
files for each of the menus to be displayed, a Cron job can be created that executes
the appropriate `mirror` command at the desired time of day on the desired days
of the week. The Crontab entries might look like:

```bash
# This is a file named menucrontab.in with cron job entries
# m h  dom mon dow   command
#
# Breakfast starts at 7am on weekdays
0 7 * * 1-5 /usr/local/bin/mirror breakfast
# Lunch starts at 11am on weekdays
0 11 * * 1-5 /usr/local/bin/mirror lunch
# Dinner starts at 4pm on weekdays
0 16 * * 1-5 /usr/local/bin/mirror dinner
# Closing time is 9pm on weekdays
0 21 * * 1-5 /usr/local/bin/mirror blank
# Breakfast starts at 8am on weekends
0 8 * * 0,6 /usr/local/bin/mirror breakfast
# Lunch starts at 1pm on weekends
0 13 * * 0,6 /usr/local/bin/mirror lunch
# Dinner starts at 5pm on weekends
0 17 * * 0,6 /usr/local/bin/mirror dinner
# Closing time is 11pm on weekends
0 23 * * 0,6 /usr/local/bin/mirror blank
```
Create the above crontab file and, on each of the MagicMirror systems in
the restaurant setup the cron job by executing, as the user with MagicMirror
administration privileges, the command:

`crontab menucrontab.in`

Verify the cron jobs are setup properly with:

`crontab -l`

No 4 times daily tasks prone to error need be performed. Changing the
menu or the schedule of menu display will be a one-time task performed in a
few minutes by the MagicMirror administrator.

## History

While typewriters are the definitive ancestor of all key-based text entry devices,
the computer keyboard as a device for electromechanical data entry and communication
derives largely from the utility of two devices: teleprinters (or teletypes) and
keypunches. It was through such devices that modern computer keyboards inherited
their layouts.

As early as the 1870s, teleprinter-like devices were used to simultaneously type
and transmit stock market text data from the keyboard across telegraph lines to
stock ticker machines to be immediately copied and displayed onto ticker tape.

The teleprinter, in its more contemporary form, was developed from 1907 to 1910
by American mechanical engineer Charles Krum and his son Howard Krum|Howard, with
early contributions by electrical engineer Frank Pearne. Earlier models were
developed separately by individuals such as Royal Earl House and Frederick G. Creed.

Earlier, Herman Hollerith developed the first keypunch devices, which soon evolved
to include keys for text and number entry akin to normal typewriters by the 1930s.

The keyboard on the teleprinter played a strong role in point-to-point and
point-to-multipoint communication for most of the 20th century, while the keyboard
on the keypunch device played a strong role in data entry and storage for just as
long. The development of the earliest computers incorporated electric typewriter
keyboards: the development of the ENIAC computer incorporated a keypunch device as
both the input and paper-based output device, while the BINAC computer also made
use of an electromechanically controlled typewriter for both data entry onto
magnetic tape (instead of paper) and data output.

The keyboard remained the primary, most integrated computer peripheral well into
the era of personal computing until the introduction of the mouse as a consumer
device in 1984. By this time, text-only user interfaces with sparse graphics gave
way to Graphical user interface|comparatively graphics-rich icons on screen.

However, keyboards remain central to human-computer interaction to the present,
even as mobile personal computing devices such as smartphones and Tablet
computer|tablets adapt the keyboard as an optional virtual, touchscreen-based
means of data entry.

The command-line interface evolved from a form of dialog once conducted by humans
over teleprinter (TTY) machines, in which human operators remotely exchanged
information, usually one line of text at a time. Early computer systems often used
teleprinter machines as the means of interaction with a human operator.

The computer became one end of the human-to-human teleprinter model. So instead of
a human communicating with another human over a teleprinter, a human communicated
with a computer.

The mechanical teleprinter was replaced by a "glass tty", a keyboard and screen
emulating the teleprinter. "Smart" terminals permitted additional functions, such
as cursor movement over the entire screen, or local editing of data on the terminal
for transmission to the computer. As the microcomputer revolution replaced the
traditional – minicomputer + terminals – time sharing architecture, hardware
terminals were replaced by terminal emulators — PC software that interpreted
terminal signals sent through the PC's serial ports. These were typically used
to interface an organization's new PC's with their existing mini- or mainframe
computers, or to connect PC to PC. Some of these PCs were running Bulletin Board
System software.

Early operating system CLIs were implemented as part of resident monitor programs,
and could not easily be replaced. The first implementation of the shell as a
replaceable component was part of the Multics time-sharing operating system.
In 1964, MIT Computation Center staff member Louis Pouzin developed the RUNCOM tool
for executing command scripts while allowing argument substitution. Pouzin coined
the term "shell" to describe the technique of using commands like a programming
language, and wrote a paper about how to implement the idea in the Multics operating
system. Pouzin returned to his native France in 1965, and the first Multics shell
was developed by Glenda Schroeder.

The first Unix shell, the V6 shell, was developed by Ken Thompson in 1971 at Bell
Labs and was modeled after Schroeder's Multics shell. The Bourne shell was
introduced in 1977 as a replacement for the V6 shell. Although it is used as an
interactive command interpreter, it was also intended as a scripting language and
contains most of the features that are commonly considered to produce structured
programs. The Bourne shell led to the development of the KornShell (ksh), Almquist
shell (ash), and the popular Bourne-again shell (or Bash).

Early microcomputers themselves were based on a command-line interface such as
CP/M, DOS or AppleSoft BASIC. During the 1980s and 1990s, the introduction of the
Apple Macintosh and of Microsoft Windows on PCs saw the command line interface as
the primary user interface replaced by the Graphical User Interface. The command
line remained available as an alternative user interface, often used by system
administrators and other advanced users for system administration, computer
programming and batch processing.

In November 2006, Microsoft released version 1.0 of Windows PowerShell (formerly
codenamed Monad), which combined features of traditional Unix shells with their
proprietary object-oriented .NET Framework. MinGW and Cygwin are open-source
packages for Windows that offer a Unix-like CLI. Microsoft provides MKS Inc.'s
ksh implementation MKS Korn shell for Windows through their Services for UNIX add-on.

Since 2001, the Macintosh operating system macOS has been based on a Unix-like
operating system called Darwin. On these computers, users can access a Unix-like
command-line interface by running the terminal emulator program called Terminal,
which is found in the Utilities sub-folder of the Applications folder, or by
remotely logging into the machine using ssh. Z shell is the default shell for
macOS; Bash, tcsh, and the KornShell are also provided. Before macOS Catalina,
Bash was the default.

## Opening the Command Line

There are varying ways of accessing command line, depending on what operating
system you use.

### Mac OS
Go to Applications → Utilities → Terminal

Alternatively, open spotlight search (default way to do this is by hitting
command and the space bar) and type in “terminal”.

Select the application called terminal and press the return key.

This should open up an app with a black background.

When you see your username followed by a dollar sign, you’re ready to start
using command line.

### Linux
It's probably under

Applications → Accessories → Terminal

or

Applications → System → Terminal

You can open Terminal by directly pressing [ctrl+alt+T] or you can search for it
by clicking the “Dash” icon, typing in “terminal” in the search box, and opening
the Terminal application.

This should open up an app with a black background.

When you see your username followed by a dollar sign, you’re ready to start using
command line.

### Microsoft Windows

One of the following should open a command line window:

- Go to the Start menu or screen, and enter "Command Prompt" in the search field.
- Go to Start menu → Windows System → Command Prompt.
- Go to Start menu → All Programs → Accessories → Command Prompt.
- Go to the Start screen, hover your mouse in the lower-left corner of the screen, and click the down arrow that appears (on a touch screen, instead flick up from the bottom of the screen). The Apps page should open. Click on Command Prompt in the Windows System section.
- Hold the special Windows key on your keyboard and press the "X" key. Choose "Command Prompt" from the pop-up menu.
- Hold the Windows key and press the "R" key to get a "Run" window. Type "cmd" in the box, and click the OK key.
On Windows 10, open the start menu and go to the shortcuts folder called
“Windows System”.

Pressing the dropdown menu should reveal a shortcut to open the Command Prompt
application.

Right click on the shortcut, press “More”, and press “Run as Administrator”.

For Windows 8, go to the start screen, press “All Apps”, and scroll right until
the “Windows System” folder shows up.

You can find Command Prompt there.

For Windows 7, open the start menu and click on “All Programs”.

Click on “Accessories” and you’ll find the Command Prompt shortcut.

Right click on the shortcut and press “Run as Administrator”.

## The Command Line Prompt
After opening a terminal window as described in the previous section,
you should see a white or black window that is waiting for your commands.
The window will display a character or string called the command line prompt,
aka 'shell prompt', or 'prompt' for short. It prompts you to input something there.

If you're on a Mac or Linux, you probably see a $ as the prompt

On Windows, you probably see a &gt; as the prompt

The command line prompt is often configured to display the username, system name,
current working directory, or other useful information but will usually end with
either a $ sign, &gt; symbol, or the &hash; symbol when logged in as the root user.
For example, my prompt right now on my Mac is:

`doctorwhen@Ronnies-Mac-Pro:~/src/MirrorCommand$`

At the prompt you can type a command which will be interpreted by the shell your
login session is configured to use. I use the Bash shell. There are many shells,
each offering its own value and all with similar but slightly different syntax,
built-ins, functions, and features.

For example, to display your username, type the following command at the prompt
followed by &lt;Return&gt;

Mac OS or Linux:

`$ whoami`

Windows:

`> whoami`

The `whoami` command should return your username followed by another shell
prompt waiting for input.

## Getting started with basic commands

### Moving files

Type in the following:

`mv target destination`

Where mv is move, target is the file you want to move, and destination is where
you want to move it to.

### Copying files and folders
The cp command can be used to copy files and folders.

To copy a file execute the following command:

`cp target destination`

The target is the target file you’re trying to copy, and destination is
the destination file you’re trying to copy it to. Note, if you have sufficient
permission this command will overwrite any previously existing destination
file. Always use caution when moving and copying files and folders as the
action can be destructive of existing files and folders.

To copy a folder execute the following command:

`cp -r target destination`

The target is the target folder you’re trying to copy, and destination is
the destination folder you’re trying to copy it to. The '-r' command line
argument indicates recursive copy - the target folder and all of its contents
will be copied to the destination. Note, if the 'destination' folder already
exists then the cp command will copy the target folder into the existing
destination folder as a sub-folder in that directory, 'destination/destination'.

The terms 'directory' and 'folder' mean the same thing.

### Wildcards
Use the '&ast;' character as a wildcard to represent all.
For example, to copy all files in a folder that start with the letter 'a',
your target should be 'a&ast;':

`cp a* destination_folder`

To copy all files of a certain extension (e.g. '.png'), your target
should be '&ast;.png':

`cp *.png destination_folder`

Use the '?' character as a wildcard to represent a single character.
For example, to copy all files in a folder that have a 3 character suffix
(e.g. the file `image.jpg` but not the file `image.jpeg`),
your target should be '&ast;.???':

`cp *.??? destination_folder`

The '.' character can be used as simply '.' in a string representation but
it also refers to the current directory. Similarly, '..' is both the two
character string '..' as well as the name of the parent directory. Files and
directories beginning with a '.' are typically hidden from view and used
as system or configuration files. Fun!

### Deleting files and folders
To delete a file or folder use the 'rm' command:

`rm target`

To delete all files in the current working directory:

`rm *`

Note that, unlike many graphical user interface systems with a Trash Can
for deletion, you cannot retrieve files deleted at the command line. Once
a file is deleted at the command line it is gone unless backups were previously
made and can be restored. Use caution when deleting files at the command line.
Always have backups to protect your work.

To request a prompt for each file you wish to delete, use the '-i' option:

`rm -i *`

To delete a folder use the 'rm' command with the '-r' option:

`rm -r target`

An empty directory/folder can be deleted with the 'rmdir' command:

`rmdir target`

### Listing files
To see the files and folders in the current working directory use the 'ls' command:

`ls target`

### Changing the current working directory
Use the 'cd' command to change your current directory:

`cd target`

### Making a new folder
Use the 'mkdir' command to create new folders/directories:

`mkdir target`

### Getting help for a command
Most commands have either a built-in help/usage message and/or a manual page
that describes the command options and usage. To view the manual page for a
command, use the 'man' command:

`man command`

where 'command' is the name of the command, e.g. `man ls` would display the
manual page for the 'ls' command.

Often commands will display their usage when invoked with the '-h' or '-u' or
-help' or '--help' option. The exact option to use for help varies from command
to command.

### Common Commands
Here is a summary of some useful commands:

<table border="0">
  <tr>
    <th style="text-align: center">Command (Windows)</th>
    <th style="text-align: center">Command (Mac OS / Linux)</th>
    <th style="text-align: center">Description</th>
    <th style="text-align: center">Example</th>
  </tr>
  <tr>
    <td>exit</td><td>exit</td><td>close the window</td><td><strong>exit</strong></td>
  </tr>
  <tr>
    <td>cd</td><td>cd</td><td>change directory</td><td><strong>cd test</strong></td>
  </tr>
  <tr>
    <td>cd</td><td>pwd</td><td>show the current directory</td><td><strong>cd</strong> (Windows) or <strong>pwd</strong> (Mac OS / Linux)</td>
  </tr>
  <tr>
    <td>dir</td><td>ls</td><td>list directories/files</td><td><strong>ls</strong></td>
  </tr>
  <tr>
    <td>copy</td><td>cp</td><td>copy file</td><td><strong>cp test/test.txt stage/test.txt</strong></td>
  </tr>
  <tr>
    <td>move</td><td>mv</td><td>move file</td><td><strong>mv test/test.txt stage/test.txt</strong></td>
  </tr>
  <tr>
    <td>mkdir</td><td>mkdir</td><td>create a new directory</td><td><strong>mkdir test</strong></td>
  </tr>
  <tr>
    <td>rmdir (or del)</td><td>rm</td><td>delete a file</td><td><strong>rm test/test.txt</strong></td>
  </tr>
  <tr>
    <td>rmdir /S</td><td>rm -r</td><td>delete a directory</td><td><strong>rm -r test</strong></td>
  </tr>
  <tr>
    <td>[CMD] /?</td><td>man [CMD]</td><td>get help for a command</td><td><strong>cd /?</strong> (Windows) or <strong>man cd</strong> (Mac OS / Linux)</td>
  </tr>
</table>

## Running scripts
To run a Python script, e.g. to run the script example.py, execute the command:

`python example.py`

For Java, use:

`java example.java`

To execute a shell script with the Bash shell interpreter, execute:

`bash example.sh`

Often it is not necessary to invoke a command or interpreter to run a script as
the script itself can specify which command/interpreter to use. In this case it
is sufficient to simply execute the script directly. For example, if the shell
script 'example.sh' begins with '#!/bin/bash' and has execute permission then
it can be executed and interpreted by Bash with the command:

`./example.sh`

## Accessing another system with the Command Line

Often it is not possible or inconvenient to access another system directly.
If remote access has been enabled, one method to do this is through the Secure
Shell (SSH), which lets you securely control and modify remote systems over
the network.

You can do this by typing in `ssh username@host_server`
where username is the account name of your account on the remote system, and
host_server is the remote host.

If you have a password to access your account on the server, SSH will
prompt you to enter that in.

If it’s your first time accessing that particular system, your computer may also
ask you if it can remember the authenticity key — type in ‘yes’ or the corresponding
phrase so that your computer doesn’t ask you this every time.

After successfully logging in to the remote system via SSH, you are presented with
a command line prompt and commands you type in the terminal window at that prompt
will be interpreted as commands to execute on the remote system. Exit the SSH
session by logging out, usually by typing the command `exit`. After exiting the
SSH session, commands issued at the command line prompt in your terminal window
will be interpreted as commands to execute on the local system.

## Permissions and ownership
In Unix and Linux systems there are the concepts of 'permission' and 'ownership'.
Every file and directory has a set of permissions and ownership associated with it.
Ownership can be specified by User, Group, and Others. Permission can be granted
differently for the User, Group, and Others. It is thereby possible for a file
or directory to be accessible by a certain user but not by others or accessible
by the members of a certain group but not by others. Permissions can govern the
ability to read, write, or execute files and directories. A user may be given
permission to read a file but not to write or modify it in any way. Similarly,
one user may be able to execute a command while another may not have permission
to execute that same command. In this way safeguards and some level of security
can easily be implemented just by changing the ownership and permission of files
and directories.

To view the ownership and permission on a file or directory, use the 'ls' command.

`ls -l filename`

will display permissions and ownership for the file 'filename'.

`ls -ld folder`

will display permissions and ownership for the directory 'folder'.

The first field returned by the 'ls -l' command is the permissions field.
The permissions field consists of 3 subfields, each of which has 3 characters.
The 3 characters are 'r', 'w', and 'x' indicating 'read', 'write', and 'execute'.
The first subfield contains the permissions for the owner of the file/directory.
The second subfield contains the permissions for the group of the file/directory.
The third subfield contains the permissions for all others.
For example, if a file has permissions listed as `-rw-r--r--` by 'ls -l' then
that file is readable and writeable by its owner, read-only by its group, and
read-only by all others.

If you receive a message similar to **Permission denied** when executing a command
at the command line it means you do not have permission to execute that file. This
can be corrected by modifying the permissions or ownership of the file with either
the `chmod` or `chown` command, if you have permission to do so. Excercise caution
when modifying permissions and ownership. There is probably a good reason the
permissions and ownership are set the way they are. Often command execution is
restricted to a particular user for security reasons or because the command performs
some actions only that user or group have permission to perform. In these cases,
rather than changing permissions or ownership, execute the command as the
appropriate user. This can be accomplished using the `sudo` command on Mac and Linux.
