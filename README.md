# Linux-Sheet
My Linux cheat-sheet
## Disk Usage
Disk usage for directories in GB, sorted.<br>
-s=summarize total -BG=Scale size to GB  -h=human readable

`du -shBG ./*/ | sort -n`
## Secure Copy
Move files and between computers.<br>
-C=Compress -r=Copy directories recursively

`scp -Cr /data/backup/gitlab stoo@10.36.33.122:/home/stoo/gitlab`
## Rsync 
Move large files between computers.<br>
-v=verbose -a=archive, keep file structure and properties -z=compress -P=Show progress and keep partial transfered files -e=specify remote shell

`rsync -vazPe ssh /data/backup/gitlab root@10.36.33.122:/data/dokcer/dockerfiles/gitlab/data/`

## Detached consol
Create a de-/re-atacheable console.<br>
Create    `screen`<br>
Detache   `ctrl-a d` <br>
List      `screen -ls` <br>
Re-attache `screen -r` <br>
Kill      `ctrl-a :quit` <br>
## Archive and compress
To archive and compress a folder, keeping permissions<br>
-c=create archive -v=verbose -z=compress with gzip -f=file name type -p=preserve-permissions

`tar cvzfp myArchive.tar.gz /myFolder-to-archive` 

To uncompress tar.gz<br>
-x=extract

`tar -xvfz myArchive.tar.gz`
