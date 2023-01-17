# My Git Tutorial

### About Git
Git is a version control system. 
I find it  helpful (and somewhat inaccurate) to think of a version control system as a grand "undo" system. 
When working on a word processor, we can "undo" what we typed by essentially "going back in time" to a prior version of our document.
Programmers need this same type functionality. But with programming, rather
than simply editing one file, we edit multiple files. Because of that,
it can be difficult to orchestrate all the "undos" we performed on our
various files. Version control systems help us manage this.

Git runs on your local machine and keeps a record
of the various edits you make to files in a project. In a word
processor, this record of edits is made for you automatically. With Git,
we have to manually specify "snapshots" of our files that
we can then "undo" if needed. These "snapshots" are called 
**commits**. The collection of commits we create for a project is called
a **repository**. With a repository, we can "go back in time" to a prior commit. 
All of these operations are typically done through a
command console.

Before getting into the structure of Git and the commands needed to use
it, I want to emphasize that Git and GitHub are fundamentally distinct.
Git is a version control system that simply runs on your local machine
(no internet connectivity required).
GitHub is an online service that allows you to share your repositories
with others and allows other users to view and (possibly) edit your repositories. Connecting
a local Git repository with a repository on GitHub can be tricky. We'll
cover the details of that later. For now, we're just going to focus on
using Git on our local machines (i.e. no internet).

### Creating a repository on a local machine

I assume you already have Git installed on your computer.
Once it is installed, you can then create a repository to store commits.
If we are working on a project in a folder called 
```my_folder```, we can put a Git repository for this project in this folder.
Navigate to this folder in your command console (using the ```cd``` command).
The following command then creates a repository in this folder:
```
git init .
```

### What Git tracks

Git classifies every file within ```my_folder``` (and its subfolders)
as either **tracked** or **untracked**. When we first create a
repository, all files are untracked. An untracked file
has no "snapshots" of it within the repository. In short, we cannot
click "undo" on untracked files. 
The following command tells Git to make ```my_file``` into a tracked file:
```
git add my_file
```
A tracked file can be in one of three states: **unmodified**, **modified**
or **staged**. The above command automatically makes ```my_file``` a tracked file
in the staged state.
Technically, we still don't yet have a snapshot of ```my_file``` in our
repository. Git wants to allow us to add more files to include in our
snapshot. Staged files are simply files that we intend to take a snapshot
of shortly. Once we are done staging/adding files, we can then take
a snapshot with the following command:
```
git commit -m "my message"
```
The above command takes a snapshot of all the staged files. All these
files remain tracked, but are now in the unmodified state.
The ```"my message"``` is simply a string of text that describes
the various changes performed. These messages can be handy for finding
a particular commit you want to revert back to or reference.

Whenever you edit a tracked file, it will automatically change to the
modified state. A tracked modified file is a file that is out of
sync with our latest commit. Beginners are encouraged to
never perform a commit when there are tracked modified files present
in your project because that means you are essentially not "backing up"
some of the edits you made. You can check if there are any tracked
modified files by running:
```
git status
```

Occassionally, you will encounter tracked files that Git apparently
thinks are both staged and modified. This peculiarity arises when
you perform ```git add my_file``` to make ```my_file``` staged, but
then edit ```my_file``` before performing the commit. When this happens,
Git assumes you want to commit the version of ```my_file``` you staged
BEFORE the edits. That version of the file is staged. The most recently
editted version of ```my_file``` will not be recorded in the next commit.
It's the modified version of the file. Again, beginners are encouraged
to never have files that are both staged and modified. You can remedy
this by "re-adding" the file before performing the commit.

### See what's changed

Often when you work on a project, you won't remember what files
you've changed. When that happens, you can use ```git status``` to see
what files are untracked or modified. But the output of ```git status```
can be a bit verbose. Moreover, it doesn't necessarily show you what
changes you have made. To view the files you've editted but have not
yet staged, run:
```
git diff
```
If instead you want to see the difference between what you've staged
and your latest commit, run
```
git diff --staged
```

### Ignoring files and easy adding/committing

When programming, we often generate files we'd rather not include in our
repository. Such files can include logs and executables (i.e. binaries).
Typically, we want our repository to contain only source code and
configuration files. We can tell Git to always ignore certain files
by including them in a ```.gitignore``` file. The ```.gitignore```
 file can consist
can consist of a simple listing of files or use wildcard expressions to
include whole families of files. You can even use regular expressions.
Once a ```.gitignore``` file is created or editted, we must stage and
commit it for changes to take effect.

Once we have a ```.gitignore``` in our repository, it's much easier
to stage and commit files in our project. The command
```
git add .
```
tells Git to stage ALL modified and untracked files in our current folder
(and its subfolders) EXCEPT those in .gitignore.
Then run ```git commit``` to perform the commit.

