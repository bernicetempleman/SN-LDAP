# SNOW-LDAP
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
