## Analyzing / Comparing Traces
The final step is to generate an analysis report using the replay traces to gain insights on performance implications of the migration.

### Prerequisites
DEA requires a SQL Server instance to import the captured and replayed traces and perform detailed statistical analysis:
Tip: You could install SQL Server in the DEA controller machine.   

#### Start New Analysis
1.	Go to Analysis Reports on the left navigation. Connect to the SQL Server where you will store your report databases. You will see the list of all reports in the server. To create a new report, click on New Report.  


<img src="/SQL-DEA/images/image-09.png"/>  


<img src="/SQL-DEA/images/image-10.png"/>   

2.	Connect to a SQL Server configured in the prerequisite section using either windows or sql authentication. The user should have create database permissions.  

<img src="/SQL-DEA/images/image-11.png"/>   
  
    
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
