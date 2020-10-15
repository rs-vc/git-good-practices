# Git proven ways
best practices or proven ways for using git in light of various circumstances

## Convention used
Some conventions have been used to properly describe how to use git in a better way to solve problems.  

1. origin: to denote the remote repository of local repository
2. upstream: to denote the remote repository which has been forked to create the current repository


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


### Pull changes from remote repository after committing some changes in local repo
```bash
# download latest changes i.e. commits
$ git remote update -p
# update the local branch
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
