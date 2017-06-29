
Talend ESB Installation Runbook
===================



## DOCUMENT

|                 |                                          |
| --------------- | ---------------------------------------- |
| DOCUMENT AUTHOR | Milan Das (mdas@talend.com)              |
| DOCUMENT OWNER  | Adam Presley (adam.presley@mycompany.com) |
| DATE CREATED    | Jun 26, 2017                             |
| LAST UPDATED    | Jun 28, 2017                             |
| COMPANY         | mycompany Industries Inc                  |



## High leve

> - Operating System and Oracle JDK 1.8 installation
> - Create Oracle Schema
> - Install Talend Product on TAC  Server
> - Install Talend ESB on ESB Server(DEV/PRD)
> - Install ESB Runtime(DEV/PRD)
> - Configure TAC
> - Create GIT Project
> - Service Details

**1.0 Operating System and Oracle JDK 1.8 installation**
-------------

> **Server Detail**
> Following the server details

|    Server Name     |        IP Address         |   IP Address    |     |
| ------------------ | ------------------------- | --------------- | --- |
| TAC                | `tac01.mycompany.net`      | `10.129.33.137` |     |
| ESB DEV            | `tesbdev01.mycompany.net`  | `10.129.33.146` |     |
| ESB PROD           | `tjrprod01.mycompany.net`  | `10.129.33.145` |     |
| JOB PROD           | `tesbprod01.mycompany.net` | `10.129.33.144` |     |
| Oracle Database    | `odb01.mycompany.net`      | `10.129.33.77`  |     |
| GIT Source Control | `tsource.mycompany.net`    | `10.129.33.147` |     |


**2.0 Create Oracle Schema**
-------------

> **Oracle Schema Detail**
> Create following users/schema in Oracle DB for ESB Product installations.

|  Schema Name  |     User      |        Service Name        |         host         |  port  |               Purpose                |     |
| ------------- | ------------- | -------------------------- | -------------------- | ------ | ------------------------------------ | --- |
| `tacadmin`    | `tacadmin`    | `talendadmin.mycompany.net` | `odb01.mycompany.net` | `1521` | Talend Adminstration Center Database |     |
| `tacaudit `   | `tacaudit`    | `talendadmin.mycompany.net` | `odb01.mycompany.net` | `1521` | Audit Database                       |     |
| `tdqpadmin`   | `tdqpadmin`   | `talendadmin.mycompany.net` | `odb01.mycompany.net` | `1521` | DQ Portal Database                   |     |
| `tdqlspagobi` | `tdqlspagobi` | `talendadmin.mycompany.net` | `odb01.mycompany.net` | `1521` | Spago BI Databases                   |     |
| `tacamc`      | `tacamc`      | `talendadmin.mycompany.net` | `odb01.mycompany.net` | `1521` | AMC databases                        |     |

> - **SQL Scripts**

```sql

CREATE USER tacaudit IDENTIFIED BY XXXXXXXX;
grant CONNECT, RESOURCE to tacaudit;

CREATE USER tdqpadmin IDENTIFIED BY XXXXXXXX;
grant CONNECT, RESOURCE to tdqpadmin;


CREATE USER tacadmin IDENTIFIED BY XXXXXXXX;
grant CONNECT, RESOURCE to tacadmin;


CREATE USER tdqlspagobi IDENTIFIED BY XXXXXXXX;
grant CONNECT, RESOURCE to tdqlspagobi;

CREATE USER tacamc IDENTIFIED BY XXXXXXXX;
grant CONNECT, RESOURCE to tacamc;
```

3.0 **Install Talend Product on TAC  Server**
-------------


#### :file_folder: Install  TAC  Server

```ssh
$ sudo ./Talend-Installer-20161216_1026-V6.3.1-linux64-installer.run
[sudo] password for [user]:
Welcome to the Talend Installation Wizard.
Please read the following License Agreement. You must accept the terms of this
agreement before continuing with the installation.
```
#### :file_folder: Accept License Agreement

