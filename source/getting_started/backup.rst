*****************
Backup
*****************

|

It's always good to have a backup method in place.  Here are the steps to a basic backup method with IungoPBXPBX. The install script on Debian will automatically copy this backup script to /etc/cron.daily/iungopbx-backup. Backups get stored in /var/backups/iungopbx/postgresql by default.

Command Line
^^^^^^^^^^^^^^

Be sure to change the password by replacing the zzzzzzzz in PGPASSWORD="zzzzzzzz" with your database password. You can get the password from /etc/iungopbx/config.php.


::

 cd /etc/cron.daily
 nano iungopbx-backup


 #!/bin/sh
 
 export PGPASSWORD="zzz"
 db_host=127.0.0.1
 db_port=5432
 
 now=$(date +%Y-%m-%d)
 mkdir -p /var/backups/iungopbx/postgresql
 
 echo "Backup Started"
 
 #delete postgres backups
 find /var/backups/iungopbx/postgresql/iungopbx_pgsql* -mtime +4 -exec rm {} \;
 
 #delete the main backup
 find /var/backups/iungopbx/*.tgz -mtime +2 -exec rm {} \;
 
 #backup the database
 pg_dump --verbose -Fc --host=$db_host --port=$db_port -U iungopbx iungopbx --schema=public -f /var/backups/iungopbx/postgresql/iungopbx_pgsql_$now.sql
 
 #package
 #tar --exclude='/var/lib/freeswitch/recordings/*/archive' -zvcf /var/backups/iungopbx/backup_$now.tgz /var/backups/iungopbx/postgresql/iungopbx_pgsql_$now.sql /var/www/iungopbx /usr/share/freeswitch/scripts /var/lib/freeswitch/storage /var/lib/freeswitch/recordings /etc/iungopbx /etc/freeswitch /usr/share/freeswitch/sounds/music/

 #source
 #tar -zvcf /var/backups/iungopbx/backup_$now.tgz /var/backups/iungopbx/postgresql/iungopbx_pgsql_$now.sql /var/www/iungopbx /usr/local/freeswitch/scripts /usr/local/freeswitch/storage /usr/local/freeswitch/recordings /etc/iungopbx /usr/local/freeswitch/conf /usr/local/freeswitch/sounds/music/
 
#sync certificate directory
rsync -avz -e 'ssh -p 22' root@$ssh_server:/etc/dehydrated/ /etc
rsync -avz -e 'ssh -p 22' root@$ssh_server:/usr/src/iungopbx-install.sh/debian/resources/letsencrypt.sh /usr/src/iungopbx-install.sh/debian/resources/
rsync -avz -e 'ssh -p 22' root@$ssh_server:/etc/dehydrated/accounts/ /etc/dehydrated/
rsync -avz -e 'ssh -p 22' root@$ssh_server:/etc/dehydrated/chains/ /etc/dehydrated/
rsync -avz -e 'ssh -p 22' root@$ssh_server:/etc/dehydrated/config/ /etc/dehydrated/
rsync -avz -e 'ssh -p 22' root@$ssh_server:/etc/dehydrated/config/ /etc/dehydrated/
rsync -avz -e 'ssh -p 22' root@$ssh_server:/etc/dehydrated/hook.sh /etc/dehydrated/
rsync -avz -e 'ssh -p 22' root@$ssh_server:/etc/dehydrated/certs/ /etc/dehydrated/certs
rsync -avz -e 'ssh -p 22' root@$ssh_server:/usr/src/dehydrated /usr/src/

 
 echo "Backup Completed"


To save the file press ctrl + x then y to save it.


You should have the script ready to execute. (Default the script will use FreeSWITCH package paths.  If you have an older install using source be sure to change this by commenting the package line #22 and uncomment the source line #25.)
 
Crontab (optional)
^^^^^^^^^^^^^^^^^

Files in /etc/cron.daily will execute automatically if they don't have an extension like .sh for this reason the backup script was renamed from iungopbx-backup.sh to iungopbx-backup and then it runs nightly without needing to use crontab.

Setting crontab -e
 
::

 crontab -e
 Choose 1 for nano
 Goto the last blank line and paste in the next line.
 0 0 * * * /bin/sh /etc/cron.daily/iungopbx-backup.sh
 press enter then save and exit.
 

Once this is complete you will have the backup ready to execute by ./iungopbx-backup or from the daily cron job. 

