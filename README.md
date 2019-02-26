# Apache -> WSGI -> Flask Web app
**Flask Web App hosted on a LAMP server, with [WSGI](https://pypi.org/project/mod_wsgi/) integration into Python**
## Install Apache HTTPD 2.4 
```
yum -y install httpd24 httpd24-httpd-devel
```
## Install Python3
```
yum -y install yum-utils
yum -y install https://centos7.iuscommunity.org/ius-release.rpm
yum -y install python35u-3.5.1
yum -y install python35u-pip
```
## In order to integrate Apache and Python, we need to install WSGI.
### Do not use ```yum -y install mod_wsgi```, because by default it will be built with Python2.
### Instead, use PIP to install, then copy to the ```/etc/httpd/modules```
```
cp /usr/local/lib/python3.5/site-packages/mod_wsgi/server/mod_wsgi_3.5.so /etc/httpd/modules/mod_wsgi_3.5.so
```





## Installing second instance of HTTPD
**Copy and modify HTTPD service, config, and system files**
```
cp /usr/lib/systemd/system/httpd.service /usr/lib/systemd/system/httpd-py2.service; cp -pr /etc/httpd /etc/httpd-py2; cp /etc/sysconfig/httpd /etc/sysconfig/httpd-py2
mv /etc/httpd-py2/run/httpd.pid /etc/httpd-py2/run/httpd-py2.pid

vim /usr/lib/systemd/system/httpd-py2.service 
vim /etc/httpd-py2/conf.d/ssl.conf 
vim /etc/httpd-py2/conf/httpd.conf 
vim /etc/sysconfig/httpd-py2 
vim /etc/httpd-py2/conf.d/flask.conf 
```
**modify SElinux to allow the secondary service to communicate**
```
semanage port -a -t http_port_t -p tcp 8080
semanage port -a -t http_port_t -p tcp 4443
```
**Reload systemctl**
systemctl daemon-reload 
systemctl start httpd-py2.service 

