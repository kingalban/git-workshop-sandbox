# Git training

git is a command line tool which protects your code. 
git protects your code from your laptop melting, 
it protects it from other people messing it up, 
and it protects it from yourself!

## Git CLI

you can interact with git via an IDE (vscode, pycharm, etc.) or via the CLI.
We will work only with the CLI today.

A git command starts with `git`, it has a _command_ (like `commit`) and maybe has some _flags_ or _arguments_:
~~~
git <command> [<args | flags>]
~~~


Housekeeping:
~~~
git config --global user.name "..."
git config --global user.email "...@example"
~~~

To make our lives easier, let's add a shortcut for something common.
git the alias 'logdag' is arbitrary
~~~
git config --global alias.logdag 'log --oneline --decorate --graph --all'
~~~
Now we can call `git logdag` instead of the full `git log --oneline --decorate --graph --all`.


# Cheat sheet
Here we'll fill in our own descriptions for each git verb


## Local work
life is simple if you don't play with others, you can make mistakes and nobody knows!
These commands are the fundamentals of working locally


### init
create a new repo in that folder


### status
show pending changes


### checkout 👀
go and look at something (a branch, an earlier commit)


### branch
create a branch (and more)
__handy__: see all branch options `git branch --list --all` 


### commit
create a new commit
__usage__: `git commit --message "manually altered financials, don't tell the regulator"`


### reset
'un-add' some files
or go to an earlier commit

### stash
put your 'index' safely in storage. It's not committed, but it's saved somewhere safe.


### rebase 
rewrite history


### merge
combine two branches (usually done via GitHub website)
eg: if you want the `main` branch to receive what's on your `feature` branch:
~~~
git checkout main
git merge feature
~~~
__tip__: if git tells you it's failed because of a conflict, you can abort with `git merge --abort` (and ask someone for help)



## Remote
You've gotta engage your brain a bit more 


### clone
copy a repo from github onto your computer


### fetch
collect things from GitHub, but just put them on the side


### pull
get the latest from GitHub and put it in front of me.
ie: this effectively does `git fetch` and `git merge <what we just fetched>`
(if you're asked to `rebase` or `merge`, choose `merge`)


### push
send your local commits to GitHub



## Git's three trees:

HEAD			Index			Working
(history)		(staging)		directory
	|				|				|
	|				|<------add-----|
	|				|-----reset---->|
	|<----commit----|				|
	|								|
	|----------checkout------------>|