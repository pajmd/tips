## TOC

1. [create a new local repo](#create-a-new-local-repo)
1. [Push newly created repo](#Push-newly-created-repo)  
1. [MASTER vs HEAD](#MASTER-vs-HEAD)
4. [diff with a remote branch](#Diff-With-a-remote-branch)
5. [Configure a remote repo](#Configure-a-remote-repo)
6. [Synchronize](#Synchronize)
6. [diff](#diff)
6. [Delete branches](#Delete-branches)
6. [Create a new branch from the current branch](#Create-a-new-branch-from-the-current-branch)
6. [Starting a project](#Starting-a-project)
6. [Create a new empty branch](#Create-a-new-empty-branch)
6. [Branches](#Branches)
6. [Tag vs Release](#Tag-vs-Release)
6. [GitHub Ssh Keys](#GitHub-Ssh-Keys)
6. [](#)
6. [](#)
6. [](#)
6. [](#)


#### create a new local repo
In the folder where the project is [located]
```
gini init
```
Add files to it
```
echo "# This is a readme" > README.md
```
Stage the newly created file.  
__Note:__ In git terms stage and add are synonymous
```
git add README.md
```
Commit the newly staged file.
```
git commit -m "Some comments"
```

#### Push newly created repo 
**Note:** github gives the intructions after create the remote (origin) repo

* go to github to create the remote repo and obtain the repo url
* add the remote repo url to git config
```
git remote add <remote_name> <remote_repo_url>
ex:  
git remote add origin https://github.com/pajmd/bash_workspace.git
```
a ref on origin is created in .git/config  

* push local branch to the remote name
```
git push -u <remote_name> <local_branch_name>
ex:  
git push -u origin master
```

#### MASTER vs HEAD
master is a reference to the end of a branch. 
By convention (and by default) this is usually the main integration branch, but it doesn't have to be.  

HEAD is actually a special type of reference that points to another reference.  
It may point to master or it may not (it will point to whichever branch is currently checked out).  
If you know you want to be committing to the master branch then push to this.  

HEAD  
  |  
  v  
MASTER--> * <-- remotes/origin/master  
          |  
          *  *<-- remotes/origin/whatever  
          | /  
          *    
          
To check where the HEAD is pointing to :
```
git symbolic-ref HEAD
```


#### diff with a remote branch
```
git diff <masterbranch_path> <remotebranch_path>
```
ex:
```
git diff master origin/master
```
IN this case origin was the name we decided to call our remote repo (see below). It could
have been a different name: origin is just a convention

#### Configure a remote repo
Let us assume there is a remote repository named coworkers_repo that contains a feature_branch  
To configure it:
```
git remote coworkers_repo git@bitbucket.org:coworker/coworkers_repo.git
```
To fetch it:
```
git fetch coworkers feature_branch
```
It create a local copy of coworkers/feature_branch.  
To integrate this into our local working copy
* git checkout command to checkout the newly downloaded remote branch.
```
git checkout coworkers/feature_branch
```
The output from this checkout operation indicates that we are in a detached HEAD state. 
This is expected and means that our HEAD ref is pointing to a ref that is not in sequence 
with our local history. Being that HEAD is pointed at the coworkers/feature_branch ref, 
we can create a new local branch from that ref. 
The 'detached HEAD' output shows us how to do this using the git checkout command:
```
git checkout -b local_feature_branch
```
Here we have created a new local branch named local_feature_branch this puts updates HEAD 
to point at the latest remote content and we can continue development on it from this point.

#### Synchronize
To synchronize local repository with the central repository's master branch.
```
git fetch origin
```
To see what commits have been added to the upstream master:
```
git log --oneline master..origin/master
```

To approve the changes and merge them into your local master 

```
git checkout master  
git log origin/master
then
git merge origin/master
```
At this point origin/master and master branches now point to the same commit, and you are synchronized 
with the upstream developments

#### diff
* local diff  
to see the changes 
```
git status 
```
to see the difference for a specifi file  
```
git diff path/file
```
* remote diff  
first fetch the remote  
```
git fetch
```
to see all the branches  
```
git branch -a
```
to diff  
```
git diff <masterbranch_path> <remotebranch_path>
ex:  
git diff master origin/master

```

#### Delete branches
Local delete: 
 git branch -d <branch_name>  
 
Example deleting the master branch:
```
git branch -d master
```
Remote delete:  
git push -d <remote_name> <branch_name>
```
git push -d origin master
```

#### Create a new branch from the current branch 
__AKA copy of the current branch__
```
git checkout -b [name_of_your_new_branch]  
```
then push it to the remote
```
git push
```
### Starting a project
We need a master branch and first feature branches from which will create PRs and then merge to master
We could either create a remote git repo and then clone it or create a local one.  
#### Starting from Local:  
```
> git init
> git status
On branch master
> git branch dev
fatal: Not a valid object name: 'master'.

> touch file # necessary to commit in master and the create the branch
#
# file should actually be just a README.md for instance
# Only one file is enough (no need for f1)
#
> git branch dev
fatal: Not a valid object name: 'master'.
> git add .
> git commit -m "f1"
> git branch dev
> git diff master dev
> echo "hi" >> f1
> git diff master dev
> git add .
> git commit -m"hi"
> git diff master dev
diff --git a/f1 b/f1
deleted file mode 100644
> git ls-tree -r --name-only master
f1
file1
> git ls-tree -r --name-only dev
file1
> git checkout dev
Switched to branch 'dev'
> git pull . master
From .
 * branch            master     -> FETCH_HEAD
> git ls-tree -r --name-only dev
f1
file1
# create remote repo and add it to the config
> git remote add origin https://github.com/PyPajmd/todel.git
> git push -u origin master
> git push -u origin dev
> 

```


#### Create a new empty branch
__NOTE:__ Tricky not advisable
```
git checkout --orphan <branchname>
```
TO untack file that could have come from another previously current branch
```
git rm --cached -r .
```

To push it to the remote empty:  
First commit it as "allow-empty".   
Then untrack if any, files that could have been created in a previous branch.  
Physically remove them in case you want to switch to a previous branch withou working on these files.
Finally push it to the remote
```
git push -u origin master
git rm --cached -r .
rm file1 file2 ..
git commit --allow-empty -m "empty fot now"
git push -u origin master
```
##### Note:  
Master and initial are entirely different commit histories. Therefore pull request wont be possible.

### Branches
To show local branches:  
```
ls -al ./.git/refs/heads/
git branch
```
To show remote branches
```
ls -al ./.git/refs/remotes/
git branch -r
```
Branches can be fetch on a per branch basis or for the entire remote
```
git fetch <remote>  # Fetch all of the branches from the repository
git fetch <remote> <branch>  # only fetch the specified branch
git fetch --all  # etches all registered remotes and their branches
git fetch --dry-run  # a demo run of the command. 
```

### Tag vs Release
Release is a github concept based on Tag but with more features linked to it
On a local repo:  
```
git tag  -m "some comment abit the tag" <tag name ex: v0.0.0> <commitid>  # create a local tag. commitid is necessary if nothing was previouly tagged
git push --tag  # push the tag to the remote origin
git tag -l  # List all the tags
git log  # show the commit history along with tags
```

### GitHub Ssh Keys
One of the good things about using ssh keys with GitHub is that it integrates nicely with IDE like VScode.
After installing ssh key in GitHub, IDEs like VSCode no longer request credentials while communicating with GitHub.  
Idem for git CLI.  

To create a Ssh Key:
```
ssh-keygen  # specify in the key name what it is for ex: id_rsa_github
            # passphrase just <enter>
```
Copy over the public key to GitHub
```
Upper right corner dropdown select settings
choose SSH & GPG Keys
Right panel select SSH Keys - New key
```



### references
* https://www.atlassian.com/fr/git/tutorials
