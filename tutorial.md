Introduction
------------

Version controls gives you the ability to keep track of changes made to
documents through time.  Git is a sophisticated, general-purpose version
control system that gives you a lot of power for managing your documents.  In
addition to tracking your documents' changes line-by-line, Git makes it easy to
work on the same project from different computers---either because the project
is collaborative or simply because you have multiple computers from which you
want to edit your work.

Git and other version control systems also have the concept of branches;
branches allow you to diverge from your main line of work, which is useful when
you want to explore new ideas or make changes to your project without changing
your main project.  Later when you're happy with the results of your branch,
you can incorporate those changes easily back into your main project; I'll show
you how to use this feature.

I've mentioned that Git keeps track of your documents, but what I mean by
document is any plain text file---whether it's source code, HTML files, or a
LaTeX manuscript.  You won't be able to track changes line-by-line for
non-plain text files, like images and Word documents because they don't have
lines of text; even though Word documents contain text, they also contain
non-text information about how to format the text.  So if you do your
manuscripts in Word or a non-plain text word processor, you'll likely use Git
mostly for source code.

Identify yourself
-----------------

Before we start using Git, let's identify ourselves to Git so that all our
revisions have an author.  This is especially important in collaborative
projects because we would like to keep track of who edited something.

    git config --global user.name "John Doe"
    git config --global user.email johndoe@example.com

To change your identity for a specific repository, omit the --global option.

Create a project and a file
---------------------------

Now we're ready to use git.  Let's start by creating a directory for our new
project; we'll call this directory "project-[INITIALS]".  For this project,
let's say that we need a program that would calculate the mean of the standard
input; let's create such a program in python.

    import sys

    sum = 0
    n = 0

    for num in sys.stdin:
      sum += float(num)
      n += 1

    print sum / n

And let's test it:

    for n in $(seq 20); do echo $n; done | python mean.py

Initialize git
--------------

We want to be able to keep track of changes to this project, which currently
has just one file.  To do that, we first initialize git to tell it that we're
interested in tracking this project:

    git init

All this did was to create an empty repository, but it won't automatically
track all of the files in the project.

Add file to repo
----------------
    
We must tell git which files we want to track; and we do this with git add:

    git add mean.py

We've just told git that we want to track this file.

Initial commit
--------------

We must tell git when to save a set of changes that we make (this is called a
"commit").  Because we've added a file to track, we now want to tell git to
save this change.

    git commit -m "Added initial file to project."

It tells you there that you've changed one file, in our case we've addeded it,
and added 10 lines of text.  We can view a history (or log) of our commits with:

    git log

This shows you the commit id (also called revision number), author, date, and
your comment.

The stage and committing
------------------------

(Switch person on terminal.) Let's make changes to our program and then commit
those. Let's add comments to our code.

  # Sum input values

At this point, the file we've changed and the file that's in the repository are
different.  We can check what those differences are with:

    git diff

You can see right here that these are the lines we've added (they have plus
signs in front of them); these lines without the plus signs are the same
between the files, but are there to show you where the changes are.  We can get
prettier output by telling git we want things in color:

    git config --global color.diff auto
    git diff

Now changes are easier to see.  We can get a less detailed report of what files
we've changed with:

    git status

This line tells us that we've modified mean.py; notice that this status is
under the headline "Changes not staged for commit." This means that we've made
changes to files, but we haven't "staged" them; that is, we haven't told git
that we want those changes stored the next time we tell git to commit.

We must add the files we've changed to the stage:

    git add mean.py

Now, let's do a status to see what git tells us:

    git status

Ok, now it tells us that the changes in mean.py will be commited next time we
run git commit; it also tells us how to unstage the file if we made a mistake.

When we first commited, we added a message inline, but if we want a long,
multi-line message, we can use a text editor to add the message.  First, we
specify which editor will be our default:

    git config --global core.editor vim

Instead of vim, you could use emacs or nano or whatever else; and because this
setting is global, you won't have to do this for your future projects.  We then
commit without specifying a message:

    git commit
      
And our editor opens up with the latest status commented out, and we add the
message at the top:

    Added a comment.

Then we save and exit; if you don't type in a message and exit, the commit is
cancelled, which is useful when you change your mind and don't want to commit
just yet.  Now status shows nothing:

    git status
    git diff

And we have a new entry in our history or log:

    git log

GitHub
------

