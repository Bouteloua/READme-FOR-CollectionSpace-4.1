#Installing CollectionSpace 4.1 on Ubuntu 14.04 LTS (Trusty Tahr)

##System Requirement for CollectionSpace 4.1
* PostreSQL 9.1 -> PostgreSQL 9.1 or later is required and the most current stable production version is recommended
* [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html) (Java SE 1.7)
* [Ant](http://ant.apache.org/bindownload.cgi) 1.8.2 or higher
* [Maven](http://maven.apache.org/download.cgi) 3.0
* [ImageMagick](http://www.imagemagick.org/) 6.0
* Ftp client (already install 14.04)
* [Git](https://github.com/)
* Gzip or anything to unzip (already install 14.04)

##Installing first requirements
```Shell
sudo apt-get update && sudo apt-get upgrade
```

```Shell
sudo apt-get git install ant maven imagemagick ftp
```

##Installing Java SE JFK 1.7.x

* Download Java SE 7 JDK from [oracle.com](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Unzip tar.gz file [replace FILENAME with the file you downloaded]

  ```Shell
  tar -zxvf FILENAME.tar.gz
  ```
* Create a jvm directory to /usr/lib

  ```Shell
  sudo mkdir -p /usr/lib/jvm
  ```
* Move to the jvm directory to /usr/lib [replace FILENAME with your filename]

  ```Shell
  sudo mv FILENAME/ /usr/lib/jvm/
  ```
* Installing java: This will assign Oracle JDK a priority of 1, which means that installing other JDKs will replace it as the default. Be sure to use a higher priority if you want Oracle JDK to remain the default.[replace FILENAME with your filename]
```Shell
  sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/FILENAME/bin/java" 1
  sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/FILENAME/bin/javac" 1
  sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/lib/jvm/FILENAME/bin/javaws" 1
```
* Correct the file ownership and the permissions of the executables [replace FILENAME with your filename]
```Shell
  sudo chmod a+x /usr/bin/java
  sudo chmod a+x /usr/bin/javac
  sudo chmod a+x /usr/bin/javaws
  sudo chown -R root:root /usr/lib/jvm/FILENAME
```
* Double check if using the right version
```
  java -version
```
** You should see something simpler to this output
```
java version "1.7.0_75"
Java(TM) SE Runtime Environment (build 1.7.0_75-b13)
Java HotSpot(TM) 64-Bit Server VM (build 24.75-b04, mixed mode)
```

** If you anything like you need fix it:
*** java version "1.6..." (or anything else earlier than 1.7)
*** OpenJDK

*** Run and pick the right version:
```
sudo update-alternatives --config java
```


This will assign Oracle JDK a priority of 1, which means that installing other JDKs will replace it as the default. Be sure to use a higher priority if you want Oracle JDK to remain the default.
    $-> sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.7.0_75/bin/java" 1
6) Correct the file ownership and the permissions of the executables
    $-> sudo chmod a+x /usr/bin/java
    $-> sudo chown -R root:root /usr/lib/jvm/jdk1.7.0_75/
7) Double check that it got installed correct
    $-> java -version
8) Output or a mistake happen somewhere down the path
    java version "1.7.0_75"
    Java(TM) SE Runtime Environment (build 1.7.0_75-b13)
    Java HotSpot(TM) 64-Bit Server VM (build 24.75-b04, mixed mode)
################################################################################
Installing POSTGRESQL 9.3
  Based off http://wiki.postgresql.org/wiki/Apt#Quickstart
________________________________________________________________________________
1) Install POSTGRESQL 9.3
  $-> sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
  $->  sudo apt-get install ca-certificates
  $->  wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
  $->  sudo apt-get update && sudo apt-get upgrade
  $->  sudo apt-get install postgresql-9.3 postgresql-contrib-9.3
2) Change to system user named postgres will automatically be created
  $-> sudo su - postgres
3) Open file pg_hba.conf to make changes
  $-> vim /etc/postgresql/9.3/main/pg_hba.conf
4) Under "IPv4 local connections" in the file add info below
  # IPv4 local connections:
    host    all             csadmin         samehost                md5
    host    all             nuxeo           samehost                md5
    host    cspace          cspace          samehost                md5
5) Under  "IPv6 local connections" in the file comment out everything in this field
    #host    all             all             ::1/128                md5
6) Save and exit
7) Open file postgresql.conf to make changes
  $-> vim /etc/postgresql/9.3/main/postgresql.conf
8) Make the changes to max_prepared_transactions
  max_prepared_transactions = 64
9) Save and exit
10) Restart postgresql server
  $-> /etc/init.d/postgresql restart
11) Setup datatype 'casts'
  $-> psql -U postgres -d template1
  $-> CREATE FUNCTION pg_catalog.text(integer) RETURNS text STRICT IMMUTABLE LANGUAGE SQL AS 'SELECT textin(int4out($1));';
  $-> CREATE CAST (integer AS text) WITH FUNCTION pg_catalog.text(integer) AS IMPLICIT;
  $-> COMMENT ON FUNCTION pg_catalog.text(integer) IS 'convert integer to text';

  $-> CREATE FUNCTION pg_catalog.text(bigint) RETURNS text STRICT IMMUTABLE LANGUAGE SQL AS 'SELECT textin(int8out($1));';
  $-> CREATE CAST (bigint AS text) WITH FUNCTION pg_catalog.text(bigint) AS IMPLICIT;
  $-> COMMENT ON FUNCTION pg_catalog.text(bigint) IS 'convert bigint to text';

12) Create the 'csadmin' user
    At psql's command prompt (ending in #), switch to the postgres database from the template1 database by entering the following (starting with a backslash character as shown)
    $-> \c postgres

13) In response, you should see a message similar to this:  You are now connected to database "postgres" as user "postgres".
 Enter the following command to create a csadmin user with appropriate privileges.  (Be sure to substitute a password of your choosing for replacemewithyourpassword below:
    $-> CREATE ROLE csadmin LOGIN PASSWORD 'huhtest' SUPERUSER INHERIT NOCREATEDB NOCREATEROLE NOREPLICATION;
14) Exit psql program with ctrl+d
15) Restart postgresql server
  $-> sudo service postgresql restart

################################################################################
Installing git ant maven imagemagick ftp
________________________________________________________________________________
  $-> apt-get update
  $-> apt-get git install ant maven imagemagick ftp
