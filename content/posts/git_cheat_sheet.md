---
title: "Git Cheat Sheet"
date: 2023-05-22T21:35:36+01:00
draft: false
cover:
    image: img/git_cheat.webp
    alt: 'Git Cheat Sheet'
    caption: 'Git Cheat Sheet'
    
tags: ["version control", "git", "programming"]
categories: ["tech", "software development"]
---

# Setting up Git

**Set your Git username**
`git config --global user.name "Your Name"`

**Set your Git email address**
`git config --global user.email "youremail@example.com"`

Store login credentials in the cache so you don't have to type them in each time

`git config --global credential.helper cache`

**List all Git configuration settings**
`git config --list`

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

**Working with branches**

List all branches

`git branch`

Create a new branch

`git branch [branch-name]`

 Switch to a specific branch

`git checkout [branch-name]`

Merge changes from a specific branch into the current branch

`git merge [branch-name]` 

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

**Other helpful commands**

`git status`: Show the status of your repository

`git log` : Show the commit history of your repository

`git diff [file]`: Show the changes made to a specific file.