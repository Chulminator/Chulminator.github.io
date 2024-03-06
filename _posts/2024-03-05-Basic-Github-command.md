---
title: Basic GitHub command
author: chulmin
date: 2024-03-05 00:00:00 -0500
categories: [HOW TO START GITHUB BLOG for beginners, Prerequisites]
tags: [GitHub, command, clone, commit, push, pull]
---

## Imagine you are collaborating on a programming project with Chulminator 
___
## Chulminator has uploaded the code and shared a repository_URL. 
How would you download the repository to your local computer?

```bash
git clone repository_URL
```
{: file='Git Bash'} 
  <br/>
Before cloning, you should change your current directory to the desired directory using _cd_ command.

```bash
cd .. 
cd subfolder
```
{: file='Git Bash'}
___
## You have updated some parts of the code. 
Then you can update the repository by

```bash
git add [file]
git add .

git commit -m "[message]"

git push [remote] [branch]
```
{: file='Git Bash'}

This command adds the specified [file] for committing. If you replace [file] with a period (.) as in the second line, it will stage all changes. Once added, commit the files with the provided message. You can replace [message] with a brief note describing the changes included in the commit for the benefit of other collaborators and yourself in the future.

Finally, push the changes to the repository. This command pushes changes from your current [branch] branch to the specified [remote] repository. If [remote] is omitted, it defaults to "origin", and if [branch] is omitted, it defaults to "master".

Expanding on the concepts of [remote] and [branch], [remote] refers to a version of the project that facilitates tasks such as code backups and management across different environments like Windows and macOS. On the other hand, [branch] allows you to work on new features, bug fixes, or experiments without impacting the main codebase. Eventually, these changes are merged into the master branch.

___

## Chulminator keeps updating the project on repository.
 And you can update the repository on your local computer by

```bash
git pull [remote] [branch]
```
{: file='Git Bash'}

This command fetches changes from the [remote] repository and merges them into your current branch. If omitted, [remote] is origin and [branch] is master.

___

## You found a bug
you can create a new branch or switch to it by
```bash
git checkout -b [branch]
```
{: file='Git Bash'}
<br/>
After you finish debugging, you can merge the changes from [branch] into your current branch.

```bash
git merge [branch]
```
{: file='Git Bash'}
___
## You want to check the current status of Git

```bash
git status
```
{: file='Git Bash'}
This command shows the status of your current working tree. It will show you any changes in files, uncommitted changes, and which branch you're currently on.
<br/>
<br/>

```bash
git branch
```
{: file='Git Bash'}
This command lists all branches in your repository.
<br/>

___

## Conclusion

This post introduces the GitHub commands, which can be challenging to grasp initially. As you gain more experience collaborating with other programmers, you'll develop a better understanding of the system. For now, the commands we've covered should be more than enough, especially since you'll likely be working on the GitHub website independently.
