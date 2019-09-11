# Apache Installation Guide 
> A curated list of awesome READMEs


## Installation

- Go to [apache site](https://httpd.apache.org/download.cgi) and download the tar.gz file latest version(httpd-2.4.41.tar.gz).
- Go to the /home/username/hms/installs/apache directory and unpack using following command.
  > tar xzvf httpd-2.4.41.tar.gz
- Let's download the dependencies
- Go to [apr and apr-utills](https://apr.apache.org/download.cgi) and download tar.gz files of apr and apr-utills.
- Go to the /home/username/hms/installs/apache directory and unpack using following commands.
  > tar xzvf apr-1.7.0.tar.gz 
 
  > tar xzvf  apr-util-1.6.1.tar.gz 
- Go to [pcre](https://ftp.pcre.org/pub/pcre/) and download tar.gz file (eg: pcre-8.43.tar.gz).
- Go to the /home/username/hms/installs/apache directory and unpack using following command.
  > tar xzvf pcre-8.43.tar.gz)
- Move to /home/username/hms/installs/apache/pcre-8.43 directory and build using following commands.
  > ./configure --prefix=/home/username/hms/installs/apache
  
  > make
  
  > make install
- Then move apr-1.7.0. and apr-util-1.6.1 to scrlib folder under httpd-2.4.41 using following commands.
  > mv apr httpd-2.4.12/srclib/
  
  > mv apr-util httpd-2.4.12/srclib/
  
- Move to /home/username/hms/installs/apache/httpd-2.4.12/ and build httpd server.
  >./configure --prefix=/home/yasora/hms/installs/apache -with-pcre=/home/yasora/hms/installs/apache 
  
  > make
  
  > make install
- To start the Apache Server just go to the your installation path (installs/apache/bin) and run command
  > /home/yasora/hms/installs/apache/bin/apachectl start
- To stop the apache server just run 
  > /home/yasora/hms/installs/apache/bin/apachectl stop


## Reverse Proxy Confuguration

- ["Art of Readme - Learn the art of writing quality READMEs."](https://github.com/noffle/art-of-readme#readme) - *Stephen Whitmore*
- ["How To Write A Great README"](https://thoughtbot.com/blog/how-to-write-a-great-readme) - *Caleb Thompson (thoughtbot)*
- ["Readme Driven Development"](http://tom.preston-werner.com/2010/08/23/readme-driven-development.html) - *Tom Preston-Werner*
- ["Top ten reasons why I won’t use your open source project"](https://changelog.com/posts/top-ten-reasons-why-i-wont-use-your-open-source-project) - *Adam Stacoviak*
- ["What I learned from an old GitHub project that won 3,000 Stars in a Week"](https://www.freecodecamp.org/news/what-i-learned-from-an-old-github-project-that-won-3-000-stars-in-a-week-628349a5ee14/) - *KyuWoo Choi*

## Tools
