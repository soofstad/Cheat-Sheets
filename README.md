# Linux-Sheet
My Linux cheat-sheet
## Disc Usage
Disc usage for directories in GB, sorted.
-s=summarize total -BG=Scale size to GB  -h=human readable

`$du -shBG ./*/ | sort -n`
## Secure Copy
Move files and folders between computers.
-C=Compress -r=Copy directories recursively

`scp -Cr /data/backup/gitlab stoo@10.36.33.122:/home/stoo/gitlab`
