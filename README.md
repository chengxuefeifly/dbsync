# dbsync
I developed a real-time synchronization tool for SQL Server databases, which operates based on the binary parsing of underlying transaction logs.


 
二、Product Usage Instructions

2.1 Log Analysis Module
Set the environment variable DBSYNC_HOME with CMD running under administrator privileges. It supports schema migration, full data migration and incremental migration (INSERT, UPDATE, DELETE), while DDL operations are not supported.

<img width="3205" height="645" alt="image" src="https://github.com/user-attachments/assets/c5f06d94-0d25-4e19-aa21-6b8bfcf5be13" />

2.2 Local File Sending Module
Read the target IP address configured in the source db.conf file and send files via port 32000.

<img width="2840" height="1265" alt="image" src="https://github.com/user-attachments/assets/eeecaaa3-ac1c-4cd8-894a-b4aea2d4bbf2" />

2.3 File Receiving Module
Enable the specified IP address to receive files on port 32000.

<img width="3263" height="1215" alt="image" src="https://github.com/user-attachments/assets/bb61fdb5-3078-4361-9bd9-c3d118a97c6a" />


2.4 Data Loading Module
Reads the target database information from db.conf, then loads the corresponding tables.
<img width="2870" height="1262" alt="image" src="https://github.com/user-attachments/assets/fdcc2569-f9f9-44b3-87da-80900594309f" />


三. Configuration File Description
Source Directory Structure
bin、config、spool、cache

Target Directory Structure
bin、config、spool、cache


3.1 Source Side Configuration


3.1.1 db.conf

[SOURCE]
SOURCE.DRIVER="{ODBC Driver 18 for SQL Server}"
SOURCE.SERVER="localhost\SQLEXPRESS"
SOURCE.DATABASE="source"
SOURCE.SCHEMA="dbo"
SOURCE.Trusted_Connection="yes"
SOURCE.Encrypt="no"
SOURCE.TrustServerCertificate="yes"
SOURCE.IP="localhost"

[TARGET]
TARGET.DRIVER="{ODBC Driver 18 for SQL Server}"
TARGET.SERVER="localhost\SQLEXPRESS"
TARGET.DATABASE="wuwen"
TARGET.SCHEMA="dbo"
TARGET.Trusted_Connection="yes"
TARGET.Encrypt="no"
TARGET.TrustServerCertificate="yes"
TARGET.IP="127.0.0.1"



3.1.2 dbsync.conf
 
# Log Analysis Mode
#0. Local (High efficiency for incremental parsing)
#1. Remote

Log_Analysis_Pattern="0"
DBSYNC_HOME=""
#exp_sync_sql=SELECT  TABLE_CATALOG,TABLE_SCHEMA,TABLE_NAME FROM INFORMATION_SCHEMA.TABLES where TABLE_NAME='Table_97'
exp_sync_sql=SELECT  TABLE_CATALOG,TABLE_SCHEMA,TABLE_NAME FROM INFORMATION_SCHEMA.TABLES where TABLE_CATALOG='source'  



3.2 Target Side Configuration


3.2.1  db.conf



[TARGET]
TARGET.DRIVER="{ODBC Driver 18 for SQL Server}"
TARGET.SERVER="localhost\SQLEXPRESS"
TARGET.DATABASE="wuwen"
TARGET.SCHEMA="dbo"
TARGET.Trusted_Connection="yes"
TARGET.Encrypt="no"
TARGET.TrustServerCertificate="yes"
TARGET.IP="127.0.0.1"


## IV. Precautions

When launching CMD, you must run it with administrator privileges, as shown below:

<img width="1610" height="513" alt="image" src="https://github.com/user-attachments/assets/931f960b-a6fa-4beb-9a81-58895256764e" />


VI. If you have any questions, you can email me at 617889031@qq.com, join our QQ group 164804617, or leave a message here.
