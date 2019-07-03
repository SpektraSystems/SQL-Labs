
# Validate your workload(s) against Azure SQL database using Database Experimentation Assistant (DEA)

DEA supports A\B testing for SQL Server upgrades, migrations to Azure SQL database and Managed Instance. However, this document will focus on Azure SQL database only, for the purpose of Ready workshop.

Database Experimentation Assistant (DEA) is an A\B testing solution for migrations to Azure SQL database. You can use DEA to capture workloads on your source SQL Server instance and replay it on Azure SQL database target to compare performance and identify compatibility issues between the two platforms. 

In this tutorial you will learn how to use DEA to:
-	 Capture the workload from your source SQL Server instance.  
-	 Replay the workload on the target Azure SQL database.  
-	 Run analysis to compare the workload between your source and target databases.  
-	 Use the analysis reports to compare performance and compatibility between source and target databases. 

## Prerequisites 

To complete this tutorial, you need to  
1.  Download and Install Database Experimentation Assistant 2.6 on your laptop.  
2.  Bring your own source database that has queries running   
    or 
    Setup a source database on your laptop using the steps described below.  
    
To setup a test database on your laptop to use as source for this exercise:  
1.	Download the backup file located here.  
2.	Open SQL Server Management Studio.  
3.	Right click on Databases and select Restore Database.  
4.	In the pop-up, select Device and click on the … button.   
5.	In the select backup devices pop-up, click Add button.   

<image>  
    
6.	In the locate backup file pop-up, select the backup file that you downloaded in step 1.  
7.	Once you select the file, click OK to come back to the main window.   
8.	In the main window, feel free to change the database name as needed and click ok again.  
9.	It will take a few minutes for the database to be created from the backup. You can see the progress in the right top corner of the window.  

    
<image>  
    
10.	Once it is 100%, you will get a confirmation. Your database is now created.  
11.	The last step in setting up the source is using the exe file located here to create a workload.  

## Capturing the workload from your source database

Capturing workload from your source database is the first step in the process and is mandatory. This step captures all the queries on your source server with their timestamps in the specified format (server-side traces or XEvents) which is replayed to target Azure SQL database.  
Before starting the capture session, please make sure that the source database has backup (in case you are testing production databases), if you already have a backup plan for your production database, start the capture session closer to the last backup. This ensures that minimal data integrity related errors while replaying the workloads.  

### Start New Capture  
1.	Launch DEA and go to All captures by clicking on the camera icon on the left navigation  

<image>  
    
2.	Click the New Capture Button to open the new capture screen  

<image>  
 
3.	Provide necessary inputs to begin capture.  
**Capture details**  
a.	_Capture name_: A name for identifying your capture session, this will also be used as part of naming the trace / xevents files.  
b.	Format: The format of the traces you want to capture, SQL Server supports two different forms of tracing, choose XEvents for this exercise.  
  i.	_Server-Side Traces (Trace in DEA)_: This is the oldest form of trace available and it is supported by all the versions of the SQL Server product. This form of traces is slightly expensive than the XEvents.  
  ii.	_XEvents_: XEvents or Extended Events are the newer form of trace that is supported from SQL Server 2012 to all the latest version of SQL Servers including the cloud version (Azure SQL Database and Azure SQL Managed Instance)  
c.	_Duration_: The amount of time you want to run the capture session, you should pick this time based on your application / workload you want to test. The ideal time would be the one that captures all the queries that gets executed on the database that is planned for migration. But if you have a workload with varying patterns, then you can just capture your workload during peak load. For this exercise, pick 10 minutes.  
d.	_Capture location_: The location where you want to store your trace files. The value for this field depends on the type of the server that you are going to run your capture session. Choose somewhere on your laptop, for this exercise.
    i.	 _SQL Server_: Drive path to store the files (ensure that you have enough storage on this drive). UNC paths are also accepted if SQL Server has permissions to write to that folder.   
    ii.	_Azure SQL DB / Azure SQL Managed Instance_: Provide Azure blob container’s Shared Access Signature (SAS) key URL. Refer this link for setting up blob container for capturing and storing XEvents from Azure SQL DB and/or Azure SQL Managed Instance.  
    iii.	_SQL Server on Linux_: Volume path  
    