Git allows you to control your project's history on your computer, but that's
not helpful if you'd like others to contribute to your project, or if you'd
like to work on your project from different computers.  GitHub is a place where
you can host your projects online, so that it can be accessible to anyone you
allow.  The concept is similar to Google Docs, where your documents are online
so that anyone you allow can access them and edit them.

Hopefully, you've all created a GitHub account.

Let's go to the github website: github.com.  Please log in to your account.
Let's create a new repository where we'll store our created project.  We'll
call it project, and give it a short description:

    Repository name: project-[INITIALS]
    Description:     Sample project

We have to make the project public for a free account; that's it! We're ready
to copy our files to our new repository. Just follow the steps givet to you.
Let's go back to the command-line, add a remote location for our repository:

    git remote add origin git@github.com:redcurry/project-[INITIALS].git

and push the files into the remote repository:

    git push origin master

Don't worry what origin and master are at the moment; we'll revisit those
later.  Now we can check our repository online and see the files that we've
added to it, along with all of the history of the project.

Pair up with another group. One group is going to edit the other group's
project. This will show you how easy it is to collaborate among people.

The other group will watch, for now. The group that is watching: tell the other
group how to get a copy of your repository. This information is on your GitHub
repository home page.

The group that is doing the editing: Go up one directory, and then make a copy
of this repository:

    git clone git@github.com:redcurry/project-[INITIALS].git

Notice you not only have the project file(s) but also its history:

    git log

Edit mean.py to make it more succinct:

  import sys

  nums = [float(num) for num in sys.stdin]
  print sum(nums) / len(nums)

Add and commit:

  git add mean.py
  git commit -m "Refactored program."

Check the log and notice your new change. Your changes have been applied
on GitHub. To do that, type:

  git push


Comparing different revisions
-----------------------------

If we want to know what changed between our older commit and our newest commit,
we use these revision numbers:

    git [older number] [newer number]

We don't even have to paste the entire number, just enough for them to be
unique.


**
Skip this if short on time

If we want to know the differences between every commit we've ever made, we
give the option -p to git log:

    git log -p

This gives us the commit message, followed by what changed from the previous
commit. Warning, if you have a long history with lots of changes, you'll have a
lot of output.  Quit the log by typing in 'q'.  More often, you just want to
know the history between the last few commits (like we did above), to do this
you can send in an option to log with a number:

    git log -3

This will output the last 3 commit messages; you can combine options, of course
(-p -3) There are a lot of other options to git log, like ways of changing the
log output format, showing various levels of detail of a commit, and even
drawing a graph showing your branch and merge history (which we'll talk about
later).
**

One useful option to git log is --relative-date, which instead of giving you
the date/time in which a commit was done, it shows you the date/time relative
to now (this is great if you have poor mental math like me):

    git log --relative-date

Another option is --since, which tells git to display the history only after a
specific date (absolute or relative); for example

    git log --since="50 minutes ago"

Screwing up a file
------------------

Let's say that we open up our mean.py to make some changes, and a cat walks
over our keyboard and accidently saves the file! Crap!  First, let's look at
the status

    git status

The status actually tells us how to undo those changes:

    git checkout -- mean.py

We open the file and... Yay... everything's good!  Now, be careful with this
command because if you had made important changes before your cat screwed them
up, this command would replace those important changes with the older version.

Branching
---------

Let's talk about a neat feature of many version control systems, but which git
makes very easy to use: branching.  Look at the output of git status:

    git status

Notice that it tells us we're on branch "master." This is our main branch of
work. But sometimes it's useful to be able to diverge from our main line of
work to experiment on some new code without messing up our main work.  For
example, let's say that we want to extend the functionality of our python
program to calculate the mean of numbers in a file.  We could start editing
mean.py and change some code, but say we're not sure yet what needs to be
changed, and we want to have the chance to play around with the code, while not
messing up mean.py.  We can create a new branch of work, where we can
experiment with code, even commit those changes and keep a local history of
those changes without even touching up our main branch.  To do create a new
branch, we say:

    git branch handle_file

A new branch has been created, we can check our branches with:

    git branch

Here you see our new branch and the master branch; the master branch has a star
next to it because we're still on it.  Let's switch to our new branch:

    git checkout handle_file
    git branch

