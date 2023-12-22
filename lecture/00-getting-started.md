##  Step 7: Learn the Git workflow

什么是Git?

What is Git?

Git是一个版本控制系统(VCS)。 

Git is a version control system (VCS). 

Pro Git book 描述了Git的用途:

The Pro Git book describes what Git is used for:

版本控制是一种系统，它记录了一段时间内对一个或一组文件的更改，以便您以后可以检索特定的版本。 

Version control is a system that records changes to a file or set of files over time so that you can recall specific versions later. 

[…]它允许你将选定的文件恢复到以前的状态，将整个项目恢复到以前的状态，比较一段时间内的更改，查看谁最后修改了可能导致问题的内容，谁引入了问题，以及何时引入了问题，等等。  

[…] It allows you to revert selected files back to a previous state, revert the entire project back to a previous state, compare changes over time, see who last modified something that might be causing a problem, who introduced an issue and when, and more. 

使用 VCS 通常还意味着，如果搞砸了事情或丢失了文件，可以很容易地恢复。

Using a VCS also generally means that if you screw things up or lose files, you can easily recover.

一些最重要的 Git 概念:

Some of the most important Git concepts:

仓库(repository): 一个文件夹，包含与项目相关的所有文件(例如，6.031问题集或团队项目)，以及提交到这些文件的整个历史记录。

repository: A folder containing all the files associated with a project (e.g., a 6.031 problem set or team project), as well as the entire history of commits to those files.

提交(或“修订”): 存储库中的文件在给定时间点的快照。

commit (or “revision”): A snapshot of the files in a repository at a given point in time.

添加(或“暂存”):在将对文件的更改提交到存储库之前，必须将相关文件添加或暂存(在每次提交之前)。 

add (or “stage”): Before changes to a file can be committed to a repository, the files in question must be added or staged (before each commit). 

这允许你一次只提交你选择的某些文件的更改，但如果你在提交之前不小心忘记添加所有你想提交的文件，这可能会有点痛苦。

This lets you commit changes to only certain files of your choosing at a time, but can also be a bit of a pain if you accidentally forget to add all the files you wanted to commit before committing.

克隆:由于 Git 是一个“分布式”的版本控制系统，因此没有集中的Git“服务器”的概念来保存最新的正式版本的代码。 

clone: Since Git is a “distributed” version control system, there is no concept of a centralized Git “server” that holds the latest official version of your code. 

相反，开发人员“克隆”包含他们想要访问的文件的远程存储库，然后提交到他们的本地克隆。  

Instead, developers “clone” remote repositories that contain files they want access to, and then commit to their local clones. 

只有当他们将本地提交的代码推送到原来的远程仓库时，其他开发人员才能看到他们的更改。

Only when they push their local commits to the original remote repository are other developers able to see their changes.

推送(push):将本地提交发送到远程仓库的行为。 

push: The act of sending your local commits to a remote repository.

同样，在添加、提交和推送更改之前，其他人无法看到它们。

Again, until you add, commit, and push your changes, no one else can see them.

拉取(pull): 检索到远程仓库的提交，并将其写入本地仓库的操作。 

pull: The act of retrieving commits made to a remote repository and writing them into your local repository. 

这就是你如何能够在你首次克隆之后看到其他人提交的代码。  

This is how you are able to see commits made by others after the time at which you made an initial clone.

---

### Creating a commit


Git 中数据的基本构建块称为“提交”。 

The basic building block of data in Git is called a “commit”. 

提交表示对一个或多个文件的更改(或创建或删除一个或多个文件)。

A commit represents some change to one or more files (or the creation or deletion of one or more files).

当您更改文件或创建新文件时，该更改并不属于存储库的一部分。 

When you change a file or create a new file, that change is not part of the repository. 

添加它需要两步。  

Adding it takes two steps. 

首先,运行:

First, run:

`Git add file.txt` (file.txt 是你要添加的文件)
 git add file.txt (where file.txt is the file you want to add)

你需要在该文件所在的目录中运行该命令，或者在文件路径中包含目录名称。

You’ll either need to run that command from the same directory as the file, or include directory names in the file path.

这将暂存该文件。 

This stages the file. 

其次，在暂存所有更改后，运行:

Second, once you’ve staged all your changes, run:

git 提交

`git commit`

这将弹出提交消息的编辑器。 

This will pop up the editor for your commit message. 

当你保存并关闭编辑器时，提交将被创建。  

When you save and close the editor, the commit will be created.

---

### Getting the status of your repository

Git有一些很好的命令可以查看存储库的状态。

Git has some nice commands for seeing the status of your repository.

其中最重要的是 `git status`。 

The most important of these is `git status`. 

随时运行这个命令，看看哪些文件被修改了但仍未暂存，哪些文件被修改了并暂存(这样，如果你Git提交，这些修改就会包含在提交中)。  

Run it any time to see which files Git sees have been modified and are still unstaged and which files have been modified and staged (so that if you git commit those changes will be included in the commit). 

请注意，如果你在运行 `git add` 后再次修改了这个文件，那么这个文件可能同时存在已暂存的和未暂存的更改。

Note that the same file might have both staged and unstaged changes, if you modified the file again after running `git add`.