```ssh
Press [Enter] to continue :/home/miland/installation-files/license

/* Read the license and continue of you agree */



Talend EULA - Product Version - November 2016


Press [Enter] to continue :


Do you accept this license? [y/n]: y

----------------------------------------------------------------------------
Talend Installer (v. 6.3.1)

Please specify the directory where Talend modules will be installed.
```
#### :file_folder: Installation Options

```ssh
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
```
#### :file_folder: Select Components
```ssh
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

```
#### :file_folder: Talend Administration Center Configuration
```ssh
Talend Administration Center Configuration

Talend Administration Center (TAC) installs on an Apache Tomcat server. Please choose one of the options below:

[1] Install an embedded tomcat8 server (recommended).
[2] Use an existing tomcat server.
Please choose an option [1] : 1


Create TAC administrator user:

Admin User Name [admin@company.com]:

Admin Password [********] :

Enable SSO (default TAC administrator user will not be created in TAC) [y/N]: n
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
```
#### :file_folder: TAC DB Configuration

```ssh
Talend Administration Center - Database Configuration

This page enables you to configure Talend Administration Center to use an
existing Oracle database.

Database host [localhost]: odb01.mycompany.net

Database port [1521]:

Database Name (SID) [talend_administrator]: talendadmin.mycompany.net

User Name [talend]: tacadmin

Password [********] :

```
#### :file_folder: Email Notification Configuration
```ssh
Email Notifications Configuration

This page allows you to setup TAC to use an SMTP server to send out automatic
email notifications. Notification emails are sent automatically when TAC is shut
down or scheduled Jobs are not able to run.



Send Email Notifications [Y/n]: y


SMTP mail server: [smtp.company.com]: mail.mycompany.com

SMTP port: [25]:

Mail user name: [myaccount]:

Mail password: [mypass]:

```
#### :file_folder:Nexus Configuration
```ssh
Talend Artifact Repository Nexus Configuration

Talend Artifact Repository Nexus Configuration

Nexus Port

 [8081]:

Nexus Host

 [0.0.0.0]: 10.129.33.137

```
#### :file_folder: Talend Log Server Configuration
```ssh
Talend Log Server Configuration


Cluster Name [talend-log-central]:

```
#### :file_folder: Talend Data Stewardship Configuration
```ssh
Talend Data Stewardship Configuration

TDS installs on Apache Tomcat server. Please choose one of option below:

[1] : Embedded Tomcat 8 server (recommended)
[2] : Use an existing Tomcat server
Please choose an option [1] : 1


Embedded Tomcat 8 server (recommended)

UI port (Tomcat port): [19999]:


Talend Data Stewardship Configuration

Choose MongoDB installation

[1] : Embedded MongoDB
[2] : External MongoDB
Please choose an option [1] : 1


Embedded MongoDB

MongoDB Port: [27017]:


Embedded MongoDB


Talend Data Stewardship Configuration

Choose Kafka installation:

[1] : Embedded Kafka and Zookeeper
[2] : External Kafka and Zookeeper
Please choose an option [1] : 1


Embedded Kafka and Zookeeper

Kafka port: [9092]:

Zookeeper port: [2181]:


Talend Data Stewardship Configuration

Talend Administration Center parameters:

TAC URL [http://TAC01:8080/org.talend.administrator/]:

TAC user: [admin@company.com]:

TAC password: [********] :

```
#### :file_folder: Talend Command Line Configuration
```ssh
Talend CommandLine Configuration

This page allows you to select a port to be used by Talend CommandLine
interface. This port is used to communicate with Talend Administration Center.

CommandLine port [8002]:

```
#### :file_folder: Talend Job Server(DEV) Configuration
```ssh
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

```
#### :file_folder: Talend Data Quality Portal Configuration
```ssh
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

```
#### :file_folder: Talend DQ Portal DB Configuration
```ssh
Data Quality Portal - Database

Database Configuration:

Talend Data Quality Studio connects to a database to store the DQ reports and
analysis. Please create a database and configure the connection here:

DB Version:

[1] ORACLE_10
[2] ORACLE_11
[3] ORACLE_12
Please choose an option [1] : 3

Database host name (IP): [localhost]: odb01.mycompany.net

Database port: [1521]:

Servicename: [talend_dq]: talendadmin.mycompany.net

User name: [sysdba]: tdqpadmin

Password :


Talend Data Quality Portal Configuration

Server Address:

 [127.0.1.1]: 10.129.33.137


Talend Data Preparation Configuration

Enable Data Preparation on Big data [y/N]: n


Talend Data Preparation Configuration

Choose MongoDB installation

[1] : Embedded MongoDB
[2] : External MongoDB
Please choose an option [1] : 1


Embedded MongoDB

MongoDB port: 27017


Embedded MongoDB

```
#### :file_folder: Talend Data Preparation Server Configuration
```ssh
Data Preparation Server configuration

Talend Administration Center parameters:

TAC URL [http://TAC01:8080/org.talend.administrator/]:

TAC user: [admin@company.com]:

TAC password: [********] :

Data Preparation Server parameters:

Server Public IP: [127.0.1.1]: 10.129.33.137

UI port: [9999]:

Backend port: [8989]:

```
#### :file_folder: Talend Dictionary Service Configuration
```ssh
Talend Dictionary Service Configuration


Tomcat configuration parameters

Tomcat port: [8187]:


Talend Dictionary Service Configuration

Choose MongoDB installation

[1] : Embedded MongoDB
[2] : External MongoDB
Please choose an option [1] : 1


Embedded MongoDB

MongoDB port: 27017


Embedded MongoDB


Talend Dictionary Service Configuration

Talend Administration Center parameters:

TAC URL: [http://TAC01:8080/org.talend.administrator]:

TAC user: [admin@company.com]:

TAC password: [********] :

```
#### :file_folder: Talend Kafka & Zookeeper Configuration
```ssh
Talend Kafka and Zookeeper configuration

Apache and Zookeeper parameters

Zookeeper data dir: [/opt/Talend-6.3.1/kafka/temp]:


Services Installation

This page allows you to setup Talend components as system services. By
installing a component as a service the application would start automatically at
system startup.
```
#### :file_folder: Check Services to be installed as  a Service
```ssh
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



Setup is now ready to begin installing Talend on your computer.

Do you want to continue? [Y/n]: y


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

Setup has finished installing Talend on your computer.
```
#### :file_folder: Create following directories
> - Open Unix prompt
> - Create following directories  owned by `talenduser:talendgroup`

