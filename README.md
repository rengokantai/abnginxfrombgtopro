# abnginxfrombgtopro


##10. SSL, Security, and Authentication
```
mkdir /etc/nginx/ssl
openssl req -nodes -days 3650 -x509 -newkey rsa:2048 -keyout /etc/nginx/ssl/private.key -out /etc/nginx/ssl/cert.crt
```
The previous command starts by creating a private.key followed with a self-signed certificate stored in cert.crt.
- req implies that it is a request.
- nodes tells openssl to avoid using a passphrase for the certificate.
- days 3650 requests a self-signed certificate for 10 years.
- x509 tells it to create a self-signed certificate instead of a certificate request.
- newkey rsa:2048 implies that you want to create a new certificate and a key file at the same time. rsa switch tells it to keep the key at 2048 bits long.
- out decides the location where the certificate will be created.  

open
```
vim /etc/nginx/conf.d/main.conf 
```
edit
```
server {
    listen       80;
    server_name  localhost;
    listen 443 ssl;
    ssl_certificate /etc/nginx/ssl/cert.crt;
    ssl_certificate_key /etc/nginx/ssl/private.key;
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }
}
```

Note:
- The cert.crt file is the public component that is sent to every client that connects to the server.
- The private.key file should be restricted since it is private to the server. However, Nginx's master process must have read access to the file.
then
```
firewall-cmd --permanent --add-port=443/tcp
systemctl restart firewalld
```
####Creating a Certificate Request
```
openssl req -new -newkey rsa:2048 -nodes -keyout /etc/nginx/ssl/mydomain.key -out /etc/nginx/ssl/mydomain.csr
```


###Web Server Security

####Creating the Password File
```
sh -c "echo -n 'user1:' >> /etc/nginx/.pwd"
```
You can add your password now:
```
sh -c "openssl passwd -apr1 >> /etc/nginx/.pwd"
```

put in configuration file
```
server {
    listen       80;
    server_name  localhost;
    listen 443 ssl;
    ssl_certificate /etc/nginx/ssl/cert.crt;
    ssl_certificate_key /etc/nginx/ssl/private.key;
    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
        auth_basic "Authentication Required";
        auth_basic_user_file /etc/nginx/.pwd;
    }
}
```

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
WINCH: Now that you have a new instance running, you can check the requests and test the new configuration. If all is well, you may proceed to kill the original set of worker processes by sending the WINCH signal . Notice how the worker processes are killed but the master process is not. At this point only the new worker processes are running with the new configuration.
```
sudo kill -WINCH 2405
```

