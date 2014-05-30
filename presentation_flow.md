# Git Presentation #

## Agenda ##

1. Objectives
2. Version Control Systems
  * Centralized VCS
  * Decentralized VCS
3. TFVC
4. Git
5. Using Git with GitHub 

## Objectives ##

* Provide an overview of Version Control Systems and the two basic flavors that they come in
* Give you the high and low points of using TFVC as it relates to my experience with it on my own team
* Illustrate how Git is different
* Show you the basics of how Git works
* Show you how GitHub uses Git and the basics of using GitHub
* Show you the GitHub GUI in action



*Disclaimer: I have experience with using both TFS and Git and am familiar with the basics of both. However, I am by no means an expert. My intention is to merely provide an introductory look and point you in the right direction to learn more*


## Version Control Systems ##

Modern Version Control Systems should allow you to do some important, specific things:

1. Concept of a project
  * define projects
  * define objects that are a part of that project
  * define who has access to those objects
2. keep track of historical changes to the objects within that project
3. make structural changes to the project
  * Add objects
  * Remove objects
  * Create an alternate version of objects (*branching*)
  * Combine alternate object versions into one object (*merging*)

There are two main ways modern Version Control Systems Work

### Centralized Version Control Systems ###

All of the work to keep track of files in a CVCS is **ONLY** done in one central place (i.e. the server). There is only ever one repository in a CVCS. Whether its 1, 10, or 1000  collaborators working on a project, there is only one repository.

**This implies a few important points about a CVCS**

  * To work on the project files, you need access to the centralized server. Since most of us arent working directly on the server hardware, this implies that we **need** network access to reach the server
  * Only "unlocked" files can be edited
  * All actions affecting the structure of the project, must be done on the server, using network and server resources. Also whatever actions are taken, immediately affect everyone with access to the server

### Decentralized Version Control Systems ###

All of the work to keep track of files in a DVCS **CAN** be done anywhere the repository exists. A repository can exist on any machine (server, "team box", my machine, your machine ... *anywhere*). There are as many copies of the repository as there are collaborators to the project

**This has a few implications too for DVCS'**

  * To work on the project files, you only need access to your local filesystem where the repository lives. You do **not need** network access to reach your repository
  * Any file can be worked on within your own copy of the repository. No one can "lock" a file in your repository
  * Actions affecting the structure of the project can be done cheaply (read: locally) and could conceivably not affect other copies of the repository if you dont want it to

### What does it all mean? ###

The workings of a CVCS should be pretty familiar to most. It follows a basic workflow of: **UNLOCK** --> **MODIFY** --> **LOCK**. This workflow is effective and is adequate in many contexts.

A well designed DVCS however is not nearly as intuituve. It is a different way of looking at development practices (not just Version Control) and can be very effective. But it also allows you to shoot yourself in the foot in far more ways than a CVCS does. 

*Lets look at some specific examples used here at and see how they compare*

## Team Foundation Version Control ##
 
  * TFVC is a CVCS, and is a component of the Team Foundation Server (TFS)
  * TFS != TFVC
    * TFS is an entire suite of Microsoft tools including a Build Server, Work Item Tracking, Code Analytics, and a Version Control System (TFVC), among others
    * The TFVC component is most comparable to Git as they are both Version Control tools

    ```
    TFVC is to TFS 
        Like 
    Git is To GitHub
    ```

### When TFVC is the right tool ###

  * Deep integration with Visual Studios and Windows
    * My team works in .NET so this is important
    
  * Centrally managed within our organization 
    * No one on my team worries about how the TFS server is running
    * There are dedicated resources devoted to housing TFS
    
  * Multiple versions of large (1MB - 5MB) binary files are not a problem for TFVC
    * All of the versions are kept on the central server
    * I only grab the version that is most interesting to me (so long as no one else is using it!)

