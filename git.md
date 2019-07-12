# My Cheat-sheets
An encyclopedia for those who _"actually know this"_
<style>
    a   {font-size:1.5em}
</style>
<div style='display:flex; justify-content:space-around;'>
    <a href="linux">
        Linux
    </a>
    <a href="python">
        Python
    </a>
    <a href="git">
        Git
    </a><a href="psql">
        PSQL
    </a>
    <a href="react">
        React
    </a>
</div>  

# Git

## Stash
Save you work for later  
`git stash`  
List stashes  
`git stash list`  
Apply your latest stash  
`git stash apply`  
Aply a spesific stash  
`git stash apply stash@{2}`

## Rebase

Make sure you have the latest master version  
`git checkout master`  
`git pull`  
Back to your branch...  
`git checkout <mybranch>`  
Squash my branch first  
`git rebase -i HEAD~4`  
`git rebase --onto master`

## Remove file from history

`git filter-branch --tree-filter 'rm (-rf) filename' HEAD`

## Pushing to a remote branch with a different name

`git push origin local-name:remote-name`

## Undo the act of commiting, but keep changes

`git reset --soft HEAD^`

## Update remote origin

`git remote set-url origin git@github.com:ORG/REPO.git`

## Permanently discard all local changes
All files  
`git reset --hard`  
Some files  
`git checkout -- file01.txt file02.txt`
