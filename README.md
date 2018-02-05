# Linux-Sheet
My Linux cheat-sheet
## Disk Usage
Disk usage for directories in GB, sorted.<br>
-s=summarize total -BG=Scale size to GB  -h=human readable <br>
`du -shBG ./*/ | sort -n`
## Iptables
Long list rules in chain 'INPUT'<br>
`iptables -L INPUT --line-numbers -v`<br>
Set chain policy<br>
`iptables --policy INPUT ACCEPT`<br>
New rule<br>
`iptables --append INPUT --in-interface eth0 --out-interface lo --source 10.36.0.0/16 --destination 10.36.0.0/16 --protocol tcp --dports 22 --match comment --comment "004 accept SSH connections" --jump ACCEPT`<br>
Delete by number<br>
`iptables --delete INPUT 3`<br>
Delete all in chain<br>
`iptables --flush INPUT` <br>
Delete all<br>
`iptables --flush`<br>
## Secure Copy
Move files and between computers.<br>
-C=Compress -r=Copy directories recursively <br>
`scp -Cr /data/backup/gitlab stoo@10.36.33.122:/home/stoo/gitlab`
## Rsync 
Move large files between computers.<br>
-v=verbose -a=archive, keep file structure and properties -z=compress -P=Show progress and keep partial transfered files -e=specify remote shell<br>
`rsync -vazPe ssh /data/backup/gitlab root@10.36.33.122:/data/dokcer/dockerfiles/gitlab/data/`

## Format Output
Select field/column. <br>
`cut --delimiter ":" --fields 2`<br>
Remove specific characters.<br>
Deletes whitespace, double quotes, and comma.<br>
`tr --delete [:blank:]\",`<br>
Replace string.<br>
's' is the sed program. Replaces 'foo' with 'bar', option '/g' for all occurrances.<br>
`sed "s/foo/bar/g"`


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

## Logical Volume Manager (CentOS/RHEL)
System to manage physical volumes, logical volumes, and volume groups.<br>
List
```
pvs
lvs
vgs
lsblk (ls partitions)
```
Add new Physical Volume to a LVM system<br>
`pvcreate /dev/<device_name>`

Create new Volume Group <br>
`vgcreate volume_group01 /dev/disk01 /dev/disk02`

Extend Volume Group <br>
`vgextend volume_group01 /dev/disk03`

Create Logical Volume in Volume Group. -L=Size bytes -l=Size %<br>
`lvcreate -n myDiskName -l 100%VG (-L50G) volume_group01`

Extend or Reduce Logical Volume. -r=Resize filesystem -L=(+||set) <br>
`lvextend(reduce) /dev/volume_group01/logical_volume01 -L+50G -r`


## GlusterFS Administration

