# Scripts

Day to helper scripts.

## ROOT

## switch to master_user
* `for user in $(cat /etc/passwd | grep master | awk -F : '{print $1}'); do su $user; done`

## WEBP

### Install webp : 

* `apt install webp`

### Exclude extensions : 

remove jpeg,jpg,png, etc from app_db_name file in 

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

## apm commands 

* `apm -s appname traffic -l 25d`

* `apm -s appname traffic -l 5m`

* `apm -s appname traffic`

* `apm -s appname`

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




