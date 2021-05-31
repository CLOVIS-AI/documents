o# Git 3: The worktree

Today, we will start typing our first real commands.

## Setting up the environment

Before we can start doing anything, we need to create a 'repository' (a project).

This command will create an empty project from a folder (empty or not):

```shell
$ git init
```

You can run this command in a folder that is not empty, if you want to start tracking the history of a project you started previously. Git will create a hidden directory called `.git`. This is where Git stores everything about the project. You do not need to open it, nor to understand what happens inside.

> `git init` might be the canonical way to create a project, in practice this is not the command we use most of the time. We will mention that in a later chapter.

Now that the repository is created, we can start to work.

## Configuration

As we saw, the most important concept of Git is a commit. A commit contains the identity of its authors, a description, a date, and files.

Before we do anything, we will need to tell Git what our identity is.

```shell
$ git config --global user.name "Firstname Lastname"
$ git config --global user.email "your.email.here@email.com"
```

The `--global` means it will be the default for all projects on this computer. You can remove it to only set your identity for this particular repository.

Note that Git does not check that you are who you say you are, nor does it even test that your email is valid, or anything else. To protect yourself from identity theft, other tools are used to sign your commits (GPG).

## The working tree

Git calls the project directory the "working tree": this is the version of the project you are working on (and that's a tree of files). Essentially, this means that at all time, you have the "current version" of the project, the one you're editing, right in the project directory, and Git stores every other version in a compressed way. At any point, Git is able to swap the current version with any other version you like, so you can take a look into the past.

For now, it is not possible to have two directories that each have a different version of the project. In practice, this is very rarely needed, so we will explain how to do it later.

## The staging area

Git is all about commits. Commits are a snapshot of the state of the project, but it's very rare that you _actually_ want to create a snapshot of the whole project. Most of the time, you edit a few files, then discover that some modifications are good, some are bad, some are half-written, etc.

The "staging area" is a metaphorical area that contains files that will be added in the next commit you will create.

We will now see the most important command of whole of Git:

```shell
$ git status
On branch 15-mutex
Your branch is up to date with 'gitlab/15-mutex'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   include/thread.h

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   thread/thread.c

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        vgcore.14781
        vgcore.15018
        vgcore.15378
```

Status is composed of 4 sections. Empty sections are not displayed, so yours will probably be shorter.

The first section tells us about the current state of the repository. We will not discuss it now, but we will come back to it many times in the next chapters.

```shell
On branch 15-mutex
Your branch is up to date with 'gitlab/15-mutex'.
```

The next section is the staging area: it appears in green, and corresponds to the future commit:

```shell
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   include/thread.h
```

The next section is the unstaged area: it appears in red, and corresponds to file that have been modified, but that haven't been staged for the next commit (changes that you've made, but that aren't ready yet):

```shell
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   thread/thread.c
```

And finally, the last section displays files that are untracked. These files have appeared, but have never been staged and are not in the history. They appear in red as well. Git has never seen them, and doesn't know what to do with them.

```shell
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        vgcore.14781
        vgcore.15018
        vgcore.15378
```

Hence, the opposite path is followed by files:

- A file you just created appears in the 'untracked' section;
- A file that previously existed, but has been edited since the last commit, appears in the 'unstaged area';
- A modification that has been selected to be a part of the next commit appears in green, in the 'staging area'.

Notice how the `git status` command gives you, for each section, the command to move a file to another section.

##### Moving files between sections

First, commands related to the untracked area:

```shell
# untracked -> unstaged
git add --intent-to-add <file>...

# untracked -> staged (bypassing unstaged)
git add <file>...
```

Second, commands related to the unstaged area:

```shell
# unstaged -> staged (whole file)
git add <file>...

# unstaged -> staged (interactively, only the parts of the file you want)
git add -p <file>...

# unstaged -> staged (interactively, ask which files I want)
git add -p

# unstaged -> cancel all edits (they will be lost)
git restore <file>...
```

Note that a file that has been tracked at some point can never be untracked again.

Lastly, commands related to the staging area:

```shell
# staged -> unstaged (I changed my mind, I don't want this modification in the next commit, so keep it for later)
git restore --staged <file>..
```

You do not need to learn all these commands now (as you saw, `status` reminds you of most of them). We included all of them here so you can easily find them in the future.

> Before we move on, it is important to remark that there are other commands that allow to bypass some steps.
> Some of these commands can appear simpler, because less steps are involved, however they exist for specific cases and will very often lead to mistakes.
> It is important to always be aware of what is, and isn't in a commit, especially when you edit many files. It is very easy to tell Git to "add every file", and later to notice that a temporary modification somewhere was added that breaks everything.
> Until you are completely used to Git, you should use commands that will force you to think about what you are doing.

What we recommend for beginners:

- When you create a new file, type `git add --intent-to-add <that file>`
- Just before you create a commit, type `git add -p` (with no files given), and follow the interactive prompt. Git will show you everything you changed, and ask you if you want to include it or not (type `?` to access the help).

This is the easiest way not to make mistakes.

## Creating a commit

Now that you have selected all files to be included in the commit (the green section of `status`), all you need to do is to confirm:

```shell
git commit
```

Your preferred text editor will open, type in an explanation of why this commit exists, and close the editor. This creates the commit, with the message you gave, and your configured identity (we configured the identity and the text editor in [1. Basics](1-basics.md)).

There are many styles and guidelines that describe how a commit description should look like. We will not detail any of those here, and beginners should not bother too much with them for now. For now, just keep the idea that a commit description should explain _why a commit exists_ and not _what it does_. It is easy to check what a commit contains, but it is hard to understand what your intent was when you created it.

```text
# This is a good message:
Fixed the memory leak when the user clicks 'help'

# These are bad messages:
fix
.
fix help
added free
```

In general, just try to be helpful. It is not immediately evident why this is important, so just trust us that it will make your life much easier in bigger projects with many people.

Don't overthink this too much either. It is not important that your commit describes everything in perfect detail. Most of the time, you will look at the list of commits and try to guess which one contains a change you're interested in. A short and clear sentence brings you 95% of the way there.

To check that everything went well, you can display the last commit:

```shell
git show
```

This should display the commit ID, your author information, the date, etc, followed by the modifications.

## Ignoring files

As we've seen, Git will not add a file to a commit if we don't explicit request it. In real life, there are many files in a project we don't want to keep versions of:

- Compiled binaries (`.o`, `.so`, …)
- Build files (`build/`, `node_modules/`, …)
- Generated files (`.pdf`, …)

> The rule of thumb is to only keep what is strictly necessary: if you can create a file automatically from other files in the repository, then that file should not be kept.
> Unneeded files make the repository bigger, synchronisation times slower, and scheming through the history harder.
> If they are edited often, they will also lead to painful conflicts (we'll mention those later).

We already know that those files will appear in the 'untracked' section of `git status`, so they will not cause issues. However, in practice, files we don't care about clutter `status`' output. We can tell git not to show them at all, to make our life easier.

We will categorize those files in two groups:

- The files you don't care about that only appear on your computer (temporary files you created manually),
- The files you don't care about that appear on all computers that have the repository (files generated by the compiler).

##### Files that appear on everyone's computer

<!-- .gitignore -->
You can tell Git to ignore files created by your project by creating a file named `.gitignore`. This file can be created anywhere in your project (in fact, you can have multiple in different folders).

In the file, you should just put the name of a file, or the name of a folder, per line:

```.gitignore
# This is a comment. Git ignores these.

# We don't care about all files named 'foo.txt':
foo.txt

# We don't care about all folders named 'bar':
bar/

# We don't care about the file 'foo.txt' in the same directory as the .gitignore 
(but we do care about the files 'foo.txt' in other directories)
/foo.txt

# Same, but for folders:
/bar/

# We don't care about all the files that end in .o (C object files)
*.o
```

Here are a few common examples:

```.gitignore
# C
*.o
*.a

# Gradle
build/

# Python
venv/

# LaTeX
*.pdf
*.log
```

In general, just start working on your project, and everytime a file shows up in `status` that you don't care about, just add it then. There are websites that claim to generate .gitignore files, however they are not always of good quality and can end up hiding files you are interested in. If you really want to take one, always check what it ignores.

Note that .gitignore are not retroactive: if someone already committed a file, adding it to the .gitignore will not stop tracking it. To stop tracking it, tell Git to remove it from the next commit without removing it in your files:

```shell
git rm --cached <your file here>
```

##### Files that only appear on your computer

Sometimes, you create a file that you want to ignore, but it doesn't make sense to tell the other users about it. In these cases, you can create the file `.git/info/exclude` in your repository. It follows the exact same syntax as `.gitignore`, but will not be shared with other team members.

## Conclusion

You now know how to choose what is and isn't in a commit, how to create a commit, and how to hide files that you don't care about.

Next time, we'll go in depth on the history.
