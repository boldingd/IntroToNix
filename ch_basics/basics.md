# The Basics of *Nix

## The Terminal

This chapter will focus on working with *commands* at a *nix terminal.
Certainly, this is not the only way to work with a modern *Nix system; recall that OS X, Android and Windows 10 can all qualify as *Nix systems.
Certainly, modern Linux systems also come with sophisticated graphical capabilities; the primary author of this text is a particular fan of Gnome 3.

Our decision to beginw with *commands at a terminal* is primariy a practical one; readers are very likely to be familiar with the "Windows/Icons/Menu/Pointer" style interface, but are far less likely to be conversant with *jobs* and *job control* via a terminal.

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
(Options can also have *arguments*; we'll see examples of options with arguments later.)

Notably, most *Nix commands have both a `--version` option, which presents the specific version of the command that you are using, and the `--help` option, which will print a short(-ish) help-text for the program.
In days past, a good first-recourse when dealing with an unfamiliar program would be to run it with `--help`; sadly, this is becoming less useful.
As the internet has grown in prominance, on-line documentation (and the ever-popular StackOverflow) have become the preferred ways to document and explain programs.  Because of this, many *Nix programs no longer include useful help resources; the presumption is that searching the internet is likely to be a more meaningful way to get guidance on how to use a program than reading through a (potentially very long!) embedded help message.

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

    $ date -i
    date: invalid option -- 'i'
    Try 'date --help' for more information.

The `-I` option is a little different than the `--help` option before -- there is only a single dash, followed by a single letter.
This is an alternate format for specifying options: single-dash-single-letter options are normally called "short options", while two-dash-whole-words options are called "long options".
The rules for *short options* and *long options* are similar -- in fact, many short options are abbreviations for certain long options!
In this case, for example, the `-I` short option is short for the (less mnemonic!) `--iso-8601` option:

    $ date --iso-8601
    2017-06-21
    $ date -I
    2017-06-21

One might ask why we would need a command that just prints the date.
\*Nix terminals actually give us very sophisticated facilities for *capturing* the output of one command, and using it as an input for another command.
`date -I` is useful in this context for automatically including the date (in a form without spaces) in scripts that generate files.

# Exploring the File System

## The `ls` Command

Let's consider some more immediately useful commands.
You can list the contents of the current directory with `ls`:

    $ ls
    'everyone knows'  lies  secrets  sources.txt  witnesses.txt

If you run this command in a directory on your computer, your output is likely to be different, of course.
Here, we see that this directory has 5 items in it: `sources.txt` and `witnesses.txt` are presumably files, while `'everyone knows'`, `lies` and `secrets` are directories.
You might wonder how we know that the last three are directories.
That is because, on my system, `ls` is configured to use *colors* while producing its output; it colors directory names blue.
Most modern \*Nix operating systems will have their terminals configured for colored output by default, since it is very useful for a command like ls; unfortunately, not *all* \*Nix operating systems will be configured that way, and many different \*Nix operating systems will use *different* colors.
(Blue-for-directories is not the only special color code that my system uses; executable files also have their names listed in green, for example.)

Note that `ls` has listed the contents of the "current directory"; in the above example, I listed the contents of a directory called "knowledge".
How did the `ls` command know to list that directory?

Every process on a \*Nix system has a *current working directory*, which is frequencly abbreviated CWD.
The CWD is part of a process's *environment*.
Each process has its own environment, and so each process can have it's own CWD.
However, environments are *inherited*: when one program starts another, the *child* program inherits a copy of the *parent*'s environment -- including the parent's CWD.
Thus, the shell has some notion of what its CWD is; when we start the `ls` command from the shell, `ls` inherits the shell's CWD.

We can tell `ls` to list another directory -- or several other directories, in fact.
We simply give each directory as an argument:

    $ ls secrets/ lies/
    lies/:
    alchemy.txt

    secrets/:
    'carcosa location.txt'  'CGB Spender Home Address.txt'

Note that we didn't put an option-name (like `-h`, for example) before the names of the directories we wanted to list; we simply named them as arguments.

Note that `secrets` is the name of a file in the CWD; in this case, we say that `secrets/` is a *relative path*, since it's relative to some other directory (often, as in thsi case, our shell's CWD).
We could also specify the entire path to that directory, all the way from the "starting point" for our file system: we call this a *full path* or *absolute path*.
For the secrets directory, that would look like this:

    $ ls /home/boldingd/Projects/IntroToNix/knowledge/secrets/
    'carcosa location.txt'  'CGB Spender Home Address.txt'

