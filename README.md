# abnginxfrombgtopro
##11. Upgrading and Migrating
```
cat /var/run/nginx.pid
ps -aux | grep nginx
```
create a firewall rule that allows port 90
```
firewall-cmd --zone=public --add-port=90/tcp --permanent
```

```
namei -om /etc/nginx/conf.d/main.conf
```
