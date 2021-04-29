# Git 0: Introduction

## What is Git?

Git was created in 2005 to host the source code of the Linux Kernel. From the start, Git was created to have very good performance no matter the size of the project. The Linux Kernel has around 75 000 commits (versions) added every year, with around 30 million lines of code ([source](https://www.phoronix.com/scan.php?page=news_item&px=Linux-Git-Stats-EOY2019)). From the start, Git needed to handle any project, and any number of files, fast.

> Git is a decentralized versioning system.

A versioning system is a tool that handles versions of your project. Many projects require that you keep track of versions, so you can remove things that do not work, so you don't lose your work, or to share it with other people.

If you've never used one, you're probably thinking of something like Google Drive's 'versions'. It's essentially the same idea, but much more powerful.

'Decentralized' means that everyone working on the project has a full copy of the whole project and all its history. Previous systems did not all have this capacity: in a centralized system, you cannot work when you are offline, only one person can edit a file at a time, etc. With Git, thousands of people can touch the same project without major issues (but it's not magical either).

## How to get Git?

If you are using a UNIX derivative (Linux, MacOS…), the preferred way is to use your package manager of choice. Git is a very well-known tool, and all distros have a package for it (and many of them install it by default).

On Windows, we recommend using the WSL. Otherwise, you can use "Git Bash".

## The command line

Git is a command-line application. There are many tools that wrap around Git to provide a graphical interface, and this guide will not explore all of them. We will give most of the examples with the command line, but we encourage you to learn whichever tool you prefer. Many Git operations are faster to perform with a powerful GUI than in the terminal, but at the end of the day you will need to know how to use the terminal if you want to understand what the GUI does.

A "healthy" introduction is to start with the command line until you understand what it does (no need to learn the commands by heart, Google is never very far away), and when you feel good with it, then you can try the buttons to see if you prefer that.
