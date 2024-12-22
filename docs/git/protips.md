# Git Protips

![type:video](https://www.youtube.com/embed/OgVQDtEf0w8)

## Definition

**Atomic commit**
> if you fix a bug, the fix should have all and nothing but the modification used for the fix.

## Configuration


Better show untracked files
```sh
git config --global alias.st status
git config --global status.showUntrackedFiles all
git config --global color.ui auto
git config --global color.status.untracked 'white red'
git config --global color.status.unmerged 'magenta italic'
```

## Stage

Show stage, see which files will be commited
```sh
git diff --staged
```

Show file state in stage
```sh
git show :0:<file>
```

See the content of the line which has changed 
```sh
git diff -w --word-diff
```

Cancel all stage
```sh
git reset
```
Cancel part of stage
```sh
git restore --staged index.html
```

## Edit commit

Modify the last commit
```sh
git config --global alias.oops commit --amend --no-edit
```
Use the alias
```sh
git add <files>
git oops
```
Erase from git but keep in current workspace (e.g S3 keys)
```sh
git rm --cached <files>
git oops
```
Change last commit message
```sh
git oops -m 'my new commit message'
```

## Stash

Put all current workspace to stash (```-u``` untracked ```-m``` to specify a stash name)
```sh
git stash push -u -m 'my stash message'
```
Show on prompt in shell when there is something in stash
```sh
export GIT_PS1_SHOWDIRTYSTATE=1
```
To get the stash back, avoid apply prefer pop to drop the stash entry once reapplied. ```--index``` restores the stage when stash.
```sh
git stash pop --index
```

## Logs

Better git log. ```abbrevCommit``` outputs the first 7 commits, which is generally sufficient.
```sh
LOG_FORMAT='%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%an %ad)%Creset'
git config --global alias.lg "log --graph --date=relative --pretty=tformat:'$LOG_FORMAT'"
git config --global log.abbrevCommit true
git config --global log.follow true
git config --global grep.extendedRegexp true
```
Git gui
```sh
gitk
```
Filtering git logs (a lot of options, by date, interval)
```sh
git lg --grep 'my pattern'
```