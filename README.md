# Linux-Sheet
My Linux cheat-sheet
## Disk Usage
Disk usage for directories in GB, sorted.
-s=summarize total -BG=Scale size to GB  -h=human readable

`du -shBG ./*/ | sort -n`
## Secure Copy
Move files and between computers.
-C=Compress -r=Copy directories recursively

`scp -Cr /data/backup/gitlab stoo@10.36.33.122:/home/stoo/gitlab`
## Rsync big files
Move large files between computers.
-v=verbose -a=archive, keep file structure and properties -z=compress -P=Show progress and keep partial transfered files

`rsync -vazP /data/backup/gitlab stoo@10.36.33.122:/data2/tempCP`

## Detached consol
Create a de-/re-atacheable console.
Create `screen`
Detache `ctrl-a d`
Re-attache `screen -r`
