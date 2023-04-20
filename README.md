# Architecture
AWS architecture

UserData
```shell
#!/bin/bash
yum update -y
yum install -y nginx
systemctl enable nginx
systemctl start nginx
cd /usr/share/nginx/html
echo "<center><h1>Hello Lucky Vanilla 3</h1><h2>This is Lucky Vanilla 3's shopping mall. </h2><h3>Working very well. Good luck</h3></center>" > index.html
```
