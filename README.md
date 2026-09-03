# n8n-login-unable-to-sign-in
Fix the n8n login issue: 

<img width="690" height="449" alt="image" src="https://github.com/user-attachments/assets/2041c814-8aa9-4e1c-b8c1-af3ae46eb60a" />

if you see that error while trying to login to your n8n instance, and you already tried the "n8n user-management: reset" fix that didn't do anything to solve it... here is how you can beat that:

first starting with the database that you are using whether it is SQLite or PostgreSQL, i have faced that issue while i am using PostgreSQL so my solution will be based on that.

1- i want you to connect to your postgresql database using that command:
sudo -u postgres psql -d <database name>
2- do this command: 
SELECT "password", email, "settings", disabled FROM "user";
3- if settings says: {"userActivated": false} then do:
UPDATE "user"
SET "settings" = '{"userActivated": true}'
WHERE "email" = 'your email';
4- exit the db using: \q
5- then restart n8n
if this still not working we need to change the password, go to Bcrypt generator online write a password then encrypt it using 10 rounds, take the resulted hash and do connect to your db as step 1 then update the password credential as follows:
UPDATE "user"
SET "password" = '$2a$10$hash_goes_here'
WHERE "email" = 'your email';
then exit the db using: \q and restart n8n.
Problem should be fixed :)