**SQL Server Connection Details**  
e.	 _Server Type_: The type of the server from which you want to capture the traces / XEvents. Choose SQL Server for this exercise.  
    i.	 _SQL Server_ – All the box product versions from SQL 2005 to SQL 2017 including SQL Server on Linux   
    ii.	 _Azure SQL DB_ – Single Database Azure SQL PaaS  
    iii. _Azure SQL Managed Instance_ – Fully managed SQL Server with option to host multiple databases.    
f.	 _Server Name_: SQL Server instance name you would like to capture trace from.  
g.	_Authentication Type_: The type of the authentication you want to use to connect to the SQL Server  
    i.	_Windows_: Use this when your SQL Server is connected to Active Directory  
    ii.	SQL Authentication: Use this when your SQL Server is configured for SQL Authentication  
h.	_Database name_: Name of the database on the SQL Server to capture trace from  
    i.	For Azure SQL DB, you need to provide the name of the database.   
i.	_Encrypt connection_: Encrypts the connection between DEA and SQL Server  
j.	_Trust server certificate_: Trust the certificate installed on the SQL Server for encryption.  
k.	_Username_: Name of the SQL User  
l.	_Password_: Password for the SQL User  
Please note that the SQL Server service account should have access to the source trace file path.  
Once you have provided all the necessary inputs, please double check that you have taken a backup of the target database(s) and check the checkbox, then press Start to start capture.  
Any validation errors will be reported at the top of the screen.  

<image>  
    

You can see the detailed error logs in DEA Log file, which is usually found in the path %TMP%/DEA. You can open the error log folder by clicking the three dots in the error notification panel.  
Upon passing all the validation checks, the application will automatically take you to the capture progress screen.  

<iamge>  

You will then see the progress of your capture, including start time, duration, and remaining time.  

To stop the currently running trace, select the capture session name from the grid and click on the stop button.   

Please note that you need to keep DEA application running while the capture session is running.  

Any errors that occur during the capture progress will be displayed in the capture progress screen.  

You can start multiple capture sessions while waiting for this capture to finish. Once your capture is completed, you can use the output trace file to start the second phase: replaying the trace file on your target servers.   
Check out this FAQ page for frequently asked questions for Capture.  

## Replaying the workload to Azure SQL Database
The second step is to replay the captured workload on your Azure SQL Database. This needs to be the exact same database you collected capture from. For this exercise use the Azure SQL database we provided.  

### Start New Replay
1.	Launch DEA and go to All replays screen by clicking on the arrow icon on the left navigation  

<image>  
    
2.	Click the New Replay Button to open the new replay screen
    
<image>  
    
3.	Provide necessary inputs to begin replay:  

**Replay Details**  
a.	Replay name: name for your replay session.  
b.	Source Trace Format: Format of the trace captured in the capture step. Select XEvents for this exercise.  
c.	Source Trace Location: Location of the trace files captured in the capture step. Choose Local for this exercise.   
  i.	Local: If your files in the local disk or any file share locations  
  ii.	Blob: If your trace files are copied over to a blob storage.  
d.	Full path to Source File: The path where you have your source trace. Provide the same path you gave in 3(d) above.  
  i.	For Blob: Provide Azure blob container’s Shared Access Signature (SAS) key URL. Refer this link for setting up blob container for capturing and storing XEvents from Azure SQL DB and/or Azure SQL Managed Instance.  
  ii.	For Local: Drive path to store the files (ensure that you have enough storage on this drive). UNC paths are also accepted if SQL Server has permissions to write to that folder.  
