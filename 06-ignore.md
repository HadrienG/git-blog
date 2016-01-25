---
layout: page
title: Version Control with Git
subtitle: Ignoring Things
minutes: 5
---
> ## Learning Objectives {.objectives}
> 
> *   Configure Git to ignore specific files.
> *   Explain why ignoring files can be useful.

What if we have files that we do not want Git to track for us, like blog post
drafts or a file listing to-dos for yourself. Let's create a few dummy files.

~~~ {.bash}
$ mkdir drafts
$ touch TODO.txt drafts/cancer-cure.txt drafts/solving-world-hunger.txt
~~~

and see what Git says:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	TODO.txt
	drafts/

nothing added to commit but untracked files present (use "git add" to track)

~~~

Putting these files under version control would be a waste of disk space.
What's worse, having them all listed could distract us from changes that 
actually matter, so let's tell Git to ignore them.

We do this by creating a file in the root directory of our project called 
`.gitignore`. In the case of the template, it already exists. 

~~~ {.bash}
$ cat .gitignore
~~~
~~~ {.output}
_site
.DS_Store
.jekyll
.bundle
.sass-cache
Gemfile
Gemfile.lock
node_modules
package.json
~~~

We are going to edit it to ignore our `TODO.txt` file and our `drafts` folder.

~~~ {.bash}
$ nano .gitignore
$ cat .gitignore
~~~
~~~ {.output}
_site
.DS_Store
.jekyll
.bundle
.sass-cache
Gemfile
Gemfile.lock
node_modules
package.json
TODO.txt
drafts/
~~~

The two new patterns at the end tell Git to ignore the file named `TODO.txt` as 
well as the entire `drafts` folder.
(If any of these files were already being tracked, Git would continue to track 
them.)

Once we have updated this file, the output of `git status` is much cleaner:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   .gitignore

no changes added to commit (use "git add" and/or "git commit -a")
~~~

The only thing Git notices now is the updated `.gitignore` file.
You might think we wouldn't want to track it, but everyone we're sharing our 
repository with will probably want to ignore the same things that we're 
ignoring.
Let's add and commit `.gitignore`:

~~~ {.bash}
$ git add .gitignore
$ git commit -m "Add the ignore file"
$ git status
~~~
~~~ {.output}
On branch master
nothing to commit, working directory clean
~~~

As a bonus, using `.gitignore` helps us avoid accidentally adding to the 
repository files that we don't want to track:

~~~ {.bash}
$ git add drafts/cancer-cure.txt
~~~
~~~ {.output}
The following paths are ignored by one of your .gitignore files:
drafts/cancer-cure.txt
Use -f if you really want to add them.
fatal: no files added
~~~

If we really want to override our ignore settings, we can use `git add -f` to 
force Git to add something.
We can also always see the status of ignored files if we want:

~~~ {.bash}
$ git status --ignored
~~~
~~~ {.output}
On branch master
Ignored files:
  (use "git add -f <file>..." to include in what will be committed)

	TODO.txt
	drafts/

nothing to commit, working directory clean
~~~

> ## Ignoring nested files {.challenge}
>
> Given a directory structure that looks like:
> ~~~
> results/data
> results/plots
> ~~~
>
> How would you ignore only `results/plots` and not `results/data`?

> ## Including specific files {.challenge}
>
> How would you ignore all `.data` files in your root directory except for 
> `final.data`?
> Hint: Find out what `!` (the exclamation point operator) does

> ## Ignoring files deep in a directory {.challenge}
>
> Given a directory structure that looks like:
> ~~~
> results/data/position/gps/useless.data
> results/plots
> ~~~
>
> What's the shortest `.gitignore` rule you could write to ignore all `.data`
> files in `result/data/position/gps`
> Hint: What does appending `**` to a rule accomplish?

> ## The order of rules {.challenge}
>
> Given a `.gitignore` file with the following contents:
> ~~~
> *.data
> !*.data
> ~~~
>
> What will be the result?
