Talend ESB Installtion document
===================
TAC Installtion
-------------
> **Steps:**
> - Prepare Ubuntu OS and install JDK
> - Create following Oracle User
> - Install Talend Product on TAC  Server
> - Install Talend ESB on ESB Server(DEV/PRD) 

#### <i class="icon-file"></i> Start Installation

```ssh
$ sudo ./Talend-Installer-20161216_1026-V6.3.1-linux64-installer.run
[sudo] password for [user]:
Welcome to the Talend Installation Wizard.
Please read the following License Agreement. You must accept the terms of this
agreement before continuing with the installation.
```
```ssh
Press [Enter] to continue :/home/miland/installation-files/license

/* Read the license and continue of you agree */



Talend EULA - Product Version - November 2016


Press [Enter] to continue :


Do you accept this license? [y/n]: y

----------------------------------------------------------------------------
Talend Installer (v. 6.3.1)

Please specify the directory where Talend modules will be installed.

Installation Directory [/opt/Talend-6.3.1]:

Please choose one of the installation styles below:

[1] Easy Install
[2] Advanced Install
Please choose an option [1] : 2

Please choose one of the installation types below:

[1] Server - Install all Talend server components using default configuration.
[2] Client - Install Talend Studio on your machine.
[3] Custom - Choose and configure each component to be installed individually.
Please choose an option [1] : 3

License File: []: /home/miland/installation-files/license

----------------------------------------------------------------------------
Select the components you want to install; clear the components you do not want
to install. Click Next when you are ready to continue.

Talend Administration Center [Y/n] :y

Talend Log Server [Y/n] :y

Talend Data Stewardship [Y/n] :y

Talend Command Line [Y/n] :y

Talend Runtime [Y/n] :n

Talend Job Server (Execution Server) [Y/n] :y

Talend Data Quality Portal [Y/n] :y

Talend Data Preparation [Y/n] :y

Talend SAP RFC Server [Y/n] :n

Talend Studio [Y/n] :n

Talend ESB [Y/n] :n

Talend Server Services [Y/n] :y

Is the selection above correct? [Y/n]: y

----------------------------------------------------------------------------
Talend Administration Center Configuration

Talend Administration Center (TAC) installs on an Apache Tomcat server. Please choose one of the options below:

[1] Install an embedded tomcat8 server (recommended).
[2] Use an existing tomcat server.
Please choose an option [1] : 1

----------------------------------------------------------------------------
Create TAC administrator user:

Admin User Name [admin@company.com]:

Admin Password [********] :

----------------------------------------------------------------------------


Enable SSO (default TAC administrator user will not be created in TAC) [y/N]: n


----------------------------------------------------------------------------
Talend Administration Center Configuration

tacDB

Talend Administration Center connects to a database to store its configuration and metadata. Please choose one of the options below:

[1] Embedded H2 database (not recommended)
[2] Connect to a MySQL database
[3] Connect to an Oracle database
[4] Connect to a MS SQL Server database
[5] Connect to a PostgreSQL database
Please choose an option [1] : 3

Talend Administration Center Port (tomcat port)

 [8080]:

Talend's webapp (directory) name under tomcat:

 [org.talend.administrator]:



Setup Email Notifications [y/N]: y


----------------------------------------------------------------------------
Talend Administration Center - Database Configuration

This page enables you to configure Talend Administration Center to use an
existing Oracle database.

Database host [localhost]: odb01.conbraco.net

Database port [1521]:

Database Name (SID) [talend_administrator]: talendadmin.conbraco.net

User Name [talend]: tacadmin

Password [********] :

----------------------------------------------------------------------------
Email Notifications Configuration

This page allows you to setup TAC to use an SMTP server to send out automatic
email notifications. Notification emails are sent automatically when TAC is shut
down or scheduled Jobs are not able to run.



Send Email Notifications [Y/n]: y


SMTP mail server: [smtp.company.com]: mail.conbraco.com

SMTP port: [25]:

Mail user name: [myaccount]:

Mail password: [mypass]:

----------------------------------------------------------------------------
Talend Artifact Repository Nexus Configuration

Talend Artifact Repository Nexus Configuration

Nexus Port

 [8081]:

Nexus Host

 [0.0.0.0]: 10.129.33.137

----------------------------------------------------------------------------
Talend Log Server Configuration

----------------------------------------------------------------------------
Cluster Name [talend-log-central]:

----------------------------------------------------------------------------
Talend Data Stewardship Configuration

TDS installs on Apache Tomcat server. Please choose one of option below:

[1] : Embedded Tomcat 8 server (recommended)
[2] : Use an existing Tomcat server
Please choose an option [1] : 1

----------------------------------------------------------------------------
Embedded Tomcat 8 server (recommended)

UI port (Tomcat port): [19999]:

----------------------------------------------------------------------------
----------------------------------------------------------------------------
Talend Data Stewardship Configuration

Choose MongoDB installation

[1] : Embedded MongoDB
[2] : External MongoDB
Please choose an option [1] : 1

----------------------------------------------------------------------------
Embedded MongoDB

MongoDB Port: [27017]:

----------------------------------------------------------------------------
Embedded MongoDB

----------------------------------------------------------------------------
Talend Data Stewardship Configuration

Choose Kafka installation:

[1] : Embedded Kafka and Zookeeper
[2] : External Kafka and Zookeeper
Please choose an option [1] : 1

----------------------------------------------------------------------------
Embedded Kafka and Zookeeper

Kafka port: [9092]:

Zookeeper port: [2181]:

----------------------------------------------------------------------------
----------------------------------------------------------------------------
Talend Data Stewardship Configuration

Talend Administration Center parameters:

TAC URL [http://TAC01:8080/org.talend.administrator/]:

TAC user: [admin@company.com]:

TAC password: [********] :

----------------------------------------------------------------------------
Talend CommandLine Configuration

This page allows you to select a port to be used by Talend CommandLine
interface. This port is used to communicate with Talend Administration Center.

CommandLine port [8002]:

----------------------------------------------------------------------------
Talend Job Server Configuration

This page allows you to configure Talend Remote Job Server. Please configure
ports used to communicate with the Talend Administration Center and other
setttings.

Command port [8000]:

File transfer port [8001]:

Monitoring port [8888]:

This option enables cleaning of any cached files and logs after a given
duration. Set this value to 0 to disable log purging

Max cache duration (days) [90]:

----------------------------------------------------------------------------
Talend Data Quality Portal Configuration

Talend Data Quality Portal is installed on top of a Tomcat application server. Please choose one of the following options:

[1] Install with Talend Administration Center Server (Tomcat): Install with the Talend Administration Center Tomcat Server
[2] Install an embedded (standalone) Tomcat 8 server: Install an embedded Tomcat 8 server in the installation directory.
Please choose an option [2] : 1

Talend Data Quality Portal needs to connect to an existing database. Please choose one of the following options:

[1] Connect to a MySQL database
[2] Connect to an Oracle database with SID
[3] Connect to an Oracle database with service name
[4] Oracle OCI
[5] Connect to a Microsoft SQL database
[6] Connect to a PostgreSQL database
Please choose an option [1] : 3

TDQ Portal Port: (valid only with embedded tomcat install)

 [8580]:

----------------------------------------------------------------------------
Data Quality Portal - Database

Database Configuration:

Talend Data Quality Studio connects to a database to store the DQ reports and
analysis. Please create a database and configure the connection here:

DB Version:

[1] ORACLE_10
[2] ORACLE_11
[3] ORACLE_12
Please choose an option [1] : 3

Database host name (IP): [localhost]: odb01.conbraco.net

Database port: [1521]:

Servicename: [talend_dq]: talendadmin.conbraco.net

User name: [sysdba]: tdqpadmin

Password :

----------------------------------------------------------------------------
Talend Data Quality Portal Configuration

Server Address:

 [127.0.1.1]: 10.129.33.137

----------------------------------------------------------------------------
Talend Data Preparation Configuration

Enable Data Preparation on Big data [y/N]: n

----------------------------------------------------------------------------
Talend Data Preparation Configuration

Choose MongoDB installation

[1] : Embedded MongoDB
[2] : External MongoDB
Please choose an option [1] : 1

----------------------------------------------------------------------------
Embedded MongoDB

MongoDB port: 27017

----------------------------------------------------------------------------
Embedded MongoDB

----------------------------------------------------------------------------
Data Preparation Server configuration

Talend Administration Center parameters:

TAC URL [http://TAC01:8080/org.talend.administrator/]:

TAC user: [admin@company.com]:

TAC password: [********] :

Data Preparation Server parameters:

Server Public IP: [127.0.1.1]: 10.129.33.137

UI port: [9999]:

Backend port: [8989]:

----------------------------------------------------------------------------
Talend Dictionary Service Configuration

----------------------------------------------------------------------------
Tomcat configuration parameters

Tomcat port: [8187]:

----------------------------------------------------------------------------
----------------------------------------------------------------------------
Talend Dictionary Service Configuration

Choose MongoDB installation

[1] : Embedded MongoDB
[2] : External MongoDB
Please choose an option [1] : 1

----------------------------------------------------------------------------
Embedded MongoDB

MongoDB port: 27017

----------------------------------------------------------------------------
Embedded MongoDB

----------------------------------------------------------------------------
Talend Dictionary Service Configuration

Talend Administration Center parameters:

TAC URL: [http://TAC01:8080/org.talend.administrator]:

TAC user: [admin@company.com]:

TAC password: [********] :

----------------------------------------------------------------------------
Talend Kafka and Zookeeper configuration

Apache and Zookeeper parameters

Zookeeper data dir: [/opt/Talend-6.3.1/kafka/temp]:

----------------------------------------------------------------------------
Services Installation

This page allows you to setup Talend components as system services. By
installing a component as a service the application would start automatically at
system startup.

It's recommended that you check all the boxes below.



Install Apache Kafka and Apache Zookeeper as services [Y/n]: y




Install MongoDB as a service [Y/n]: y




Install Talend Administration Center as a service [Y/n]: y




Install Talend Log Server as a service [Y/n]: y




Install Talend Command Line as a service [Y/n]: y




Install Talend Job Server as a service [Y/n]: y




Install Talend Data Quality Portal as a service [Y/n]: y




Install Talend Data Stewardship as a service [Y/n]: y




Install Talend Data Preparation as a service [Y/n]: y




Install Talend Dictionary Service as a service [Y/n]: y


----------------------------------------------------------------------------
Setup is now ready to begin installing Talend on your computer.

Do you want to continue? [Y/n]: y

----------------------------------------------------------------------------
Please wait while Setup installs Talend modules on your computer.

 Installing
 0% ______________ 50% ______________ 100%
 ##################################
Error: Error running /usr/lib/jvm/java-8-oracle/jre/bin/java -cp
/opt/Talend-6.3.1/tdqp/ojdbc7.jar:org.talend.dataprofiler.datamart.init.jar
org.talend.dataprofiler.datamart.Datamart datamart.properties : Exception in
thread "main" java.lang.ExceptionInInitializerError: Check connection failed:
the Database Name is invalid.
        at
org.talend.dataprofiler.datamart.utils.DatamartUtils.handleDatamart(DatamartUtils
.java:594)
        at org.talend.dataprofiler.datamart.Datamart.main(Datamart.java:61)
Press [Enter] to continue :
######about to fork child process, waiting until server is ready for connections.
forked process: 18532
child process started successfully, parent exiting
#

----------------------------------------------------------------------------
Setup has finished installing Talend on your computer.


-----------------------------------
```

