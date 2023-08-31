## Learning Objectives:
- Distributed Git
	- Remote Branches
	- Distributed Workflows
	- Collaboration - Workflows
	- Contributing to a project
- Working with Repository
	- Own server
	- Hosted service - GitHub

## Distributed Git
Recall that in a Distributed Version Control System that Developers copy the repository including its full history

In a DVC, it allows the use of several remote repositories. Developers can collaborate with different groups of people in different ways simultaneously with the same project

### Remote (Hosted) Repository
A remote repository is generally a *simple repository*
	- the contents of your project is just the .git directory and nothing else

In a one-person project, remote repositories are not really necessary but is convenient if you are using different machines.
- Generally, local repository should suffice and it allows you to track changes and maintain a history of the project

In team-based project, remote repository provides a centralized location where all team members can access and it is better to have a common point of reference and backup for the project

The remote repository also allows team members to "push" their changes to the remote repository and "pull" changes that other teammates have done. 

With multiple members having access to a repository, it would be wise to manage and coordinate permissions to prevent unintended overwrites and ensure code is reviewed before being pushed


### Remote Branches
- **Remote References** - pointers in your remote repos
	- Pointers to branches
- **Remote-Tracking Branches** - pointers to the state of remote branches
	- Local references you cannot move; Git moves them for you so they accurately represent the state of a remote repo

When you clone a repository, you have remote references which points to other branches in the repository while Remote-Tracking Branches are branches that are present in your local machine and in the repository so when you pull, it updates the state of the branch to the latest commit and merge changes into your local machine

- **Remote References** - 
	- `git ls-remote`: gets full list of remote references
	- `git remote show`: gets list of remote branches
- **Remote-Tracking Branches** - 
	- Follows the naming convention `<remote>/<branch>`
		- Checks the origin/master branch to see the master branch on your origin remote look like

![[Pasted image 20230815104203.png]]

In the example above, we are cloning from a repository from the company and let's try and explain step by step.

1. `git clone URL`
- This command will name the remote repository `origin` 
- Pulls down all the data including branches, commits, etc.
- It creates a pointer to master branch and call it `origin/master`
- Sets our local master branch as the origin's master branch
- Note: `origin` is the default name, but if you want to change it you can use `git clone -o myBranch` to name your default remote branch myBranch/Master
2. Imagine you do some commits on your local machine and another developer pushes updates to the master branch in the remote repository
![[Pasted image 20230815111734.png]]
3. You can sync your and your teammate's work using `git fetch origin`
- Fetches changes you do not have from the remote repository and update to your local repository
	- Moves the origin/master pointer
- It does not mess up the current branch you are on but adds the new commits in the repo and if you want to check the new branch you can `git checkout master`
![[Pasted image 20230815111850.png]]

4. Pushing changes to the origin
Lets say we finished our `serverfix` and we want to update the remote branch and to do this we can use `git push origin serverfix`
- `git push origin serverfix` takes our local `serverfix` and pushes it to the remote repository so that it updates or creates the remote's `serverfix` branch with my local changes
- We can also use `git push origin serverfix:serverfix` which is more explicit using the syntax, `git push <remote> <local-branch>:<remote-branch>` where it takes the local `serverfix` branch and creates or updates the `serverfix` branch on the remote repository


## Distributed Git
### Centralized VCSs
A centralized VCS follows a centralized workflow, meaning there's one main repository where all the code is stored and every change by every developer has to be synchronized to this one location

in a centralized VCS model, pushing changes to the central repository especially when working with other people can arise many conflict.

If Joe and Mary clones from the repository and Joe pushes some changes, Mary must first pull from the latest changes from the central repository then push to the repository with her changes

## Distributed VCS

In a distributed DVCS, each developer's local copy of the repository is a full-fledged repository with its complete history. This makes each developer a 'node' in the network of contributors

A developer can also act as a public repository where other people can base their work on by pushing their changes or creating pull request

