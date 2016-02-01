---
layout: page
title: Version Control with Git
subtitle: Conflicts
minutes: 15
---
> ## Learning Objectives {.objectives}
>
> *   Explain what conflicts are and when they can occur.
> *   Resolve conflicts resulting from a merge.

As soon as people can work in parallel, it's likely someone's going to step on 
someone else's toes. 
This will even happen with a single person: if we are working on a piece of 
software on both our laptop and a server in the lab, we could make different 
changes to each copy. 
Version control helps us manage these
[conflicts](reference.html#conflicts) by giving us tools to
[resolve](reference.html#resolve) overlapping changes.

To see how we can resolve conflicts, we must first create one. 
Let's say the Owner wants to add a disclaimer at the bottom of the guest post
that the opinions expressed therein aren't representative of those held by the
blog owner. 
The Owner edits the blog post accordingly:

~~~ {.bash}
$ nano _posts/2016-02-03-Guest-Post.md
~~~

And then push the change to GitHub:

~~~ {.bash}
$ git add _posts/2016-02-03-Guest-Post.md
$ git commit -m "Add disclaimer about opinions"
~~~
~~~ {.output}
[master 5ae9631] Adding a line in our home copy
 1 file changed, 1 insertion(+)
~~~
~~~ {.bash}
$ git push origin master
~~~
~~~ {.output}
Counting objects: 7, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 436 bytes | 0 bytes/s, done.
Total 4 (delta 2), reused 0 (delta 0)
To https://github.com/<username>/<username>.github.io.git
   d28ec36..86cb171  master -> master
~~~

At the same time, the Collaborator wants to amend his piece by adding a source
at the end. 

~~~ {.bash}
$ nano _posts/2016-02-03-Guest-Post.md
~~~

We can commit the change locally:

~~~ {.bash}
$ git add _posts/2016-02-03-Guest-Post.md
$ git commit -m 'Add sources'
~~~
~~~ {.output}
[master b2212c6] Add sources
 1 file changed, 2 insertions(+)
~~~

but Git won't let us push it to GitHub:

~~~ {.bash}
$ git push origin master
~~~
~~~ {.output}
To https://github.com/<username>/<username>.github.io.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:brunogrande/my-blog.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
~~~

![The conflicting changes](fig/conflict.svg)

Git detects that the changes made in one copy overlap with those made in the 
other and stops us from trampling on our previous work.
What we have to do is pull the changes from GitHub,
[merge](reference.html#merge) them into the copy we're currently working in, 
and then push that.
Let's start by pulling:

~~~ {.bash}
$ git pull origin master
~~~
~~~ {.output}
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 4 (delta 2), reused 4 (delta 2), pack-reused 0
Unpacking objects: 100% (4/4), done.
From github.com/<username>/<username>.github.io
 * branch            master     -> FETCH_HEAD
   d28ec36..86cb171  master     -> origin/master
Auto-merging _posts/2016-02-03-Guest-Post.md
CONFLICT (content): Merge conflict in _posts/2016-02-03-Guest-Post.md
Automatic merge failed; fix conflicts and then commit the result.
~~~

`git pull` tells us there's a conflict, and marks that conflict in the affected 
file:

~~~ {.bash}
$ cat _posts/2016-02-03-Guest-Post.md
~~~
~~~ {.output}
---
layout: post
title: Guest Post!
---

This is a guest post!

<<<<<<< HEAD
I forgot to include my sources: google.com
=======
Opinions expressed therein aren't representative of those held by the
blog owner.
>>>>>>> 86cb171b60a13de1e56aa72443971b3c725f8ae4
~~~

Our change&mdash;the one in `HEAD`&mdash;is preceded by `<<<<<<<`.
Git has then inserted `=======` as a separator between the conflicting changes
and marked the end of the content downloaded from GitHub with `>>>>>>>`.
(The string of letters and digits after that marker identifies the commit we've 
just downloaded.)

It is now up to us to edit this file to remove these markers and reconcile the 
changes.
We can do anything we want: keep the change made in the local repository, keep
the change made in the remote repository, write something new to replace both,
or get rid of the change entirely.
Here, we will edit the file to include both changes, the Collaborator's 
amendment and the Owner's disclaimer, in this order. 

~~~ {.bash}
$ cat _posts/2016-02-03-Guest-Post.md
~~~
~~~ {.output}
---
layout: post
title: Guest Post!
---

This is a guest post!

I forgot to include my sources: google.com

Opinions expressed therein aren't representative of those held by the
blog owner.
~~~

To finish merging, we add `_posts/2016-02-03-Guest-Post.md` to the changes 
being made by the merge and then commit:

~~~ {.bash}
$ git add _posts/2016-02-03-Guest-Post.md
$ git status
~~~
~~~ {.output}
On branch master
Your branch and 'origin/master' have diverged,
and have 1 and 1 different commit each, respectively.
  (use "git pull" to merge the remote branch into yours)
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

	modified:   _posts/2016-02-03-Guest-Post.md
~~~
~~~ {.bash}
$ git commit -m "Merging changes from GitHub"
~~~
~~~ {.output}
[master f3b3c86] Merge changes from GitHub
~~~

Now we can push our changes to GitHub:

~~~ {.bash}
$ git push origin master
~~~
~~~ {.output}
Counting objects: 8, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (8/8), done.
Writing objects: 100% (8/8), 707 bytes | 0 bytes/s, done.
Total 8 (delta 6), reused 0 (delta 0)
To https://github.com/<username>/<username>.github.io.git
   86cb171..f3b3c86  master -> master
~~~

Git keeps track of what we've merged with what, so we don't have to fix things 
by hand again when the collaborator who made the first change pulls again:

~~~ {.bash}
$ git pull origin master
~~~
~~~ {.output}
remote: Counting objects: 8, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 8 (delta 6), reused 8 (delta 6), pack-reused 0
Unpacking objects: 100% (8/8), done.
From https://github.com/<username>/<username>.github.io
 * branch            master     -> FETCH_HEAD
   86cb171..f3b3c86  master     -> origin/master
Updating 86cb171..f3b3c86
Fast-forward
 _posts/2016-02-03-Guest-Post.md | 2 ++
 1 file changed, 2 insertions(+)
~~~

We get the merged file:

~~~ {.bash}
$ cat _posts/2016-02-03-Guest-Post.md
~~~
~~~ {.output}
---
layout: post
title: Guest Post!
---

This is a guest post!

I forgot to include my sources: google.com

Opinions expressed therein aren't representative of those held by the
blog owner.
~~~

We don't need to merge again because Git knows someone has already done that.

Version control's ability to merge conflicting changes is another reason users 
tend to divide their programs and papers into multiple files instead of storing 
everything in one large file.
There's another benefit too: whenever there are repeated conflicts in a 
particular file, the version control system is essentially trying to tell its 
users that they ought to clarify who's responsible for what, or find a way to 
divide the work up differently.

> ## Solving Conflicts that You Create {.challenge}
>
> Clone the repository created by your instructor.
> Add a new file to it,
> and modify an existing file (your instructor will tell you which one).
> When asked by your instructor,
> pull her changes from the repository to create a conflict,
> then resolve it.

> ## Conflicts on Non-textual files {.challenge}
>
> What does Git do
> when there is a conflict in an image or some other non-textual file
> that is stored in version control?
