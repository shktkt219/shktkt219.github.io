---
layout: post
title:  "Note about Git Command"
date:   2017-01-23 02:10:40 +0000
---


When I get started on a project, I always check how to make a new directory and transform the directory into a Git-enabled directory. That's why I'm writing this post.

# Create a new directory
The first step is to make a new directory on my computer in the development directory. To do so in the terminal, type like this.

![](http://i.imgur.com/gV0NleT.png)

I created a new directory with `mkdir test-app` and then moved into it by changing my directory with `cd test-app`. I typed `ls` to see all the files and folders in my new project, and now it was empty.

# Git init
The first step is to transform this directory into Github, I should do it as `git init`.

`git init` initializes a new Git repository in my current directory. Type `git init` from within my `test-app` project directory. After entering `git init`, the output in my terminal like this.

![](http://i.imgur.com/WFd2WIw.png)

At this point, there's nothing for Git to keep track of. I'll have to create some files and folders in my project first.

# Git status
I'll start my project by creating a `README.md`. Make my new file by typing `touch README.md` from within the `test-app` directory. When I type `ls`, I will see a new file has been created.

![](http://i.imgur.com/SikKsHE.png)

Now I've created my first project file, so I try to have Git start tracking changes. Type `git status`, I can find the current status.

![](http://i.imgur.com/Q9lgIbo.png)

Git is telling me that this is a brand new repository and right now Git isn't keeping track of any of the files.

# Git add, Git commit
I'm good at typing `git add` and `git commit`. So I will omit this part.

![](http://i.imgur.com/97nJ5IW.png)

# Creating a remote repository on Github
From my Github profile, click the "Create New Repository" button. After that, I should see a setup page. Type `git remote add origin https://github.com/shktkt219/test-app.git`, `git push -u origin master`.

![](http://i.imgur.com/irQoWEy.png)

"Origin" is simply the default alias assigned to my new remote repo.

# Git pull
When I collaborate with other people, inevitably they will push some code. So we must pull down the new code from the remote repo to our local with `git pull`. 

# Git branch
To keep master clean, when I want to start a new feature, I should do it in an isolated feature branch.

To make a new branch type: `git branch <branch name>`. In the case of a branch relating to a new feature, I'd name the branch `intro` like so: 

![](http://i.imgur.com/MSiis8S.png)

# Git checkout
I need to checkout or move into our `intro` timeline or branch. I can move between branches with `git checkout <branch name>`.

![](http://i.imgur.com/ZavMzON.png)

Let's make a commit in this intro and make a new file.

![](http://i.imgur.com/rAYqucN.png)

I can see the commit I made was made in the context of the `intro` branch.

I can create and checkout a new branch in one command using: `git checkout -b new-branch-name`. That will both create the branch `new-branch-name` and move into it by checking it out.

# Git checkout master
I can always move between branches with `git checkout`. Since I am currently on `intro`, I can move back to master with `git checkout master`.

![](http://i.imgur.com/Ad6iwY9.png)

# Git merge
I will try to bring the commit that occurred on the `intro` branch into the `master` so that at the end of the operation.

When I merge a branch with `git merge`, it's important to be currently working on my target branch, the branch I want to move into. The first step for my `intro` merge is to checkout `master`. Then type: `git merge <branch name>`. In this case, branch name is `intro`.

![](http://i.imgur.com/tws2cMn.png)

# Conclusion
I always stuck with `git checkout` and `git merge`. I used to build my project only in the master branch, but I will try to make and move to the different branch to make some new features for collaborating with other people.











