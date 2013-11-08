Remote repository
-----------------

Log in to GitHub. If you don't see anything about macroevolution,
go to the website https://github.com/macroevolution/bamm-play.

Let's clone bamm:

    git clone https://github.com/macroevolution/bamm-play.git

This creates a directory called "bamm-play". Go to it and list the files.
You can compile the program, if you want.

You now have the complete repository of bamm. Look at its history:

    git log

By the way, to get a brief summary of the log, try:

    git log --pretty=oneline --abbrev-commit

The original repository is a remote repository, to which you can pull
or push updates to the code. Look at your remote repositories:

    git remote --verbose

As you can see, the name for the remote repository is called "origin."
It is actually a type of branch in your local repository.
You can view all your branches (include remote-tracking branches) with:

    git branch -a

Make a change to a source file, something like adding a comment.
I would like each of you to make a different change, preferably to
a different file. Commit your changes. Any changes you commit
are completely local, they are not in the remote repository.
To see this, compare the log files of the two repositories:

    git log
    git log origin

Note that "git log origin" reports the last changes you pulled,
not any changes made by others since you last pulled.

Now let one person push the changes you made to the remote repository:

    git push

This will do two things: (1) send your changes to the remote repository
and (2) update the origin branch with those changes.

First, let's go to GitHib and verify that those changes are there.
Second, let's see the last log entries in our local and remote branches:

    git log -1
    git log -1 origin

They are the same.

For those of you who didn't push, if you tried to push now
you will get an error message.

Because the remote repository has changed, you (another person)
can't easily push your changes because they could override
the changes made to the remote repository (you have an old copy).
What you need to do is update your local repository first.

    git pull

If there are any merge conflicts, you will have to resolve them now.
When done, push your changes:

    git push

Note that if you're working on large changes, you can pull any time
without pushing to keep your local copy up-to-date.
Think of it as preventing your copy from diverging too much;
this will make it easier to upload your changes later (fewer conflicts).


Remote branches
---------------

The remote repository may have branches, which you can check with:

    git branch -a

At the moment, you can't pull or push the contents of those branches
in your local repository. To do that, you must create a local branch
to track the remote branch (e.g., remote-branch):

    git checkout remote-branch

Now the branch is listed when you type:

    git branch

And when you push or pull, the contents of that branch will be updated
either in your local or remote repository.

When you create a new local branch or when you delete a local branch,
the change will not occur in the remote repository even if you do a push.
You need to specifically create or delete these branches from the repo.

For example, create a branch, make a change, commit the change,
then try to push. To create a remote branch:

    git push -u origin [branch-name]

Delete the remote branch (it won't be deleted locally):

    git push origin --delete [branch-name]


GitHub interface
----------------

At the top, below the description of bamm, there are several links,
one of which says "6 commits." Click on it.

Here you see the history of how the project has evolved through time.
Clicking on the title of any commit shows you the changes that were done.

On the upper-right hand corner of the screen, there's a button called "Fork."
This allows you to get a clone of the repository you're looking at
into your own account (right now the bamm repository is under macroevolution).
You don't need to do this for bamm because I've given you all pull & push
access to the repository, but for repositories you're not permitted to push,
you can clone the repository instead and make your own changes to it.

So let's say you've forked one of my projects and you make changes to it.
When you think you've done a good contribution to the project,
you then push your changes as a new branch into your forked copy
and then send me a "pull request," which tells me that you would like
me to merge your changes into my repository.

Before accepting your pull request, I can review the changes you've made,
make comments on your changes, and then accept or reject your request.
For example, I can comment on a specific line of code and ask about
why you chose a particular algorithm.

Another cool thing about GitHub is their Issues feature. This allows you
to comment on any bugs, features, or simply ask questions about the project.
New issues are "opened" and when resolved, they're "closed."
