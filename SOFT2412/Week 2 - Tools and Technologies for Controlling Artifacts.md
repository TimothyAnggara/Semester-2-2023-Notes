
## Learning Objectives
- Software development process models
	- Activities and artifacts
- Agile Development Tools
	- Software development - Artifacts
	- Version Control Systems
	- Version Control with Git
	- Using Git Commands

### Waterfall Model Phases - Revisit
1.  Requirement's analysis and definition
	- Requirements document
2. System and software design
	- Design document based on requirements document
3. Implementation and unit testing
	* Code and test documents
4. Integration and system testing
	* Software components are integrated and the resulting system is tested
5. Operation and maintenance

### Requirements Engineer Process - Revisit
![[Pasted image 20230808095410.png]]

## Software Artifact - Software Requirements Specifications

Software Requirements Specification is a document that outlines the requirements for a particular software system or project
- What should the system do
- How it should behave
- The rules it must follow
- Other specifications that are necessary

These documents are typically involve a time-consuming and a lot of effort from customers, stakeholders, end-users, etc. The process of defining the requirements and gathering information is a tedious task

These documents are typically formatted but there are different ways these documents are written:
- In agile, it is typically formatted as "User stories"
- In more traditional methodologies, huge Word documents in a standard template is used
	- Once the document is finalized, it is important that it is signed off by relevant stakeholders as an agreement that the software written in the specifications is the software being produced
- In these documents, it is also important to track changes and ensure to keep a version of each Software Requirement Specification (SRS)

## Software Artifacts - Code

Software artifacts pertaining to code are often spread over different files and organized within a directory structure, following specific language conventions or requirements. For example, in Java, there's typically a separate file for each class, and the organization may reflect a package hierarchy.

Documentation might also be derived directly from the source code, using special comments or annotations. The maintainability and extensibility of the code, regarded as non-functional properties for a software system, are crucial.

Both code and documentation depend on proper design and architecture, with examples including widely-used patterns like Model-View-Controller (MVC) or a layered architectural approach. These design practices help ensure that the code remains robust, manageable, and adaptable to changes over time.

## Artifacts

Artifacts are items that are being produced or work is being done during the software development process

These artifacts go through **evolution** and it is important to keep track of these evolutions


## Code Artifacts - Version Control

### What is Version Control?

Simply, it is a method for recording changes to a file or a set of files over time so that you can recall specific versions later

We can create, maintain, and track history during the software development life cycles for all Artifacts that we have

We use these tools to see what kind of changes are happening and how the code is evolving 


### What is Version Control System (VCS)

Software tools to software teams to be able to manage changes to the source code over time. These tools generally have a repository which is accessed by the whole team

This software can also revert selected files back to a previous state and compare changes over time
	- For example, if 2 members at the same time and working on the same code and they do different changes in the same file, this tool can be used to choose which one to commit and to reject

See who last modified something that might be causing a problem and who introduced an issue and when. We can also use it to compare earlier versions to help fix bugs while minimizing disruptions to all team members

Version Control Systems have many more benefits


## Local Version Control
- This was used in the early ages of computing and was called the Revision Control System (RCS)
- RCS works by keeping patch sets
	- Patch sets - differences between files

## Centralized Version Control

Centralized Version Control (CVCS) supports collaborative development by having a Central VCS Server in the network and have multiple users connect to the Server where users can use

This method is better than the local VCS as it keeps everyone updated and easier to admin but the problem with this system is that there is a single point of failure and is easily disrupted by damage to storage
	- Hard disk becomes corrupted, entire history lost if no back ups

## Distributed Version Control
Distributed Systems means that instead of having a single server we can have multiple backups of the repository but we still have a central repository

In this system, developers fully mirror the repository including the full history and there are serval remote repositories
- Developers can collaborate with different groups of people in different ways simultaneously with the same project
- Can setup serval types of workflows

## Git Fundamentals

### Version Control - SW Development Scenarios

Multiple versions of the same software deployed in different sites and developers working simultaneously on updates

Developers fixing some bugs /issues may introduce some others as the program develops

Two versions of the software may be developed concurrently and many more

### Git
A version control system that helps development teams to manage changes to source code overtime with public and private repositories

This system is a web-based central repository of code and track of changes
- Tracing history of changes, commits, branches, merges, conflict resolution
- Collaborate and update repository through command-line and GUI

## Delta-based VCSs (Differences)

- VCSs that store as a set of files and the changes made to each file over time
![[Pasted image 20230808105222.png]]

### Git - Snapshots Not Differences

Git thinks about its data as a stream of snapshots of a small file system. Git doesn't store unchanged files, it just link to previous identical file already stored

![[Pasted image 20230808105338.png]]

### Git - Basics

In Git, nearly every operation that you do is done on your local repository and not needing to communicate to the remote server
- This means that when using Git, you can browse, work, and commit changes to your files locally

Git also has built-in integrity by using a hashing algorithm which makes the contents of a file detectable to corruption or unauthorized changes.
- It uses the check-sum (SHA-1 hash) before content is stored 
- Git stores everything in its database by the hash value of its content

When using Git, the commits that you make on a file, history of that file is preserved. This means that it is very difficult to lose data or earlier versions of your file

Files that are stored using Git has 3 primary states:
- Modified - State for files that have been changed since last commit but not staged for the next commit
	- Changes you have made but are not tracked