Now we see the star next to handle_file.  Let's make some changes to mean.py:

    # Print the mean of the numbers given in a file

    import sys

    if len(sys.argv) == 1:
      print 'Error: No arguments given.'
      exit()

    sum = 0
    n = 0                                         # Added

    for num in open(sys.argv[1]):                 # Changed
      sum += float(num)
      n += 1                                      # Added

    print sum / n                                 # Changed

Add a data.txt file with numbers in it, and test the program.  It seems to
work, so let's commit those changes now.

    git commit -am 'Changed to handle numbers in a file."

I'm using the shorthand -a to tell git to automatically stage those files that
it knows about (those being tracked).  And let's look at our new log:

    git log

Notice that the commits we made in the main branch are there, that's because
it's a branch, not an independent root (if you will).  When we're happy about
the changes we've made, we can integrate (or merge) those changes with the main
branch.  We must first switch to our master branch, and notice that mean.py is
now how it used to be:

    git checkout master
    vim mean.py

Also notice that the data.txt file is there, even though we added it in the
handle_file branch; this is because we never told git to track this file, so
git doesn't do anything with it.  Now we merge the changes into master from the
handle_file branch:

    git merge handle_file

Let's verify that mean.py contains the changes we made in the handle_file
branch, and then we can just delete that branch:

    git branch -d handle_file

Merge conflicts
---------------

Merging can become a little more complicated when you make parallel changes on
the same line in different branches; for example, let's make a new branch and
change a line (we'll do it in a single step this time):

    git checkout -b new_change
***
    ...
    file_name = sys.argv[1]
    for num in open(file_name):
    ...

Test it, and then commit those changes:

    git commit -am "Created file_name variable."

Let's switch back to our master branch, and make changes to a line we changed
in our other branch:

    git checkout master
***
    ...
    for number in open(sys.argv[1]):
      sum += float(number)
    ...

Test it, and commit it:

    git commit -am "Changed variable name."

Now let's merge the changes we made in our new_change branch into our master
branch:

    git merge new_change

And it fails. Git can't easily merge the changes because we changed some of the
same lines differently.  We must resolve the conflict manually, but git helps
us by telling us where the conflicts are in each file.  Just open the file
mean.py; the stuff in between `<<<<<<<< HEAD` and `==========` is in the master
branch, and the stuff between `============` and `>>>>>>>>>>>> new_change` is
in the new_change branch.  Let's manually merge these changes:

    ...
    file_name = sys.argv[1]
    for number in open(file_name):
      sum += float(number)
    ...

To mark the file as resolved, we simply say:

    git add mean.py

And then we commit the merge and edit the message:

    git commit

Now let's look at our log to confirm that the merge is in our history:

    git log

As you can see, the latest log tells you what was marged, and includes the
history of the changes made in the other branch.  Finally, let's delete the old
branch:

    git branch -d new_change

Using branches
--------------

So the way you want to use branches is like this:

1. You come up with an idea you want to test
2. You make a branch and work on that idea (committing to it)
3. If you like the results, merge the new changes back into master

This way your master branch can be clean of possible changes that may end up
being thrown away.  In fact, some developers have strict development workflows,
where, for example, the master branch must always be stable; in other words, it
always works, and any possible new features or bug fixes must be worked on a
separate branch.  Once a new feature has been tested very well, it can be
merged back into the master branch.



Cloning
-------

Let's say you worked on this project at work, and now you need to go home, but
continue to work on a different computer.  This is the kind of situation that
version control makes easy to solve; just download the project using Git.
First, let's go up one directory and create a new one to simulate being on a
different computer.

    cd ..
    mkdir home-computer
    cd home-computer

Now let's download the project:

    git clone git@github.com:redcurry/project.git

This creates a new directory with our project in it:

    ls
    cd project
    ls

Notice that data.txt is not there because we never asked git to track it, so we
lost it forever.  We can now work on our project locally: make changes, commit
them, make more changes, commit those, etc.  All these commits will be kept
locally, just as if your local project was a new branch.  We'll edit mean.py
and commit the changes:

    ...
    print 'The mean is',
    print sum / n
***
    git commit -am 'Added a polite message.'

When we're done, we can update our remote repository:

    git push

The next day we go back to work and need to get those changes we made at home:

    cd ..
    cd project

First we fetch the changes that have been made:

    git fetch

Right now we can check the differences between what is in our current directory
and what we fetched:

    git diff origin/master

Here, "origin/master" indicates to look at the main branch of the remote
repository (which we just fetched).

Now we merge:

    git merge origin/master

Done.
