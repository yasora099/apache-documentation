# Apache Installation Guide 



## Installation

- Go to [apache site](https://httpd.apache.org/download.cgi) and download the tar.gz file latest version(httpd-2.4.41.tar.gz).
- Go to the /home/username/software directory and unpack using following command.
> tar xzvf httpd-2.4.41.tar.gz
- Let's download the dependencies
- Go to [this site](https://apr.apache.org/download.cgi) and download tar.gz files of apr and apr-utills.
- Go to the /home/username/software directory and unpack using following commands.
> tar xzvf apr-1.7.0.tar.gz 

> tar xzvf apr-util-1.6.1.tar.gz 
- Go to [this site](https://ftp.pcre.org/pub/pcre/) and download tar.gz file (eg: pcre-8.43.tar.gz).
- Go to the /home/username/software directory and unpack using following command.
> tar xzvf pcre-8.43.tar.gz)
- Move to /home/username/software/pcre-8.43 directory and build using following commands.
> ./configure --prefix=/home/username/software/pcre

> make

> make install
- Then move apr-1.7.0. and apr-util-1.6.1 to scrlib folder under httpd-2.4.41 using following commands.
> mv apr httpd-2.4.12/srclib/

> mv apr-util httpd-2.4.12/srclib/

- Move to /home/username/software/httpd-2.4.41/ and build httpd server.
>./configure --prefix=/hms/installs/httpd-2.4.41 -with-pcre=/home/username/software/pcre 

> make

> make install
- To start the Apache Server just go to the your installation path (installs/apache/bin) and run command
> /hms/installs/apache/bin/apachectl start
- To stop the apache server just run 
> /hms/installs/apache/bin/apachectl stop


## Reverse Proxy Configuration

- Open the Apache httpd.conf file under /home/username/hms/installs/ directory.
- Uncomment there lines (remove #)

> Include etc/extra/httpd-vhosts.conf 

> LoadModule proxy_module modules/mod_proxy.so

> LoadModule proxy_http_module modules/mod_proxy_http.so


- First create multiple instances of tomcat server by copying /home/username/hms/installs/tomcat folder and name it as tomcat.
- Go to tomcat2/conf/ and open server.xml file and change the port numbers.  
- Then go to /hms/installs/httpd-2.4.41/conf/extra/ and open httpd-vhosts.conf file and add below lines.
````
<VirtualHost *:80>
ServerName example.com
ServerAlias www.example.com
ServerAdmin webmaster@example.com

ProxyRequests Off

ProxyPass /web http://127.0.0.1:8080/web/
ProxyPassReverse / http://127.0.0.1:8080/web/
ProxyPass /auth http://127.0.0.1:8090/auth/
ProxyPassReverse / http://127.0.0.1:8090/auth/

</VirtualHost>
````

- Then go to (/etc) and open hosts file and add example.com in front of 127.0.0.1
- Restart the apache server by
 
  > /hms/installs/httpd2.4.41/bin/apachectl restart

- In browser when you type example.com/auth you will be redirect to http://127.0.0.1:8081/auth/ and when you type example.com/auth you will be redirect to http://127.0.0.1:8080/web/
## Load Balancer Configuration

- Go to apache configuration folder (httpd-2.4.41/conf) and open httpd.conf file. and uncomment below lines.

  > LoadModule proxy_balancer_module modules/mod_proxy_balancer.so

  > LoadModule lbmethod_byrequests_module modules/mod_lbmethod_byrequests.so

  > LoadModule slotmem_shm_module modules/mod_slotmem_shm.so

- Then go to (apache/conf/extra/) and open httpd-vhosts.conf file and make changes as below.
```
<VirtualHost *:80>

   ProxyRequests Off
   ServerName example.com

 <Proxy balancer://cluster>
   BalancerMember http://127.0.0.1:8080/webapp/
   BalancerMember http://127.0.0.1:8090/auth/
   ProxySet lbmethod=byrequests
   Require all granted
 </Proxy>
 
 <Location /balancer-manager>
   SetHandler balancer-manager
  </Location>

  ProxyPass /balancer-manager !
  ProxyPass / balancer://cluster/
  ProxyPassReverse / balancer://cluster/
  
</VirtualHost>
```
- ```ProxySet``` directive declares how you want to balance. This example uses a “byrequest” balancing algorithm.
- Finally open the browser and point to ```example.com/```. It displays the content from any of your configured web server.
- You can now access load balancer manager by using a Web browser to access the page ```example.com/balancer-manager```.
- Balancer manager enables dynamic update of balancer members.

  ![balancer-manager](/home/yasora/Pictures/screenShot.png)
  