On \*Nix systems, the "starting point" for the file system -- the *root* of the file system -- is specified by `/`.
`/` is also used as the *directory separator* (as opposed to `\` on a Windows system).

## Mounting File Systems

Someone familiar with Windows systems might also wonder why we *didn't* specify a drive letter -- we seem to be assuming that, on a Unix system, that there is exactly *one* root for exactly one file-system (as opposed to one root for each partition of each drive, as in Windows).

There is, in fact, exactly one "file-system name space" on a Unix system.
We can still have multiple drives (and multiple partitions) on a Unix system, but, when we attach them, the contents of those files systems are *merged* into the single "file name space" at a particular point, which is required to be an existing directory name; we call that directory the *mount point* for that file-system.
After we do this, the contents of the file-system on that particular partition are listed under that mount-point, exactly like they were "normal" contents of that directory.

Years ago, we needed to explicitly tell the system to mount drives at certain points (using the `mount` command), and we then had to explicitly disconnect those drives when we were done with them (using the `umount` command).
If we had a particular drive that we were going to be mounting at a particular location often, we might make an entry in the `/etc/fstab` file for it; `/etc/` is a special directory where system configuration files are stored, and `fstab` controls how disk drives are connected.
Today, "desktop" users will not need to do this by hand: when you plug in a USB drive, for example, the system will automatically mount it at a specific location, and your graphical file-browser will include a "quick link" to the contents of that drive.
We will defer a discussion of `mount` and `umount` to a later chapter.

## `ls` options

Returning to `ls` for a moment, `ls` has some common options that we should mention.

We can get significantly more information about the files in a directory by giving `ls` the `-l` option, like this:

    $ ls -l
    total 12
    drwxr-xr-x. 2 boldingd boldingd 4096 Apr 22 06:29 'everyone knows'
    drwxr-xr-x. 2 boldingd boldingd 4096 Apr 22 06:32 lies
    drwxr-xr-x. 2 boldingd boldingd 4096 Apr 22 06:22 secrets
    -rw-r--r--. 1 boldingd boldingd    0 Apr 22 06:07 sources.txt
    -rw-r--r--. 1 boldingd boldingd    0 Apr 22 06:10 witnesses.txt

This includes much more information about each file, including the file's *permissions*, the *link count*, the user ID and group ID associated with the file, the file's size, and the last-modification time (and, of course, the file's name).

By default, `ls` will not list files whose names begin with `.`; these files are *hidden*.
To list hidden files, we can give `ls` either the `-a` or the `--all` option.

    $ ls -a
    .   '.deep lore'      lies     sources.txt
    ..  'everyone knows'  secrets  witnesses.txt
    $ ls --all
    .   '.deep lore'      lies     sources.txt
    ..  'everyone knows'  secrets  witnesses.txt

One useful thing about *short* options is that they can be combined: if we want to give `ls` both the `-a` and `-l` short options, we can do so like this:

    $ ls -al
    total 24
    drwxr-xr-x. 6 boldingd boldingd 4096 Jun 22 00:08 .
    drwxr-xr-x. 6 boldingd boldingd 4096 May 25 20:30 ..
    drwxr-xr-x. 2 boldingd boldingd 4096 Jun 22 00:08 '.deep lore'
    drwxr-xr-x. 2 boldingd boldingd 4096 Apr 22 06:29 'everyone knows'
    drwxr-xr-x. 2 boldingd boldingd 4096 Apr 22 06:32 lies
    drwxr-xr-x. 2 boldingd boldingd 4096 Apr 22 06:22 secrets
    -rw-r--r--. 1 boldingd boldingd    0 Apr 22 06:07 sources.txt
    -rw-r--r--. 1 boldingd boldingd    0 Apr 22 06:10 witnesses.txt

## Changing Directories

We can change the shell's current working directory by using the `cd` command, like this:

    $ cd secrets/
    $ ls
    'carcosa location.txt'  'CGB Spender Home Address.txt'

(You don't need to include the trailing slash in the above command: just `cd secrets` will also work.)
Notice that cd didn't produce any output: I had to run `ls` again to see the contents of my new CWD.
This is not unusual: many \*nix commands will only produce output when something goes wrong.
If the command had failed -- if I had mistyped the name of the directory, for example -- `cd` would have printed an error message:

    $ cd sterces
    bash: cd: sterces: No such file or directory

Intuitively, it might make sense for commands to not clutter the terminal with lots of "I succeeded!" messages, but one might wonder why `cd` (or my shell) didn't automatically list the contents of my new directory -- wouldn't we *usuaully* want to see what's actually in a directory that we've just changed into?
Often, but not always: one particular time that we probably wouldn't want that behavior would be if we were using `cd` inside a *script*.
At a deeper level, though, this is also a bit of *Unix Philosophy* -- namely, that commands should "do one thing and do it well."
The idea is that the `cd` command should *only* change your directory -- that's it.
You can then invoke some *other* command (probably `ls`) to see what's *inside* the directory if that's what you want to do.
Separating these two roles allows us to make the `cd` and `ls` commands both very lean and very good at their specific jobs.
`ls` has a lot of options that we didn't cover; if `cd` automatically `ls`-ed each directory you changed into, you'd either have to build support into `cd` for carrying all those options into `ls`, or you'd have to call `ls` with the options you wanted anyway.
You would quickly find yourself having to discard a *lot* of pointless terminal output from `cd`, and the people who were maintaining the `cd` utility would be expending extra work to make it generate all that output you don't care about!

