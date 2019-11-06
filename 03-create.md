---
layout: page
title: Version Control with Git
subtitle: Creating a Repository
minutes: 10
---

> ## Learning Objectives {.objectives}
>
> -   Create a local Git repository.

Once Git is configured, we can start using it.
For this lesson, we will create a personal blog and host it on GitHub.
Let's create a directory for our blog and then move into that directory:

```{.bash}
$ cd
$ mkdir my-blog
$ cd my-blog
```

Then we tell Git to make `my-blog` a [repository](reference.html#repository)
&mdash;a place where Git can store versions of our files:

```{.bash}
$ git init
```

If we use `ls` to show the directory's contents, it appears that nothing has
changed:

```{.bash}
$ ls
```

But if we add the `-a` flag to show everything, we can see that Git has created
a hidden directory within `my-blog` called `.git`:

```{.bash}
$ ls -a
```

```{.output}
.	..	.git
```

Git stores information about the project in this special sub-directory.
If we ever delete it, we will lose the project's history.

We can check that everything is set up correctly by asking Git to tell us the
status of our project:

```{.bash}
$ git status
```

```{.output}
On branch master

Initial commit

nothing to commit (create/copy files and use "git add" to track)
```

> ## Places to Create Git Repositories {.challenge}
>
> You want to create two Git repositories for two separate projects.
> You enter the following sequence of commands to create a Git repository
> inside another:
>
> ```{.bash}
> cd                 # return to home directory
> mkdir project-1    # make a new directory project-1
> cd project-1       # go into project-1
> git init           # make the project-1 directory a Git repository
> mkdir project-2    # make a sub-directory project-1/project-2
> cd project-2       # go into project-1/project-2
> git init           # make the project-2 sub-directory a Git repository
> ```
>
> Why is it a bad idea to do this?
> How can you "undo" your last `git init`?
