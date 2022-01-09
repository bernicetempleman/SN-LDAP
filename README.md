# SNOW-LDAP

- Administrators integrate with a Lightweight Directory Access Protocol (LDAP) directory to streamline the user login process and to automate administrative tasks such as creating users.
-  An LDAP integration allows ServiceNow to use your existing LDAP servers as the master source of user data. 
-  The integration uses a read-only connection that never writes to the LDAP directory. The integration only queries for information, and then updates its internal database accordingly.

## There are two aspects to the integration:

- Data population
- Authentication

## DATA POPULATION

- Integration to the LDAP servers allows you to quickly and easily populate ServiceNow with user records from the existing LDAP database.
-  To prevent data inconsistencies, configuration settings provide the ability to create, ignore, or skip incoming LDAP records.
-   You can also limit the data the integration imports by specifying LDAP attributes. 
-   If you do not specify any LDAP attributes, the integration imports all available object attributes from the LDAP server. 
-   The instance stores imported LDAP data in temporary import set tables, so the more attributes you import, the longer the import time.
-    By default, ServiceNow does not delete any entries after they disappear from LDAP. This is because deleting an entry also deletes the entire history and references to the deleted entry.

 
## AUTHENTICATION

- When a user enters domain credentials in the ServiceNow login page, the instance passes those credentials to each defined LDAP server. 
- The LDAP server responds with an authorized or unauthorized message that ServiceNow uses to determine whether access should be granted. 
- By authenticating against your LDAP server, users access ServiceNow with the same credentials that they use for other internal resources on your domain.

## Architecture
- LDAP Integration provides the streamlining of user login process and to automate administrative tasks such as creating users
-  Through SSL PKI Certificate, 
- This LDAP integration ensures security by connecting from a single machine that uses a fixed IP address through a specific port on the firewall. 
- Furthermore, the connection requires a read-only LDAP account of your choosing for authentication.
-  To establish a LDAPS connection, ServiceNow admin will load the public side of LDAP server's SSL certificate on ServiceNow instance. Both Third-Party and Self-Signed certificates are supported.
-   The integration uses the certificate to encrypt all communication between the LDAP server and ServiceNow. 
-   An SSL-encrypted LDAP integration (LDAPS) communicates over TCP on port 636 by default.
-    The diagram below depicts how communications will be secured using LDAPS with SSL Certificate!
-    After setting up a secure connection, ServiceNow Admin will be able to complete LDAP integration setup.
![image](https://user-images.githubusercontent.com/12488769/148688355-b5c7fd8f-0697-4705-bf5a-2584353b0489.png)

![image](https://user-images.githubusercontent.com/12488769/148688350-ffaad885-8dae-418e-b0e1-f9f7e696d9c0.png)

## Pre-Requisites
The LDAP integration requires:
- An LDAP v3 compliant directory services server

- Allows inbound network access (enable SNOW IP and ports) through the firewall of customer network

- The external IP address or fully-qualified domain name of the LDAP server

- A read-only LDAP account for Secure connection between Service Now and LDAP Server’s over internet

- A PKI SSL certificate is required, to secure communication

## There are 7 major steps to complete LDAP Integration:

- Load X.509 Certificate for SSL

- Create Server 

- LDAP Configuration 

- OU Definitions 

- Define Data source 

- Define Transform map 

- Create a Schedule 



## Ldap
![image](https://user-images.githubusercontent.com/12488769/147888072-1dad702d-5dab-494a-9917-995bc5286a84.png)

### 1.	Certificate (only if using port 636)
-	New
-- X.509 cert: ssl Attach cert from client PEM or DER format
-	Autofils rest
	
### 2.	LDAP -> Create new  server
-		Type other (using online server)
-		Name Demo - ldap integration
-		Server URL: Replace hostname with ldap://ldap.forumsys.com:389/
-		Can add multiple hosts with space between them
-		More information? Leave blank if want all (for demo use dc=example,dc=com)
-		Submit

### 3.	Configure ldap server
-		Login distinguished name: (ldap readonly account): cn=read-only-admin,dc=example,dc=com 
-		password
-		Leave mid blank for now
-		Save
-		Connect successfully
-		Advanced (if port 636 check ssl) leave unchecked for port 389
-		Check paging (smaller chuncks of data)
-		save

### 4.	LDAP OU definition (scroll down)
-		Delete groups
-		Click users
-		Name: Users
-		RDN: 
-		Query field: uid
####	Filter: default (objectClass=person)
-	(uid=e*)
-	Test connection
-	Browse Ldap nodes
-		Can change filter and test/browse again
-		Go back to ldap ou definition

### 5.	Data Source 
-		Click on link : Demo-ldap integration/Users
-		Click link: Test load 20 records

###6.	Transform map
-		Ldap servers- click server- users- data source- transforms-Table transform map
-		Check mapping
-		If incorrect – select all field  maps and delete
-		Mapping assist
--	Uid map to User Id
--	Save
-	Coalesce true// avoid duplicates
-	Script: don’t delete lines

#### 	Add company for domain separation
-	Current.company= sys_id='sys_id'
-	Companie->User admin-> companies
-	Select company, rt click /copy sysid and add to script
-	Save
-	Transform
-	Transform history/ check inserts
-	Ldap servers/ demo /ou definitions/users/data sources/load 20 records/ Run transform/ transform/transform history/

### 7.	Schedule
-	To run nightly: Ldap-> schedule loads
-	New 
