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

# Python-Sheet
My Python cheat-sheet
## Virtual Environment
Install the package<br>
`sudo apt install virtualenv`

Create the virtualenv in the projects directory(optional specify py-version)<br>
```
cd /home/stoo/myPyProject
virtualenv -p python3 venv
```

Source the newly created venv<br>
`source venv/bin/activate[.fish]`

Install any desired python package in the venv<br>
`(venv)pip install MyPyPackage`

Export all installed packages to a file<br>
`(venv)pip freeze > requirements.txt`

Install packages from file<br>
`pip install -r requirements.txt`
