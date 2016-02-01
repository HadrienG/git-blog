---
layout: page
title: Version Control with Git
subtitle: Collaborating
minutes: 25
---
> ## Learning Objectives {.objectives}
>
> *  Collaborate pushing to a common repository.

For the next step, get into pairs.
One person will be the "Owner" (this is the person whose Github repository will 
be used to start the exercise) and the other person will be the "Collaborator" 
(this is the person who will be cloning the Owner's repository and making 
changes to it).
The collaborator will make a "guest blog post" on the owner's blog. 

> ## Practicing by yourself {.callout}
>
> If you're working through this lesson on your own, you can carry on by opening
> a second terminal window, and switching to another directory (e.g. `/tmp`).
> This window will represent your partner, working on another computer. You
> won't need to give anyone access on GitHub, because both 'partners' are you.

The Owner needs to give the Collaborator access.
On GitHub, click the settings button on the right,
then select Collaborators, and enter your partner's username.

![Adding collaborators on GitHub](fig/github-add-collaborators.png)

The Collaborator needs to work on this project locally. 
He or she should `cd` to another directory, such that `ls` doesn't show a 
`planets` folder, and then make a copy of the Owner's repository. 

~~~ {.bash}
$ cd
$ mkdir Collaborations
$ cd Collaborations
$ git clone https://github.com/<username>/<username>.github.io.git
~~~

Replace both instances of `<username>` with the Owner's GitHub username.

`git clone` creates a fresh local copy of a remote repository.

![After Creating Clone of Repository](fig/github-collaboration.svg)

The Collaborator can now make a change in his or her copy of the repository. 
In our case, the Collaborator will add a new post in `_posts`. 
You can start by duplicating the existing post:

~~~ {.bash}
$ cd <username>.github.io
$ cp _posts/2014-3-3-Hello-World.md _posts/2016-02-03-Guest-Post.md
$ nano _posts/2016-02-03-Guest-Post.md
~~~

Then, you can edit the title and the content. 
Leave the `layout` as `post`. 
After the Collaborator has edited their guest post, they should be familiar by
now with the process of committing the new file. 

~~~ {.bash}
$ git add _posts/2016-02-03-Guest-Post.md
$ git commit -m "Add guest post"
~~~
~~~ {.output}
[master d28ec36] Add guest post
 1 file changed, 6 insertions(+)
 create mode 100644 _posts/2016-02-03-Guest-Post.md
~~~

Then push the change to GitHub:

~~~ {.bash}
$ git push origin master
~~~
~~~ {.output}
Counting objects: 4, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 417 bytes | 0 bytes/s, done.
Total 4 (delta 1), reused 0 (delta 0)
To https://github.com/<username>/<username>.github.io.git
   0f6895f..d28ec36  master -> master
~~~

Note that we didn't have to create a remote called `origin`:
Git does this automatically when we clone a repository using the name "origin".
(This is why `origin` was a sensible choice earlier when we were setting up 
remotes by hand.)

We can now download changes into the original repository on our machine:

~~~ {.bash}
$ git pull origin master
~~~
~~~ {.output}
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (4/4), done.
From https://github.com/<username>/<username>.github.io
 * branch            master     -> FETCH_HEAD
   0f6895f..d28ec36  master     -> origin/master
Updating 0f6895f..d28ec36
Fast-forward
 _posts/2016-02-03-Guest-Post.md | 6 ++++++
 1 file changed, 6 insertions(+)
 create mode 100644 _posts/2016-02-03-Guest-Post.md
~~~

> ## Review changes {.challenge}
>
> The Owner push commits to the repository without giving any information
> to the Collaborator. How can the Collaborator find out what has changed with 
> command line? And on GitHub? 
> 
> ## Comment changes in GitHub {.challenge}
>
> The Collaborator has some questions about one line change made by the Owner and
> has some suggestions to propose. 
> 
> With GitHub, it is possible to comment the diff of a commit. Over the line of 
> code to comment, a blue comment icon appears to open a comment window. 
> 
> The Collaborator posts its comments and suggestions using GitHub interface.