Some folks don't like having to enter in separate add and commit commands
for Git. They'd rather there just be one command.
Here's how you can do that:
```
git commit -a -m "my message"
```
The ```-a``` flag tells git to stage every modified tracked file. This command
works great when you want to update existing files tracked in your 
repository. But if you create a new (untracked) file, then the above
command will NOT add it to the repository. Your best bet in this instance
is to use the old-fashioned two step method.

### Removing a file from a repository

Sometimes we need to delete a file from our project. If we simply
delete the file from our local system, Git is going to notice its
disappearance.  But hope springs eternal with Git. It keeps hoping
against hope that, perhaps, one day that beloved file will return
to the repository. When you run ```git status``` you will see
Git mention that the deleted file has "changed". In short, the deleted
file is still "tracked". To let Git know that this relationship is
officially over, use the command
```
git rm my_file
```

### Branching

Complex projects often encounter problems where more than one solution comes to mind. When that happens,
we might want to explore these various solution options concurrently. You might do this by simply
duplicating your project on your local machine and having two different versions of your project, one
that explores approach A and another that explores approach B. This is a fine solution if in the
end one of these approaches is the "winner take all". But in reality, you might find that
portions of each approach find there way into the final version. Consolidating these two separate
branches of your project then becomes a real challenge. Git allows us to easily explore multiple solutions
(without needlessly duplciating files) and enables
us to easily recombine appropriate code portions from each approach into a final solution.

When we create a Git repository, we have an accompanying **branch** created.
You might have already seen references to it.
When you run ```git status```, you should encounter the following text:
```
On branch master
```
This tells us that our repository is on a branch called "master".
Every git repository has a ```master``` or ```main``` branch.

Suppose we want to explore two separate branches distinct from the master branch. We can do this
with the following commands:
```
git branch approach_a
git branch approach_b
```
Now we have three branches: ```master```, ```approach_a``` and ```approach_b```.
A quick look at the command prompt should show that we are still in the ```master``` branch.
The following command switches to the ```approach_a``` branch:
```
git checkout approach_a
```
Any changes we make in this branch will not not appear in the others (provided we perform a commit!).
We can test this out by creating a quickly creating some files in the approach_a branch of the repository:
```
echo "Hello from A" > hello.txt
echo "Howdy from A" > howdy.txt
git add .
git commit -m "Added hello.txt and howdy.txt"
```
Let's also create some files in approach_b branch of the repository:
```
git checkout approach_b
echo "Goodbye from B" > goodbye.txt
echo "Howdy from B" > howdy.txt
git add .
git commit -m "Added goodbye.txt and howdy.txt"
```
Try navigating to different branches using ```git checkout```. When you list the directory contents with ```ls```, you will
find that ```hello.txt``` does not appear in the ```master``` and ```approach_b``` branches.
Likewise, ```goodbye.txt``` does not appear in the ```master``` and ```approach_a``` branches.
You will not find ```howdy.txt``` in the ```master``` branch,
but it will be in both the ```approach_a``` and ```approach_b``` branches, albeit different versions.

### Merging branches
Multiple branches are useful when we are exploring multiple approaches concurrently, but our end goal is to merge
these branches together back into the ```master``` branch. The following merges ```approach_a``` into ```master```:
```
git checkout master
git merge approach_a
```
The files ```hello.txt``` and ```howdy.txt``` are now in ```master```.
We can then delete the ```approach_a``` branch since it is merged back with ```master```:
```
git branch -d approach_a
```
If we attempt to merge ```approach_b``` into ```master```, we will run into trouble:
```
git merge approach_b
```
The error message indicates that we are unable to merge ```howdy.txt``` into ```master``` because the version of ```howdy.txt``` already 
in ```master``` (which we merged from ```approach_a```) conflicts with the version of ```howdy.txt``` in ```approach_b```.
If we look at the contents of ```howdy.txt```, we see the following:
```
<<<<<<< HEAD
Howdy from A
=======
Howdy from B
>>>>>>> approach_b
```
Using a text editor, we modify the file (in the ```master``` branch) to the following:
```
Howdy from A and B
```
We then run
```
git add howdy.txt
git commit -m "resolving conflict"
```
to complete the merge of ```approach_b``` into ```master```.

### Remote repositories

Online services like GitHub allow users to create local machine copies of hosted repositories.
These hosted repositories are called **remotes**.
For instance, we can create a copy of this repository by running:
```
git clone https://github.com/brandonbate/My-Git-Tutorial.git
```
This will create a folder called ```My-Git-Tutorial``` within the working directory.
When we navigate to this folder, we'll find we are automatically in the ```main``` branch for this repository.
If we run ```git status```, we will see the following:
```
On branch main
Your branch is up to date with 'origin/main'.
```
This tells that on our local machine, we are the ```main``` branch of our copy of this repository and
that this ```main``` branch is "up to date" with ```origin/main```.
Git uses ```origin``` as a reference to the remote repository we cloned from.
We can see the actual web address for this repository by running
```
git remote -v
```

