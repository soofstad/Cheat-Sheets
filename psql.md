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

# PSQL
```
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
