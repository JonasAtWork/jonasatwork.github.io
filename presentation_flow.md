# Git Presentation #

## Agenda ##

1) Version Control Systems

  - Centralized VCS
  - Decentralized VCS

2) TFVC
3) Git

## Objectives ##



## Version Control Systems ##

modern Version Control Systems should allow you to do some important, specific things:

1) Concept of a project

  - define projects
  - define objects that are a part of that project
  - define who has access to those objects

2) keep track of historical changes to the objects within that project

3) make structural changes to the project

  - Add objects
  - Remove objects
  - Create an alternate version of objects (*branching*)
  - Combine alternate object versions into one object (*merging*)

There are two main ways modern Version Control Systems Work

### Centralized Version Control Systems ###

All of the work to keep track of files in a CVCS is **ONLY** done in one central place (i.e. the server). There is only ever one repository in a CVCS. Whether its 1, 10, or 1000  collaborators working on a project, there is only one repository.

**This implies a few important points about a CVCS**

  - To work on the project files, you need access to the centralized server. Since most of us arent working directly on the server hardware, this implies that we **need** network access to reach the server
  - Only "unlocked" files can be edited
  - All actions affecting the structure of the project, must be done on the server, using network and server resources. Also whatever actions are taken, immediately affect everyone with access to the server

### Decentralized Version Control Systems ###

All of the work to keep track of files in a DVCS **CAN** be done anywhere the repository exists. A repository can exist on any machine (server, "team box", my machine, your machine ... *anywhere*). There are as many copies of the repository as there are collaborators to the project

**This has a few implications too for DVCS'**

  - To work on the project files, you only need access to your local filesystem where the repository lives. You do **not need** network access to reach your repository
  - Any file can be worked on within your own copy of the repository. No one can "lock" a file in your repository
  - Actions affecting the structure of the project can be done cheaply (read: locally) and could conceivably not affect other copies of the repository if you dont want it to

### What does it all mean? ###

The workings of a CVCS should be pretty familiar to most. It follows a basic workflow of doing: **UNLOCK** --> **MODIFY** --> **LOCK**. This workflow is effective and still works well in many contexts.

A well designed DVCS however is not nearly as intuituve. It is a different way of looking at development practices (not just Version Control) and can be very effective. But it also allows you to shoot yourself in the foot in far more ways than a CVCS does. 

*Lets look at some specific examples used here at and see how they compare*

## Team Foundation Version Control ##
 
  - TFVC is a CVCS, and is a component of the Team Foundation Server (TFS)
  - TFS != TFVC
    - TFS is an entire suite of tools including a Build Server, Work Item Tracking, Code Analytics, and a Version Control System (TFVC), among others
    - The TFVC component is most comparable to Git as they are both Version Control tools

    ```
    TFVC is to TFS 
        Like 
    Git is To GitHub
    ```

### When TFVC is the right tool ###

  - Deep integration with Visual Studios and Windows
    - My team works in .NET so this is important
  - Centrally managed within our organization 
    - No one on my team worries about how the TFS server is running
    - There are dedicated resources devoted to housing TFS
  - Multiple versions of large (1MB - 5MB) binary files are not a problem for TFVC
    - All of the versions are kept on the central server
    - I only grab the version that is most interesting to me (so long as no one else is using it!)

### When TFVC may not be the right tool ###

  - More difficult to integrate with TFS/Visual Studio if you aren't actually developing in .NET 
    - Looking at you Java Developers :)
    - a platform agnostic tool may more suitable
  
  - It may be overkill to use the enterprise maintained TFVC for smaller projects
    - Provisioning RM's, Access Requests, etc...
    - Sometimes you want to get up and running quickly
  
  - Working Remotely
    - Much more of a problem with a CVCS. Remember, *EVERY* action needs to be done through the network on the server
    - Also, network/server issues do (rarely) occur even within the firewall
  
  - Rapid Development, Lots of changes, Need to be able to test and discard/promote features quickly
    - This type of development is becoming more prominent as we move to more agile practices
    - This type of development is done with easy *branching* and *merging*
    - TFVC is not known for easy *branching* and *merging*. My personal experience is that in its more trouble than its worth :( 

## Git ##



