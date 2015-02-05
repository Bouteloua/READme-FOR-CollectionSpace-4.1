#Installing CollectionSpace 4.1 on ubuntu 14.04 LTS

##System Requirement for CollectionSpace 4.1
* PostreSQL 9.1
* JDK 7 (Java SE 1.7) (CollectionSpace 3.2.1 or higher.)
* Ant 1.8.2 or higher
* Maven 3.0
* ImageMagick 6.0
* FTP client (already install 14.04)
* GIT
* Gzip or anything to unzip (already install 14.04)

##Installing first requirements
'''
sudo apt-get update && sudo apt-get upgrade
'''
'''
apt-get git install ant maven imagemagick ftp
'''
##JAVA:
________________________________________________________________________________
1) Download Java SE Development Kit 7u75
    $-> wget http://download.oracle.com/otn-pub/java/jdk/7u75-b13/jdk-7u75-linux-x64.tar.gz
2) Unzip tar.gz file
    $-> tar -zxvf jdk-7u75-linux-x64.tar.gz
3) Create a JDK directory to /usr/lib
    $-> sudo mkdir -p /usr/lib/jvm
4) Move to the JDK directory to /usr/lib
    $-> sudo mv jdk1.7.0_75/ /usr/lib/jvm/
5) run to install:
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
