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