DVCS also offers a lot of flexibility on how teams want to collaborate. Different teams may prefer different workflows and a DVCS can make that work

### Integration-Manager Model
In the Integration Manager model, there is one blessed repository that represents the 'official' project and there is a integration manager with many developers.

Each developer was their own public repository and has write access to their own repo and read-access to everyone else's

Developers clones their public repos and push changes on to it and will inform the maintainer (integration manager) of the main project to pull their changes

The manager will add the developer's repo as a remote and test their changes locally and merge them into the branch if suitable

### Integration-Manager - Workflow
1. Project maintainer pushes to their public repository
2. Contributor clones that repository and makes changes
3. Contributor pushes their changes to their own public copy
4. Contributor emails the maintainer asking for pull changes
5. Maintainer adds the contributor's repo as a remote and merges locally
	- Note that the maintainer has a local clone of the blessed repository
	- Adds the contributor's repo on their local machine and pull the changes from the contributor's repo to their local environment and test
6. Maintainer pushes merged changed to the main (blessed) repository

### Integration-Manager - Use
- Very common workflow in hosted servers such as GitHub and GitLab
- Easy to fork a project and push changes into the fork for everyone to see
- Developers can continue to work on their repos while the maintainer of the blessed repo can pull their changes at anytime
- Contributors do not have to wait for the project to incorporate their changes
	- Each can work on their pace

![[Pasted image 20230815121855.png]]

## Distributed VCS - Dictator and Lieutenants

This model is a variation of multiple-repository workflows where, in a nutshell, developers report to Lieutenants and Lieutenants reports to a Dictator. 

The Dictator is the person responsible for the final, official, 'blessed' repository. They have ultimate authority over which changes get incorporated into the main project

Lieutenants are trusted members of the contributor community who have been granted access to oversee specific parts or components of the project. They review and merge changes related to their domain

Contributors are developers who submit changes to the project which will be reviewed by Lieutenants

This type of model is typically used where a project has a vast number of contributors and a hierarchical structure is deemed beneficial or is needed for specialization of specific domains

## Contributing to a Project

There are several factors how you can contribute effectively to a project
1. Active Contributor Count
	- How many active users are contributing code to this project and how often?
		- If 2-3 develops with a few commits a day vs hundreds with hundreds of commits
		- There is a relationship between number of developers and commits, and potential conflict/merge issues
2. Project workflow: What project workflow is being utilized?
	- Centralized with equal write access? Have a maintainer or integration manager? Or has a lieutenant system you have to submit your work to first?
3. Commit Access: How the commit access would affect the contribution workflow?
	- Do you have write-access to the official repo?
		- If not, how can your contributed work be accepted?
	- How much work a developer may contribute and how often?

### Commit Guidelines
- No whitespace errors:
	- Do not commit files which has any trailing whitespaces
- Commit logically separate changeset
	- Do not work on many different issues in your code and submit them as one commit. 
	- Separate each issues with a commit of their own
- Use quality commit message
	- A concise description of the change followed by a blank line then a detailed explanation


## Contributing to a Private Small Project
In a private small project, commonly, they have a few developers and they all have push access to the repository. They have a centralized workflow with offline committing and branching and merging

**Scenario**:
- John clones the repo, makes a change and commits locally
	- `git clone repo.url`
	- `some changes`
	- `git commit -am 'made some changes'`
- Jessica clones the repo, makes a change and commits locally
	-  `git clone repo.url`
	- `some changes`
	- `git commit -am 'made some changes'`
- Jessica pushes her work to the server
	- `git push origin master`
- John makes changes, commits locally, and tries to push to the same server
	- `git push origin master`
	- Rejected, error: failed to push some refs
	- John's push fails because of Jessica's earlier push
- John fetches Jessica's changes
	- `git fetch origin`
	![[Pasted image 20230815233042.png]]
- John can now merge Jessica's work since he fetched into his own local work
	- `git merge origin/master`