|   Component   |                                     Directories                                     |
| ------------- | ---------------------------------------------------------------------------------- |
| Audit         | `/opt/Talend/Audit/reports`                                                        |
| CommandLine   | `/opt/Talend/CommandLine/exports` and `/opt/Talend/CommandLine/userComponent`          |
| Job Conductor | `/opt/Talend/Administrator/generatedJobs` and `/opt/Talend/Administrator/executionLogs` |
| Log4j         | `/opt/Talend/log/technical.log` and `/opt/Talend/log/business.log`                     |


#### :file_folder: Configure TAC  Server

> **Navigate to TAC URL:**

> - Download `ojdbc8.jar` from [Oracle](http://www.oracle.com/technetwork/database/features/jdbc/jdbc-ucp-122-3110062.html)  and copy to `/opt/Talend-6.3.1/tac/apache-tomcat/endorsed`
> - Remove all other copy of `ojdbc*.jar` file from all sub directories under `/opt/Talend-6.3.1/tac/`
> - Restart TAC: `sudo /etc/init.d/talend-tac-6.3.1 stop` and `sudo /etc/init.d/talend-tac-6.3.1 start`
> - Navigate to [Talend Administration Center](http://tac01.mycompany.net:8080/org.talend.administrator/)
> - On start up screen Configure parameters below as below.


| Database type |                                 Oracle                                 |     |
| ------------- | ---------------------------------------------------------------------- | --- |
| driver        | `oracle.jdbc.driver.OracleDriver`                                      |     |
| url           | `jdbc:oracle:thin:@//odb01.mycompany.net:1521/talendadmin.mycompany.net` |     |
| username      | `tacadmin`                                                             |     |
| password      | `xxxxxxx`                                                              |     |


** 4.0 Install Talend ESB on TAC Server**
-------------
#### :file_folder:Configure TAC  Server
> - login to unix console as `talenduser` ( if `\opt` owened by `talenduser` account on ESB server) and unzip  `unzip /home/miland/installation-files/Talend-ESB-V6.3.1-20161215184526.zip -d /opt/`  into `/opt/Talend-6.3.1/`. It should create a directory `Talend-ESB-V6.3.1`
> - start Talend ESB
>> - Login to Unix shell at start `trun`
>> - `/opt/Talend-6.3.1/Talend-ESB-V6.3.1/container/bin/trun`
> - This will open Karaf Consol. Run the following commands below in Karaf console
> - Create ESB wrapper
>> - `feature:install service-wrapper`
>> - `wrapper:install -n talend-esb-service`
> - Create  Service for ESB. Open a Unix a console and crete the follwoing link below.
>> - Create Service `ln -s /opt/Talend-Runtime-V6.3.1/bin/talend-esb-tesbprod01-service /etc/init.d/`

** 5.0 Install Talend ESB Runtime & Active MQ**
-------------
#### :file_folder:Configure ESB runtime Server
> - login to unix console as `root` ( if `\opt` owened by root account on ESB server) and unzip  `unzip /home/miland/installation-files/Talend-Runtime-V6.3.1-20161215184526.zip -d /opt/` into `/opt`. It should create a directory `Talend-ESB-V6.3.1`
> - create  user `talenduser` and group `talendgroup`
>> - `adduser talenduser`
>> - `groupadd -g 1004 talendgroup`
>> - `usermod -a -G talendgroup talenduser`
>> - `chown -R talenduser:talendgroup /opt/Talend-Runtime-V6.3.1/`
>> - `chown -R talenduser:talendgroup activemq.5.14.1/`
> - start Talend ESB
>> - Login to Unix shell at start `trun`
>> - `/opt/Talend-Runtime-V6.3.1/bin/trun`
> - This will open Karaf Consol. Run the following commands below in Karaf console
> - Create ESB Runtime wrapper
>> - `feature:install service-wrapper`
>> - `wrapper:install -n talend-esb-tesbdev01`
>> - `feature:install webconsole`
> - Change following Properties

|                                     FileName                                      |                                                      Property Name                                                      | Property value |     |
| --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- | -------------- | --- |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.esb.authorization.pep.cfg`             | `tesb.pdp.address = http://TAC01.mycompany.net:8040/services/pdp/authorize`                                              |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.apache.cxf.xkms.client.cfg`                   | `xkms.endpoint=http://TAC01.mycompany.net:8040/services/XKMS/`                                                           |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.esb.sam.agent.cfg`                     | `service.url=http://TAC01.mycompany.net:8040/services/MonitoringServiceSOAP`                                             |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.esb.auxiliary.storage.client.rest.cfg` | `auxiliary.storage.service.url = http://TAC01.mycompany.net:8040/services/AuxStorageService`                             |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.esb.auxiliary.storage.client.rest.cfg` | `sts.wsdl.location = http://TAC01.mycompany.net:8042/services/SecurityTokenService/UT?wsdl`                              |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.ops4j.pax.url.mvn.cfg`                        | `    http://admin:Talend123@TAC01.mycompany.net:8081/nexus/content/repositories/releases@id=tesb.release,\`              |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.ops4j.pax.url.mvn.cfg`                        | `    http://admin:Talend123@TAC01.mycompany.net:8081/nexus/content/repositories/snapshots@snapshots@id=tesb.snapshot, \` |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.esb.job.client.sts.cfg`                | `#sts.wsdl.location = http://TAC01.mycompany.net:8080/SecurityTokenService/UT?wsdl`                                      |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.esb.job.client.sts.cfg`                | `sts.wsdl.location = http://TAC01.mycompany.net:8040/services/SecurityTokenService/UT?wsdl`                              |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.esb.job.client.sts.cfg`                | `sts.x509.wsdl.location = http://TAC01.mycompany.net:8040/services/SecurityTokenService/X509?wsdl`                       |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.esb.registry.client.wsdl.cfg`          | `registry.url=http://TAC01.mycompany.net:8040/services/registry/lookup`                                                  |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.esb.registry.client.wsdl.cfg`          | `sts.wsdl.location = http://TAC01.mycompany.net:8042/services/SecurityTokenService/UT?wsdl`                              |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.esb.locator.cfg`                       | `locator.endpoints=TAC01.mycompany.net:2181`                                                                             |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.esb.locator.cfg`                       | `endpoint.http.prefix=http://TAC01.mycompany.net:8040/services`                                                          |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.esb.locator.cfg`                       | `endpoint.https.prefix=https://TAC01.mycompany.net:9001/services`                                                        |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.esb.registry.client.policy.cfg`        | `registry.url=http://TAC01.mycompany.net:8040/services/registry/lookup`                                                  |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.esb.sts.server.cfg`                    | `tidmServiceUrl=http://TAC01.mycompany.net:8080/syncope/rest/`                                                           |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.apache.karaf.management.cfg`                  | `rmiRegistryHost = TESBDEV01.mycompany.net`                                                                              |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.apache.karaf.management.cfg`                  | `rmiServerHost = TESBDEV01.mycompany.net`                                                                                |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/talend-esb-tesbdev01-wrapper.conf`                | `wrapper.java.additional.10=-Djava.rmi.server.hostname=TESBDEV01.mycompany.net`                                          |                |     |
| `/opt/Talend-Runtime-V6.3.1/etc/org.talend.remote.jobserver.server.cfg`           | `org.talend.remote.jobserver.server.TalendJobServer.LOCAL_HOST=TESBDEV01.mycompany.net`                                  |                |     |

> - Install  Active MQ Server
>> - copy `/installation-files/Talend-ESB-V6.3.1/activemq` to `/opt`
>> - rename `/opt/activemq` to `/opt/activemq.5.14.1`
>> - Create Service `ln -s /opt/activemq.5.14.1/bin/linux-x86-64/activemq /etc/init.d/activemq`
> - edit `/opt/activemq.5.14.1/bin/linux-x86-64/activemq` add a line below `ACTIVEMQ_HOME` `RUN_AS_USER="talenduser"`
> - Create  Service(ESB and ActiveMQ)

|  Server 	| Script   	| Purpose  	|  
|---	|---	|---	|
|ESB|`ln -s /opt/Talend-Runtime-V6.3.1/bin/talend-esb-tesbdev01-service /etc/init.d/`|ESB server service|
|ActtiveMQ|`ln -s /opt/activemq.5.14.1/bin/linux-x86-64/activemq /etc/init.d/`|Active MQ Service|

** 6.0 TAC Configurations **
-------------
|                              Screenshot                              |                                         Details                                          |
| -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| ![alt text](./images/TAC-login-01.jpg "Login")                       | Navigate to TAC URL <http://tac01.mycompany.net:8080/org.talend.administrator> and login. |
| ![alt text](./images/TAC-configurations-01.jpg "configurations")     | Open Configurations                                                                      |
| ![alt text](./images/TAC-audit-01.jpg "configurations")              | Configure Audit                                                                          |
| ![alt text](./images/TAC-monitoring-01.jpg "monitoring")             | Configure monitoring                                                                     |
| ![alt text](./images/TAC-esbsam-01.jpg "ESBSAM")                     | Configure ESB Service Locator and SAM                                                    |
| ![alt text](./images/TAC-nexus-01.jpg "nexus")                       | Configure artifact Repository nexus                                                      |
| ![alt text](./images/TAC-iam-01.jpg "iam")                           | Configure Identity Access Management                                                     |
| ![alt text](./images/TAC-servreg-01.jpg "servreg")                   | Configure ESB Service Registry                                                           |
| ![alt text](./images/TAC-esbprov-01.jpg "esbprov")                   | Configure ESB Provisioning Service                                                       |
| ![alt text](./images/TAC-jobconductor-01.jpg "jobconductor")         | Configure Job Conductor                                                                  |
| ![alt text](./images/TAC-log4j-01.jpg "log4j")                       | Configure log4j                                                                          |
| ![alt text](./images/TAC-smtp-01.jpg "smtp")                         | Configure smtp                                                                           |
| ![alt text](./images/TAC-git-01.jpg "git")                           | Configure git                                                                            |
| ![alt text](./images/TAC-userlib-01.jpg "userlib")                   | Configure user libraries                                                                 |
| ![alt text](./images/TAC-creategitproject-01.jpg "creategitproject") | Create project in GIT                                                                    |
| ![alt text](./images/TAC-projects-01.jpg "projects")                 | Navigate to  projects                                                                    |
| ![alt text](./images/TAC-addnewprojects-01.jpg "addnewprojects")     | Add new projects                                                                         |



** 7.0 Service Details **
-------------
#### :file_folder:TAC Server (`TAC01.mycompany.net`)

|        Service Name         |    lcoation    |               Purpose               |
| --------------------------- | -------------- | ----------------------------------- |
| `talend-cmdline-6.3.1`      | `/etc/init.d/` | Talend Comamnd Line Service         |
| `talend-dataprep-6.3.1`     | `/etc/init.d/` | Talend Data Preparation Service     |
| `talend-dqdict-6.3.1`       | `/etc/init.d/` | Talend Data Quality Service         |
| `talend-kafka-6.3.1`        | `/etc/init.d/` | Kafka Service                       |
| `talend-mongodb-6.3.1`      | `/etc/init.d/` | mongodb Service                     |
| `talend-rjs-6.3.1`          | `/etc/init.d/` | Talend remote  Job Server DEV       |
| `talend-tac-6.3.1`          | `/etc/init.d/` | Talend Adminstration Center Service |
| `talend-tcomp-6.3.1`        | `/etc/init.d/` | Talend Data Preparation component   |
| `tartifactrepo-nexus-6.3.1` | `/etc/init.d/` | Nexus Artifact repo                 |

#### :file_folder:ESB runtime (`TESBDEV01.mycompany.net` and `TESBPROD01.mycompany.net`)

|  Service Name 	| lcoation   	| Purpose  	|  
|---	|---	|---	|
|`talend-esb-tesb<xxx>01-service`|`/etc/init.d/`|Talend Comamnd Line Service|
|`activemq`|`/etc/init.d/`|Talend Data Preparation Service|


#### :file_folder:Job Server (`TAC01.mycompany.net` and `TJRPROD01.mycompany.net`)

|           Service Name            |    lcoation    |      Purpose       |
| --------------------------------- | -------------- | ------------------ |
| `talend-<env>jobserver01-service` | `/etc/init.d/` | Talend JOB  Server |


** 7.0 Service URL Details **
-------------
#### :file_folder: URL's

|        Service Name        |                                  URL                                  |  Default User Name  |     |
| -------------------------- | --------------------------------------------------------------------- | ------------------- | --- |
| TAC                        | <http://tac01.mycompany.net:8080/org.talend.administrator>             | `admin@company.com` |     |
| Nexus                      | <http://tac01.mycompany.net:8081/nexus/>                               | `admin`             |     |
| Nexus Snapshot             | <http://tac01.mycompany.net:8081/nexus/content/repositories/snapshots> | `admin`             |     |
| Nexus Release              | <http://tac01.mycompany.net:8081/nexus/content/repositories/releases>  | `admin`             |     |
| Apache Syncope Console IDM | <http://tac01.mycompany.net:8080/syncope-console>                      | `admin@company.com` |     |
| Active MQ Console          | <http://tesbdev01.mycompany.net:8161/admin/>                           | `admin`             |     |
| GIT                        | <http://tsource.mycompany.net/>                                        |                     |     |
| Data Quality Portal        | <http://tac01.mycompany.net:8580/tdqportal>                            |                     |     |
| karaf/ESB Runtime          | `ssh TESBDEV01.mycompany.net:8101`                                     | `tadmin`            |     |
| ESB Runtime webconsole     | <http://tesbdev01.mycompany.net:8040/system/console>                   | `tadmin`            |     |
