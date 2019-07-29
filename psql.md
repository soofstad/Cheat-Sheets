# My Cheat-sheets

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

# PSQL

## Backup and restore

To login as the user 'nextcloud' and dump the 'nextcloud-database' into the file '/data/database_dump'
```bash
pg_dump -U nextcloud  -f /data/database_dump nextcloud-database
```

To import/restore the database from the dump;
```bash
psql -U nextcloud nextcloud-database < /data/database_dump
```
Note: The database must exist beforehand

## List databases and tables

List databases  
`\list`  
`\l`  

Change database  
`\connect`  
`\c`  

List tables  
`\dt`  

## Some unordered select and modify
TODO: Order
```psql
docker exec -it sql01.prd.git.ofstad.xyz psql -U gitlab

wide output: \x on
list tabels:  \dt
list columns on tabel 'user': \d+ users

update single email:
UPDATE users SET email='stoo@gmail.com' where username='STOO';

get all users with statoil email:
select username from users where email like '%gmail.com';

get all users with 1 in the username:
select username,email from users where username like '%1%';

Delete users with username ending in '1':
DELETE FROM users WHERE username LIKE '%1';

delete identiy by id:
DELETE FROM identities where id='872';

UPDATE email for all:
UPDATE users SET email = replace(email, 'old', 'new');
```
