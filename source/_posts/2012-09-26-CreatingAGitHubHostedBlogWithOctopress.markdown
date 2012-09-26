---
layout: post
title: "Creating a GitHub hosted blog with Octopress"
date: 2012-09-26 13:55
comments: true
categories: octopress ruby github
---

[Octopress](http://octopress.org/) is a blogging framework created by Brandon Mathis. Octopress has great documentation on how to setup Octopress on a [GitHub hosted blog](http://pages.github.com/) which this guide is not trying to replace. The following guide is a simple overview of how to setup Octopress with Windows being used to create the blog posts and push them to GitHub.

##Prerequisites

1. You have GitHub account.
2. You have a Git client, such as Tortoise Git, installed.

##Software Versions

This guide is based on the using the following software:

* Octopress 2.0
* Windows 7 (client machine)
* Ruby 1.9.3

##How to setup a GitHub based blog with Octopress

1. Download and install Ruby 1.9.3 from [http://rubyinstaller.org/downloads/](http://rubyinstaller.org/downloads/)
2. Add ruby to your path

        PATH=c:\ruby193\bin

3. Download the [Ruby devkit](https://GitHub.com/oneclick/rubyinstaller/wiki/Development-Kit) from [http://rubyinstaller.org/downloads/](http://rubyinstaller.org/downloads/)

    Extract to c:\RubyDevKit

    Change into this folder and run

        ruby dk.rb init
        ruby dk.rb install

    If you do not install the Ruby devkit then you will get an error when running setting up Octopress and running gem in step 5): 

        Gem::InstallError: The 'posix-spawn' native gem requires installed build tools.

4. Clone octopress

        git://GitHub.com/imathis/octopress.git octopress

5. Setup Octopress

    Open a command prompt in the octopress folder. Run the following commands

        gem install bundler
        bundle install
        rake install
        rake generate
        rake preview
	
    Then open a browser and go to [http://localhost:4000](http://localhost:4000) to view your blog.

6. Create a GitHub repository for your blog

    See the [Octopress guide](http://octopress.org/docs/deploying/github/).

    Create a new repository in GitHub that has your GitHub username followed by .github.com (e.g. mrward.github.com).

7. Deploy your blog to GitHub

    From the octopress folder run

        rake setup_GitHub_pages
        rake generate
        rake deploy

    The deploy command will cause your blog to be pushed to the master branch of your GitHub repository. Your blog will then be published and accessible via http://username.github.com (where username is replaced with your GitHub username).

    Then you will need to commit your changes made in the source branch and push them to GitHub.

    You now have a repository on GitHub containing your blog. It will have two branches master and source. The master branch contains your blog web pages. The source branch contains the octopress files and your original blog posts. On your local machine the master branch will exist in the \_deploy folder. The octopress folder will be tracking the source branch.

8. Configure your blog

    You will want to configure various settings, such as google analytics, for your blog. The [documentation on the Octopress site](http://octopress.org/docs/configuring/) has the full details on how to configure everything.

9. Creating blog posts

    From the Octopress folder run:

        rake new_post["Name of your post"]

    This will generate a file in the source\\\_posts folder which you can edit.

    To generate your blog and preview it in the browser run

        rake generate
        rake preview

    The generate command will regenerate your blog web pages in the public folder. You will need to copy these files to the \_deploy folder. The final step is then to commit and push your changes to GitHub. You will want to commit the changes made in the Octopress folder (i.e. the source branch) and also your blog pages in the \_deploy folder (i.e. the master branch). You can use the rake deploy command to automate this copying of pages, committing and pushing your blog web pages to GitHub but if you want more control over the commit message then you can do this final set of steps yourself and commit the pages using your favourite Git client.
	