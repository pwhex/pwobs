

First let's revise the difference between ```git pull``` and ```git fetch```. 
git pull copies changes from a remote repository directly into our working directory.
![[Pasted image 20240927220351.png]]

A point to remember is: an aborted git pull is a fetch.

```git fetch --prune``` is something I learned today.
I deleted some branches (both locally and remote) from a PC and the changed didn't reflect on another PC. This is when I found the git fetch --prune command as the solution. 
git fetch --prune (prune means cut back) is used to remove outdated remote branches from a shared repository.
1. Connects to the remote repository and fetches all remote branch references.
2. Deletes remote references that are no longer in use on the remote repository



