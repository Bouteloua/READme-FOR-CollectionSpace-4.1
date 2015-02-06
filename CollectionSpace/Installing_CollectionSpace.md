#Installing CollectionSpace
## Getting CollectionSpace
* Make sure you are super user:
```Shell
sudo su
```

* Go to the directory where you want to install CollectionSpace:

```Shell
cd /usr/local/share
```
* Download the tarball via FTP:

```Shell
ftp source.collectionspace.org
```

* Log in with the username anonymous and any password. Then:

```
cd pub/collectionspace/releases/4.1.1
get cspace-server-4.1.1.tar.gz
exit
```
 * Or use gwet
 ```Shell
 wget ftp://source.collectionspace.org/pub/collectionspace/releases/4.1.1/cspace-server-4.1.1.tar.gz
 ```

* Unpack tarball and it will create an apache-tomcat-6.0.33 directory

```Shell
tar -zxvof cspace-server-4.1.1.tar.gz
```

* Make necessary files executable:

```Shell
chmod u+x apache-tomcat-6.0.33/bin/*.sh
```


* Optionally remove the tarball:
```Shell
rm cspace-server-4.1.1.tar.gz
```

## Setting up CollectionSpace

* You need an administrative user account in PostgreSQL that CollectionSpace can use: one that has been granted full privileges and can grant privileges to other users. You can either set up a new user account (recommended), or use the postgres user account that you already set up in the prerequisites section of this page. The values myusername and mypassword, below, should be replaced by the actual username and password of this user.
Set up environment variables. Make sure that the following values correspond to your environment:

**CSPACE_JEESERVER_HOME:** The full path to the apache-tomcat-6.0.33 directory that was created when you unpacked the CollectionSpace tarball.
**CATALINA_HOME:** The full path to the apache-tomcat-6.0.33 directory, identical to the value of CSPACE_JEESERVER_HOME.
**CATALINA_PID:** The full path to a file which will hold the Tomcat server process number. This is used by the Tomcat shutdown script.
**CATALINA_OPTS:** Environment options for the Tomcat server process. The recommended options below are Java Virtual Machine (JVM) memory settings which permit allocation of up to 1 GB of RAM to the Java heap and up to 384 MB of RAM for Java Permanent Generation (PermGen) space.
JAVA_HOME: The full path to your Java directory. If you are unsure where this variable should point to, you can execute this command: readlink -f `which java` and take the part that comes before jre.
**DB_CSADMIN_PASSWORD:** The password for the administrative database user for the CollectionSpace application. This must match the password for that user that you specified while setting up PostgreSQL, via the instructions in PostgreSQL Installation under Linux. (This administrative user by default will be named csadmin, and will be a superuser. However, this user will have fewer privileges than the overall database administrator superuser, by default named postgres.)
**DB_NUXEO_PASSWORD:** Any database-legal password of your choice for the nuxeo database user. This user can work with CollectionSpace data stored in Nuxeo, the content management system underlying CollectionSpace.
**DB_CSPACE_PASSWORD:** Any database-legal password of your choice for the cspace database user. This user can work with CollectionSpace data for users, roles, and access permission.
**DB_READER_PASSWORD:** Any database-legal password of your choice for the reader database user. This user will have read-only access to CollectionSpace data stored in Nuxeo.
**ANT_OPTS:** Environment options for the Apache Ant build tool. The recommended options below are Java Virtual Machine (JVM) memory settings which permit allocation of up to 768 MB of RAM to the Java heap and up to 512 MB of RAM for Java Permanent Generation (PermGen) space.
**MAVEN_OPTS:** Environment options for the Maven build tool. The recommended options below are Java Virtual Machine (JVM) memory settings which permit allocation of up to 768 MB of RAM to the Java heap and up to 512 MB of RAM for Java Permanent Generation (PermGen) space.

```Shell
export CSPACE_JEESERVER_HOME="/usr/local/share/apache-tomcat-6.0.33"
export CATALINA_HOME=$CSPACE_JEESERVER_HOME
export CATALINA_PID="$CSPACE_JEESERVER_HOME/bin/tomcat.pid"
export CATALINA_OPTS="-Xmx1024m -XX:MaxPermSize=384m"
export JAVA_HOME="/usr/java/jdk1.6.0_30"
export DB_CSADMIN_PASSWORD="your_csadmin_database_user_password_here"
export DB_NUXEO_PASSWORD="your_nuxeo_database_user_password_here"
export DB_CSPACE_PASSWORD="your_cspace_database_user_password_here"
export DB_READER_PASSWORD="your_reader_database_user_password_here"
export ANT_OPTS="-Xmx768m -XX:MaxPermSize=512m"
export MAVEN_OPTS="-Xmx768m -XX:MaxPermSize=512m"
```