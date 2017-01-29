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
#### Command-Line Parameters
```
/usr/sbin/nginx -t -c /some/other/config.conf -g "worker_processes 2;"
```


```
kill -SIGNAL $( cat /var/run/nginx.pid )
```
SIGNAL:
```
TERM, INT: Fast Shutdown
QUIT: Graceful Shutdown
HUP: Start new worker processes with a new configuration, and gracefully shut down the existing worker processes. Notice the following commands. The command spawns 2 worker processes and the HUP signal kills all processes except master process. The PID 2405 doesn't change, but every other PID changes.
```