- Staged
	- State for files that are ready to be included in the next commit, to modify the state of a file you use the command `git add`
- Committed
	- State for files that have been securely saved to the Git database, to modify the state of a file to this state, you use the `git commit`

### Git - Structure
- **Working Directory (Tree)**
	- Where the files you are actively working on are located, your current workspace,
	- When you check out a branch or specific commit, the relevant files are extracted from the Git database and into your working directory
- **Staging Area (index)**
	- Temporary space where you can place changes you want to include in your next commit
- **Git directory (repository)**
	- Where the entire history of the project is stored and the repository's state and history
		- Commits, brnaches, tags, and other metadata
	- When you clone a repository you're creating a copy of this Git directory onto your local machine
### Git - Basic Workflow
![[Pasted image 20230809144839.png]]
## Git Concepts and Scenarios

### Git Repository (Repo)

A Git repository is basically a special directory contains all the project files including the source code and special metadata used by Git. 

The repository stores and tracks all changes to the files in your project. The tracking allows us to view history of changes, revert to previous versions, and collaborate with other people.

Git also adds special sub-directories to store history of changes about the project's files and directory. Within a Git repo, there is a sub-directory called .git which contains all the information Git needs to manage the repository

When creating a git repo on your local machine we use the command `git init` and for cloning an existing repository we use `git clone`
![[Pasted image 20230809161302.png]]

### Metadata 
Each version of a snapshot should have some form of meta data. Usually it is in the form:
- Unique Name
- Date
- Author
- Etc.

### Git - Recording changes to a Repo

For project files, there are two more generally types of state:
- Tracked: Git knows about it (unmodified, modified, staged)
- Untracked: Git does not know about it

When a repo is cloned, then all files are tracked and unmodified and when you make changes to those files, Git classifies them as modified.

Once you staged and committed all the staged files, you have a **clean directory**

### Git - Branching

Branching is when you diverge from the *main line* of development and continue to do work without messing the main line

When creating a new branch, you are required to create a new copy of the source code directory but in Git, it is lightweight.
- With every commit, Git stores a commit object that contains a pointer to the staged snapshot, author's name, email, commit message, and the parent's commit

### Git Branching - Example

Assume you have 3 files in your project directory:
`git add README test.rb LICENSE`

We staged these files and git computes a checksum of each file and stores the files in the git repo as blobs
- Imagine blobs as containers storing the content the each file in the Git repository which is labelled by  a SHA-1 hash

Let's now commit using 'git commit -m "Initial Commit"'

Now, our project directory contains 5 objects, 
- 3 Blobs - Content of each file
- One tree - lists the directory contents and which files names are stored as which blobs
- One Commit with the pointer to the root and the commit metadata
![[Pasted image 20230810170056.png]]

Now, if we make some changes and commit again, the next commit will store a pointer to the commit that came immediately before it

![[Pasted image 20230810170342.png]]

### Git Branching - Pointer's Perspective
Branches in Git is lightweight because of this light movable pointer that points to specific commits.

In Git, the default branch is called **master** and when you start making commits in a Git repository, the master branch automatically points to the most recent commit you made

![[Pasted image 20230810171135.png]]

### Git - Creating a New Branch

When creating a new branch, lets say **testing** inside of our project, a new pointer will be created which will point to the same commit we are currently at

![[Pasted image 20230810171300.png]]

Once we do create this new branch / pointer, how does Git know which branch we are currently on?

Git knows which branch we are by maintaining a **special** pointer called **HEAD**, so by default our **HEAD** pointer is on master but when we want to switch branches using `git checkout testing` the **HEAD** pointer now points to testing

![[Pasted image 20230810171948.png]]
![[Pasted image 20230810171954.png]]


### Git - Merging Scenario

Let's imagine a scenario where you have a website and there is an urgent hotfix that needs to be made. Before this hotfix was made aware, you created a branch called ,`iss53`, which is used to add a new footer to the website.

You were on the `iss53 branch` and because of the hotfix you `git checkout master` and you created a new branch `git branch hotfix && git checkout hotfix`
![[Pasted image 20230810173329.png]]
You fixed the emergency issue and created a commit, `git add . && git commit -m "Fixed emergency issue"`. Once you created the commit, you want to merge the hotfix and deploy back to production, `git checkout master && git merge hotfix`

![[Pasted image 20230810173418.png]]

You then delete the hotfix branch, `git banch -d hotfix` and you switch back to the `iss53` branch and commit changes to `git commit -a -m 'finished new footer'`. The new footer is now completed and is ready to be merged back into the master branch. 

![[Pasted image 20230810173621.png]]

Because of this unique scenario, Git makes a "*three-way merge*" using two snapshots. Git creates a new snapshot and automatically creates a new commit that points to it.
- This 'special' merge has more than one parent and is referred to as a **merge commit**

![[Pasted image 20230810173733.png]]

### Git - Conflict Resolution

Commits, branches, and merging in Git can get very complicated and because of that conflicts can arise. These conflicts are the result of two or more branches creating changes to the same file.

Git has **conflict-resolution markers**, which are special markets that are added automatically by Git to files that have conflict to guide you where the conflicts

![[Pasted image 20230810174140.png]]

In this example, everything above the === is the master branch version and version iss53 is below it. To resolve this conflict, you have to either choose one side or the other, or merge and remove those special markers. 

Once the conflict have been resolve, add each file to the staging area and Git will mark the conflict as resolved