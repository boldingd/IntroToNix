# The Basics of *Nix

## The Terminal

This chapter will focus on working with *commands* at a *nix terminal.
Certainly, this is not the only way to work with a modern *Nix system; recall that OS X, Android and Windows 10 can all qualify as *Nix systems.
Certainly, modern Linux systems also come with sophisticated graphical capabilities; the primary author of this text is a particular fan of Gnome 3.

Our decision to beginw with *commands at a terminal* is primariy a practical one; readers are very likely to be familiar with the "Windows/Icons/Menu/Pointer" style interface, but are far less likely to be conversant with *job control* via a terminal.

## Picking a Terminal

The first question a reader might have is, "how do I run the terminal?"
This question depends on which of the many *Nix system the reader is actually using.

For Linux and OS X users, running a terminal is easy; both of their operating systems include robust terminal emulators as primary tools.
Linux users can locate a terminal emulators in their launchers (for Gnome, this is gnome-terminal; for KDE users, this is konsole).

Windows users face a particular agony of choice.

## A First Command

Let's begin with a very simple command: `whoami`.
The `whoami` command simply prints your username.
(Well, sort of.
As it turns out, there are quite a few caveats and subtleties to that statement, but we will consider those later.)

To run `whoami`, simply type it and press enter; you should see output like this:

    $ whoami
    David Bolding

This is typical of the examples that we will be using throughout this text.
The $ is the prompt; you shouldn't type that.
Instead, it indicates that what follows is a line that I typed, and that you should type.
Note that the prompt your terminal will print might be different; that's normal.

`David Bolding` is the result that the `whoami` command produced; of course, you should see your own username printed instead.

Note that you (should have) gotten your terminal back immediately -- that is, `whoami` started, printed its result and exited, leaving you back at the input prompt, ready to print another command.
Not all commands will do this; we will explore quite a few commands that will "take over" your terminal until they are terminated (or suspended, or sent to the background...)

It might not be obvious why we would ever need to be reminded *who we are*.
Unix provides us the ability to effectively "become" other users (provided, of course, that we are actually able to log in as that user -- i.e. that we have the appropriate password for that account).
This is useful, for example, when a specialized account might exist to provide access to a particular resource.
When we start becoming different users, it is possible to loose track of who were are "acting as" *right now*; `whoami` exists so that we can remind ourselves who we are.

Does `whoami` do anything else?
Not much.
It does have two useful *options*, `help` and `version`.
We can give `whoami` the `version` option, like this:

    $whoami --version
    whoami (GNU coreutils) 8.25

(There was more output, which was not included.)

Note the two preceding dashes -- it's `--version`.
This is a common form in \*Nix commands: dash-dash-something is an *option*.

Notably, most *Nix commands have both a `--version` option, which presents the specific version of the command that you are using, and the `--help` option, which will print a short(-ish) help-text for the program.
In days past, a good first-recourse for an unfamiliar program would be running it with `--help`; sadly, this is becoming less useful.
As the internet has grown in prominance, many *Nix programs no longer include useful help resources; the presumption is that searching the internet is likely to be a more meaningful way to get guidance on how to use a program than reading through a (potentially very long!) embedded help message.

## A Second Simple Command: `date`

The `date` command is worth a mention.
When run, `date` prints date and time:

    $ date
    Wed, Apr 19, 2017 11:52:44 PM

Unlike `whoami`, `date` has several options, mainly relating to how to format the output.
Particularly useful is `-I`, which uses prints just the date, in "international format":

    $ date -I
    2017-04-19

Note that that is a capitol-I.
\*Nix in general - and the terminal in specific - are case-sensitive.
Lower-case i will not work:

Again, one might ask why we would need such a command.
\*Nix terminals actually give us very sophisticated facilities for *capturing* the output of one command, and using it as an input for another.
`date -I` is useful in this context for automatically including the date (in a form without spaces) in scripts that generate files.

## Exploring the File System

Let's consider some more immediately useful commands.
You can list the contents of the current directory with `ls`:

    $ ls
    'everyone knows'  lies  secrets  sources.txt  witnesses.txt

Your output is likely to be different, of course.
Earlier, we said that `ls` would list the contents of the "current" directory; in the above example, I listed the contents of a directory called "knowledge".
How did the `ls` command know to list that directory?

Every process on a \*Nix system has a *current working directory*, which is frequencly abbreviated CWD.
The CWD is part of a process's *environment*.
Each process has its own environment, and so each process can have it's own CWD.
However, environments are *inherited*: when one program starts another, the *child* program inherits a copy of the *parent*'s environment -- including the parent's CWD.
Thus, the shell has some notion of what its CWD is; when we start the `ls` command, `ls` inherits the shell's CWD.

We can tell `ls` to list another directory -- or several other directories, in fact.
We simply give each directory as an argument.

    $ ls secrets/ lies/
    lies/:
    alchemy.txt

    secrets/:
    'carcosa location.txt'  'CGB Spender Home Address.txt'


