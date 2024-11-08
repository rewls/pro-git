# Getting Started

- We will begin by explaining some background on version control tools, then move on to how to get Git running on your system and finally how to get it set up to start working with.

- At the end of this chapter you should understand why Git is around, why you should use it and you should be all set up to do so.

## About Version Control

- Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later.

<br>

- It allows you to revert selected files back to a previous state, revert the entire project back to a previous state, compare changes over time, see who last modified something that might be causing m problem, who introduced an issue and when, and more.

- Using a VCS also generally means that if you screw things up or lose files, you can easily recover.

- In addition, you get all this for very little overhead.

### Local Version Control Systems

- Many people’s version-control method of choice is to copy files into another directory (perhaps a time-stamped directory, if they’re clever).

- This approach is very common because it is so simple, but it is also incredibly error prone.

<br>

- To deal with this issue, programmers long ago developed local VCSs that had a simple database that kept all the changes to files under revision control.

### Centralized Version Control Systems

- The next major issue that people encounter is that they need to collaborate with developers on other systems.

- To deal with this problem, Centralized Version Control Systems (CVCSs) were developed.

- These systems have a single server that contains all the versioned files, and a number of clients that check out files from that central place.

- For many years, this has been the standard for version control.

<br>

- This setup offers many advantages, especially over local VCSs.

- For example, everyone knows to a certain degree what everyone else on the project is doing.

- Administrators have fine-grained control over who can do what, and it’s far easier to administer a CVCS than it is to deal with local databases on every client.

- However, this setup also has some serious downsides. 

- The most obvious is the single point of failure that the centralized server represents.

- If that server goes down for an hour, then during that hour nobody can collaborate at all or save versioned changes to anything they’re working on.

- If the hard disk the central database is on becomes corrupted, and proper backups haven’t been kept, you lose absolutely everything — the entire history of the project except whatever single snapshots people happen to have on their local machines.

- Local VCSs suffer from this same problem — whenever you have the entire history of the project in a single place, you risk losing everything.

### Distributed Version Control Systems

- This is where Distributed Version Control Systems (DVCSs) step in.

- In a DVCS, clients don’t just check out the latest snapshot of the files; rather, they fully mirror the repository, including its full history.

- Thus, if any server dies, and these systems were collaborating via that server, any of the client repositories can be copied back up to the server to restore it.

- Every clone is really a full backup of all the data.

- Furthermore, many of these systems deal pretty well with having several remote repositories they can work with, so you can collaborate with different groups of people in different ways simultaneously within the same project. 

- This allows you to set up several types of workflows that aren’t possible in centralized systems, such as hierarchical models.

## A Short History of Git

- As with many great things in life, Git began with a bit of creative destruction and fiery controversy.

- The Linux kernel is an open source software project of fairly large scope.

- During the early years of the Linux kernel maintenance (1991–2002), changes to the software were passed around as patches and archived files.

- In 2002, the Linux kernel project began using a proprietary DVCS called BitKeeper.

- In 2005, the relationship between the community that developed the Linux kernel and the commercial company that developed BitKeeper broke down, and the tool’s free-of-charge status was revoked.

- This prompted the Linux development community to develop their own tool based on some of the lessons they learned while using BitKeeper.

- Some of the goals of the new system were as follows:

    - Speed

    - Simple design

    - Strong support for non-linear development (thousands of parallel branches)

    - Fully distributed

    - Able to handle large projects like the Linux kernel efficiently (speed and data size)

<br>

- Since its birth in 2005, Git has evolved and matured to be easy to use and yet retain these initial qualities.

- It’s amazingly fast, it’s very efficient with large projects, and it has an incredible branching system for non-linear development (see Git Branching).

## What is Git?

- So, what is Git in a nutshell?

- This is an important section to absorb, because if you understand what Git is and the fundamentals of how it works, then using Git effectively will probably be much easier for you.

### Snapshots, Not Differences

- The major difference between Git and any other VCS is the way Git thinks about its data.

- Conceptually, most other systems store information as a list of file-based changes.

> ##### Figure 4. Storing data as changes to a base version of each file

- Instead, Git thinks of its data more like a series of snapshots of a miniature filesystem.