### When TFVC may not be the right tool ###

  * More difficult to integrate with TFS/Visual Studio if you aren't actually developing in .NET 
    * Looking at you Java Developers :)
    * a platform agnostic tool may more suitable
  
  * It may be overkill to use the enterprise maintained TFVC for smaller projects
    * Provisioning RM's, Access Requests, etc...
    * Sometimes you want to get up and running quickly
  
  * Working Remotely
    * Much more of a problem with a CVCS. Remember, *EVERY* action needs to be done through the network on the server
    * Also, network/server issues do (rarely) occur even within the firewall
  
  * Rapid Development, Lots of changes, Need to be able to test and discard/promote features quickly
    * This type of development is becoming more prominent as we move to more agile practices
    * This type of development is done with easy *branching* and *merging*
    * TFVC is not known for easy *branching* and *merging*. My personal experience is that in its more trouble than its worth :( 

## Git ##

  * At it's core, git is a command line tool, but you dont need to use it that way if you dont want to

  * Just about every IDE, text editor, and development tool worth it's salt allows for integration with git in some way or another

  * If the command line is not your thing, there are also alot of different GUI clients that abstract away some of the complexity of git to make it easier to understand and use

  * Keep in mind that git does not enforce a specific development workflow like a other Version Control systems do, so in addition to thinking about version control, you need to also think about what type of development workflow will work best for your team

### What Git has going for it ###

  * Deeply integrated with just about everything, on just about every platform, including Visual Studios (Git for TFS) and Windows (TortoiseGit)

  * Can be as simple or complex as needed
    * Want to have a centrally managed repository that depends on verfified code check-ins to make changes... Git can do that
    * Want to work with a few developers pushing code back and forth to the repositories on each other's machines... Git can do that
    * Have a large project with alot of complexity and developers, needing separation of responsibilities, verified code reviews... Git can do that 
    * Want to treat Git just like CVCS to ease the transition... Git can do that

  * Once you have the repository on your machine... whether you've *cloned* it or created it from scratch, you no longer need access to the network to work on your code

  * Git encourages *branching* and *merging* frequently. When these actions are being done locally, it turns out that it is alot easier to manage. 
    * Want to make some drastic changes for a new feature... create a *branch* and work it on all you want
    * Once the new feature is finished and your sure it works the way you expect it to *merge* it into the main branch of code, and share the changes with your team

  * Git can be self documenting
    * Due in part to the *staging area* and the ability to reorganize commits or *rebasing* (more on this later), the timeline of changes to the repository is often used as changelog documentation
    * Descriptive branch names self-document what you are working on while merges define integration points, as well as provide easy rollback points
    * Clearly defining Development, Staging, and Production *branches* as well as *tagging* specific points in the history, provide nicely documented release points

### Pitfalls of Git ###

  * Git shines when working with code, and other text based files. With binary files, there are a few things to watch out for:
    * While branching and merging of binary files are still handled with Git, you lose the ability to check the diff's between binary file versions
    * The more large binary files you have in a repository, the more data needs to be downloaded everytime a repository is shared from one machine to another. This can lead to bloat and is subject to network issues as the repository becomes large. This is single biggest drawback to using git over a TFVC

  * The ability to have flexible workflows in Git does not mean different workflows. Everyone needs to agree on a convention and stick to it

  * A cloned repository is not necessarily a back up
    * A backup process should be implemented, but do not consider a peer's repository as a backup

## Using Git and GitHub ##

### Using Git ###

Remember that Git is a command line tool first. Whether or not you use Git on the command line, it is important to understand some basics about how Git works behind the scenes. I will illustrate some of these basics, as well as the command line commands that make them work.

#### The basics ####

A Copy of a repository is called a *clone*. Cloning a repository copies the entire repository from a *remote* location and recreates it on your machine. 
```
  git clone git://github.com/jonasatwork/jonasatwork.github.io.git
```

You always have the ability to work on the *cloned* repository locally, making as many changes as you like. You also will always be able to synchronize your *cloned* repository with the most recent changes from the *remote* repository.
```
  git pull origin
```

<em>push</em>ing your changes back to the *remote* repository depends on your workflow and how the *remote* is setup. You will be able to either:

  * *push* your local changes to the *remote* repository you originally cloned from
  ```
    git push origin master
  ```

  * submit your changes to the repository owner as a *patch* so that they may implement your changes (more on this later...)

Git is all about saving *snapshots* of repository objects. Git keeps track of the tree that connects one *snapshot* to another by their <em>diff</em>erences. *Snapshots* are identified by a hash (SHA-1 to be exact). The important things to note is that the hash allows a specific *snapshot* to be addressed and used later; and even though the hash is 40 characters long, it is usually enough to identify it with only the first 7 characters.

A *branch* is one particular line in that tree. We *checkout* a particular *branch* to make it active. When a *branch* is active all changes made to files are compared to the most recent *snapshot* in that *branch*
```
  git checkout awesomeNewFeature
```

As changes are made, Git is aware of modifications done to files, when a file is added, and when a file is removed. It is up to you to decide what changes get grouped together into a *commit*. You group these changes together by <em>add</em>ing them to the *staging area*.
```
  git add index.html css/main.css
```

Once all the changes you want are in the *staging area* it's time to *commit* these changes. A *commit* creates a new *snapshot* consisting of the items in the *staging area*, and adds that snapshot to the end of the current branch. 

Though not strictly required, it is important to add a *commit message* to document the changes that are being added.
```
  git commit -m "adjusting element width and color on the main page"
```

After working on a *branch* for a while, you may get to the point where you are ready to merge the *commits* you've made into another *branch*. This is accomplished with a *merge*. We need to tell Git to move to the *branch* we are merging **to**, and perform the *merge* action on the *branch* the changes are coming **from**.  
```
  git checkout master
  git merge awesomeNewFeature
```

### Using GitHub ###

GitHub is a great way to use Git. It adds some great features on top of Git like: 

  * Work Item and Bug Tracking
  * Code Analytics
  * Social Coding
  * Integration with just about everything
  * Convenient GUI's for the WEB and your machine

More importantly, Disney has an DTSS Enterprise version of GitHub available at [github.disney.com](https://github.disney.com). All of the GUI tools available from [github.com](https://github.com) work with the Enterprise version. 

Think of GitHub as a cloud hosted Git repository that makes it easy to collaborate on a project as well as organize your documentation and code. To get staretd with GitHub, you need to create a repository through the GitHub UI (Web or Desktop), then *clone* it to your local machine. 

GitHub takes some of the basic concepts we discussed earlier and expands on them by formalizing user security, introducing code review, and enforcing basic workflows.

#### User Security ####

Grouped under your GitHub user ID (whether you use the enterprise LDAP user ID, or the GitHub user ID) is all of the repositories that you have asked GitHub to store for you. Remember that in a DVCS like Git, any machine can have a full copy of the repository, so a GitHub server is simply another spot to keep repositories that are meaningful to you. 

In addition to creating your own repositories, GitHub allows you to *fork* someone else's repository. *Forking* is simply the process of *cloning* another user's repository to your own list of GitHub repositories. For all intents and purposes it is like any other repository in your list except that GitHub keeps track of which user it was *cloned* from.

The public GitHub site makes all repositories you own accessible for *cloning* to anyone else by default. To make a *private repository*, github.com requires a paid account. Thankfully, the Disney Enterprise github.disney.com allows *private repositories* without any additional configuration. A *private repository* allows you as the repository owner to define which users can *clone*, *pull*, *push*, *merge* or even see the repository. 

#### Pull Requests ####

GitHub has a concept of a *pull request*. This is a formal way to submit changes to a particular repository. The workflow generally goes something like this:

  1. *Fork* a repository from another GitHub user
  2. *Clone* your GitHub copy of that repository to your local machine
  3. *Commit* new changes (often times in a new *branch*)
  4. *Push* your changes back up to your GitHub copy of the repository
  5. Submit a *pull request* of the specific *commits* or *branch* to the original owner of the repository

At this point the owner of the original repository will be notified (through GitHub, or email if that is configured) that a *pull request* has been submitted. They can review the changes that are highlighted in the *diff* contained in the *pull request*. If approved, GitHub submit's a *pull* operation on behalf of the original repository to your repository for the changes conatined in the *pull request*. 

#### Collaboration ####

GitHub also provides alot of great tools to document and organize code. Some examples:

  * A discussion thread is attached to each *pull request* to allow collaborators to discuss and potentially prompt for edits before a *pull request* is accepted

  * GitHub provides a wiki to help with documentation of your project

  * A full Issue Tracker is provided complete with auto-assignment, categories, discussion, and specific *commit* references

  * Several visual tools are available to represent the Git *commit* history, contributing users, frequency of contributions, and others