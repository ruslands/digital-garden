  
# squash commits in one

# will make a soft reset of the last 20 commits. This leaves your changes in the files, but removes the commits.

git reset --soft HEAD~20 

# Add files for the commit.  
git add …  
git commit -m "commit message goes here"

Deleting the .git folder may cause problems in your git repository. If you want to delete all your commit history but keep the code in its current state, it is very safe to do it as in the following:

1. Checkout  
    git checkout --orphan latest_branch
2. Add all the files  
    git add -A
3. Commit the changes  
    git commit -am "commit message"
4. Delete the branch  
    git branch -D main
5. Rename the current branch to main  
    git branch -m main
6. Finally, force update your repository  
    git push -f origin main

PS: this will not keep your old commit history around

  
Developing creating middle branch  
  
Stage -> Epic branch -> Feature branch  
Feature branch merge without squash to Epic branch merge to Stage with squash

Удаление удаленного репозитория: git remote rm REPO_NAME  
Удалить локальный репозиторий: rm -rf .git*

# get remote branch

git branch -f remote_branch_name origin/remote_branch_name  
git checkout remote_branch_name

# delete remote branch

git push <remote_name> --delete <branch_name>

git rm --cached <file>

# fix untracked folder (modified) / remove remote folder

git  rm -rf --cached <folder_name>

git commit -m 'Remove the now ignored directory "some-directory"'  
git push origin master

git rm -r --cached allpapers-storage/postgresql/

#create empty branch

git checkout --orphan V2.2

git add --all

git commit -a

git log # history started from scratch

# show difference between

git diff HEAD

# Перезаписать локальные файлы во время git pull

git fetch --all

git reset --hard origin/master

git reset --hard commit_sha

git rm --cached file

# list all the files currently being tracked under the branch master

git ls-tree -r master --name-only

## delete commits

git push  frontend master --force

# удалить незакомиченные изменения

git stash save --keep-index

git stash drop

# force push

git push -f wide master

# merge hot fix to master

git checkout master

git merge hotfix

git push wide master

git merge --strategy-option theirs

git ls-tree -r stage --name-only # tracked files

#Deleted in source but modified in destination

#git mergetool (first need to install KDiff)

# Your local changes to the following files would be overwritten by merge:

Вариант 1. - Делаете коммит, а котом git pull и оно вам предложит смержить.

Вариант 2. - Делаете git stash (прячет все незакомиченые локальные изменения); затем git pull; затем git stash pop, а дальше мержите.

Вообще есть третий вариант (на будущее) - создаете локально еще одну ветку, в ней работаете, а потом делаете git pull в основной ветке (точнее в той, от которой вы отпочковались) и мержите с ней свою ветку. (опять же, можете сделать stash, затем "отпочковаться", затем слить изменения с репозитория и потом stash pop в свою ветку, а затем мержитесь, когда надо, с той веткой, откуда отпочковались).

Тут у вас есть несколько проблем:

1. команда git pull это на самом деле алиас для git fetch + git merge

работает она так, сначала через git fetch получает новое состояние ветки из origin, а потом передает управление git merge, он в свою очередь если ваша ветка разошлась делает merge, если нет то делает fast forward как следствие могут появиться не нужные комиты вида "Merge branch "test" to "test""

По этому советую вместо git pull делать всегда git fetch, а потом смотреть git status а там уже либо git rebase origin/test либо git pull

2. так же как вам уже сказали выше у вас есть изменения в локальных файлах, в целом в этом нет ничего плохого, их можно либо спрятать через git stash либо сделать комит

# Скачать отдельную папку с GIT. Нужно в пути добавить trunk вместо tree/master  
svn checkout [https://github.com/Yorko/mlcourse_open/trunk/jupyter_notebooks/topic05_bagging_rf](https://github.com/Yorko/mlcourse_open/trunk/jupyter_notebooks/topic05_bagging_rf)