- With Git, every time you commit, or save the state of your project, Git basically takes a picture of what all your files look like at that moment and stores a reference to that snapshot.

- To be efficient, if files have not changed, Git doesn't store the file again, just a link to the previous identical file it has already stored.

- Git thinks about its data more like a **stream of snapshots**.

> ##### Figure 5. Storing data as snapshots of the project over time

- This makes Git more like a mini filesystem with some incredibly powerful tools built on top of it, rather than simply a VCS.

- We’ll explore some of the benefits you gain by thinking of your data this way when we cover Git branching in Git Branching.

### Nearly Every Operation Is Local

- Most operations in Git need only local files and resources to operate — generally no information is needed from another computer on your network.

- Because you have the entire history of the project right there on your local disk, most operations seem almost instantaneous.

### Git Has Integrity

- Everything in Git is checksummed before it is stored and is then referred to by that checksum.

- This means it’s impossible to change the contents of any file or directory without Git knowing about it. 

- This functionality is built into Git at the lowest levels and is integral to its philosophy.

- You can’t lose information in transit or get file corruption without Git being able to detect it.

<br>

- The mechanism that Git uses for this checksumming is called a SHA-1 hash.

- This is a 40-character string composed of hexadecimal characters (0–9 and a–f) and calculated based on the contents of a file or directory structure in Git.

### Git Generally Only Adds Data

- When you do actions in Git, nearly all of them only add data to the Git database.

- It is hard to get the system to do anything that is not undoable or to make it erase data in any way.

<br>

- This makes using Git a joy because we know we can experiment without the danger of severely screwing things up. 

- For a more in-depth look at how Git stores its data and how you can recover data that seems lost, see Undoing Things.

### The Three States

- Git has three main states that your files can reside in: *modified*, *staged*, and *committed*:

    - Modified means that you have changed the file but have not committed it to your database yet.

    - Staged means that you have marked a modified file in its current version to go into your next commit snapshot.

    - Committed means that the data is safely stored in your local database.

<br>

- This leads us to the three main sections of a Git project: the working tree, the staging area, and the Git directory.

<br>

- The working tree is a single checkout of one version of the project.

- These files are pulled out of the compressed database in the Git directory and placed on disk for you to use or modify.

<br>

- The staging area is a file, generally contained in your Git directory, that stores information about what will go into your next commit.

- Its technical name in Git parlance is the “index”, but the phrase “staging area” works just as well.

<br>

- The Git directory is where Git stores the metadata and object database for your project.

- This is the most important part of Git, and it is what is copied when you clone a repository from another computer.

<br>

- The basic Git workflow goes something like this:

    1. You modify files in your working tree.

    2. You selectively stage just those changes you want to be part of your next commit, which adds only those changes to the staging area.

    3. You do a commit, which takes the files as they are in the staging area and stores that snapshot permanently to your Git directory.

<br>

- If a particular version of a file is in the Git directory, it’s considered *committed*.

- If it has been modified and was added to the staging area, it is *staged*.

- And if it was changed since it was checked out but has not been staged, it is *modified*.

- In Git Basics, you’ll learn more about these states and how you can either take advantage of them or skip the staged part entirely.

## The Command Line

- There are a lot of different ways to use Git.

- For this book, we will be using Git on the command line.

- For one, the command line is the only place you can run all Git commands — most of the GUIs implement only a partial subset of Git functionality for simplicity.

<br>

- So we will expect you to know how to open Terminal in macOS or Command Prompt or PowerShell in Windows.

### Installing Git

- You can either install it as a package or via another installer, or download the source code and compile it yourself.

> ##### Information
>
> - This book was written using Git version 2.
>
> - Since Git is quite excellent at preserving backwards compatibility, any recent version should work just fine.
>
> - Though most of the commands we use should work even in ancient versions of Git, some of them might not or might act slightly differently.

#### Installing on Linux

- If you want to install the basic Git tools on Linux via a binary installer, you can generally do so through the package management tool that comes with your distribution.

- There are instructions for installing on several different Unix distributions on the Git website, at https://git-scm.com/download/linux.

## First-Time Git Setup

- Now that you have Git on your system, you'll want to do a few things to customize your Git environment.

