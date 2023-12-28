# SQL_XAMPP
### This is a note to use XAMPP to run MYSQL Query

1. Install XAMPP 
2. Start XAMPP and run 
*   MySQL Database, and 
*   Apache Web Server
3. Open browser and go to https://localhost/phpmyadmin/
4. You will find the database schema on the left panel
5. start terminal and run the following commend to start a new database
```bash
cd /Applications/XAMPP

mysql -u root -p
# press enter if the terminal ask for password
```

6. run the following comment to build a database schema
```bash
# a new database called G will be created
create database G;
```

7. 