# Git training

__Please make additions to this file__

git is a command line tool which protects your code. 
git protects your code from your laptop melting, 
it protects it from other people messing it up, 
and it protects it from yourself!

## Git CLI

you can interact with git via an IDE (vscode, pycharm, etc.) or via the CLI.
We will work only with the CLI today.

A git command starts with `git`, it has a _command_ (like `commit`) and maybe has some _flags_ or _arguments_:
~~~
$ git <command> [<args | flags>]
~~~

## Note: 
* in this file the dollar sign `$` means it's something typed into the terminal, no `$` means it's something printed in the terminal.
* something wrapped in `<arbitrary-name>` means you should fill it in yourself. eg: `git branch <new-branch-name>` should be written as `git branch reformat_sql_ctes`

Tip: to make our lives easier, let's add a shortcut for something common (git the alias 'logdag' is arbitrary, you can give it any name you want)
~~~
$ git config --global alias.logdag 'log --oneline --decorate --graph --all'
~~~
Now we can call `git logdag` instead of the full `git log --oneline --decorate --graph --all`.



# Cheat sheet
Here we have a list of git nomenclature, commands and their usage.
Plus a list of _'HELP! I want to do X'_ and which commands to use.

Git has it's own builtin documentation you can access via:
~~~
$ git <command> --help
$ git stash --help
~~~


## Nomenclature
Git uses a bunch of specific terms, they don't always mean what you think:
* origin: this is a usually a pseudonym for 'GitHub'
* history: the branches and commits that record the committed changes in the repo.
* HEAD: this is the name for the point in the _history_ which you're currently looking at.
* tree: a file system of folders and files, you working directory is a tree.
* main/master: a common name for the default branch. _main_ is more cool these days, but they're just names.
* Git's three trees: git works with three tree (like) things: the HEAD in _history_, the _index_ or staging area and your actual working directory.
~~~
HEAD					Index					Working
(history)			(staging)			directory
(inside .git)	(inside .git)	(outside .git)
	|								|								|
	|								|<------add-----|
	|								|-----reset---->|
	|<----commit----|								|
	|								|								|
	|----------checkout------------>|
~~~

## Local work
life is simple if you don't play with others, you can make mistakes and nobody knows!
These commands are the fundamentals of working locally


### init
create a new repo in that folder (not to be confused with `clone`)
~~~ 
$ git init
Initialized empty Git repository in ...
~~~


### status
Shows you what git thinks of each file, is it tracked, staged, etc.
This all about where things are on the __Three Trees__: is a file on the _working directory_, or the _index_.
It answers the question: "if I do a commit now, what will be included"
~~~ 
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   eggplant.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   eggplant.py

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	aubergine.txt
~~~
Here git tells us that the _index_ contains some of the changes in `eggplant.py`, but some are not staged. We also have `aubergine.txt` wich git is not tracking, it won't be committed (or changed when using `$ git checkout`)


### checkout 👀
go and look at something (a branch, an earlier commit).
~~~ 
$ git checkout <branch-name>
Switched to branch 'feature'
~~~
Git change the files in your working directory to represent the branch `<branch-name>`.


### branch
create a branch
~~~ 
$ git branch <new-branch-name>
~~~
or list branches:
~~~ 
$ git branch -l
~~~


### commit
create a new commit on the branch you have selected
~~~
$ git commit --message "manually altered financials, don't tell the regulator"
~~~


### reset
This can do many things: 
* "un-add" some files: `$ git reset` will remove files from `index`, ie: remove the list to be committed
* __delete all__ uncomitted changes: `$ git reset HEAD --hard`


### stash
put your 'index' safely in storage. It's not committed, but it's saved somewhere safe.
This is super handy for safely storing your work in progress!
* add to your stash: `$ git stash`
* list your stash stack: `$ git stash list`
* get a stash back into your _working directory_: `$ git stash apply stash@{<stack-number>}`, eg: `$ git stash apply stash@{0}` 


### rebase 
This allows you to make alterations to commits, move a branch starting point, combine commits etc.
This is very powerful and useful, but needs practice.


### merge
combine two branches (usually done via GitHub website)
eg: if you want the `main` branch to receive what's on your `feature` branch:
~~~
git checkout main
git merge feature
~~~
__tip__: if git tells you it's failed because of a conflict, you can abort with `git merge --abort` (and ask someone for help)



## Remote
You've gotta engage your brain a bit more when working with others.
_Remotes_ are just other computers running git, usually at GitHub.com


### clone
copy a repo from github onto your computer.
~~~
$ git clone https://github.com/.../.git
~~~


### fetch
collect things from GitHub, but don't combine them into your local branches.
This can be useful if you think something funky has been done and you need to inspect it.
~~~
$ git fetch --all
~~~


### pull
get the latest from GitHub and update my _working directory_ to that state.
ie: this effectively does `git fetch` and `git merge <what we just fetched>`
~~~
$ git pull
~~~
If something _funky_ has been done then git may create a new commit to resolve this, if you're asked to `rebase` or `merge`, choose `merge`.


### push
send your local commits to GitHub.
~~~
$ git push
~~~


## _Happy Path_ Workflow
The most common way we will work with git is following the,
let's say we want to fix a dbt model called `dim_huge_profits.sql` and `main` is your default branch:
1. Get the latest copy of main/master with `$ git checkout main`, `$ git pull`
2. Create a new branch and go to it: `$ git branch fix_huge_profits` and `$ git checkout fix_huge_profits` (or `$ git checkout -b fix_dim_huge_profits`)
3. Make your fixes, change some files, etc.
4. Add your fixes for staging with `$ git add .`
5. Commit your staged changes with `$ git commit -m "fixed negative sign error in dim_huge_profits"`
6. Push your changes to GitHub with `$ git push`
7. Go to GitHub.com and see start the pull request -> review -> merge process


## I have a problem, what do I do!
Lets look at some common problems and how to tackle these.
If you're unsure, first step is usually to __ask for help__.

For most of these problems, they can be solved by using `reset` in different ways, but this can be risky.
If you make a mistake you can lose work, overwrite someone else's work, create merge conflicts, etc.


### HELP I've made my commit but need to make a change!
If you __haven't__ pushed your commit, then _ammend_ it.
Stage the changes you _should_ have included in the commit.
Then use `$ git commit --ammend` to add them to the previous commit (use `--no-edit` if you're happy with the commit message) 



### HELP I've committed to the wrong branch!
Two options:
* You can move your commit to another branch by taking them out of git (using `reset`) and then committing them where you want.
* __Preferred:__ You can ask git to __copy__ the commit to a new branch using `$ git cherry-pick` (you can then delete the original commit later with `reset`)
	1. find the _hash_ of the commit in question, eg: `8714ea3`
	2. Move to the destination branch: `$ git checkout <the-branch-I-should-have-committed-to>`
	3. tell git to copy the commit to __here__: `$ git cherry-pick 8714ea3`
	4. Optional: remove the commit from 


### HELP I've added a secret to my commit!
First, can you invalidate the secret, if it's a password can you change it?
Assuming you haven't pushed this commit:
1. `$ git reset HEAD~1 --soft`
2. Remove the secret from the file.
3. `$ git add .`, `$ git commit -m "<same message as before...>`


### HELP I've committed to a branch that was already merged!
One option: forget about it. It's not so pretty, but it won't do much harm.
If you _really_ want that commit to be on a different/new branch, 
then you can use `cherry-pick` in the above _wrong branch_ example.
