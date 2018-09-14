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

# Linux-Sheet
## Users and Groups
Add a new system user 'john' with PID < 500 and no homedir.  
`useradd -r john`  
Give john a password.  
`passwd john`  
Delete john and his homedir.  
`userdel john -r`  
Create a new group called 'hockey'.  
`groupadd hockey`  
Add user 'john' to the group 'hockey' as a supplementary group.  
`gpasswd --add john hockey`  
Delete user 'john'from the group 'hockey'.  
`gpasswd --delete john hockey`  
Delete group 'hockey'.  
`groupdel hockey`  

## Permissions
Change owner and group recursively  
`chown -R root:stig <directory>`  
Set "fine-grained"  
`chmod ugo[+-=]rwx <file>`  
Add additional user and set permissions recursively   
`setfacl -R -m u:stig:rwx <directory>`    

## Disk Usage
Disk usage for directories, in GB, sorted.  
-s=summarize total -BG=size to GB  -h=human readable   
`du -shBG ./*/ | sort -n`
## Iptables
Long list rules in chain 'INPUT'  
`iptables -L INPUT --line-numbers -v`  
Set chain policy  
`iptables --policy INPUT ACCEPT`  
New rule  
`iptables --append INPUT --in-interface eth0 --out-interface lo --source 10.36.0.0/16 --destination 10.36.0.0/16 --protocol tcp --dports 22 --match comment --comment "004 accept SSH connections" --jump ACCEPT`  
Delete by number  
`iptables --delete INPUT 3`  
Delete all in chain  
`iptables --flush INPUT`   
Delete all  
`iptables --flush`  

## Rsync 
Move large files between computers.  
-v=verbose -a=archive, keep file structure and properties -z=compress -P=Show progress and keep partial transfered files  
`rsync -vazP /data/backup/gitlab root@10.36.33.122:/data/dokcer/dockerfiles/gitlab/data/`

## Format Output
Select field/column.   
`cut --delimiter ":" --fields 2`  
Remove specific characters.  
Deletes whitespace, double quotes, and comma.  
`tr --delete [:blank:]\",`  
Replace string.  
's' is the substitution command. Replaces 'foo' with 'bar', option 'g' for all occurrances.  
`sed "s|foo|bar|g"`

## Archive and compress
To archive and compress a folder, keeping permissions  
-c=create archive -v=verbose -z=compress with gzip -f=file name type -p=preserve-permissions   
`tar cvzfp myArchive.tar.gz /myFolder-to-archive`   
To uncompress tar.gz  
-x=extract   
`tar -xvzf myArchive.tar.gz`

## Create a file with Here Document
``` bash
cat << EOF > /etc/apt/apt.conf
Aquire::http::proxy "http://www-proxy.statoil.no:80";
Aquire::https::proxy "http://www-proxy.statoil.no:80";

EOF
```

## Create files and fill with random data

```bash
touch myFile{1..10}.txt
head -c 5M </dev/urandom > myFile10.txt
```

## Network stuff (with iproute2 tools)
List all listening TCP sockets, resolv IP, show port a numeric, and display PID  
`ss --listen --tcp --resolv --numeric --process`
`ss -ltrnp`

List the kernels routing table.  
`ip route` or `route` or `netstat -r`

Delete a route entry.  
`ip route del ip route add 192.168.2.0/24 via 192.168.1.2`

Add new default route.  
`ip route add default via 192.168.1.1`

Add a a route with a netmask.  
`ip route add 192.168.2.0/24 via 192.168.1.2`

List NIC's.  
`ip addr`

Disabel/Enable NIC.  
`ip link set eth0 down`

`ip link set eth0 up`

Configure NIC with IP, broadcast, and netmask.  
`ip addr add 192.168.1.2/24 broadcast 192.168.1.255 dev eth0`


## Inspect processes
List all processes with full format.  
`ps -ef`

List all processes for current user.  
`ps -af`

List only process with PID 2020 with full format and 'word-wrap'.  
`ps -fwwp 2020` 

## Certificates (WIP)
Convert .cer to unix .pem and .key  
`openssl x509 -inform der -in certificate.cer -out certificate.pem`

## SSH Access
To get passwd-less root ssh login.  
```bash
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
To list devices and mountpoint;  
`lsblk`  
If the device have been formated, you need to make a file system on the disk first.    
`mkfs --type xfs /dev/sdb`  
Create a dir to mount in and then mount.  
```bash
mkdir /data
mount /dev/sdb /data
```
Consider adding to fstab.    
To get UUID of all devices;    
`blkid`  
Edit /etc/fstab  
`UUID=41c22818-fbad-4da6-8196-c816df0b7aa8  /data      xfs    defaults,errors=remount-ro 0       1`

## Add or Extend swap-file

## Tmux
New named session  
`tmux new -s mysession`  
Detach from session  
`Ctrl+b d`  
List sessions  
`tmux ls`  
Attach to last session  
`tmux a`  
Attach to named session
`tmux a -t mysession`
## Logical Volume Manager
System to manage physical volumes, logical volumes, and volume groups.  
List  
`pvs`  
`lvs`  
`vgs`  

Add new Physical Volume to a LVM system  
`pvcreate /dev/<device_name>`

Create new Volume Group   
`vgcreate volume_group01 /dev/disk01 /dev/disk02`

Extend Volume Group   
`vgextend volume_group01 /dev/disk03`

Create Logical Volume in Volume Group. -L=Size bytes -l=Size %  
`lvcreate -n myDiskName -l 100%VG (-L50G) volume_group01`

Extend or Reduce Logical Volume. -r=Resize filesystem -L=(+||set)   
`lvextend(reduce) /dev/volume_group01/logical_volume01 -L+50G -r`


## ZFS Administration
https://docs.oracle.com/cd/E19253-01/819-5461/index.html  
__`zpool` for pool administration, `zfs` for filesystem administration.__  
Create pool named "tank" with mirror on sda1 and sdb2, sdc3 as cache and mount point "/data"  
`zpool create tank mirror sda1 sdb2 cache sdc3 -m /data`  
Extend pool  
`zpool add tank sdd4`  

List pool information
```bash
zpool status
zpool list
zpool iostat
```
List, get, and set properties  
```bash
zfs get all tank
zfs get compression tank
zfs set compression=on tank
```
Create snapshot  
`zfs snapshot tank@snapname`  
List Snapshots  
`zfs list -t snapshot -o name,used,creation`  
Rollback to snapshot  
`zfs rollback tank@snapname`  
Destroy snapshot  
`zfs destroy tank@snapname` 