e.	Replay Tool: The type of replay tool you want to use to replay your workloads. Two types of replay methods are supported. We will use InBuilt for this exercise.  
  i.	DReplay: SQL Server Distributed Replay that ships with SQL Server product. Currently supports only replaying trace files and to SQL Box products configured to use windows authentication  
  ii.	InBuilt: Use this option to replay XEvents based traces against any SQL Server version, supports both windows and SQL Server authentication.  
f.	Replay Trace location: The location where you want to store your trace files that we capture while replaying your workload to target server. The value for this field depends on the type of the server that you are going to run your capture session.  
   i.	Azure SQL DB / Azure SQL Managed Instance: Provide Azure blob container’s Shared Access Signature (SAS) key URL. Refer this link for setting up blob container for capturing and storing XEvents from Azure SQL DB and/or Azure SQL Managed Instance.  
  ii.	SQL Server: Drive path to store the files (ensure that you have enough storage on this drive). UNC paths are also accepted as long as SQL Server has permissions to write to that folder
  iii.	SQL Server on Linux: Volume path

**SQL Server connection details**  
g.	Server Type: The type of the server to which you want to replay the traces / XEvents. For this tutorial, select Azure SQL Database.
  i.	SQL Server – All the box product versions from SQL 2005 to SQL 2017 including SQL Server on Linux  
  ii.	Azure SQL DB – Single Database Azure SQL PaaS  
  iii.	Azure SQL Managed Instance – Fully managed SQL Server with option to host multiple databases.  
h.	Server Name: SQL Server instance name you would like to capture trace from.  
  i.	Authentication Type: The type of the authentication you want to use to connect to the SQL Server  
  i.	Windows: Use this when your SQL Server is connected to Active Directory  
  ii.	SQL Authentication: Use this when your SQL Server is configured for SQL Authentication  
j.	Database name: Name of the database on the SQL Server to capture trace from   
i.	For Azure SQL DB, you need to provide the name of the database.   
k.	Encrypt connection: Encrypts the connection between DEA and SQL Server  
l.	Trust server certificate: Trust the certificate installed on the SQL Server for encryption.  
m.	Username: Name of the SQL User  
n.	Password: Password for the SQL User  
After providing all necessary inputs, double check that you have restored the backup from the first step, check the checkbox to indicate this, and press Start to start replay. Much like New Capture, you will be able to see the status of your replay. After replaying the source trace on both of your target servers, are ready to generate analysis report.  
Check out this FAQ page for commonly asked questions for Replay.   

## Analyzing / Comparing Traces
The final step is to generate an analysis report using the replay traces to gain insights on performance implications of the migration.

### Prerequisites
DEA requires a SQL Server instance to import the captured and replayed traces and perform detailed statistical analysis:
Tip: You could install SQL Server in the DEA controller machine.   

#### Start New Analysis
1.	Go to Analysis Reports on the left navigation. Connect to the SQL Server where you will store your report databases. You will see the list of all reports in the server. To create a new report, click on New Report.  

<image>  

2.	Connect to a SQL Server configured in the prerequisite section using either windows or sql authentication. The user should have create database permissions.  

<image>  
    
 If you are connecting to a remote server including SQL Azure DB, then you need to choose authentication type as SQL Server.

3.	Click the new analysis button and provide the necessary details.  

**Report information**  
a.	Report name: name of the analysis report to be created.  
b.	Server name: Server where you want to store the analysis reports.  

**Trace files to Analyze**

c.	Trace for Target 1 SQL Server: path for the trace file from replaying on Target 1.  
d.	Trace for Target 2 SQL Server: path for the trace file from replaying on Target 2.   


After providing these inputs, click Start to generate report. You will see your newly generated report on top of the list, and the icon next to it will turn to a green checkmark when it is completed; now you can view the analysis report to gain insights provided by your A/B test.  
Check out this FAQ page for commonly asked questions for Analysis.  




    






