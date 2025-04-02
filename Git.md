# GIT

## Version Control System (VCS)

A Version Control System (VCS) helps developers track and manage changes to code. There are two main types: Centralized Version Control Systems (CVCS) and Distributed Version Control Systems.

### Centralized VCS vs Distributed VCS

| Feature | Centralized VCS (CVCS) | Distributed VCS (DVCS) |
|---------|------------------------|------------------------|
| **Architecture** | Single central repository | Multiple copies of the repository |
| **Examples** | SVN, Perforce | Git, Mercurial |
| **Working Model** | Developers pull and push changes from a central server | Developers clone the full repository, work locally, and push when ready |
| **Speed** | Slower, as it depends on the server | Faster, since most operations are local |
| **Offline Work** | Limited (requires server access) | Possible (local commits, branches, history available) |
| **Collaboration** | Dependent on a central server, risk of downtime | Peer-to-peer, more fault-tolerant |
| **Backup & Failure** | If the server crashes, all history is lost (unless backups exist) | Each developer has a complete copy, reducing data loss risk |
| **Branching & Merging** | Difficult and time-consuming | Easier and more efficient |

## Introduction to Git

### What is Git?

Git is a version control system (VCS). Unlike traditional centralized control systems where even checking out a file required admin privileges, Git allows any work to be done locally and may diverge as you want.

### Git Levels: High Level (Porcelain) vs Low Level (Plumbing)

1. HIGH LEVEL (Porcelain)

2. LOW LEVEL (Plumbing)

> We primarily use high-level commands.

### Key Git Terms

- repo: a git tracked project.
- commit: A point in time representing the project in its entirety.
            The SHA (Secure Hash Algorithm) that represents a commit (40 a-f-0-9 characters) is calculated from the contents of the change, author, time, and author.
- index: the term used to represent the staging area.

> The git index is a crucial data structure in Git. It serves as the 'staging area' between the files you have on your filesystem and your commit history. When you run `git add .`, the files from your working directory are hashed and stored as objects in the index, leading them to be staged changes.

- squash: to take several commits and turn them into one commit.
- work tree, working tree: This is the set of files that represent your project. Your working tree is set up by `git init` or `git clone`.

```flow
Untracked -> Staging Area -> Tracked
```

### Key Facts about Git

1. Git is an acyclic graph.
2. In Git, each commit is a node in the graph and each pointer is a child in a parent relationship.
3. If you delete untracked files, they are lost forever. Commit early and often; you can always change history to make it one commit.
4. `man git -<o>` for a friendly manual.

### Checking Git State

```
git config --get --global init.defaultBranch
master # any branch name will show up
git config --global --unset init.defaultBranch
```

Ensure that `rerere` isn't true

```
git config --global rerere.enabled
true
git config --global --unset rerere.enabled
git config --global rerere.enabled
```

### Configuring Git

#### Key Facts

- All config keys are in the following shape:
    `<section>.<key>`
- The --global flag will ensure you set this key value for all future git and repos.
- user.name and user.email are the keys used in commits tied to you.
- You can view any value of git config by executing `git config --get <key>`.

#### Add username and email

```
git config --add --global user.name "Your Name"
git config --add --global user.email "your_email@gmail.com"
```

## Basic Git Commands

### Creating a new repo

To initialize git

```
git init
```

Files in git repo
`.git` : contains all state of the git repo.

> If you want, you can remove it from being a repo by deleting the .git folder.

### The Basics

- `git add <path-to-file/ pattern>` will add 0 or more files to the index (staging area).
- `git commit -m "<message>"` will commit the changes present in the index.
- `git status` will describe the state of the git repo, which will include tracked, staged, and untracked files.
- `git log` has many options to make viewing history easier.

### Understanding SHAs (Secure Hash Algorithm)

Git comes with a SHA (a hash 0-9, a-f character). You can specify the first 7 characters of the SHA for Git to identify what you are referring to. To get the SHA of a commit, we can use the log command and copy the long hex string.

Commits exist in the .git/objects directory with the first 2 letters as a directory and the remaining 38 as a file.

`git cat-file -p <some-sha>`: Echo contents of SHA.

### Key Concepts

- tree : tree is analagous to directory.
- blob : blob is analogous to file.

> Git does not store diffs, git stores complete version of the entire source at the point of each commit. In other words, each commit contains all the information to completely reconstruct the srouce code that was tracked.

## Branching, Merging, and Rebasing in Git

### Git Configuration and Locations

`--add`
Every key is made up of two distinct parts section and keyname

`git config --add <section> <keyname> <value>`

Create and add 3 values with the section fem (frontend masters but different keynames and we will use this for our platform and exploration).

fem.dev is great
fem.marc is ok
fem.git would

```
git config --add fem.dev "is great"
git config --add fem.marc "is ok"
git config --add fem.git "would"
```

#### Listing out values

- --list : where it will list out entirety of config.
- --get-regexp <regex> : this takes a pattern and looks for all names matching.

```
git config --list | grep fem
```

or

```
git config --get-regexp fem
```

#### Unsetting

You can "unset" a value and you can remove an entire section.
-- unset : Unsets one key.
-- unset-all : Unsets all matching keys.

```
git config --unset fem.dev
```

Removes the section : 
```
git config --remove-section fem
```

#### Locations
`--local` : for repository level settings.
`--global` : for global level settings.

when using --get --add --list --unset.

### Creating and Managing Branches

Sometimes you need a feature that is developed off the main line, such that you can return to the main line, update the code, branch off and perform some immediate fix.

This is where branches comes in. 

#### Creating a Branch
the command used to create branch : 
```
Create branch `foo`
git checkout -b <branch name>
```



#### Viewing Branches
``` git branch ```

#### Switching Branches
```
git branch <branch name>
git checkout <branch name>
```
`checkout` is a more versatile operation.
#### Delete Branch
`-d` or `-D` can be used to delete branch.

### Merging Branches
A merge is attempting to combine two histories together that have diverged at some point in the past. 
There is a common point between the two, referred to as the best common ancestor (often called 'merge base' in the documentation).

#### How to merge
The branch you are on is the target branch and the branch you name is <branch name> will be the source branch.

```
git merge <branch name>
```

### Rebasing Branches



### Understanding HEAD and Reflog

HEAD: a pointer to what we are currently using.

reflog : shows you where head has been.
```
git reflog
```

## Working with Remote Repositories

### Remote Git and Fetching
A remote is just a copy of repo somewhere else.

```
git remote add <name> <uri>
```

```
git remote -v
```

will list out your remotes and their locations.

#### Gitism
Remote is the project repo; typically, when you have a remote git repo, it is called `origin`. This is the source of truth repo.

Remote is your fork of some other repo
sometimes you have your remote repo (fork), which you will name origin and you have the project repo which is typically named upstream.

```
git fetch
```

### Pulling Changes
Fetches the changes and merges the changes.

```
git pull <remote> <branch>
```


### Pushing Changes
```
git push <remote> <local name> : <remote name> # allows you to push and have it received with a different name.
```
```
git push <remote> : <remote name> #will delete a branch on the remote
```

## Resolving Conflicts

### Using Git Stash
git stash will take every change tracked by git (change to index + change to work tree) and store that result, much like commit, into the "stash".

> Stash is a STACK of temporary changes.

```
git stash
```
```
git stash -m "<your message>"
```
Stashes can be listed out :

```
git stash list
```
pop the latest stash
``` 
git stash pop

git stash pop --index <index>
```
