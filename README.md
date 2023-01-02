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
$ git config --global user.name "..."
$ git config --global user.email "...@example"
~~~


# Cheat sheet
Here we can fill in our own descriptions for each command. (you can see an example on the branch 'cheatsheet')


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

### commit

### reset

### stash

### rebase 

### merge


## Remote


### clone


### fetch


### pull


### push




## Git's three trees:


HEAD			Index			Working
(history)		(staging)		directory
	|				|				|
	|				|<------add-----|
	|				|-----reset---->|
	|<----commit----|				|
	|								|
	|----------checkout------------>|