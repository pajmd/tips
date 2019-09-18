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

#### Synchonize
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