- You should have to do these things only once on any given computer; they’ll stick around between upgrades.

- You can also change them at any time by running through the commands again.

- Git comes with a tool called `git config` that lets you get and set configuration variables that control all aspects of how Git looks and operates.

- These variables can be stored in three different places:

    1. `[path]/etc/gitconfig` file: Contains values applied to every user on the system and all their repositories.

        - If you pass the option `--system` to `git config`, it reads and writes from this file specifically.

        - Because this is a system configuration file, you would need administrative or superuser privilege to make changes to it.

    2. `~/.gitconfig` or `~/.config/git/config` file: Values specific personally to you, the user.

        - You can make Git read and write to this file specifically by passing the `--global` option, and this affects all of the repositories you work with on your system.

    3. `config` file in the Git directory (that is, `.git/config`) of whatever repository you’re currently using: Specific to that single repository.

        - You can force Git to read from and write to this file with the `--local` option, but that is in fact the default.

        - Unsurprisingly, you need to be located somewhere in a Git repository for this option to work properly.

- Each level overrides values in the previous level, so values in `.git/config` trump those in `[path]/etc/gitconfig`.

- You can view all of your settings and where they are coming from using:

    ```sh
    $ git config --list --show-origin
    ```

### Your Identity

- The first thing you should do when you install Git is to set your user name and email address.

- This is important because every Git commit uses this information, and it’s immutably baked into the commits you start creating:

    ```sh
    $ git config --global user.name "John Doe"
    $ git config --global user.email johndoe@example.com
    ```

- Again, you need to do this only once if you pass the `--global` option, because then Git will always use that information for anything you do on that system.

- If you want to override this with a different name or email address for specific projects, you can run the command without the `--global` option when you’re in that project.

### Your Editor

- Now that your identity is set up, you can configure the default text editor that will be used when Git needs you to type in a message.

- If not configured, Git uses your system’s default editor.

- If you want to use a different text editor, you can do the following:

    ```sh
    $ git config --global core.editor vim
    ```
    
### Your default branch name

- By default Git will create a branch called *master* when you create a new repository with `git init`. 

- From Git version 2.28 onwards, you can set a different name for the initial branch.

- To set *main* as the default branch name do:

    ```sh
    $ git config --global init.defaultBranch main
    ```

### Checking Your Settings

- If you want to check your configuration settings, you can use the `git config --list` command to list all the settings Git can find at that point:

    ```sh
    $ git config --list 
    user.name=John Doe 
    user.email=johndoe@example.com 
    color.status=auto 
    color.branch=auto 
    color.interactive=auto 
    color.diff=auto 
    ...
    ```

- You may see keys more than once, because Git reads the same key from different files.

- In this case, Git uses the last value for each unique key it sees.

<br>

- You can also check what Git thinks a specific key’s value is by typing `git config <key>`:

    ```sh
    $ git config user.name 
    John Doe
    ```

> ##### Information
>
> - Since Git might read the same configuration variable value from more than one file, it’s possible that you have an unexpected value for one of these values and you don’t know why.
>
> - In cases like that, you can query Git as to the origin for that value, and it will tell you which configuration file had the final say in setting that value:
>
›   ```sh
›   $ git config --show-origin rerere.autoUpdate 
›   file:/home/johndoe/.gitconfig   false 
›   ```

## Getting Help

- If you ever need help while using Git, there are three equivalent ways to get the comprehensive manual page (manpage) help for any of the Git commands:

    ```sh
    $ git help <verb> 
    $ git <verb> --help 
    $ man git-<verb>
    ```

- If the manpages and this book aren’t enough and you need in-person help, you can try the `#git`, `#github`, or `#gitlab` channels on the Libera Chat IRC server, which can be found at https://libera.chat/.

- These channels are regularly filled with hundreds of people who are all very knowledgeable about Git and are often willing to help.

- In addition, if you don’t need the full-blown manpage help, but just need a quick refresher on the available options for a Git command, you can ask for the more concise "help" output with the `-h` option.

## Summary

- You should have a basic understanding of what Git is and how it’s different from any centralized version control systems you may have been using previously.

- You should also now have a working version of Git on your system that’s set up with your personal identity.

- It’s now time to learn some Git basics.
