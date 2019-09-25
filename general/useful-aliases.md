# aliases

A few of my favorite aliases:

```
alias gs="git status"
alias gac="git add . && git commit -m"
alias gp="git push"
```
I have talked about these aliases before; they make interacting with Git just a little bit easier. Hat-tip to Ire Aderinokun for the idea.

```
alias gits="find ./ -name '.git' 2> /dev/null"
```
This neat snippet searches the current directory, and all sub directories, for Git repos. I have a lot of coding projects in the works, and this helps me keep track of them.

```
alias r="source ~/.bash_profile"
```
This lets me refresh my bash profile whenever I make a change — for example, adding a new alias or script. Simple, and handy.

```
alias search="grep -rnw './' -e "
```
This alias gives me search functionality in the terminal, so I can avoid opening up Finder for most simple queries.

```
alias tree="find . -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'"
```
This neat little command displays the current directory contents, and all its sub-directories, as a tree. I used this to create the tree in the First Crack readme file. Hat-tip to OS X Daily for this one.