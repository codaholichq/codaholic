+++
title = 'How to Delete Git Commit History'
slug = 'delete-git-commit-history'
date = 2023-08-30T16:08:25+01:00
description = 'Learn to erase Git commit history efficiently, ensuring clean repositories and preserving privacy with simple deletion techniques.'
draft = false
[cover]
image = 'img/00-delete-git-commit-history.webp'
alt = 'How to Delete Git Commit History'
caption = 'How to Delete Git Commit History'
+++

### Introduction

In [Git](/git-cheat-sheet), you can remove or change the past changes in your repository by deleting the commit history. This might be useful if you want to get rid of private info, eliminate unimportant commits, or start a fresh project without showing what happened before.

Remember that changing the past can make it harder for others to understand how things got here and work together on the project. There are different ways to do this, like using `git reset`, `git rebase`, or `git filter-branch`. 

Each method has its pros and cons, so it's important to know them before making any changes.
<br/>

### Reasons to Delete Your Commit History
**Sensitive Information:** If commits contain sensitive data, such as passwords or private keys, removing them is important to maintaining security.

**Clean Start:** Starting a new project or [open-source](https://github.com/codaholichq) contribution, it can be helpful to have a fresh beginning with no old problems or issues to deal with.

**Repository Cleanup:** As time goes by, the history of a repository can get messy with too many unnecessary or repeated commits. Cleaning up these unwanted commits helps keep the history short and easy to understand.

**Undo Mistakes:** If you made mistakes while working on your code, like accidentally committing something by mistake, you can fix them by deleting and re-writing the history.

**Reorganizing Commits:** Sometimes we make changes to our codebase by committing them one at a time. But sometimes it can be helpful to group those changes into smaller clusters so that everything makes sense and is easier to understand later on.

**Repository Size:** It's a good idea to clean up your Git repository by getting rid of big or unimportant files that are taking up space in its history.

**Security Compliance:** Sometimes, due to regulatory or legal obligations, it's important to erase or modify old commit messages.

**Forked Projects:** When creating a copy (fork) of an existing project on GitHub, some people prefer to start with a blank slate by removing the original repository's commit history.

Want to erase old commits from a branch? Here's how you can do it without sacrificing your most recent progress!

`Disclaimer:` Here are some Git commands designed for the terminal if you are on Mac or Linux or Command Prompt if you are on Windows. You need to ensure that you have Git installed. Lastly, don’t do this in a shared repository as it might break stuff for them.

![How to Delete Git Commit History](/img/01-delete-git-commit-history.webp)

To start, make sure you're in the main folder for your project and create a new branch.

```shell
git checkout --orphan new_branch
```

This creates a new, empty (“orphaned”) branch that is empty and doesn't have any changes made to it yet. Next, add all the files in your working directory:

```shell
git add -A
```

Next, commit all your changes.

```shell
git commit -am "initial commit"
```

Now that you've moved everything important from the old branch to the new one, it's okay to get rid of the old branch, e.g. the `main` branch:

```shell
git branch -D main
```

Next, rename the current branch (the one you just created) into `main`:

```shell
git branch -m main
```

And finally, update the remote repository using the forceful Git push.

```shell
git push --force-with-lease origin main
```

<br/>

`Note:` Don't use this command `git push --force`
It won't overwrite the commits of others just in case the branch has changed in the meantime

`Credits:` This post was inspired by this answer on [StackOverflow](https://stackoverflow.com/questions/13716658/how-to-delete-all-commit-history-in-github/26000395#26000395). We created this post so that our community members can quickly reference this post when the need arises.

<br/>

### Frequently Asked Questions

**Can Git history be deleted?**
If you accidentally commit sensitive information like a password or SSH key to a Git repository, there are ways to remove it from the history without compromising your privacy. Two tools that can help you achieve this are `git filter-repo` and `BFG Repo-Cleaner`, both of which are designed to clean up an existing Git repository by removing specific commits or files from its history.
<br/>

**How do you reset and remove history in Git?**
If you want to go back to an earlier version of your Git project, you can use the `--hard` flag when running `git reset`. This will restore the project to the state it was in at a specific point in time (as indicated by a particular commit) and erase any changes made since then, including those that haven't yet been committed.
<br/>

**How do I delete pushed commits?**
Want to get rid of some commit history on your Git repository? You have two options depending on whether the commits are close together at the top of your branch (like one after another). If they are, you can use `git reset`. Otherwise, it's better to do an "interactive rebase". Once you remove the unwanted commits on your local machine, you can send the new changes to the remote location by running `git push` with the `force` option.
<br/>

**How do I see a list of commits in Git?**
The easiest and most useful way to see the changes made to a Git repository is by using the `git log` command. When you run it without any additional options, Git will display a list of all the commits made in the repository in reverse chronological order (i.e., the most recent commit comes first).
<br/>

**How to clear all Git data?**
To get rid of a local Git repository on your computer, follow these easy steps:
1. Find the main folder where you stored your Git repository. This is usually located in the root directory of your project.
2. Take out everything inside this folder, including all the files and subfolders. This will empty the folder.
3. Next, locate the hidden `.git` folder within the root folder using either File Explorer or the command line. When you find it, delete it.
4. Run the `git status` command afterward to confirm that the Git repository has been successfully removed. If you see an error message saying "fatal: not a git repository," then you know the job is done!
