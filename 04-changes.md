---
layout: page
title: Version Control with Git
subtitle: Tracking Changes
minutes: 20
---
> ## Learning Objectives {.objectives}
> 
> *   Go through the modify-add-commit cycle for single and multiple files.
> *   Explain where information is stored at each stage of Git commit workflow.

To make our lives easier, we will use a blog template called Jekyll Now.
We can download it here: 
[https://github.com/barryclark/jekyll-now/archive/v1.1.0.tar.gz](https://github.com/barryclark/jekyll-now/archive/v1.1.0.tar.gz). 
Once we've downloaded it, navigate to our Downloads folder, decompress it and 
move all of its contents into our `my-blog` repository:

~~~ {.bash}
$ cd /path/to/downloads
$ tar xzf jekyll-now-1.1.0.tar.gz
$ mv jekyll-now-1.1.0/* ~/my-blog/
~~~

If we move back to our `my-blog repository`, we can see the new files and 
folders.

~~~ {.bash}
$ ls
~~~
~~~ {.output}
404.md      LICENSE     _config.yml _layouts    _sass       feed.xml    index.html
CNAME       README.md   _includes   _posts      about.md    images      style.scss
~~~

If we check the status of our project again, Git tells us that it's noticed the 
new files:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore
	404.md
	CNAME
	LICENSE
	README.md
	_config.yml
	_includes/
	_layouts/
	_posts/
	_sass/
	about.md
	feed.xml
	images/
	index.html
	style.scss

nothing added to commit but untracked files present (use "git add" to track)
~~~

The "untracked files" message means that there are files in the directory that 
Git isn't keeping track of.
We want Git to track these files and can do so using `git add`:

~~~ {.bash}
$ git add .     # Add the entire current directory
~~~

We can then check that the right thing happened:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   .gitignore
	new file:   404.md
	new file:   CNAME
	new file:   LICENSE
	new file:   README.md
	new file:   _config.yml
	new file:   _includes/analytics.html
	new file:   _includes/disqus.html
	new file:   _includes/meta.html
	new file:   _includes/svg-icons.html
	new file:   _layouts/default.html
	new file:   _layouts/page.html
	new file:   _layouts/post.html
	new file:   _posts/2014-3-3-Hello-World.md
	new file:   _sass/_highlights.scss
	new file:   _sass/_reset.scss
	new file:   _sass/_svg-icons.scss
	new file:   _sass/_variables.scss
	new file:   about.md
	new file:   feed.xml
	new file:   images/404.jpg
	new file:   images/config.png
	new file:   images/first-post.png
	new file:   images/jekyll-logo.png
	new file:   images/jekyll-now-theme-screenshot.jpg
	new file:   images/step1.gif
	new file:   index.html
	new file:   style.scss
~~~

Git now knows that it's supposed to keep track of all of these files, but it 
hasn't recorded these changes as a commit yet.
To get it to do that, we need to run one more command:

~~~ {.bash}
$ git commit -m "Add template files"
~~~
~~~ {.output}
[master (root-commit) 0dd20d9] Added template files
 28 files changed, 899 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 404.md
 create mode 100644 CNAME
 create mode 100644 LICENSE
 create mode 100644 README.md
 create mode 100644 _config.yml
 create mode 100644 _includes/analytics.html
 create mode 100644 _includes/disqus.html
 create mode 100644 _includes/meta.html
 create mode 100644 _includes/svg-icons.html
 create mode 100644 _layouts/default.html
 create mode 100644 _layouts/page.html
 create mode 100644 _layouts/post.html
 create mode 100644 _posts/2014-3-3-Hello-World.md
 create mode 100644 _sass/_highlights.scss
 create mode 100644 _sass/_reset.scss
 create mode 100644 _sass/_svg-icons.scss
 create mode 100644 _sass/_variables.scss
 create mode 100644 about.md
 create mode 100644 feed.xml
 create mode 100644 images/404.jpg
 create mode 100644 images/config.png
 create mode 100644 images/first-post.png
 create mode 100644 images/jekyll-logo.png
 create mode 100644 images/jekyll-now-theme-screenshot.jpg
 create mode 100644 images/step1.gif
 create mode 100644 index.html
 create mode 100755 style.scss
~~~

When we run `git commit`, Git takes everything we have told it to save by using 
`git add` and stores a copy permanently inside the special `.git` directory.
This permanent copy is called a [commit](reference.html#commit) (or 
[revision](reference.html#revision)) and its short identifier is `f22b25e` 
(your commit may have another identifier.)

We use the `-m` flag (for "message") to record a short, descriptive, and 
specific comment that will help us remember later on what we did and why.
If we just run `git commit` without the `-m` option, Git will launch `nano` 
(or whatever other editor we configured as `core.editor`) so that we can write 
a longer message.

[Good commit messages][commit-messages] start with a brief (<50 characters) 
summary of changes made in the commit. 
If you want to go into more detail, add a blank line between the summary line 
and your additional notes.

If we run `git status` now:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
On branch master
nothing to commit, working directory clean
~~~

it tells us everything is up to date.
If we want to know what we've done recently, we can ask Git to show us the 
project's history using `git log`:

~~~ {.bash}
$ git log
~~~
~~~ {.output}
commit 0dd20d9222f2a6d7e96d2876c327094b1fa34f58
Author: Bruno Grande <bgrande@sfu.ca>
Date:   Sun Jan 24 15:27:36 2016 -0800

    Add template files
~~~

`git log` lists all commits made to a repository in reverse chronological 
order.
The listing for each commit includes the commit's full identifier (which starts 
with the same characters as the short identifier printed by the `git commit` 
command earlier), the commit's author, when it was created, and the log message 
Git was given when the commit was created.

> ## Where Are My Changes? {.callout}
>
> If we run `ls` at this point, we will still see just the same files as 
before.
> That's because Git saves information about files' history in the special 
`.git` directory mentioned earlier so that our filesystem doesn't become 
cluttered (and so that we can't accidentally edit or delete an old version).

Now, we are going to personalize the blog.
We need to edit the configuration file (`_config.yml`) to add our name. 
(Again, we'll edit with `nano` and then `head` the file to show the first 10 
lines; you may use a different editor, and don't need to `head`.)

~~~ {.bash}
$ nano _config.yml
$ head _config.yml
~~~
~~~ {.output}
#
# This file contains configuration flags to customize your site
#

# Name of your site (displayed in the header)
name: Bruno Grande

# Short bio or description (displayed in the header)
description: Web Developer from Somewhere
~~~

When we run `git status` now, it tells us that a file it already knows about 
has been modified:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   _config.yml

no changes added to commit (use "git add" and/or "git commit -a")
~~~

The last line is the key phrase: "no changes added to commit".
We have changed this file, but we haven't told Git we will want to save those 
changes (which we do with `git add`) nor have we saved them (which we do with 
`git commit`).
So let's do that now. It is good practice to always review our changes before 
saving them. We do this using `git diff`.
This shows us the differences between the current state of the file and the 
most recently saved version:

~~~ {.bash}
$ git diff
~~~
~~~ {.output}
diff --git a/_config.yml b/_config.yml
index 494da8c..3c18501 100644
--- a/_config.yml
+++ b/_config.yml
@@ -3,7 +3,7 @@
 #

 # Name of your site (displayed in the header)
-name: Your Name
+name: Bruno Grande

 # Short bio or description (displayed in the header)
 description: Web Developer from Somewhere
~~~

The output is cryptic because it is actually a series of commands for tools 
like editors and `patch` telling them how to reconstruct one file given the 
other.
If we break it down into pieces:

1.  The first line tells us that Git is producing output similar to the Unix 
`diff` command comparing the old and new versions of the file.
2.  The second line tells exactly which versions of the file Git is comparing; 
`494da8c` and `36bbe2a` are unique computer-generated labels for those 
versions.
3.  The third and fourth lines once again show the name of the file being 
changed.
4.  The remaining lines are the most interesting, they show us the actual 
differences and the lines on which they occur. In particular, the `-` markers 
in the first column show the lines that were removed and the `+` markers show 
the lines that were added. The pattern of a `-` line followed by a `+` sign is 
common; it typically indicates that a line has been changed while explicitly 
giving the original and updated versions. 

After reviewing our change, it's time to commit it:

~~~ {.bash}
$ git commit -m "Add name to configuration"
~~~
~~~ {.output}
On branch master
Changes not staged for commit:
	modified:   _config.yml

no changes added to commit
~~~

Whoops: Git won't commit because we didn't use `git add` first.
Let's fix that:

~~~ {.bash}
$ git add _config.yml
$ git commit -m "Add name to configuration"
~~~
~~~ {.output}
[master ce26584] Add name to configuration
 1 file changed, 1 insertion(+), 1 deletion(-)
~~~

Git insists that we add files to the set we want to commit before actually 
committing anything because we may not want to commit everything at once.
For example, suppose we're adding a few citations to our supervisor's work to 
our thesis.
We might want to commit those additions, and the corresponding addition to the 
bibliography, but *not* commit the work we're doing on the conclusion (which we 
haven't finished yet).

To allow for this, Git has a special *staging area* where it keeps track of 
things that have been added to the current 
[change set](reference.html#change-set) but not yet committed.

> ## Staging area {.callout}
> If you think of Git as taking snapshots of changes over the life of a 
project, `git add` specifies *what* will go in a snapshot (putting things in 
the staging area), and `git commit` then *actually takes* the snapshot, and 
makes a permanent record of it (as a commit).
> If you don't have anything staged when you type `git commit`, Git will prompt 
you to use `git commit -a` or `git commit --all`, which is kind of like 
gathering *everyone* for the picture!
> However, it's almost always better to explicitly add things to the staging 
area, because you might commit changes you forgot you made. (Going back to 
snapshots, you might get the extra with incomplete makeup walking on the stage 
for the snapshot because you used `-a`!)
> Try to stage things manually, or you might find yourself searching for "git 
undo commit" more than you would like!

![The Git Staging Area](fig/git-staging-area.svg)

Let's watch as our changes to a file move from our editor to the staging area 
and into long-term storage.
First, we'll add a quick personal description to our configuration. 

~~~ {.bash}
$ nano _config.yml
$ head mars.txt
~~~
~~~ {.output}
#
# This file contains configuration flags to customize your site
#

# Name of your site (displayed in the header)
name: Bruno Grande

# Short bio or description (displayed in the header)
description: Computational biologist in cancer genomics
~~~
~~~ {.bash}
$ git diff
~~~
~~~ {.output}
diff --git a/_config.yml b/_config.yml
index 3c18501..36bbe2a 100644
--- a/_config.yml
+++ b/_config.yml
@@ -6,7 +6,7 @@
 name: Bruno Grande

 # Short bio or description (displayed in the header)
-description: Web Developer from Somewhere
+description: Computational biologist in cancer genomics

 # URL of your avatar or profile pic (you could use your GitHub profile pic)
 avatar: https://raw.githubusercontent.com/barryclark/jekyll-now/master/images/jekyll-logo.png
~~~

So far, so good: we've added one line to the end of the file (shown with a `+` 
in the first column).
Now let's put that change in the staging area and see what `git diff` reports:

~~~ {.bash}
$ git add _config.yml
$ git diff
~~~

There is no output: as far as Git can tell, there's no difference between what 
it's been asked to save permanently and what's currently in the directory.
However, if we do this:

~~~ {.bash}
$ git diff --staged
~~~
~~~ {.output}
diff --git a/_config.yml b/_config.yml
index 3c18501..36bbe2a 100644
--- a/_config.yml
+++ b/_config.yml
@@ -6,7 +6,7 @@
 name: Bruno Grande

 # Short bio or description (displayed in the header)
-description: Web Developer from Somewhere
+description: Computational biologist in cancer genomics

 # URL of your avatar or profile pic (you could use your GitHub profile pic)
 avatar: https://raw.githubusercontent.com/barryclark/jekyll-now/master/images/jekyll-logo.png
~~~

it shows us the difference between the last committed change and what's in the 
staging area.
Let's save our changes:

~~~ {.bash}
$ git commit -m "Add personal description to configuration"
~~~
~~~ {.output}
[master 544e588] Add personal description to configuration
 1 file changed, 1 insertion(+), 1 deletion(-)
~~~

check our status:

~~~ {.bash}
$ git status
~~~
~~~ {.output}
On branch master
nothing to commit, working directory clean
~~~

and look at the history of what we've done so far:

~~~ {.bash}
$ git log
~~~
~~~ {.output}
commit 544e58833afa58d034f77d3b03cdbb3902331dfc
Author: Bruno Grande <bgrande@sfu.ca>
Date:   Sun Jan 24 17:09:38 2016 -0800

    Add personal description to configuration

commit ce265847f395040f61e8a8bf40c13ad1492e82c2
Author: Bruno Grande <bgrande@sfu.ca>
Date:   Sun Jan 24 17:03:37 2016 -0800

    Add name to configuration

commit f570573f86fd615208d5900358fa06dd563a9402
Author: Bruno Grande <bgrande@sfu.ca>
Date:   Sun Jan 24 15:27:36 2016 -0800

    Add template files
~~~

To recap, when we want to add changes to our repository, we first need to add 
the changed files to the staging area (`git add`) and then commit the staged 
changes to the repository (`git commit`):

![The Git Commit Workflow](fig/git-committing.svg)

> ## Choosing a commit message {.challenge}
>
> Which of the following commit messages would be most appropriate for the 
> last commit made to `_config.yml`?
> 
> 1. 
>
>     "Changes"
> 2. 
>
>     "Changed line to 'Computational biologist in cancer genomics' in _config.yml"
> 3. 
>
>     "Add personal description to configuration"

> ## Committing Changes to Git {.challenge}
>
> Which command(s) below would save the changes of `myfile.txt` to my local Git repository?
>
> 1. 
>
>     ~~~
>     $ git commit -m "my recent changes"
>     ~~~
> 2. 
>
>     ~~~
>     $ git init myfile.txt
>     $ git commit -m "my recent changes"
>     ~~~
> 3. 
>
>     ~~~
>     $ git add myfile.txt
>     $ git commit -m "my recent changes"
>     ~~~
> 4. 
>
>     ~~~
>     $ git commit -m myfile.txt "my recent changes"
>     ~~~

> ## `bio` Repository {.challenge}
>
> Create a new Git repository on your computer called `bio`.
> Write a three-line biography for yourself in a file called `me.txt`, commit 
> your changes, then modify one line, add a fourth line, and display the 
> differences between its updated state and its original state.

> ## Author and Committer {.challenge}
>
> For each of the commits you have done, Git stored your name twice.
> You are named as the author and as the committer. You can observe
> that by telling Git to show you more information about your last
> commits:
>
> ~~~
> $ git log --format=full
> ~~~
>
> When commiting you can name someone else as the author:
>
> ~~~
> $ git commit --author="Vlad Dracula <vlad@tran.sylvan.ia>"
> ~~~
>
> Create a new repository and create two commits: one without the
> `--author` option and one by naming a colleague of yours as the
> author. Run `git log` and `git log --format=full`. Think about ways
> how that can allow you to collaborate with your colleagues.

[commit-messages]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
