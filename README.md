# Architecture
AWS architecture

UserData
```shell
#!/bin/bash
yum update -y
yum install -y httpd
systemctl enable httpd.service
systemctl start httpd
cd /var/www/html
echo "<h1>Hello Lucky Vanilla 3</h1><h2>Working very well. Good luck</h2>" > index.html
```
