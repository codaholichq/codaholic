---
title: "Git Cheat Sheet"
date: 2023-06-25T09:23:09+0100
cover:
    image: img/git_cheat.webp
    alt: 'Git Cheat Sheet'
    caption: 'Git Cheat Sheet'
    
tags: ["git", "version control", "collaboration", "programming", "developer"]
categories: ["tech", "software development"]
---

### What is Git?

Git is a distributed version control system (VCS) used in software development to manage source code and track changes over time. It allows multiple developers to work on a project simultaneously and collaborate effectively.

At its core, Git helps developers track modifications made to files in a project. Instead of saving complete copies of each file for every change, Git focuses on capturing and storing the differences or "delta" between versions. This approach makes Git efficient in terms of storage and allows for faster operations.

Git operates on the concept of a repository, which is a collection of files and their complete history. Each developer has their own local copy of the repository, enabling them to work independently and commit changes to their local version. These commits create a timeline of the project's development, forming a complete history.

Git supports branching, allowing developers to create separate lines of development. Branches can be used to work on new features, bug fixes, or experiments without affecting the main codebase. Branches can be merged back into the main branch when the changes are complete.

Collaboration is a key aspect of Git. It enables developers to share their changes by pushing their local commits to a remote repository hosted on a server. Remote repositories, such as GitHub, GitLab, and Bitbucket act as centralized points for sharing and syncing code between team members. This allows developers to work together, review each other's changes, and resolve conflicts that may arise.

### Key benefits of Git include:

1. Version control: Git tracks changes to files, making it easy to revert to previous versions or investigate the history of the project.

2. Collaboration: Git facilitates effective collaboration among team members, allowing them to work simultaneously on different parts of a project.

3. Branching and merging: Git's branching and merging capabilities provide flexibility in managing parallel lines of development and integrating changes.

4. Speed and efficiency: Git's design enables fast and efficient operations, making it suitable for projects of any size.

Git is not the only version control system or tool out there, however, it is widely adopted in the software development due to its reliability, flexibility, and ability to handle both small and large-scale projects effectively.

Git comes with different commands, which can be overwhelming. Below are the list of commands that you will get to use more often.

**Set your Git username**
`git config --global user.name "Your Name"`

**Set your Git email address**
`git config --global user.email "youremail@example.com"`

Store login credentials in the cache so you don't have to type them in each time
`git config --global credential.helper cache`

**List all Git configuration settings**
`git config --list` or `git config -l`

**Creating a new repository**

Create a new local repository
`git init` 

Clone an existing repository from a remote source
`git clone [url]`

**Adding and committing changes**

Add changes to the staging area
`git add filename_here`

Add all changes to the staging area 
`git add .` 

Commit changes to the local repository with a message
`git commit -m "Commit message"` 

Commit changes and skip the staging area
`git commit -a -m"your commit message here"`
or
`git commit -am "your commit message here"`

Amend the most recent commit message
`git commit --amend "the correct commit message"`

Revert unstaged changes
`git restore filename`

Revert staged changes
`git restore --staged filename`

Rollback the last commit
`git revert HEAD`

Rollback to an old commit
`git revert commit_id`

**Working with branches**

List all branches
`git branch`

Create a new branch
`git branch [branch-name]`

 Switch to a specific branch
`git checkout [branch-name]`

Create a new branch and switch to it instantly
`git checkout -b [branch_name]`

Merge changes from a specific branch into the current branch
`git merge [branch-name]` 

Delete a branch
`git branch -d [branch_name]`

Push changes to a remote repository
`git push origin [branch-name]` 

**Working with remote repositories**

Add a remote repository
`git remote add origin [url]` 

 Fetch and merge changes from a remote repository
`git pull` 

Push changes to a remote repository
`git push` 

List all remote repositories
`git remote -v` 

Display all remote branch(es) being tracked
`git branch -r`

Remove a remote branch
`git push --delete origin [branch_name]`

**Other helpful commands**

`git status`: Show the status of your repository

`git log` : Show the commit history of your repository

`git diff [file]`: Show the changes made to a specific file.

`git push -f` This will forcefully push a git commit to remote repository

### Conclusion:

These git commands will improve your productivity as a developer. You don't have to memorize them – that's the reason for this cheat sheet. Please go ahead and bookmark this page or print this page out for future reference.