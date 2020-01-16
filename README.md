# ScalableDayTrader_SENG468
A scalable day-trading system for "Software System Scalability" course

## Development

#### Getting started:

Optional: make a code folder in your home directory. This will simplify shared scripts.
```
cd ~
mkdir code
```

Clone the repo:
```
cd ~/code
git clone https://github.com/JVerwolf/320_project.git

```

To make a virtualenv with python3:
```
cd ~/code/320_project
pip install virtualenvwrapper
mkvirtualenv --python=/usr/bin/python3 320_project
```

To install the dependencies:
```
# From within the app root directory:
workon 320_project
pip install -r requirements.txt
```

#### Git Workflow:
A branching workflow keeps things sane when multiple devs are working on
the same codebase.  
* A branch should represent the smallest atomic
addition of functionality (ideally).  
* In order to stay up to date, rebase your branch onto `origin/master` often.
* In order to keep others up to date, and reduce merge conflicts on your behalf,
don't merge your branch as soon as possible, adhere to the first point
(within reason).
* We will use a rebasing workflow to keep our master branch linear.

###### Starting new unit of work (branch):
```
git checkout master
git fetch

# Note: make sure you are now on master, and do not have any work on master, because the next step will nuke it. Use `git stash` before-hand if you do.
git reset --hard origin/master 
git checkout -b 'branch-name-goes-here'
```

###### Checking the staging area:
Git performs action on files that have been placed in the staging area, which acts as a triage.
To see what files are in the staging area:
```
git status
```

###### To add a file/directory to the staging area:
```
# To add a single file
git add some_file.py

# To add every file in the current directory:
git add .
```

###### To `commit`, i.e. save files in staging area to the local git repo:
```
git commit -m "A <50 char title describing WHAT functionality the commit adds

An optional message body describing WHY.  Note: the commit should not go into 
the WHATs of specific details, as it is redundant, since this is what the commit is storing"
```
NOTE: [Commit Message Best Practices](https://chris.beams.io/posts/git-commit/)

NOTE: Do not commit compilation artifacts, editor config files, and random junk.

###### To push your branch to the remote repository (gitHub):
```
git push origin branch-name 
```

###### To merge your work into master:
Note: We use a `no-ff` merge because we have rebased onto master (so 
that that our master branch stays linear), and we want to generate a merge
commit for clarity/posterity. The message has the form "Merge branch ..." 
so that it is easy to spot merge commits in the log.
```
git checkout your-branch-to-merge
git fetch
git rebase origin/master

#Note: if you have merge conflicts at this stage, you will need to fixt them.  After, repeat from the beggining.
git checkout master

# Note: make sure you are now on master and do not have any work on master, because the next step will nuke it.
git reset --hard origin/master
git merge --no-ff 'your-branch-name' -m "Merge branch 'your-branch-name'"
git push origin master
```

###### NEVER DO THIS:
Never edit public history.  You can always edit/change local and remote
history on your own branches (using commit amending, interactive rebasing,
resetting, force pushing, etc.).  However, you should NEVER change history on 
`remote/origin/master`, and you should never edit other people's branches. 
(Unless you _really_ know what you are doing and have the author's permission).



## Appendix:

#### Virtualenv:
The virtualenv is used to install requirements in a local scope.
Without the virtualenv, pip would have to install the requirements 
globally.  This could lead to dependency conflicts and namespace 
pollution.  The virtualenv lives at this filepath on your machine 
(this is where pip installs
libraries):
`~/.virtualenvs/320_project`

The virtualenv must be activated in order for python to know how to
find the libraries in it's `import` statements, and in order for pip
to install the dependencies in the right location.  (This is effectively 
done with symlink magic).

```
# To activate the virtual env:
workon 320_project

# To deactivate the virtual env:
deactivate 320_project

# To delete the virtualenv, and all deps installed inside:
rmvirtualenv 320_project
```


#### Installing a New Dependency

```
# Ensure the virtual env is activated:
workon 320_project

pip install my-new-dep
```

Then add the dependency to the `requirements.txt` file:
```
pip freeze >> requirements.txt
```
Then rearrange the dep to be in alphabetical order in 
the `requirements.txt` file.

#### Other useful git commands:
```
# See log of git commits:
git log

# See branches:
git branch

# Selectively add to staging area:
git add your-file -p

# To see what parts of the file have chaged:
git diff
git diff .
gif diff your-file.py

# Interactive Rebase For Changing History (only change your own history):
git rebase -i <commit hash, ie 23lk2j34lk23lk4>

# To add changes to your last commit:
git add Changed_file.py
git commit --ammend

```
