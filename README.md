# Github Version Control in VS Code
*(January 2025)*

The Integrated Development Environment (IDE) Visual Studio Code (VS Code) supports version control for code files through integration with Git and GitHub. However, using external Git commands via the console alongside the Git commands available in the VS Code menu can lead to conflicts. The following notes outline the VS Code commands for managing version control effectively.

The current version of VS Code I'm using is:
- Version: 1.96.4 (Universal)
- Commit: 3 cd4ee3b1c348a13bafd8f9ad8060705f6d4b9cba
- Date: 2025-01-16T 00:16:19.038Z
- Electron: 32.1.6
- ElectronBuildId: 10629634
- Chromium: 128.0.6613.186
- Node.js: 20.18.1
- V8: 12.8.374.38-electron.0
- OS: Darwin arm64 24.2.0

Version control is especially useful to synchronize teamwork methods for a hackathon.

*Side note: VS Code is developed on the Electron. Electron is an open-source framework that allows developers to build cross-platform desktop applications using web technologies such as HTML, CSS, and JavaScript. It combines the Chromium rendering engine and the Node.js runtime, enabling developers to create applications that can run on Windows, macOS, and Linux with a single codebase.*

*Electron was originally developed by GitHub and is now maintained by the Electron team at GitHub. It has gained popularity due to its ability to simplify the development process for desktop applications, allowing web developers to leverage their existing skills to create native-like applications. Some well-known applications built with Electron include Visual Studio Code, Slack, and Discord, as well as the AI Coding assistants Cursor and Windsurf.*

# Quick look at git
Since both Visual Studio Code (VS Code) and GitHub utilize Git, we will begin with a brief overview of Git. The illustration below provides a comprehensive overview of the main Git commands.
![Git basics](./images/0-Git_basics.png)

## Github Fork, start from a repo of someone else (more often)
•	Forking is a concept in GitHub that facilitates collaboration. A GitHub fork creates a copy of another developer's repository under your own account, allowing you to work independently. When you are ready, you can discuss your changes with the original repository owner to prepare a Pull Request (commonly referred to as a "PR"). Once the original owner approves your modifications, a pull from the original repository will be executed to merge your changes into it.

## Git clone, start working from a forked github repository
A clone command creates in your local disk a clone repository of the remote Github repository. We use cloning in hackathons to work on the same project. 

When you run this command, Git performs the following actions:

1. Creates a Local Repository: A new directory is created in the specified local folder, which will contain the cloned repository.
2. Copies the Repository: It downloads all the files, branches, and commit history from the remote repository to your new local directory.
3. Sets Up Remote Tracking: The cloned repository is configured to track the original remote repository, allowing you to easily fetch updates and push changes back to it.
4. Checks Out the Default Branch: By default, Git checks out the main branch (often called `main` or `master`), so you can start working with the code immediately.

*Branches allow developers to work on different features, bug fixes, or experiments in isolation from the main codebase, typically referred to as the "main" or "master" branch.*

Command example:
``` zsh
git clone https://github.com/user/repo.git
```
## Use Git clone from VS Code
The following procedure makes sure that you have no surprise to do version control from inside VS Code.

1. In the repository page in Github web site, click on the big green button labelled "**Code**" and copy the URL
2. In VS Code, navigate to the local folder where you want the cloned repository to be
3. In VS Code, type **Cmd-Shift-P** to display the command popup, choose the command **Git:Clon**e, paste the URL and hit Return. 
4. Choose the local folder inside which the cloned repository will be store and hit Choose. The cloned repository will appear at the desired storage location.
5. Once you have worked with your code in VS Code, you can **Commit** in your local git and Sync the files to the remote repository, directly from VS Code.

For most of the development work, the console commands below will not be needed.

# Working on a repository
In the folder where you want the repository to be located, in the console, the following commands are available for the most frequent actions:
- `git remote -v`: check from where the current version is cloned.
- `git status`: check what files and folders are staged, unstaged, and untracked.

Make changes in the code file, save them. Your workflow is as follows:
![Git workflow](./images/1-Git_workflow.png)

- `git add -A`: declare all changes and all files of the folder as to be tracked by the repository, depending on `.gitignore` ("stage" the changes made to the files)
- `git commit -m` "message": declare changed files to repository. Don't forget the flag `-m` "message"
- `git commit -am` [commit message]: add and commit all changes of the folder in a same command
- `git push`: update the remote version in GitHub. Push by default to `origin`, by default to branch `main`, the current committed branch.

There are several git push variants
- `git push origin` branchName: variant to specify which branch of the remote to update, branch branchName (`origin` or `remotes`/`origin` is the named URL of the repository is cloned from. You can verify it with `git remote -v`),
- `git push origin` localBranchName:remoteBranchName: variant to update the version in GitHub, branch remoteBranchName with contents of localBranchName,
- `git push --set-upstream origin` [new-branch-name]: variant to create a new branch new-branch-name on the Github repository and push to it.

