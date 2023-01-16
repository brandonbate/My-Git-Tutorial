# My Git Tutorial

### About Git
Git is a version control system. Think of a version control system
as a grand "undo" system. 
When working on a word processor, we can "undo" what we typed by essentially "going back in time" to a prior version of our document.
Programmers need this same functionality. But with programming, rather
than simply editting one file, we edit multiple files. Because of that,
it can be difficult to orchestrate all the "undos" we performed on our
various files. Version control systems help us manage this.

Git runs on your local machine and keeps a record
of the various edits you make to files in a project. In a word
processor, this record of changes is made for you automatically. With Git,
we have to manually specify "snapshots" of our files that
we can then "undo" back to if needed. These "snapshots" are called 
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
and **staged**. The above command automatically puts our untracked
file in the staged state.
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
The ```-a``` flag tells git to stage every tracked file. This command
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
end you will one of these approaches is the "winner take all". But in reality, you might find that
portions of each approach find there way into the final version. Consolidating these two separate
branches of your project then becomes a real challenge. Git allows us to easily explore multiple solutions
(without needlessly duplciating files) and enables
us to easily recombine appropriate code portions from each approach into a final solution.

When we create a Git repository, we have an accompanying **branch** created.
You might have already seen references to it.
When you run ```git status```, yo shouldu encounter the following text:
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
Any changes we make in this branch will not not appear in the others.
We can test this out by creating a quickly creating some files in the approach_a branch of the repository:
```
echo "Hello from A" > hello.txt
echo "Howdy from A" > howdy.txt
git add .
git commit -m "Added hello.txt"
```
Let's also create some files in approach_b branch of the repository:
```
git checkout approach_b
echo "Goodbye from B" > goodbye.txt
echo "Howdy from B" > howdy.txt
git add .
git commit -m "Added goodbye.txt"
```
Try navigating to different branches using ```git checkout```. When you list the directory contents with ```ls```, you will
find that ```hello.txt``` does not appear in the ```master``` and ```approach_b``` branches.
Likewise, ```goodbye.txt``` does not appear in the ```master``` and ```approach_a``` branches.
You will not find ```howdy.txt``` in the ```master``` branch,
but it will be in both the ```approach_a``` and ```approach_b``` branches, albeit different versions.