Over time, we should expect the remote repository (i.e. ```origin```) to mutate.
You'd think that running ```git status``` would tell us if our copy of the repository is incompatible with
the online version.
It doesn't. When it says our repository is "up to date" it simply means that, last time it checked, its up to date.
If you've been working on this repository locally the whole time, the "last time it checked" was when you cloned it.
That means that when we work with remote repository, we need to manually tell Git to check if our copy is up to date.
We do this by running
```
git fetch
```

Running ```git fetch``` will not change youre local machine's repository. Your files won't suddenly be updated.
But ```git fetch``` will get information on all the branches and commits on the remote repository.
With that information, you can update your local machine by merging your local machine's ```main``` branch with the
the ```origin/main``` branch (just updated by ```git fetch```):
```
git merge origin/main
```
Beware that when you call ```git merge origin/main```, you are merging the ```origin/main``` that existed on the
remote repository at the time you called ```git fetch```. If someone updates the remote repository in between the
time you call ```git fetch``` and ```git merge origin/main```, then your local machine's repository will not have
that most recent update.

As with adding and committing, some folks would prefer we have a single command for fetching and merging.
You can do this by running
```
git pull
```
If you are familiar with GitHub, you may have seen the term "Pull Request". That's something different that we'll get to shortly.

### Pushing

GitHub is one of the most popular services for hosting remote repositories.
If you haven't already, go to github.com and create an account.
Once you are logged in, you can create repository by clicking the "New" button (currenty green and located on the left).
After creating a repository, GitHub graciously provides you with commands to link this newly created remote repository
with your local computer. If you are starting a brand new project and have no files yet created, GitHub will
recommend you run something like this from an appropriate working directory:
```
echo "# Example" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/brandonbate/Example.git
git push -u origin main
```
If you already have files in your working directory you should instead create local repository as described in the prior sections
(use ```git init .```). After doing that, you can force the remote repository to update to your local machine version:
```
git remote add origin https://github.com/brandonbate/Example.git
git branch -M main
git push -u origin main
```

Both of these options introduce new commands. The ```git remote add origin https://github.com/brandonbate/Example.git``` adds
a link to the remote repository to our local machine repository. When we use ```git clone my_url```, Git automatically
perform ```git remote add origin my_url``` for us.
Another new command is ```git branch -M main```. This tells our local machine repository to rename the ```master``` branch to ```main```.
On local machines, the default branch is ```master```, but on Github its ```main```.
The command ```git push -u origin main``` moves the content in your local machine repository to the remote repository.
The ```-u``` flag tells Git to keep the ```main``` branch on our local machine linked to the ```main``` branch on the remote repository.

When performing a **push**, you will be prompted to enter in your GitHub credentials.
You cannot push to a remote repository unless you have permissions to do so.
When you create a repository, you automatically get permissions to push.
When you own a remote repository, you can allow other users to have permission to push to your remote repository.

### Forks and Pull Requests

This section is brief because it gives a conceptual overview of GitHub's workflow.
Imagine you are managing a large project with thousands of potential contributers.
In a situation like this, you do not want to allow all these potential contributers to have permission to push to your repository.
Instead, GitHub allows users to create a **fork** of your repository.
A fork essentially creates a copy of the target repository that the user can then edit.
The user can also keep their copy up to date with the target repository when it changes.

Imagine a user has added a new feature to a forked version of your repository.
That user can suggest you add their code to your repository by creating a **pull request** on GitHub.
This is distinct from using ```git pull```.
A pull request basically allows the owner of a repository to merge one of the contributer's branch into the ```main```
branch for the remote repository.

### Restoring

Inevitably, you will want to use Git to restore a file to a version from a previous commit.
How you do this will depend upon how far along you are past that point you want to restore to.
For instance, if you accidentally deleted ```my_file``` and haven't staged any files for a commit, you can quickly
restore to the version in the last commit by running:
```
git restore my_file
```
If you have already staged files but not yet committed, then you will need to run this command instead:
```
git restore --staged --worktree my_file
```
If you want to restore a file to a version on a past commit, you will need to look up the id for that commit.
There are a variety of ways to do this.
One option is to run:
```
git log
```
This will show all the commits on the repository. Another option is to run:
```
git log -- my_file
```
This will identify all commits where the file has been changed.
Once you have your commit id, you can restore a file from that commit with the following:
```
git checkout [insert commit id here] -- my_file
git commit -m 'restored my_file'
```
