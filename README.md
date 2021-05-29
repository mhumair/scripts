# Scripts

Day to helper scripts.

## ROOT

### switch to master_user
* `for user in $(cat /etc/passwd | grep master | awk -F : '{print $1}'); do su $user; done`

### webp
* `apt install webp`

