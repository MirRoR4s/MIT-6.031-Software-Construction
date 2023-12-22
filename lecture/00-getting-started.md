## Step 7: Learn the Git workflow

This section includes links to a Git tutor called **GitStream**. 

GitStream allows you to practice Git on your machine: for each exercise, you clone a GitStream repository, then follow the instructions on the web page. GitStream will give you feedback in both the terminal and on the web as you complete each exercise.

As you read: complete the GitStream exercises to practice using Git.

***GitStream will not work with multiple exercise pages open\*** at the same time.

Don’t open exercises in multiple tabs. If an exercise doesn’t work, please close all open GitStream pages and try again.

If you encounter a problem, please post on Piazza.

### What is Git?

Git is a version control system (VCS). The [*Pro Git* book](http://git-scm.com/book) describes what Git is used for:

> Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later. […] It allows you to revert selected files back to a previous state, revert the entire project back to a previous state, compare changes over time, see who last modified something that might be causing a problem, who introduced an issue and when, and more. Using a VCS also generally means that if you screw things up or lose files, you can easily recover.

Some of the most important Git concepts:

- **repository:** A folder containing all the files associated with a project (e.g., a 6.031 problem set or team project), as well as the entire history of *commits* to those files.
- **commit (or “revision”):** A snapshot of the files in a repository at a given point in time.
- **add (or “stage”):** Before changes to a file can be *committed* to a repository, the files in question must be *added* or *staged* (before each commit). This lets you commit changes to only certain files of your choosing at a time, but can also be a bit of a pain if you accidentally forget to add all the files you wanted to commit before committing.
- **clone:** Since Git is a “distributed” version control system, there is no concept of a centralized Git “server” that holds the latest official version of your code. Instead, developers “clone” remote repositories that contain files they want access to, and then commit to their local clones. Only when they *push* their local commits to the original remote repository are other developers able to see their changes.
- **push:** The act of sending your local commits to a remote repository. Again, until you add, commit, *and* push your changes, no one else can see them.
- **pull:** The act of retrieving commits made to a remote repository and writing them into your local repository. This is how you are able to see commits made by others after the time at which you made an initial clone.

### Cloning

You start working with Git repos in 6.031 by cloning a remote repository into a local repository on your computer.

To do this, open the terminal (use Git Bash on Windows) and use the `cd` command to change to the directory where you would like to store your code. Then run:

```
git clone *URI-of-remote-repo*
```

or

```
git clone *URI-of-remote-repo* *project-name*
```

Replace `URI-of-remote-repo` with the location of the remote repository, and replace `project-name` with the appropriate project name, like `ps0`. The result will be a new directory `project-name` with the contents of the repository. This is your local repository.

After you have cloned a repository, you should navigate into the repository on your command prompt using `cd`. This lets you run `git` commands on the repository.

*GitStream* → [Practice `git clone`](https://gitstream.mit.edu/?theGitGo)

Note that GitStream doesn’t keep track of whether you’ve already done this exercise. To see which GitStream exercises you’ve already done, look at [Omnivore](https://omni.mit.edu/6.031/sp21/user/classes/02-basic-java/gitstream-exercises-grade).

**Cloning problem sets**: for each problem set in 6.031, you will have a repository on github.mit.edu.

Initially this remote repository only contains some template code.

To start working on the problem set, you will *clone* that repository onto your machine.

As you complete each part of the problem set, you will *commit* your changes to the local repository and then *push* them to the remote repository.

When the time comes for grading your assignment, we will clone the remote repository and look at the last commit you made *and pushed there* before the deadline.

### Creating a commit

The basic building block of data in Git is called a “commit”. A commit represents some change to one or more files (or the creation or deletion of one or more files).

When you change a file or create a new file, that change is not part of the repository. Adding it takes two steps. First, run:

`git add file.txt` (where `file.txt` is the file you want to add)

You’ll either need to run that command from the same directory as the file, or include directory names in the file path.

This *stages* the file. Second, once you’ve staged all your changes, run:

```
git commit
```

This will pop up the editor for your *commit message*. When you save and close the editor, the commit will be created.

### Getting the status of your repository

Git has some nice commands for seeing the status of your repository.

The most important of these is `git status`. Run it any time to see which files Git sees have been modified and are still unstaged and which files have been modified and staged (so that if you `git commit` those changes will be included in the commit). Note that the same file might have both staged and unstaged changes, if you modified the file again after running `git add`.

When you have unstaged changes, you can see what the changes were (relative to the last commit) by running `git diff`. Note that this will *not* include changes that were staged (but not committed). You can see those by running `git diff --staged`.

### Pushing

After you’ve made some commits, you’ll want to push them to a remote repository. In 6.031, you should have only one remote repository to push to, called `origin`. To push to it, you run the command:

```
git push origin main
```

The `origin` in the command specifies that you’re pushing to the `origin` remote. By convention, that’s the remote repository you cloned from.

The `main` refers to the `main` branch, the default branch in our Git repositories. We won’t use branches other than `main` in 6.031. All our commits will be on `main`, and that’s the branch we want to push.

Once you run this, you will be prompted for your password and hopefully everything will push. You’ll get a line like this:

```
a67cc45..b4db9b0  main -> main
```

*GitStream* → [Practice the add-commit-push workflow](https://gitstream.mit.edu/?basicWorkflow)
*GitStream* → [Practice adding a new file](https://gitstream.mit.edu/?createPushNewFile)
*GitStream* → [Practice deleting a file](https://gitstream.mit.edu/?helloGoodbye)

### Merges

Sometimes, when you try to push, things will go wrong. You might get an output like this:

```
! [rejected]      main -> main (non-fast-forward)
```

What’s going on here is that Git won’t let you push to a repository unless all your commits come after all the ones already in the remote repository. If you get an error message like that, it means that there is a commit in your remote repository that you don’t have in your local one (on a project, probably because a teammate pushed before you did). If you find yourself in this situation, you have to pull first and then push.

### Pulling

To perform a pull, you should run `git pull`. When you run this, Git actually does two things:

1. It downloads the changes and stores them in its internal state. At this point, the repository doesn’t look any different, but it knows what the state of the remote repository is and what the state of your local repository is.
2. It incorporates the changes from the remote repository into the local repository by *merging*, described below.

### Merging

If you made some changes to your repository and you’re trying to incorporate the changes from another repository, you need to merge them together somehow. In terms of commits, what actually needs to happen is that you have to create a special *merge commit* that combines both changes. How this process actually happens depends on the changes.

If you’re lucky, then the changes you made and the changes that you downloaded from the remote repository don’t conflict. For example, maybe you changed one file and your project partner changed another. In this case, it’s safe to just include both changes. Similarly, maybe you changed different parts of the same file. In these cases, Git can do the merge automatically. When you run `git pull`, it will pop up an editor as if you were making a commit: this is the commit message of the merge commit that Git automatically generated. Once you save and close this editor, the merge commit will be made and you will have incorporated both changes. At this point, you can try to `git push` again.

### Merge conflicts

Sometimes, you’re not so lucky. If the changes you made and the changes you pulled edit the same part of the same file, Git won’t know how to resolve it. This is called a *merge conflict*. In this case, you will get an output that says `CONFLICT` in big letters. If you run `git status`, it will show the conflicting files with the label `Both modified`. You now have to edit these files and resolve them by hand.

First, open the files in Eclipse. The parts that are conflicted will be really obviously marked with obnoxious `<<<<<<<<<<<<<<<<<<`, `==================`, and `>>>>>>>>>>>>>>>>>>` lines. Everything between the `<<<<` and the `====` lines are the changes you made. Everything between the `====` and the `>>>>` lines are the changes you pulled in. It’s your job to figure out how to combine these. The answer will of course depend on the situation. Maybe one change logically supercedes the other, or maybe they can be merged somehow. You should edit the file to your satisfaction and remove the `<<<<`/`====`/`>>>>` markers when you’re done.

Once you have resolved all the conflicts (note that there can be several conflicting files, and also several conflicts per file), `git add` all the affected files and then `git commit`. You will have an opportunity to write the merge commit message (where you should describe how you did the merge). Now you should be able to push.

*GitStream* → [Practice resolving a merge conflict](https://gitstream.mit.edu/?mergeConflict)

Avoid merges and merge conflicts:

### *Pull before you start working*

Before you start working, **always `git pull`**. That way, you’ll be working from the latest version of your code, and you’ll be less likely to have to perform a merge later.

### Getting the history of the repository

You can see the list of all the commits in the repository (with their commit messages) using `git log`. You can see the last commit on the repository using `git show`. This will show you the commit message as well as all the modifications.

**Long output**: if `git log` or `git show` generate more output than fits on one page, you will see a colon (`:`) symbol at the bottom of the screen. You will not be able to type another command! Use the arrow keys to scroll up and down, and quit the output viewer by pressing `q`. (You can also press `h` for see the viewer’s other commands, but scrolling and quitting are the most important to know.)

**Commit IDs**: every Git commit has a unique ID, the hexadecimal numbers you see in `git log` or `git show`. The commit ID is a unique cryptographic hash of the contents of that commit. Every commit, not just within your repository but within the *universe* of all Git repositories, has a unique ID (with extremely high probability).

You can reference a commit by its ID (usually just by the first few characters). This is useful with a command like `git show`, where you can look at a particular commit rather than only the most recent one.

You will also see commits identified by ID in tools like [github.mit.edu](https://github.mit.edu/6031-sp21/) and [Didit](https://didit.mit.edu/6.031/sp21/).

### Reverting to previous versions

If you’d like to practice using the version history to undo a change:

*GitStream* → (optional exercise) [Practice `git log` and `git revert`](https://gitstream.mit.edu/?logRolling)

------

We will revisit Git and learn more about version control in future classes. If you’ve installed the software, set up Eclipse, and completed the GitStream exercises above, you’re ready to move on to [Problem Set 0](https://web.mit.edu/6.031/www/sp21/psets/ps0/).

### Have fun in 6.031!