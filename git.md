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
My Git cheat-sheet
## Git Rebase
Make sure you have the latest master version  
`git checkout master`  
`git pull`  
Back to your branch...  
`git checkout <mybranch>`  
Squash my branch first, do the rebase interactively, and rewrite last 4 commits    
`git rebase -i master HEAD~4`

## Permanently discard all local changes
All files  
`git reset --hard`  
Some files  
`git checkout -- file01.txt file02.txt`