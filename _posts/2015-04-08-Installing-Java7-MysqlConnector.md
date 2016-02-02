---
layout: post
title: Installing Java 7 and MySQL JDBC Connector
date: '2015-04-08'
---

**Installing Java 7 and MySQL JDBC Connector**

I recently had to setup an Ubuntu 14.04 server with Java and the MySQL JDBC connector. Here's the steps I took...

_First_

Add this PPA in Ubuntu to be able to install Java Oracle 7

~~~

sudo apt-get install python-software-properties
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update

~~~
_Second_

Install Java Oracle 7
`sudo apt-get install oracle-java7-installer`
(You can add the `-set-default` flag to set this version of Java as default).

_Third_

Set `JAVA_HOME` Java Home environment variable by editing the `/etc/environment`

~~~

sudo vi /etc/environment

~~~

Add the following

~~~

JAVA_HOME="/path/to/yourInstallation"

~~~


_Optional_

Install `libmysql-java`

~~~

sudo apt-get install libmysql-java

~~~

_Fourth_

Copy (or symlink) MySQL JDBC Connector driver (.jar) to your Java installation **Extentions** directory

~~~

cp /path/to/mysql-connector-java-version-number.jar /path/to/java/extension/directory/

~~~

I found the MySQL connector driver at `/usr/share/java/...` and I copied it to `/usr/lib/jvm/java-7-oracle/jre/lib/ext` ***But this may be different for you!***

_Fifth_

Hopefully everything went well and you got the connector to work. Enjoy!

-Eduardo
