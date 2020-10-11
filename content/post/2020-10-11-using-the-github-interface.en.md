---
title: "Using the GitHub interface"
author: "Jakob Willforss"
date: '2020-10-11'
output:
  html_document:
    df_print: paged
Description: ''
categories: []
DisableComments: no
Tags: []
slug: using-the-github-interface
tags: []
Categories: []
---

# Introduction to using GitHub

This tutorial is an illustrated step-by-step guide on using GitHub to present your code or data analysis.

# Creating an account

If you have never used GitHub before, start creating a new account (picking your Username, Email and Password).

![github_login_screen](/post/2020-10-11-using-the-github-interface.en_files/login.png)

# Creating a new repository

Next, create a new repository. After logging in, you will find a green "New" button to the left. You can navigate to this GitHub main menu by clicking the logo in the upper left corner to get to this page.

![Creating new repository](/post/2020-10-11-using-the-github-interface.en_files/new_repo_marked_trim.png)

Give the new repository a name (any name that makes sense). You can give it a brief description to make it clearer what the project it will contain is about.

The repository can be either 'Public' or 'Private'. In this case - make it public. Public repositories will be visible to anyone in the world. Private repositories require a GitHub premium account (free for students) and are shown only to those you want to share it with. Overall, try to keep your repositories visible so that others can benefit from them!

Finally, you can optionally include the following files:

* A README - A file giving some description about the projects (for instance, installation instructions, and what problem it solves, in the case of software)
* A .gitignore file - When working with Git on your computer, this tells Git not to keep track of certain files that you don't want to be shared (for instance, log-files)
* A license - An important file showing other what they can and cannot do with your software

In this case, I chose only to include the README file.

![Settings for the new repository](/post/2020-10-11-using-the-github-interface.en_files/3_create_new_repo.png)

After clicking "Create repository" your first repo is created! For me, I came to a page looking like the following picture.

Note how the README uses the description I provided to give an overview of the repository's purpose.

You can view it yourself on GitHub: https://github.com/Jakob37/Python2020Demo

![New repo done](/post/2020-10-11-using-the-github-interface.en_files/4_new_repo_done.png)

# Using a repository

You now have a public repository on GitHub where you both can show off your work and access it from anywhere. Further, other people can interact with it, comment about issues and do pull requests.

Here, we will demonstrate some ways you can use the GitHub interface.

## Adding new files

You can upload existing files to the GitHub repository. You do this by clicking "Add file/Upload files", select the files to upload and write a commit message - a description of the changes you are making. This message will be part of the history you later can look through, so make sure to write something concise but understandable.

![](/post/2020-10-11-using-the-github-interface.en_files/11_upload_file.png)

Click the "Commit changes" button when done with uploading and writing the commit message.

![](/post/2020-10-11-using-the-github-interface.en_files/12_upload_file_2.png)

Now you should see an added file (in my case called "my_great_script.py").

![](/post/2020-10-11-using-the-github-interface.en_files/13_file_uploaded.png)

Try this out for your repository - Upload an existing file from your computer to your repository!

## Editing existing files

We can also edit existing files through the interface. By clicking the file in the main view, you will open a file specific view. Next, click the pen (on the right edge, next to the trashcan). This opens an in-browser editor. Here, you can make further edits to the file (in this case, I added another `print` statement).

![](/post/2020-10-11-using-the-github-interface.en_files/15_history_button.png)

![](/post/2020-10-11-using-the-github-interface.en_files/14_edit_file_from_gui.png)

Commit it similarly to you previously did when adding a new file. Navigate back to the file to make sure it looks OK!

## Investigating the history of a file

One of the key strengths of using a version control system is that you can access the history and see which changes the file has gone through. You can see this by clicking "History" (shown to the right in the file view, above the pen we previously used to edit it.).

In this case, our history consists of two commits - each representing a change made to the file. If you click it, you will see the exact changes made to the file in that commit.

![](/post/2020-10-11-using-the-github-interface.en_files/16_check_history.png)

![](/post/2020-10-11-using-the-github-interface.en_files/16_see_exact_changes.png)

## Making a pull request

If you want to propose changes to an existing repository, you do a pull request. Here, you can propose the addition of new files, or changes to existing files.

To do this, first navigate to an existing repository. Here, I went to one of Aaron's repositories "grollup" (you can use my course repository "Python2020Demo" found [here](https://github.com/Jakob37/Python2020Demo)).

![](/post/2020-10-11-using-the-github-interface.en_files/17_pull_request.png)

Next, I added a new file (by clicking "Add file / Create new file"). Then I made the content shown below.

![](/post/2020-10-11-using-the-github-interface.en_files/18_adding_file.png)

Finally, I wrote out a message similarly to when I did a commit, and clicked "Propose new file".

![](/post/2020-10-11-using-the-github-interface.en_files/19_pr_message.png)

You will come to a page showing exactly the changes made in your proposed pull request.

![](/post/2020-10-11-using-the-github-interface.en_files/20_pr.png)

After clicking "Create pull request", you will come to a page where the pull request can be discussed. Often this will end in a back-and-forth with the owner, and adjustments to the pull request. If and when both you and the owner are satisfied, the pull request can be accepted.

![](/post/2020-10-11-using-the-github-interface.en_files/21_pr.png)

Feel free to try out leaving an issue or creating a pull request on my repository: [https://github.com/Jakob37/Python2020Demo](https://github.com/Jakob37/Python2020Demo).

# Summary

This tutorial was a brief introduction to get going with GitHub. Further reading into Git (the system running beneath GitHub), I recommend you to check out the follow-up tutorial and compendia on the topic:

* [First steps with Git blogpost](/post/first-steps-with-git/)
* [Introduction to Git for Bioinformaticians (and others)](/post/introduction-to-git-for-bioinformaticians/)
