# Linux-Sheet
My Linux cheat-sheet
## Disk Usage
Disk usage for directories in GB, sorted.<br>
-s=summarize total -BG=Scale size to GB  -h=human readable <br>
`du -shBG ./*/ | sort -n`
## Secure Copy
Move files and between computers.<br>
-C=Compress -r=Copy directories recursively <br>
`scp -Cr /data/backup/gitlab stoo@10.36.33.122:/home/stoo/gitlab`
## Rsync 
Move large files between computers.<br>
-v=verbose -a=archive, keep file structure and properties -z=compress -P=Show progress and keep partial transfered files -e=specify remote shell<br>
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
-c=create archive -v=verbose -z=compress with gzip -f=file name type -p=preserve-permissions <br>
`tar cvzfp myArchive.tar.gz /myFolder-to-archive` <br>
To uncompress tar.gz<br>
-x=extract <br>
`tar -xvfz myArchive.tar.gz`
## Create files and fill with random data
For testing...
```
touch myFile{1..10}.txt
head -c 5M </dev/urandom > myFile10.txt
# find . -name "*.txt" -exec head -c 5M </dev/urandom > {} \;
```

## Permissions
Change owner and group recursively<br>
`chown -R root:stig <directory>`<br>
Set "fine-grained"<br>
`chmod ugo[+-=]rwx <file>`<br>
Add additional user and set permissions recursively <br>
`setfacl -R -m u:stig:rwx <directory>` <br> 

## SSH Access
To get passwd-less root ssh login.<br>
```
[client]
cd ~/.ssh
ssh-keygen -t rsa
(accept defaults, copy content of id_rsa.pub)
[server]
nano /root/.ssh/authorized-keys
(paste the public key)
cat >> /etc/security/access.conf
+ : root : <client-IP>
ctrl+c
```