当你有未暂存的更改时，可以通过运行 `git diff` 查看(相对于上次提交)的更改。注意，这不包括已暂存(但未提交)的更改。 

When you have unstaged changes, you can see what the changes were (relative to the last commit) by running `git diff`. Note that this will not include changes that were staged (but not committed). 

你可以运行 `git diff --staged` 命令查看这些文件。  

You can see those by running `git diff --staged`.

---

### Merges

有时，当你试图推送时，事情会出错。 

Sometimes, when you try to push, things will go wrong. 

你可能会得到这样的输出:

You might get an output like this:

```
! [rejected]      main -> main (non-fast-forward)
```

这里的情况是，Git不允许你推送代码到仓库，除非你的所有提交都在远程仓库中已经存在的提交之后。 

What’s going on here is that Git won’t let you push to a repository unless all your commits come after all the ones already in the remote repository. 

如果出现这样的错误消息，说明远程仓库中有一个提交，而本地仓库中没有(在项目中，可能是因为有队友在你之前推送了代码)。  

If you get an error message like that, it means that there is a commit in your remote repository that you don’t have in your local one (on a project, probably because a teammate pushed before you did). 

如果你发现自己处于这种情况，你必须先拉取，再推送。  

If you find yourself in this situation, you have to pull first and then push.

---

### Merging

如果您对自己的存储库进行了一些更改，并试图包含来自另一个存储库的更改，则需要以某种方式将它们合并在一起。 

If you made some changes to your repository and you’re trying to incorporate the changes from another repository, you need to merge them together somehow. 

就提交而言，实际需要发生的是你必须创建一个特殊的合并提交来合并这两个更改。  

In terms of commits, what actually needs to happen is that you have to create a special merge commit that combines both changes. 

这个过程实际如何发生取决于这些变化。

How this process actually happens depends on the changes.

如果幸运的话，所做的更改和从远程仓库下载的更改不会冲突。 

If you’re lucky, then the changes you made and the changes that you downloaded from the remote repository don’t conflict. 

例如，可能您更改了一个文件，而您的项目合作伙伴更改了另一个文件。  

For example, maybe you changed one file and your project partner changed another. 

在这种情况下，只包含这两个更改是安全的。  

In this case, it’s safe to just include both changes. 

类似地，你可能修改了同一个文件的不同部分。  

Similarly, maybe you changed different parts of the same file. 

在这种情况下，Git可以自动进行合并。  In these cases, Git can do the merge automatically. 

当你运行 `git pull` 时，它会弹出一个编辑器，就像你在提交一样: 这是 git 自动生成的merge 提交的提交消息。  When you run git pull, it will pop up an editor as if you were making a commit: this is the commit message of the merge commit that Git automatically generated. 

保存并关闭此编辑器后，将进行 merge 提交，并将合并这两个更改。  

Once you save and close this editor, the merge commit will be made and you will have incorporated both changes. 

此时，你可以再次尝试 git push。  

At this point, you can try to git push again.

---

###  Merge conflicts

有时候，你就没那么幸运了。 

Sometimes, you’re not so lucky. 

如果你所做的更改和你所拉取的更改编辑了同一文件的相同部分，Git 将不知道如何解决它。  

If the changes you made and the changes you pulled edit the same part of the same file, Git won’t know how to resolve it. 

这被称为**合并冲突** 。  

This is called a *merge conflict*. 

在这种情况下，你会得到一个大写的`CONFLICT`输出。  

In this case, you will get an output that says `CONFLICT` in big letters. 

如果你运行`git status`，它会显示标签为`Both modified`的冲突文件。  

If you run `git status`, it will show the conflicting files with the label `Both modified`. 

您现在必须编辑这些文件并手动解析它们。  

You now have to edit these files and resolve them by hand.



First, open the files in Eclipse. 

The parts that are conflicted will be really obviously marked with obnoxious `<<<<<<<<<<<<<<<<<<`, `==================`, and `>>>>>>>>>>>>>>>>>>` lines. 

Everything between the `<<<<` and the `====` lines are the changes you made. 

Everything between the `====` and the `>>>>` lines are the changes you pulled in. 

It’s your job to figure out how to combine these. 

The answer will of course depend on the situation. 

Maybe one change logically supercedes the other, or maybe they can be merged somehow. 

You should edit the file to your satisfaction and remove the `<<<<`/`====`/`>>>>` markers when you’re done.

一旦你解决了所有的冲突(注意，可能有多个冲突的文件，每个文件也可能有多个冲突)，执行 `git add` 命令添加所有受影响的文件，然后执行`git commit`命令。 

Once you have resolved all the conflicts (note that there can be several conflicting files, and also several conflicts per file), `git add` all the affected files and then `git commit`. 

你将有机会编写合并提交消息(你应该描述你是如何进行合并的)。  You will have an opportunity to write the merge commit message (where you should describe how you did the merge). 

现在你应该能推了。  

Now you should be able to push.



避免合并和合并冲突:

Avoid merges and merge conflicts:

开始工作前先拉

Pull before you start working

在开始工作之前，请使用 git pull。 

Before you start working, always git pull. 

这样，你就可以从最新版本的代码开始工作，而且以后不太可能执行合并操作。  

That way, you’ll be working from the latest version of your code, and you’ll be less likely to have to perform a merge later.