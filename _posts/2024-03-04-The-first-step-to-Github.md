---
title: The first step to GitHub!
author: chulmin
date: 2024-03-04 00:00:00 -0500
categories: [HOW TO START GITHUB BLOG for beginners, Prerequisites]
tags: [GitHub, blog, webpage, Git, HTML, Hello World]
---

This article is written based on Windows 10

# What is **GitHub**?

Imagine you're preparing a final report for a class and you need to collaborate with your classmates. Most likely, you would create a new document on Google Docs or Microsoft Word Online, and share it with your group. These platforms provide live updates on document changes, and even allow you to restore previous versions of the report.

Now, let's say you're starting on a substantial project involving several other people. This is where **code hosting service** comes into play. **code hosting service** functions in a similar way to Google Docs, but it's specifically designed for programmers. It provides a platform where developers can collaborate on software projects, essentially serving as a virtual workspace. Within this space, programmers can store their code, track modifications, and collectively contribute to the project. Moreover, **code hosting service** offers a standardized method for publishing code to the public, making it a valuable tool for open-source projects. There are some popular **code hosting services** such as **GitHub** and **Bitbucket**. In this series, we are using **GitHub** to construct a our personal website. 


# How to use **GitHub**?
## Step 1. Create a repository 
The first step in utilizing **GitHub** begins with creating an account. To initiate a new project, click the "New" button located on the right side of the page. Each project is stored within a repository, serving as a designated space for its files and resources. In our case, we'll be generating the code for our website within this new repository. In the subsequent step, it's crucial to name the repository as _username_.GitHub.io if your purpose is to constuct your personal website (in my case, it would be chulminator.GitHub.io). This name will serve as the address for your blog. Finally, ensure to check the box to add a README file. Additionally, for public visibility, the repository should be set to public, allowing others to view your website.


## Step 2. Download Git bash
What are **Git** and **GitHub**?
Professor ChatGPT said
"**GitHub** is a web-based platform that utilizes **Git** for version control. While **GitHub** is one of the most popular platforms for hosting **Git** repositories, it's important to note that **Git** itself is a separate tool that can be used independently of GitHub."

You can download the **Git** bash from their website, <http://git-scm.com>. 



## Step 3. Hello world with HTML
1. Go on the repository, _username_.GitHub.io, and click the green button labeled 'code'. 
2. Copy the _repository_URL_. 
3. Locate or create a proper directory in your local computer. 
4. Create index.html file and write "hello world" using a text editor such as notepad.
5. Launch Git bash.
6. Change your current directory to the one from 3. using '**_cd_**' command. **_cd .._** moves up to the parent folder and **_cd subfolder_** navigates to the _subfolder_ in your current directory.
7. Type **git clone _repository_URL_** on Git bash.
8. Type **git add .**
9. Type **git commit -m "Hello World"**
10. Type **git push**
11. Check "index.html" added in the repository on GitHub.
11. After approximately 5 to 10 mins, you wil find "Hello World" displayed on _username_.GitHub.io

## Conclusion

In this post, we established a new repository for our personal blog, downloaded it to our local computer, and then proceeded to enhance it by adding a new HTML file. Lastly, we uploaded the updated HTML file to our GitHub repository. Stay tuned for the next post, where I will delve into the Git commands utilized in this process.