The other, less frequent, git commands are:
- `git stash`: memorize changes and remove them from working folder (`git stash drop`: drop the last memorized changes)
- `git diff --cached`: show difference between staged changes and last commit
- `git diff HEAD`: show difference between working directory and last commit 

## Git pull 
It is best to do a `git pull` to merge the last commit in the repository before doing a push.

To fetch any new files on the server since last time you cloned or fetched this GitHub repository, `cd` inside this directory and…
- `git remote -v`: show the URL from which the repository has been cloned
``` zsh
origin	https://github.com/kvutien/Machu-Picchu.git (fetch)
origin	https://github.com/kvutien/Machu-Picchu.git (push)
``` 
- `git pull` [REMOTE URL]: update your current repository with the remote repository, equivalent to fetch plus merge 

For example: `git pull github.com/kvutien/Machu-Picchu.git`.
- Then you can do `git push origin`, or `git push`: update your origin repository with your changes 

## Branch and merge a repository 
(`https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging`):
- `git branch`: list the existing local branches
- `git branch -a`: list the existing local branches and remote
- `git branch newbranch`: create a new git pointer newbranch to the current commit, for example to share with someone in read-only
- `git checkout -b` newbranch: create a new git pointer newbranch to the current commit and set it as the current pointer for further development
- `git checkout` remotebranch: copy locally a remote branch to work on it. This must be done if you want to push your production to this remote branch
- `git commit -a -m` [commit message]: after local changes, do usual commit, as above
- `git checkout main` followed by `git merge` newbranch (locally merge the changes from branch newbranch to the `main` branch), followed by `git push` to update the `main` branch of `origin`.
- or `git push origin` branchName to update a specific branch on `origin`.
- or `git push https://kvutien:`[PASSWORD]`@github.com/kvutien/Machu-Picchu.git` newbranchName 

## Branch contingencies:
- `git branch -a`: list all branches in repository
- `git branch -d` branchToDelete: delete the branch
- `git branch -D` branchToDelete: force delete the branch even if there are changes not committed
- `git checkout -- Pepito/client/src/App.js`: undo local changes on file `App.js` (for example)

# Git special cases
## Git special case, pushing to another repository
If needed to push to another GitHub repository, different from the working repository. This is like a Pull Request, but you lose all the history of the changes. For example, in the drawing below, the working repository is Rajan's, the pushed repository is the team repository, and you want to push Rajan's to a branch of the team repository named RPS.
![Git push to another repositorys](./images/2-Git_push2other_repo.png)
- `git clone` [Rajan's repo] [local folder 1]: clone the repository of Rajan (by default `master` branch is checked out else use `git checkout` to choose branch) to the local folder `Rajan` for example
- `git clone` [Team repo] [local folder 2]: clone the team repository (by default `master` branch is checked out else use `git checkout` to choose branch) to the local folder `Team` for example
- Overwrite the whole *local folder 2* (except the `.git` folder) with the contents of *local folder 1* (except the `.git` folder)
- `git commit -a -m "moved work of Rajan"`: update the `.git` folder to reflect the new content of the folder *local folder 2* 
- `git push origin RPS`: push the local branch to the `RPS` branch of the team repository, that is the `origin` remote repository

This command works only if you have write access to the server you cloned from and if nobody has pushed in the meantime. If you and someone else clone and they push upstream before you, you’ll have to make a `pull` (to `fetch` their work and `merge` it into yours) before you’ll be allowed to `push`.

## Git special case, creating a git repository from existing local folder
When Android Studio creates a Flutter project, it creates a local folder with `.gitignore` but the folder is not yet a git repository. To make it a repository, do the following:
``` zsh
cd <localDir>
git init
git add .
git commit -m "initial commit"
git remote add origin https://github.com/kvutien/<repositoryName.git>
git pull origin main --allow-unrelated-histories
git push --force-with-lease
```
Detailed explanations of the above sequence of commands:
**1 Initialize Local Repository**
- First, in the local folder, create the local Git database with
`cd <localDir>
git init`
- Add all files for version control with
`git add .`
- Create a commit with a message of your choice
`git commit -m 'my commit message'`

**2 Initialize Remote Repository**
- Create a repository on GitHub and copy the URL of this repository. as shown below:

![Github clone URL screen shot](./images/3-Github_Clone_URL.png)

**3 Link Remote repo with Local repo**
- Now use copied URL to link your local repo with the remote GitHub repo. When you clone a repository with git clone, it automatically creates a remote connection called origin pointing back to the cloned repository. The command remote is used to manage a set of tracked repositories.
`git remote add origin https://github.com/kvutien/<repositoryName.git>`

**4 Synchronize**
- Now we need to merge local code with remote code. This step is critical otherwise we won't be able to push code on GitHub. **You must call 'git pull' before pushing your code.**
`git pull origin main --allow-unrelated-histories`

**5 Commit your code**
- Finally, push all changes on GitHub
`git push --force-with-lease -u origin main`