![[Pasted image 20230815233111.png]]
- John tests new code and makes sure Jessica's work is unaffected and he finally merged his work to the server
	- `git push origin master`
![[Pasted image 20230815233212.png]]
- Jessica created a new topic branch `issue54` and made three commits to that branch. She also hasn't fetched any of John's changes so her commit history looks like:
![[Pasted image 20230815234222.png]]
- Jessica wants to see John's new work from the repo so she:
	- `git fetch origin`
![[Pasted image 20230815234257.png]]
- Jessica's topic branch is ready and needs to check what part of John's fetched work she needs to merge into her work so she can push
	- `git log --no-merges issue54..origin/master`
		- This commands asks git to display only those commits on origin/master that are not on issue54
		- Output tells her there is a single commit that John has made that is not in Jessica's machine
- Jessica first merges her topic work into her master branch, then merge John's work, and then push back into origin/master
	- `git checkout master`
	- `git merge issue54`
	- `git merge origin/master`
![[Pasted image 20230815234937.png]]
- Finally, Jessica can now push to origin/master
	- `git push origin/master`
![[Pasted image 20230815234957.png]]
## Useful (Remote) Git Commands
- Remote Repositories
	- `git remote add <name> <url>` - creates a new connection to a remote repo
	- `git fetch <remote> <branch>` - Fetches a specific branch from the repo 
		- `git fetch <remote>` - Fetches all remote references
	- `git pull <remote>` - Fetch the specified remote's copy of current branch and immediately merge it into the local copy
	- `git push <remote> <branch>` - Push the branch to remote, along with necessary commits and objects 
		- Creates named branch in the remote repo if it doesn't exist
- Git Config
	- `git config --global user.name <name>` - Define the author name to be used for all commits by the current user
	- `git config --global user.email <email>` - Define the author email to be used for all commits by the current user
	- `git config --global alias. <alias-name> <git-command>` - Creates shortcut for a Git command - Creates shortcut for a Git command
		- `alias.glog "log --graoh --oneline"` will set `git glog` == `log --graph --oneline`
	- `git config --system core.editor <editor>` - Set text editor used by commands for all users on the machine
	- `git config --global --edit` - Opens the global configuration file in a text editor for manual editing
- Git Pull
	- `git pull --rebase <remote>` - Fetch the remote's copy of the current branch and rebases it into the local copy
		- Use git rebase instead of merge to integrate the branches
- Git Push
	- `git push <remote> --force` - Forces git to push even if it results in a non-fast-forward merge
		- DO NOT USE THIS UNLESS YOU KNOW WADDUP
	- `git push <remote> -all` - Push all your local branches to the specified remote
	- `git push <remote> --tags` - Tags are not automatically pushed when you push a branch or use the `--all` flag
		- `--tags` flag sends all your local tags to the remote repository

## Remote Repository - Running your own Server
If you are trying to host your code or projects on your own server, you need to configure which protocols your server communicates with. Here are some typical server set-ups using configured protocols

| Protocol    | Pros                                                         | Cons                                                                                   |
| ----------- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| File System | Simple, support access control                               | Public share is difficult to setup                                                     |
| SSH         | Easy to setup, fast, and supports authenticated write access | 335 No anonymous access (even read access)                                             |
| HTTP        | Unlikely to be blocked                                       | Can become difficult to setup                                                          |
| Git         | Fastest protocol, allows anonymous public access             | Difficult to setup, lack of authentication, use non-standard port which can be blocked |

### Running on a Hosted Server
You can setup your own repository on a hosted server and we would not need to concern ourselves with security or privacy and there are many hosting services including GitHub, GitLab, BitBucket, and Mercurial.

Note that we are hosting Git itself, but Git repositories where we can host our own projects and open it up for collaboration
- Public repositories: All **GitHub** functionality with free accounts but all projects are public
- Private repositories: Protected access and full control but it may not be free with some hosting services
- 