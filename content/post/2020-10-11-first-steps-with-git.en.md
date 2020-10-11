---
title: "First steps with Git"
author: "Jakob Willforss"
date: '2020-10-11'
output:
  html_document:
    df_print: paged
Description: ''
categories: []
DisableComments: no
Tags: []
slug: first-steps-with-git
tags: []
Categories: []
---

# Introduction

This tutorial aims to get your feet wet using the Git terminal to manage your files locally and understand how to send these to GitHub.

To illustrate each step, I use the visualizations provided by the web page [Visualizing Git](http://git-school.github.io/visualizing-git/). Highly useful for understanding Git and what it does!

# Downloading Git

**NOTE: This is only required if you don't already have `git` installed on your computer. You can check this by running `git --version`. If installed properly, this should give you a version message.**

```{bash eval=F}
git --version 
```

```
> git version 2.17.1
```

If installed properly, proceed to `Creating a new repository`.

If not installed, navigate to [https://git-scm.com/](https://git-scm.com/) and find the download for the system you are using. Carry through the installation.

# Creating a new repository

When we start a new project, we start creating a new folder (`mkdir PythonHelloProject`). At this point, `Git` is not involved - we have only made a new folder in our file tree. 

We navigate into the folder, and then initialize a new repository using the command `git init`. This action creates a hidden folder `.git` inside the `PythonHelloProject`. Now `Git` will track what is happening within this folder.

```{bash results='hide', eval=FALSE}
mkdir PythonHelloProject
cd PythonHelloProject
git init
```

```
> Initialized empty Git repository in /home/jakob/PythonHelloProject/.git/
```

# Creating and adding your first two files

Here, we will go through the steps of adding new files to our repository. This is a three-step procedure:

1. Create a new file in the file tree
2. Add the newly created files to the stage, selecting those to include in the next commit
3. Commit the staged files, creating a snapshot where these files have been included

## Including the first file in the history

First, we create a Python hello-world script. You can open the file in any text editor and add the lines. The file is made from the terminal here for demonstration purposes.

```{bash include=FALSE, eval=FALSE}
echo '#!/usr/bin/env python3' > hello_python.py
echo "print('Hello World!!')" >> hello_python.py
```

![](/post/2020-10-11-first-steps-with-git.en_files/python_edit1.png)

We can inspect how the file looks. At this point, we have created a new file on the computer. Git is not involved yet.

```{bash eval=FALSE}
cat hello_python.py
```

```
> #!/usr/bin/env Python3
> print('Hello World!!')
```

We can always check with `git` what is the status of the repository. Here, we see that Git notices that there are new files added, which are not part of the current history.

```{bash eval=FALSE}
git status
```

```
> On branch master
>
> No commits yet
> 
> Untracked files:
>   (use "git add <file>..." to include in what will be committed)
> 
> 	hello_python.py
> 
> nothing added to commit but untracked files present (use "git add" to track)
```

We add this file to the `staging area` using the command `git add`. This command is used to select what file or files to be included in the next commit.

```{bash eval=FALSE}
git add hello_python.py
```

We are ready to create a commit and make new history. 

```{bash eval=FALSE}
git commit -m "first commit"
```

```
> [master (root-commit) ee89d19] first commit
>  1 file changed, 2 insertions(+)
>  create mode 100644 hello_python.py
```

At this point, our Git history looks the following way:

![](/post/2020-10-11-first-steps-with-git.en_files/2_git_commit1.png)

## Creating and including the second file

Secondly, we also include a README file.

We create a new file, either as the command below or through a text editor.

```{bash include=FALSE, eval=FALSE}
echo "This repository contains Hello-world scripts in different languages" > README.md
```

![](/post/2020-10-11-first-steps-with-git.en_files/readme_edit1.png)

Next, we add the second file to the stage, preparing it to be committed.

```{bash eval=FALSE}
git add README.md
```

We create a second commit, including this file in the Git history.

```{bash eval=FALSE}
git commit -m "Add README"
```

```
> [master 5724d25] Add README
> 1 file changed, 1 insertion(+)
> create mode 100644 README.md
```

The repository at this stage can be illustrated in the following way. Here, the `HEAD` points to the version we are looking into, and the `master` (or `main`) points to our main branch.

![](/post/2020-10-11-first-steps-with-git.en_files/3_git_commit2.png)

We can inspect the history at this point using the command `git log`. Compare the output to what is shown in the figure above.

```{bash eval=FALSE}
git log
```

```
> commit 5724d2592a13b32ca7256d8cd4cb31a275f9af22 (HEAD -> master)
> Author: Jakob Willforss <jakob.willforss@hotmail.com>
> Date:   Sun Oct 11 15:57:33 2020 +0200
> 
>     Add README
> 
> commit ee89d19ee6eab220f92f37f2016ca9cf34f7b08f
> Author: Jakob Willforss <jakob.willforss@hotmail.com>
> Date:   Sun Oct 11 15:56:59 2020 +0200
> 
>     first commit
```

# Updating existing files

Now we know how to add newly created files to the Git repository. Beyond this, we can also commit *modifications* to the history and the *removal* of files. Here, I will demonstrate how we can commit changes made to a file to the history.

We update the existing Python-script by appending a second line - saying "bye".

```{bash include=FALSE, eval=FALSE}
echo "print('That's all, bye!')" >> hello_python.py
```

![](/post/2020-10-11-first-steps-with-git.en_files/python_edit2.png)

As before, check the status.

```{bash eval=FALSE}
git status
```

```
> On branch master
> Changes not staged for commit:
>   (use "git add <file>..." to update what will be committed)
>   (use "git checkout -- <file>..." to discard changes in working directory)
> 
> 	modified:   hello_python.py
> 
> no changes added to commit (use "git add" and/or "git commit -a")
```

```{bash eval=FALSE}
git add hello_python.py
```

![](/post/2020-10-11-first-steps-with-git.en_files/4_git_commit3.png)

# Moving back and forth in history

One of the powerful features of Git is that we, at any point, can go back to any previous snapshots. By doing this, the `file tree` (the files we see on our computer) changes into the state the files were in at that point. This allows us to, for instance, explore how our scripts looked before introducing bugs.

By using the command `git log` we can see the history, and the commit IDs related to each step.

```{r eval=FALSE}
git log
```

```
> commit 5724d2592a13b32ca7256d8cd4cb31a275f9af22 (HEAD -> master)
> Author: Jakob Willforss <jakob.willforss@hotmail.com>
> Date:   Sun Oct 11 15:57:33 2020 +0200
> 
>     Add README
> 
> commit ee89d19ee6eab220f92f37f2016ca9cf34f7b08f
> Author: Jakob Willforss <jakob.willforss@hotmail.com>
> Date:   Sun Oct 11 15:56:59 2020 +0200
> 
>     first commit
```

Using the first part of the ID is one way to access that commit (for instance `5724d25`, normally a few letters is enough, but if using too little, for instance `572`, Git may not be able to uniquely identify the commit).

Next, you can checkout the previous commit.

```{bash eval=FALSE}
git checkout ee89d19e
```

```
> M	hello_python.py
> Previous HEAD position was 5724d25 Add README
> HEAD is now at ee89d19 first commit
```

![Git checkout](/post/2020-10-11-first-steps-with-git.en_files/5_git_checkout.png)

If we check the log from here, the history looks different. This is because each commit only is aware of what has happened before it.

```
> commit ee89d19ee6eab220f92f37f2016ca9cf34f7b08f (HEAD)
> Author: Jakob Willforss <jakob.willforss@hotmail.com>
> Date:   Sun Oct 11 15:56:59 2020 +0200
>
>    first commit
```

At any point, we can navigate back to the latest commit by using the command `git checkout` and specifying the head of the branch (either `master` or `main`).

```{bash eval=FALSE}
git checkout master
```

```
> M	hello_python.py
> Previous HEAD position was ee89d19 first commit
> Switched to branch 'master'
```

![Commit 3](/post/2020-10-11-first-steps-with-git.en_files/4_git_commit3.png)

# Sending a repository to GitHub

Setup a new repository on GitHub. Give it a name, and assign it as public. Don't add the README this time! (As you already have created one.)

![](/post/2020-10-11-first-steps-with-git.en_files/hellopython_repo.png)

You will come to a page similar to the following. Follow the instructions for 'push an existing repository from the command line'

![](/post/2020-10-11-first-steps-with-git.en_files/link_to_repo.png) 

Note that there are two protocols for linking GitHub repositories - https and ssh. Using ssh allows you to interact with GitHub without using a password every time, but requires you to set it up properly. Instructions for this are found [here](https://jdblischak.github.io/2014-09-18-chicago/novice/git/05-sshkeys.html). Using the https protocol works well, so don't worry about it.

Adapt the command below accordingly - you want to add your repository as the origin, not mine!

```{bash eval=FALSE}
# Replace "Jakob37" with your user and "HelloPython" with the name of your repository!
git remote add origin https://github.com/Jakob37/HelloPython.git
git branch -M main
git push -u origin main
```

# Concluding words

This tutorial only provides a teaser for the functionality of Git. By putting some more time into working through a workshop, you will have a powerful and useful tool at your disposal. I would strongly encourage you to start using Git in your next coding- or analysis-project, and gradually get used to it. When proficient, you will have the full benefit of Git with very little effort.

Some resources for further reading:

* My materials
* Free and interactive online tutorial to get started with Git: [link](https://www.codecademy.com/learn/learn-git)
* Non-free, but great and comprehensive tutorial at DataCamp: [link](https://www.datacamp.com/courses/introduction-to-git-for-data-science)
* For a deeper understanding of what goes on 'under the hood', I recommend this post: [link](https://www.sbf5.com/~cduan/technical/git)








