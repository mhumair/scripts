# Scripts

Day to helper scripts.

## ROOT

## switch to master_user
* `for user in $(cat /etc/passwd | grep master | awk -F : '{print $1}'); do su $user; done`

## WEBP

### Install webp : 

* `apt install webp`

### Exclude extensions : 

### remove jpeg,jpg,png, etc from app_db_name file in 

* `/etc/nginx/sites-enabled/`

### Exclude the following from varnish : 

* `/(.+\.(jpeg|jpg|png))?$`

## RESTART SERVICE

* `/etc/init.d/apache2 restart`
 
* `/etc/init.d/nginx restart`

## PROXY CGI TIMEOUT

### cd into the following directory : 

* `cd /etc/apache2/fcgi/`

### replace app_db_name with the application's db name and run :

* `sed -i -e s'#</IfModule>#ProxyTimeout 3600\n</IfModule>#'g app_db_name.conf` 
 
## Application Most Frequent Requests

* `for A in $(ls -la | awk '{print $NF}'); do echo $A && awk '{print $1,$7}' $A/logs/apache_*.access.log | cut -d? -f1 | sort | uniq -c |sort -nr | head -n 5 | awk -F";" '{print $1}' ; done`

## APM commands 

* `apm -s db traffic -l 25d`

* `for A in $(ls | awk '{print $NF}'); do echo $A && apm traffic -s $A --ip 201.234.213.50 -l 1h; done`

* `apm -s db traffic -l 5m`

* `apm -s db traffic`

* `apm -s db`

* `apm traffic -s bjutydjmkf -f 22/09/2021:12:00 -u 22/09/2021:14:00`

*  `for A in $(ls | awk '{print $NF}'); do echo $A && apm traffic -s $A -l 1d; done`
*  `for A in $(ls | awk '{print $NF}'); do echo $A && apm mysql -s $A -l 1d; done`
*  `for A in $(ls | awk '{print $NF}'); do echo $A && apm php -s $A -l 1d; done`
*  `for A in $(ls | awk '{print $NF}'); do echo $A && apm traffic -s $A -f 22/09/2021:12:00 -u 22/09/2021:14:00; done`

## Elastic Search

### Elastic Search Health Check 

* `curl http://localhost:9200/_cluster/health?pretty`

## Clear Logs 

* `cd /var/logs`
* `du -sh *` 
* `truncate -s 100 php7.3-fpm.log`

## Check IP Logs 

* `tail -f /var/log/auth.log`
* `tail -f /var/log/syslog`

## PID wise memory consumption

### display the top 10 PIDs sorted by the most memory consumed :

* `awk '{SUM[$11] += $13} END {for (s in SUM) print s, SUM[s]/1024/1024 " MB" | "sort -nbrk2,2 | head"}' php-app.access.log`

### Limit by hour : 

* `sed -n "/$(date --date='2 hours ago' '+%d\/%b\/%Y:%H')/,\$p" php-app.access.log | awk '{SUM[$11] += $13} END {for (s in SUM) print s, SUM[s]/1024/1024 " MB" | "sort -nbrk2,2 | head"}'`

### Limit by minutes : 

* `sed -n "/$(date --date='40 minutes ago' '+%d\/%b\/%Y:%H:%M')/,\$p" php-app.access.log | awk '{SUM[$11] += $13} END {for (s in SUM) print s, SUM[s]/1024/1024 " MB" | "sort -nbrk2,2 | head"}'`

## Count number of Concurrent users

### faster and accurate way of counting the number of concurrent users on http(s) port:

* `watch -xtn 1 awk '$2 ~ /:01BB/ || $2 ~ /:0050/ {count +=1;} END {print count}' /proc/net/tcp`

### And here's the one to count average number of concurrent users during the past n hours:

* `for i in {01..05};do awk -v time="$(date --date="$i hours ago" '+%d/%b/%Y:%H')" -v hour="$i" 'BEGIN {count=0} $3 ~ time {count +=1} END {print hour " hours ago", count/3600}' nginx-app.status.log;done`

## Bandwdith

### To be run inside application's log directory;

* `for i in {30..0};do zcat -f *_*.access.log*| awk -v day="$(date --date="$i days ago" '+%d/%b/%Y')" '$4 ~ day {sum += $10} END {print day, sum/1024/1024 " MB"}';done`

## Get Top Plugin List from Slow Logs : 

### For Normal

* `for A in $(ls -la | awk '{print $NF}'); do echo $A && cat $A/conf/server.nginx && cat $A/logs/php-app.slow.log.1 | grep -ai 'wp-content/plugins' | cut -d " " -f1 --complement | cut -d '/' -f8 | sort | uniq -c | sort -nr ; done`

### For /mnt/data : 

* `for A in $(ls -la | awk '{print $NF}'); do echo $A && cat $A/conf/server.nginx && cat $A/logs/php-app.slow.log.1 | grep -ai 'wp-content/plugins' | cut -d " " -f1 --complement | cut -d '/' -f10 | sort | uniq -c | sort -nr ; done`


## HELPER SCRIPT : 

* `curl -o /var/cw/systeam/helper.sh https://raw.githubusercontent.com/ifaizan/helper/main/helper.sh
chmod u+x /var/cw/systeam/helper.sh
/var/cw/systeam/helper.sh --help`

## Inodes Clear : 
* `for i in ./*; do echo $i; find $i |wc -l; done`
* `find /pathtodirectory/ -type f -mtime +1 -delete`

## New Relic : 
* `newrelic-install install`

