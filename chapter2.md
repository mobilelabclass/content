# Intro to Git & Github

[Git](https://git-scm.com/) is a free and open source distributed **version control system**.

In simpler terms, it is a way to save your code, make modifications safely without losing previous work, rollback changes, and collaborate with other people on a code base over time.

When you install Xcode (Xcode Command Line Tools) git is also installed.

[Github](https://github.com/) is a web-based hosting service for git. It's the largest host of source code in the world and the de facto standard for code sharing and storage. More or less every popular open source library in the world is stored on github.

**All of the example code for Mobile Lab is hosted on Github** at [github.com/mobilelaboratory](https://github.com/mobilelaboratory). You should create a github account if you don't already have one and store all of your own code there as well. If you would like the instructors to review your code, you will need to provide us with a Github link. Students can register for free [here](https://education.github.com/pack).

While we will cover some basics to get you started, please watch Dan Shiffman's video series [Git & Github for Poets](https://www.youtube.com/playlist?list=PLRqwX-V7Uu6ZF9C0YMKuns9sLDzK6zoiV) if you are brand new to git.

We also recommend that you spend some time with the official documentation at [git-scm.com/book/en/v2/](https://git-scm.com/book/en/v2/) or the [15 minute interactive tutorial](try.github.io) created by Code School.

---

## Xcode + Git + Github

Let's pick up from where we left off in [Chapter 1 - Hello Xcode](https://www.mobilelabclass.com/chapter1.html).

Now that we've created our first iOS app, we want to make sure that we don't lose our hard work. We will create a git repository, add and commit our code, and push our changes to a repository on Github.

Create a github account [here](https://services.github.com/on-demand/intro-to-github/create-github-account) if you have not already.

### Creating a new Github repository

The first time you log in, you may see a screen like this:

![alt text][github-start]

If this is the case, click the button with the text **Start a project**.

Otherwise, click the **New repository** button.

![alt text][github-new-repo]

You should see the following screen:

![alt text][github-create-repository]

* Enter **helloworld** under the the field **Repository name**
* Enter a description of our [Chapter 1 - Hello Xcode](https://www.mobilelabclass.com/chapter1.html) project, for example: "First Xcode project" or "Single View Application with a label".
* Choose whether to make your code public or private. We'll keep it public for this exercise. To make your repository private you must have a paid Github account.
* Make sure that the field labeled **Intialize this repository with a README** is **unchecked**
* Click create repository

You should now see the following:

![alt text][github-created-new-repo]

Make sure that the box labeled **HTTPS** is selected. Copy the URL from the adjacent box. This is the URL of your new repository on the Github servers!

### Github + Xcode

<!--
Extra credit:
try.github.io
-->

<!--
  Image references
-->

[github-start]: https://mobilelaboratory.s3.amazonaws.com/labs/2-git-github/github-start.jpg "Github start a new project"

[github-new-repo]:https://mobilelaboratory.s3.amazonaws.com/labs/2-git-github/github-new-repo-button.jpg "Github new repo button"

[github-create-repository]:https://mobilelaboratory.s3.amazonaws.com/labs/2-git-github/github-create-repository-form.jpg "Github create repository"

[github-created-new-repo]:https://mobilelaboratory.s3.amazonaws.com/labs/2-git-github/github-created-new-repo.jpg "Github create new repo success"

