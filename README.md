# Linux-Sheet
My Linux cheat-sheet
## Disk Usage
Disk usage for directories in GB, sorted.<br>
-s=summarize total -BG=size to GB  -h=human readable <br>
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
's' is the substitution command. Replaces 'foo' with 'bar', option 'g' for all occurrances.<br>
`sed "s|foo|bar|g"`

## Archive and compress
To archive and compress a folder, keeping permissions<br>
-c=create archive -v=verbose -z=compress with gzip -f=file name type -p=preserve-permissions <br>
`tar cvzfp myArchive.tar.gz /myFolder-to-archive` <br>
To uncompress tar.gz<br>
-x=extract <br>
`tar -xvzf myArchive.tar.gz`

## Create a file by a Here Document
```
cat << EOF > /etc/apt/apt.conf
Aquire::http::proxy "http://www-proxy.statoil.no:80";
Aquire::https::proxy "http://www-proxy.statoil.no:80";

EOF
```

## Create files and fill with random data
For testing...
```
touch myFile{1..10}.txt
head -c 5M </dev/urandom > myFile10.txt
# find . -name "*.txt" -exec head -c 5M </dev/urandom > {} \;
```
## Users and Groups
Add a new system user 'john' with PID < 500 and no homedir.<br>
`useradd -r john`<br>
Give john a password.<br>
`passwd john`<br>
Delete john and his homedir.<br>
`userdel john -r`<br>
Create a new group called 'hockey'.<br>
`groupadd hockey`<br>
Add user 'john' to the group 'hockey' as a supplementary group.<br>
`gpasswd --add john hockey`<br>
Delete user 'john'from the group 'hockey'.<br>
`gpasswd --delete john hockey`<br>
Delete group 'hockey'.<br>
`groupdel hockey`<br>

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
## Mount a device
To list devices and mountpoint;<br>
`lsblk`<br>
Might need to make a file system on the disk first...<br>
`mkfs --type ext4 /dev/sdb`<br>
Create a dir to mount in and then mount.<br>
```
mkdir /data
mount /dev/sdb /data
```
Consider adding to fstab.<br>
To get UUID of all devices;<br>
`blkid`<br>
Edit /etc/fstab<br>
`UUID=41c22818-fbad-4da6-8196-c816df0b7aa8  /data      ext4    defaults,errors=remount-ro 0       1`

## Logical Volume Manager
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


## ZFS Administration
https://docs.oracle.com/cd/E19253-01/819-5461/index.html
#### `zpool` for pool administration, `zfs` for filesystem administration.<br>
Create pool named "tank" with mirror on sda1 and sdb2, sdc3 as cache and mount point "/data"<br>
`zpool create tank mirror sda1 sdb2 cache sdc3 -m /data`<br>
Extend pool<br>
`zpool add tank sdd4`<br>
List pool information<br>
```
zpool status
zpool list
zpool iostat
```
List, get, and set properties<br>
```
zfs get all tank
zfs get compression tank
zfs set compression=on tank
```
Create snapshot<br>
`zfs snapshot tank@snapname`<br>
List Snapshots<br>
`zfs list -t snapshot`<br>
Rollback to snapshot<br>
`zfs rollback tank@snapname`<br>



