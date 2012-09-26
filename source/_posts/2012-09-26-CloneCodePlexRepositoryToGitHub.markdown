---
layout: post
title: "Cloning a CodePlex Repository over to GitHub"
date: 2012-09-26 16:36
comments: true
categories: github codeplex
---
How do you clone a CodePlex Git repository over to GitHub?

Here we take a look at how to clone the[ Entity Framework Git repository](http://entityframework.codeplex.com/), which is available on CodePlex, over to GitHub. The goal is to be able to take new commits from the original CodePlex Git repository and update the repository being hosted on GitHub.

1. Create a repository on GitHub. 

    Do not initialise the repository with a readme file just create the repository. We are going to call this repository entityframework-sharpdevelop since this will be a port of the Entity Framework which will work with SharpDevelop.

2. Clone the CodePlex repository to your local machine.

    Using your favourite Git client clone the Entity Framework repository at CodePlex to your local machine.
    
        git clone https://git01.codeplex.com/entityframework entityframework

3. Configure Git Remotes

    Our goal is to have a setup similar to the following.

		git remote -v
		
		origin  git@github.com:mrward/entityframework-sharpdevelop.git (fetch)
		origin  git@github.com:mrward/entityframework-sharpdevelop.git (push)
		upstream        https://git01.codeplex.com/entityframework (fetch)
		upstream        https://git01.codeplex.com/entityframework (push)
    
    We want an upstream remote that points to the original Entity Framework repository on CodePlex. We also want an origin remote that points our GitHub repository
    
    First we create the upstream remote by renaming the origin remote. From inside the entityframework folder we run the following command.
    
        git remote rename origin upstream
    
    Now we add the new origin remote.
    
        git remote add origin git@github.com:mrward/entityframework-sharpdevelop.git
    
    Run **git remote -v** to check that the configuration is correct.
    
5. Push the code to GitHub

        git push -u origin master

6. Get the latest Entity Framework Commits into GitHub

    Now when you want to get the latest Entity Framework commits from CodePlex you can run the following commands to merge these commits into your local master branch.
   
        git fetch upstream
        git merge upstream/master

    Then you can push the changes up to GitHub.

