# Git 1: Basic configuration

## Some short configuration

Before using any tool, some basic configuration needs to be done.

Have you used a terminal-based text editor before? We recommend you tell Git which one you prefer now:

```shell
# Tell Git to use 'nano' (recommended for beginners and anyone that doesn't feel too good with the terminal)
# When 'nano' opens: all shortcuts are displayed at the bottom of the screen, ^ means CTRL (so ^X is CTRL X)
git config --global core.editor nano

# Tell Git to use vi/vim (not recommended unless you have used it before)
git config --global core.editor vi
git config --global core.editor vim

# Tell Git to use Emacs (not recommended unless you have used it before)
git config --global core.editor 'emacs -nw'
```

Git will also need some basic information on you: your email address, and your name. Note that the email address doesn't need to be valid (some people use their real ones, some people replace @ with 'at', etc). Tools use this information to know who worked on what.

```shell
# Your email address.
git config --global core.email "your.email.here@gmail.com"
git config --global core.name "Your Name Here"
```

## Aliases

During the following chapters, we will introduce many Git commands. Git has many commands, and often the simpler one has defaults that are not recommended for beginners.

We *highly* recommend that you create _aliases_: custom commands that do what you want. This will let you type less, while still being precise in what you mean.

For example, if you want to type:

```shell
git status --short --branch --untracked=all
```

That is a bit long to type. You could use the short form, but it's a lot harder to understand and remember:

```shell
git status -sb -uall
```

Instead, we encourage you to create an alias:

```shell
# Syntax:
git config --global alias.<name> <command without 'git'>

# Example:
git config --global alias.st 'status --short --branch --untracked=all'
```

Now, you can just type:

```shell
git st
```

As a bonus, here is the alias to display the list of aliases:

```shell
git config --global alias.aliases 'config --get-regexp ^alias\.'
```
