## Replaying the workload to Azure SQL Database  

The second step is to replay the captured workload on your Azure SQL Database. This needs to be the exact same database you collected capture from. For this exercise use the Azure SQL database we provided.  

### Start New Replay
1.	Launch DEA and go to All replays screen by clicking on the arrow icon on the left navigation  

<img src="/SQL-DEA/images/image-07.png"/>  
  
    
2.	Click the New Replay Button to open the new replay screen
    
<img src="/SQL-DEA/images/image-08.png"/>   
  
    
3.	Provide necessary inputs to begin replay:  

**Replay Details**  

a.	**_Replay name_**: name for your replay session.  

b.	**_Source Trace Format__**: Format of the trace captured in the capture step. Select XEvents for this exercise.  

c.	**_Source Trace Location_**: Location of the trace files captured in the capture step. Choose Local for this exercise.   
  i.	**_Local-**: If your files in the local disk or any file share locations  
  ii.	**_Blob_**: If your trace files are copied over to a blob storage.  

d.	Full path to Source File: The path where you have your source trace. Provide the same path you gave in 3(d) above.  
  i.	**_For Blob__**: Provide Azure blob container’s Shared Access Signature (SAS) key URL. Refer this link for setting up blob container for capturing and storing XEvents from Azure SQL DB and/or Azure SQL Managed Instance.  
  ii.	**_For Local_**: Drive path to store the files (ensure that you have enough storage on this drive). UNC paths are also accepted if SQL Server has permissions to write to that folder.  

e.	**_Replay Tool_**: The type of replay tool you want to use to replay your workloads. Two types of replay methods are supported. We will use InBuilt for this exercise.  
  i.	**_DReplay_**: SQL Server Distributed Replay that ships with SQL Server product. Currently supports only replaying trace files and to SQL Box products configured to use windows authentication  
  ii.	**_InBuilt_**: Use this option to replay XEvents based traces against any SQL Server version, supports both windows and SQL Server authentication.  

f.	**_Replay Trace location_**: The location where you want to store your trace files that we capture while replaying your workload to target server. The value for this field depends on the type of the server that you are going to run your capture session.  
   i.	**_Azure SQL DB / Azure SQL Managed Instance_**: Provide Azure blob container’s Shared Access Signature (SAS) key URL. Refer this link for setting up blob container for capturing and storing XEvents from Azure SQL DB and/or Azure SQL Managed Instance.  
  ii.	**_SQL Server_**: Drive path to store the files (ensure that you have enough storage on this drive). UNC paths are also accepted as long as SQL Server has permissions to write to that folder
  iii.	**_SQL Server on Linux_**: Volume path

**SQL Server connection details**  

g.	**_Server Type_**: The type of the server to which you want to replay the traces / XEvents. For this tutorial, select Azure SQL Database.
  i.	**_SQL Server_** – All the box product versions from SQL 2005 to SQL 2017 including SQL Server on Linux  
  ii.	**_Azure SQL DB_** – Single Database Azure SQL PaaS  
  iii.	**_Azure SQL Managed Instance_** – Fully managed SQL Server with option to host multiple databases.  

h.	**_Server Name_**: SQL Server instance name you would like to capture trace from.  
  i.	**_Authentication Type_**: The type of the authentication you want to use to connect to the SQL Server  
  i.	**_Windows_**: Use this when your SQL Server is connected to Active Directory  
  ii.	**_SQL Authentication_**: Use this when your SQL Server is configured for SQL Authentication  

j.	**_Database name_**: Name of the database on the SQL Server to capture trace from   

i.	For Azure SQL DB, you need to provide the name of the database.   

k.	**_Encrypt connection_**: Encrypts the connection between DEA and SQL Server  

l.	**_Trust server certificate_**: Trust the certificate installed on the SQL Server for encryption.  

m.	**_Username_**: Name of the SQL User  

n.	**_Password_**: Password for the SQL User  
After providing all necessary inputs, double check that you have restored the backup from the first step, check the checkbox to indicate this, and press Start to start replay. Much like New Capture, you will be able to see the status of your replay. After replaying the source trace on both of your target servers, are ready to generate analysis report.  
Check out this FAQ page for commonly asked questions for Replay.   