## Sed Search Replace : 
* `grep -lr 'old_url' | xargs sed -i 's|old_url|new_url|g'`

## htaccess html : 
* `# BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On  
RewriteBase /  
RewriteCond %{REQUEST_FILENAME} !-f  
RewriteCond %{REQUEST_FILENAME} !-d  
RewriteRule . /index.html [L]  
</IfModule>  
DirectoryIndex index.html`


## Send Mail PHP Script : 
* `<?php
$to = "email@domain.com";
$subject = "Testing";
$txt = "Hello world!";
mail($to,$subject,$txt,'From: email2@domain2.com  ');
?>`

## Get DB name from domain : 
* `grep -r --include "*.nginx" danielsim.com .`

## Cron Logs : 
* `grep CRON /var/log/syslog | grep db`

## Get Logs Backup fail : 
* `cat /etc/sensu/plugins/data/backup_errors.json`

## NGINX IP Deny : 
* `location / {
   deny 1.2.3.4;
 }`

## redirect 301 : 
* `Redirect 301 http://abc.com http://xyz.com/mylink/`

## get backup files : 
* `/var/cw/scripts/bash/duplicity_restore.sh --src dbname -r --dst /home/master/applications/db/tmp/ --time '2021-06-05T'`

## nodejs proxy enable : 
* `a2enmod proxy*`

## npm install package : 
* `cd ~ && echo "alias svgo='/home/master/bin/npm/lib/node_modules/bin/svgo'" >> .bash_aliases
cd ~ && echo "export PATH='$PATH:/home/master/bin/npm'" >> .bash_aliases
cd ~ && echo "export NODE_PATH='$NODE_PATH:/home/master/bin/npm/lib/node_modules'" >> .bash_aliases
npm config set prefix "/home/master/bin/npm/lib/node_modules"
npm install -g svgo`

## get backups list : 
* `/var/cw/scripts/bash/duplicity_restore.sh --src nkctsgxvam -c`

## backups permissions : 
* `chmod -R u=rwX,g=rwX,o=rX mysql/`

## backups change ownership : 
* `chown -R master_wsyamvkjxk:www-data mysql/`

## rsync : 
* `rsync -azrvh`

## scp : 
* `scp -i private.key -P <portnumber> -r source_username@source_ip:/path/to/the/directory/ /destination/directory/`

## sftp : 
* `sftp -r source_username@source_ip:/source/directory/filename.tar.gz /destination/directory/`

## ftp : 
* `wget -m --source-user=xxxxxxx --source-password=xxxxxxx ftp://X.X.X.X/source-path-to-file/`

## backups fact.d : 
* `cat /etc/ansible/facts.d/backup.fact`

## ssl dry run : 
* `/usr/local/bin/letsencrypt-auto certonly --no-self-upgrade --dry-run --text --non-interactive --webroot -w /opt/letsencrypt/ -d www -d --agree-tos`

## find file : 
* `find ./ -type f -name "*.htacess"`

## composer update : 
* `composer self-update 2.0.11`

## extract tar gz : 
* `tar -xzvf yourfile.tar.gz -C /dir`

## Find directory : 
* `find ./ -type d -name 'files'`

## Increase Swap Memory : 
* `fallocate -l 1G /swapfile
ls -lh /swapfile
chmod 600 /swapfile
ls -lh /swapfile
mkswap /swapfile
swapon /swapfile
swapon --show
free -h`

## PHP Config details : 
* `php-fpm7.4 -tt`

## Increase Tmpfs Space : 
* `mount -o remount,size=1G,noexec,nosuid,nodev,noatime /run`

## Get Banned IP Addresses : 
* `cat /var/log/fail2ban.log | grep banned`

## Redirect all urls to a new domain : 
* `    RewriteEngine On
    RewriteCond %{HTTP_HOST} ^wordpress-647969-2125807.cloudwaysapps.com$ [OR]
    RewriteCond %{HTTP_HOST} ^test.humair.pk$
    RewriteRule (.*)$ http://test.humair.pk/$1 [R=301,L]
`

## Composer : 
* `composer self-update --v`

## Redis : 
* `redis-cli config set "save" ""`
*  `redis-cli config rewrite `

## Take Dump Of All Databases : 
* `for A in $(ls | awk '{print $NF}'); do mysqldump $A > /home/master/$A; done`


## Move all databases : 
* `for A in $(ls | awk '{print $NF}'); do mv /var/lib/mysql/$A /home/master/mysql_var_backups/; done`

## Restore Dump Of All Database : 
* `for i in $(ls -l | grep '^d' | awk '{print $9}'); do echo "Dropping $i"; echo "Y" | mysqladmin drop $i; echo "Creating $i"; mysqladmin create $i;echo "Importing $i" ;mysql $i < /home/master/db/$i.sql;done`


## size : 
* `ls -l --block-size=M`

## disk space : 
* `du -shc /* 2>/dev/null | sort -rh`

## Get DB Backups from Upstream : 

* `cp /var/cw/scripts/bash/duplicity_restore.sh /var/cw/systeam && cd /var/cw/systeam && sed -i '192,199d;209,211d' duplicity_restore.sh`
* `for A in $(ls | awk '{print $NF}'); do echo $A && mkdir /home/master/db_backups/$A && /var/cw/systeam/duplicity_restore.sh --src $A -r --dst /home/master/db_backups/$A --time '2021-10-22' && echo "DONE" ; done`

## install xsend : 
* `apt-get install libapache2-mod-xsendfile`

## New : 
* `install`


