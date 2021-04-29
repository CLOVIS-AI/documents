# Git 2: Commits and the history

## Commits

First and foremost, Git is a versioning system.

Git calls versions 'commits'. A commit is a small snapshot of the state of the 'repository' (project) at a certain point in time.

In practice, a commit is identified by a hash; a long string a letters and digits. Don't worry too much about those, you will never need to be able to understand them, at most we will copy-paste them.

```text
021aafec65f4e92d732794e6128e710bbcfbd07b
```

This is a commit ID. The reason it is so long is to ensure you never have two different commits that have the same ID, as that would break everything. It is really rare that two commits have an ID that are even slightly similar, so Git allows you to only write the start of the ID:

```text
021aafe
```

It is time to learn our first command (the output is simplified).

```shell
$ git show 8621a7f
commit 8621a7f3d2294201081ed4ec85925fd0371c8873
Author: Ivan “CLOVIS” Canet <ivan.canet@gmail.com>
Date:   Thu Apr 29 19:59:20 2021 +0200

    What is Git, and how to download it?

[...]
```

As we can see, git stores:

- The full ID: `8621a7f3d2294201081ed4ec85925fd0371c8873`,
- The author of the commit, and their email address,
- The date of the commit creation,
- A short description of what happened,
- Something weird with files.

> Tip: Commit should be kept as small as possible. Small commits are easier to work with.
> Commits do not represent "versions you can send to your users", but more "internal step of development".

Essentially, a commit is a 'save' of the project, with a nice little description, so you can easily find it later. Some people create a commit every ten minutes or so, do not be afraid of seeing many commits.

## Files

The whole point of a versioning system is to store versions of files.

A commit is a 'snapshot' of the project. That doesn't mean, however, that Git stores a full copy of the project everytime you create a commit: Git is clever, and will not copy files that haven't changed (and many other optimizations).

`git show` displays something like this:

```shell
$ git show e5606bf
[...]
diff --git a/server/src/main.c b/server/src/main.c
index 6462c31..7da875b 100644
--- a/server/src/main.c
+++ b/server/src/main.c
@@ -1,10 +1,13 @@
 #include <stdio.h>
+#include <string.h>
+#include <pthread.h>
 #include "loader.h"
 
 int main() {
        struct config config;
+       log_message(INFO, "Loading configuration file...");
        config_load(&config);
@@ -13,6 +16,9 @@ int main() {
-       start_server(&config);
+       pthread_t server_thread;
+       pthread_create(&server_thread, NULL, (void *(*)(void *)) &start_server, &config);
+
+       pthread_join(server_thread, NULL);
        return 0;
 }
```

Let us deconstruct this output together, line by line.

First, Git is telling us that it will display what changed in the file `server/src/main.c`. The name is written twice, because Git understands when you rename a file (the `a/` means "the previous version", the `b/` means "the current version"), however this is not the case here, so both filenames are the same.

```shell
diff --git a/server/src/main.c b/server/src/main.c
```

It is important to note here that Git does not understand folders. It is not possible to add an empty folder in a commit.

The next three lines essentially repeat that information with more details: it displays the ID of the file (previous, then current version), some metadata (you might recognize UNIX access rights in there), and then the name of the two versions of the file, again.

```shell
index 6462c31..7da875b 100644
--- a/server/src/main.c
+++ b/server/src/main.c
```

Now, Git will show us what has changed. Because the file could potentially be big, Git splits into small chunks: the next line tells us that a chunk is starting:

```shell
@@ -1,10 +1,13 @@
```

Git will now display what changed:

```shell
 #include <stdio.h>
+#include <string.h>
+#include <pthread.h>
 #include "loader.h"
```

The first column of the view tells us what happened:

- a space means nothing happened,
- `+` means a line was added (colored in green in the terminal),
- `-` means a line was removed (colored in red in the terminal).

We understand that this commit adds two `#include` lines.

Next, Git gives us more changes and shows another chunk:

```shell
 int main() {
        struct config config;
+       log_message(INFO, "Loading configuration file...");
        config_load(&config);
@@ -13,6 +16,9 @@ int main() {
-       start_server(&config);
+       pthread_t server_thread;
+       pthread_create(&server_thread, NULL, (void *(*)(void *)) &start_server, &config);
+
+       pthread_join(server_thread, NULL);
        return 0;
 }
```

Git shows edited lines as removed, then added back (so there is no need for a "modified" symbol).

This whole chapter is quite representative of Git itself. As you can see, nothing we saw was complicated, but without a good explanation it is far from obvious. This is way it is important to learn how to properly use Git.

## Parents

So far, in this chapter, we've seen in a lot of details what commits are. However, the project is not going to go that far if it only has a single version.

> Each commit knows which one happened just before itself.

In practice, that means that each commit stores the ID of its predecessor. This way, it is possible to go through the entire history, towards the past, one commit at a time:

```text
c <- d
```

Here, the most recent commit is `d`. `d` knows that the previous commit was `c`, so we draw an arrow from `d` to `c`. Of course, we could have many more commits:

```text
a <- b <- c <- d
```

`d`'s parent is `c`, its parent is `b`, its parent is `a`.

Notice how the arrow point towards the past: it is not possible any other way (how could you create a commit that knows the next commit, when it doesn't yet exist?). Take advantage of the arrows to know what the direction of the graph is. When drawing the graph horizontally, we will always draw it with the future on the right, but graphs drawn vertically sometimes have the future at the top (most common) or the bottom.

The idea is that, starting from any commit, we can get the previous one, then the previous one, etc, until we reach the very first commit (which doesn't have any previous commits, we call it the 'root').

## History

At the start of this chapter, we mentioned that each commit is identified by an ID.

This ID is computed by Git as the 'hash' of that particular commit. A hash is a number created from some data, with the aim that even a slight modification to the data would give a completely different number. The hash itself doesn't mean anything.

The point is that it's really easy to compute a hash from a commit (for Git, not for humans), but it's impossible* to create a commit that has the hash you exactly want (*: it would take thousands of years).

All the information we've seen before (changes, author identity, date, parent hash, …) is taken into account when calculating a hash.

This particular property has many benefits:

- If the project is corrupted or tampered with in any way, Git will know and will tell you;
- If you only have the latest commit in a repository, you can use its parent's hash to ask someone else for the parent commit. By checking that the commit they gave matches the hash, you can tell if they are lying or not, and because that commit also has the hash of its parent, you can ask for the commit that came before that one, etc. You can go back to the very start of the project and be sure that nothing was changed;
- If you want to share a specific version of the project with someone else, you only have to give them the commit hash (but we can do much better).

Git guarantees that a commit is _never edited_, in any way.

Next up, we will learn how to [create commits](3-worktree.md).
