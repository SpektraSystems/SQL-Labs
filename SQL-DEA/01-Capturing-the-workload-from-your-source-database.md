## Capturing the workload from your source database

Capturing workload from your source database is the first step in the process and is mandatory. This step captures all the queries on your source server with their timestamps in the specified format (server-side traces or XEvents) which is replayed to target Azure SQL database.  
Before starting the capture session, please make sure that the source database has backup (in case you are testing production databases), if you already have a backup plan for your production database, start the capture session closer to the last backup. This ensures that minimal data integrity related errors while replaying the workloads.  

### Start New Capture  
1.	Launch DEA and go to All captures by clicking on the camera icon on the left navigation  

<img src="/SQL-DEA/images/image-03.png"/>  
  
    
2.	Click the New Capture Button to open the new capture screen  

<img src="/SQL-DEA/images/image-04.png"/>  
  
 
3.	Provide necessary inputs to begin capture.   

**Capture details**  

a.	**_Capture name_**: A name for identifying your capture session, this will also be used as part of naming the trace / xevents files.  
b.	**_Format_**: The format of the traces you want to capture, SQL Server supports two different forms of tracing, choose XEvents for this exercise.  
  i.	**_Server-Side Traces (Trace in DEA)_**: This is the oldest form of trace available and it is supported by all the versions of the SQL Server product. This form of traces is slightly expensive than the XEvents.  
  ii.	**_XEvents_**: XEvents or Extended Events are the newer form of trace that is supported from SQL Server 2012 to all the latest version of SQL Servers including the cloud version (Azure SQL Database and Azure SQL Managed Instance)  

c.	**_Duration_**: The amount of time you want to run the capture session, you should pick this time based on your application / workload you want to test. The ideal time would be the one that captures all the queries that gets executed on the database that is planned for migration. But if you have a workload with varying patterns, then you can just capture your workload during peak load. For this exercise, pick 10 minutes.  

d.	**_Capture location_**: The location where you want to store your trace files. The value for this field depends on the type of the server that you are going to run your capture session. Choose somewhere on your laptop, for this exercise.
    i.	 **_SQL Server_**: Drive path to store the files (ensure that you have enough storage on this drive). UNC paths are also accepted if SQL Server has permissions to write to that folder.   
    ii.	 **_Azure SQL DB / Azure SQL Managed Instance_**: Provide Azure blob container’s Shared Access Signature (SAS) key URL. Refer this link for setting up blob container for capturing and storing XEvents from Azure SQL DB and/or Azure SQL Managed Instance.  
    iii.  **_SQL Server on Linux_**: Volume path  
    
**SQL Server Connection Details**  

e.	 **_Server Type_**: The type of the server from which you want to capture the traces / XEvents. Choose SQL Server for this exercise.  
    i.	 **_SQL Server_** – All the box product versions from SQL 2005 to SQL 2017 including SQL Server on Linux   
    ii.	 **_Azure SQL DB_** – Single Database Azure SQL PaaS  
    iii. **_Azure SQL Managed Instance_** – Fully managed SQL Server with option to host multiple databases.    

f.	 **_Server Name_**: SQL Server instance name you would like to capture trace from.  

g.	**_Authentication Type_**: The type of the authentication you want to use to connect to the SQL Server  
    i.	**_Windows_**: Use this when your SQL Server is connected to Active Directory  
    ii.	**_SQL Authentication_**: Use this when your SQL Server is configured for SQL Authentication  

h.	**_Database name_**: Name of the database on the SQL Server to capture trace from  
    i.	For Azure SQL DB, you need to provide the name of the database.   

i.	**_Encrypt connection_**: Encrypts the connection between DEA and SQL Server  

j.	**_Trust server certificate_**: Trust the certificate installed on the SQL Server for encryption.  

k.	**_Username_**: Name of the SQL User  

l.	**_Password_**: Password for the SQL User   


Please note that the SQL Server service account should have access to the source trace file path.  
Once you have provided all the necessary inputs, please double check that you have taken a backup of the target database(s) and check the checkbox, then press Start to start capture.  
Any validation errors will be reported at the top of the screen.  

<img src="/SQL-DEA/images/image-05.png"/>  
  
    

You can see the detailed error logs in DEA Log file, which is usually found in the path %TMP%/DEA. You can open the error log folder by clicking the three dots in the error notification panel.  
Upon passing all the validation checks, the application will automatically take you to the capture progress screen.  

<img src="/SQL-DEA/images/image-06.png"/>  
  

You will then see the progress of your capture, including start time, duration, and remaining time.  

To stop the currently running trace, select the capture session name from the grid and click on the stop button.   

Please note that you need to keep DEA application running while the capture session is running.  

Any errors that occur during the capture progress will be displayed in the capture progress screen.  

You can start multiple capture sessions while waiting for this capture to finish. Once your capture is completed, you can use the output trace file to start the second phase: replaying the trace file on your target servers.   
Check out this FAQ page for frequently asked questions for Capture.  
