# Git proven ways
best practices or proven ways for using git in light of various circumstances

## Convention used
Some conventions have been used to properly describe how to use git in a better way to solve problems.  

1. `origin`: to denote the remote repository of local repository
2. `upstream`: to denote the remote repository which has been forked to create the current repository


## Proven ways

### Push local repo to remote
```bash
# initialize local directory as git repository
$ git init
# add changes if any
$ git add .
# commit changes
$ git commit -m "some appropriate commit message"
# git creates master branch by default
# let's rename it to main to support github's movement
# to remove master-slave related terms 
$ git branch -m master main
# add remote
$ git remote add origin <remote-repo-url>
# check remote repo
$ git remote -v
# push changes to origin
$ git push -u origin main
```


### Pull changes from remote repo(i.e. upstream branch) after committing some changes in local repo(i.e. local branch)
When working on a feature branch, often we see that corresponding upstream branch has been updated. And sometimes, we feel the need to include the changes of updated upstream branch into our feature branch. For that, we can just do `$git pull @{u}`, or `$git pull --rebase @{u}`, but `git pull` has a problem here, and that is, it creates a new merge commit as it's a 3-way merge and we don't want that, cause, we want our git history to be clean and very much readable. So, we need a proper way to include upstream changes into our feature branch.  
  
To, solve the problem, we can fetch the upstream changes and then, at first try to do a `--ff-only` merge. If that's not possible, then we try `rebase with preserve merges` i.e. `rebase --preserve-merges`, and also verify the merge using `log`, so that, we may ensure everything is okay.

```bash
# download latest changes i.e. commits
# note: here <remote-name> is optional, if it is not specified
#       then, all remote will be updated i.e. new commits will
#       be fetched
$ git remote update <remote-name> -p
# update the local branch with new commits from its upstream
# in a fast forward only
# note: we're trying to avoid creating new commits
$ git merge --ff-only @{u}
# if the above fails, that means, local branch and remote branch both has
# received commits after being seperated from common commit
# note: -p stands for --preserve-merges which ensures merge histories stay in order
$ git rebase -p @{u}
# its better to review the results to push changes to remote if you use git rebase -p @{u}
$ git log --graph --oneline --decorate --date-order --color --boundary @{u}..
```
  
To know details about this procedure, please take a look at the following links:
1. [Why am I merging “remote-tracking branch 'origin/develop' into develop”?](https://stackoverflow.com/questions/6406762/why-am-i-merging-remote-tracking-branch-origin-develop-into-develop)
2. [What exactly does git's “rebase --preserve-merges” do (and why?)](https://stackoverflow.com/questions/15915430/what-exactly-does-gits-rebase-preserve-merges-do-and-why)
  
In the light of the above discussions, its certain that, this procedure is much better and safe to use compared to `git pull --rebase`, specially when care about merge commits and a cleaner and proper history record.
