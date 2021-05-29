# Scripts

Day to helper scripts.

## ROOT

### switch to master_user
* `for user in $(cat /etc/passwd | grep master | awk -F : '{print $1}'); do su $user; done`

### webp
Install webp : 

* `apt install webp`

Exclude extensions : 

remove jpeg|jpg|png| etc from app_db file in /etc/nginx/sites-enabled/

Exclude the following URL from varnish : 

* `/(.+\.(jpeg|jpg|png))?$`
 
