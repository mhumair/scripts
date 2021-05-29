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
 
