# PostgreSQL Administration Training
* no way that I am installing virtualbox on a core duo with 2GB of RAM
* [link to presentation](http://slides.keithf4.com/pg_admin_training_centos/#/)
* [link for VM stuff](https://cloud.keithfiske.com/index.php/s/WqHiH2CrPig9ynA)
* can use a .pgpass file to allow password authentication without having to type in the user's password
* Note for myself, remove pg_hba.conf changes made for the conference and restart postgres
* Get rid of replication user and change password for dan user
* archive commands can be changed without having to restart the database however turning it on requires a restart.  Once on you're good to go
* if you setup the VM and you get to slide 10 you have to make all the changes in yellow in your postgresql.conf file
* presenter highly stressed understanding vacuums, freezing, and MVCC, see slide 12 **ESPECIALLY FOR ADMINS**
* pg_repack will try to do something like a VACUUM FULL with less locking - however as my other notes say, think hard before trying to do something like a VACUUM FULL
* things are very specific to cent 7 - don't bother blindly following the directions on a Debian based system it won't go well
