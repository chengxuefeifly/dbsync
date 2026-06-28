# dbsync
I developed a real-time synchronization tool for SQL Server databases, which operates based on the binary parsing of underlying transaction logs.


二、产品使用



2.1 日志分析模块
设置环境变量DBSYNC_HOME,需要用cmd的管理模式,支持结构迁移、全量迁移、增量迁移【INSERT、UPDATE、DELETE】，不支持DDL

<img width="3205" height="645" alt="image" src="https://github.com/user-attachments/assets/c5f06d94-0d25-4e19-aa21-6b8bfcf5be13" />

2.2 落地文件发送模块
读取源端db.conf中的目标的IP地址,进行文件发送,借助端口32000
<img width="2840" height="1265" alt="image" src="https://github.com/user-attachments/assets/eeecaaa3-ac1c-4cd8-894a-b4aea2d4bbf2" />

2.3 文件接收模块
启用IP地址,进行文件接收,端口32000
<img width="3263" height="1215" alt="image" src="https://github.com/user-attachments/assets/bb61fdb5-3078-4361-9bd9-c3d118a97c6a" />


2.4 数据装载模块
读取db.conf目标端数据库信息,然后将相关的表装进去
<img width="2870" height="1262" alt="image" src="https://github.com/user-attachments/assets/fdcc2569-f9f9-44b3-87da-80900594309f" />


三、配置文件说明
源端目录架构
bin、config、spool、cache

目标端目录架构
bin、config、spool、cache


3.1 源端配置


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
 
#日志分析模式0、本地(增量解析的效率高)     1、远程

Log_Analysis_Pattern="0"
DBSYNC_HOME=""
#exp_sync_sql=SELECT  TABLE_CATALOG,TABLE_SCHEMA,TABLE_NAME FROM INFORMATION_SCHEMA.TABLES where TABLE_NAME='Table_97'
exp_sync_sql=SELECT  TABLE_CATALOG,TABLE_SCHEMA,TABLE_NAME FROM INFORMATION_SCHEMA.TABLES where TABLE_CATALOG='source'  



3.2 目标端配置


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


4、注意事项

启动cmd的时候,需要用管理员权限,如下所示:

<img width="1610" height="513" alt="image" src="https://github.com/user-attachments/assets/931f960b-a6fa-4beb-9a81-58895256764e" />

