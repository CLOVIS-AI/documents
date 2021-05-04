# Kotlin 0: Getting started

This chapter is meant for complete beginners in programming, that have never used an IDE before. If you already have: install IntelliJ and open a Kotlin scratch file.

## An IDE

An IDE (or Integrated Development Environment) is a tool that contains everything you need to work with a language or technology: it's both a text editor, a tool to run your program, and a tool to find issues inside it.

In programming, there are two big kinds of IDEs:

- Light IDEs (Atom, VS Code, …) are programs that can be started fast, are minimalist, but are often not very powerful. They are preferred by web developers, Python developers, …
- Heavy IDEs (Eclipse, IntelliJ, Visual Studio, …) are much bigger programs that are slow to start, but are incredibly more powerful. They are preferred by Java developers…

IntelliJ IDEA is created by JetBrains, the same language that creates Kotlin, and is therefore the best IDE to write Kotlin. IntelliJ is a heavy IDE: much like Photoshop, you are not expected to have many instances of the IDE running at the same time. You will open a _project_, instead of a single _file_.

IntelliJ has two editions:

- Community, free and open source.
- Ultimate, paid (free for students, discounted for startups, etc). Extra features include UML diagrams, support for JEE, etc.

In this course, we will not use any feature specific to Ultimate, so you are free to choose which one you use.

To install the IDE, I recommend:

- Community: if you have an official package in your distribution (`apt`, etc), you can use that.
- In any other case (Ultimate, Windows, or if there are no official package available), install the [JetBrains Toolbox](https://www.jetbrains.com/toolbox-app/). It is a very light installer, that will keep track of your licenses, updates, etc. (some distributions, like Arch, have a package of the toolbox itself)

## Setting up

When you first start IntelliJ, it will prompt you for some information (if you prefer light or dark mode, etc). You can simply follow the prompts.

When you get to the menu screen, find the 'create a new project' button. You will notice that IntelliJ already has presets to create Kotlin projects (and many other technologies).

Select 'Kotlin', and fill in the options:

- Project name: whatever you want,
- Project location: where the files will be saved, whatever you want
- Template: select 'application'
- Build system: select IntelliJ (in real projects, we would use one of the two Gradle options, however this is just a playground)
- Project JDK: select one of the JDKs you have installed (I recommend at least JDK 8, if you do not have any installed, the list should give you an option to install one). Any JDK should do.

After confirming, you should get the project to open, with a 'project' tab on the left, that contains the files of the project.

You should find:

- A folder named after the project, with a subfolder `libs` (we won't be discussing it at all), `src/main/kotlin` (where we would put our code) and `src/test/kotlin` (where we would put our tests).
- An option 'External Libraries' (we won't be discussing it)
- An option 'Scratches and consoles', which has a 'Scratches' folder.

For the first part of the course, we will simply use scratches as a playground. We will discuss later how to create real projects.

Right click on any folder, 'new', 'scratch file' (`CTRL SHIFT ALT Insert`). The file that opens should have two columns, if it doesn't, press the 'side panel output mode' icon on the top right of the file.

The first column is where you will type code. The second column is a simple output window, in which IntelliJ will tell you what you code does (this makes it much easier to poke around and try things). Ensure that 'Interactive mode' is ticked, and 'Use REPL' isn't.

According to traditions, the first thing you should learn in a new language is how to display "Hello World". Go ahead, and type:

```kotlin
println("Hello World")
```

Save, and "Hello World" should appear in the right column (the first time you save can take a bit of time).

That's it, you've written your first Kotlin line. In the next chapter, we'll explain what it means and how it works.
