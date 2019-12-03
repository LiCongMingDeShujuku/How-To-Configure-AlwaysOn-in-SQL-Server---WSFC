![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 如何在SQL Server & WSFC中搭建AlwaysOn
#### How To Configure AlwaysOn in SQL Server & WSFC

## Contents

- [中文](#中文)
- [English](#English)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
如何在Windows Server 的故障转移群集（WSFC）中搭建SQL Server 2012 AlwaysOn。 在此环境中，我使用2个成员服务器（SQLC-01和SQLc-02）配置了基本群集。 群集是使用Node和FileShare Majority创建的，没有共享存储。你必须至少有2台服务器已在群集中配置，如果你使用的是多子网环境，则每个子网必须拥有2个可用的IP地址。这些不能提前分配给任何服务器或在DNS设置。


## English
How to configure SQL Server 2012 AlwaysOn in a Windows Server Failover Cluster ( WSFC ). In this environment I have configured a basic Cluster using 2 Member Servers ( SQLC-01 & SQLc-02 ). The cluster was created using a Node and FileShare Majority without shared storage. You must have at least 2 servers that have already been configured in a cluster, and if you are using a multi-subnet environment you must have 2 available IP addresses one for each subnet. These must not be formerly assigned to any server or setup in DNS.

1. Login to the Server where SQL Server has been installed and open ‘SQL Server Configuration Manager’.
1.登陆已安装SQL Server和开启‘SQL Server Configuration Manager’功能的服务器

![#](images/01-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")

 
2. Right-click the database service and select ‘Properties’.
2.右键单击数据库服务，然后选择“Properties’”

![#](images/02-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
 
3. Select the tab ‘AlwaysOn High Availability’, and check ‘Enable AlwaysOn Availability Groups’, and click ‘OK’.
Note: This process must be repeated for both nodes in the cluster ( Primary and Secondary Servers ).
3.选择“AlwaysOn High Availability”选项卡，选中“Enable AlwaysOn Availability Groups”，然后单击“OK”。
注意：此过程必须在群集中的两个节点（主服务器和辅助服务器）重复。

![#](images/03-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
 
4. Next; you will need to open the ‘SQL Server Management Studio’. Ensure you are running it under the Database Service Account. In this case we are using a database service account called “SQL_SVC”.
4.下一步; 你需要打开“SQL Server Management Studio”。确保你在数据库服务帐户下运行。在这种情况下，我们使用名为“SQL_SVC”的数据库服务帐户。

![#](images/04-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
 
5. Specify the appropriate credentials and click ‘OK’.
5.确定相应的凭据，然后单击“OK”。

![#](images/05-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
 
6. Click ‘Connect’.
6.单击“Connect”。

![#](images/06-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
 
7. Right-click ‘AlwaysOn High Availability’ and select ‘New Availability Group Wizard’.
7.右键单击“AlwaysOn High Availability”，然后选择“New Availability Group Wizard’”。

![#](images/07-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
 
8. Click ‘Next’.
8. 单击 ‘Next’.
![#](images/08-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")

 
9. Type the name of your Availability Group ( In this example we are using SQLCAG ) and click ‘Next’.
9.键入可用性组的名称（在此示例中，我们使用的是SQLCAG），然后单击“Next”。
![#](images/09-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
 
10. Any database you have available will automatically be checked in the list. This is your opportunity to uncheck any databases you wish NOT to be configured for AlwaysOn. Simply Uncheck the databases you want to exclude from the configuration. In this configuration I have created a sample database called DBSYSMON. Click ‘Next’ to continue.
10. 你可以使用的任何数据库都将自动在列表中进行检查。你可以取消选中您不希望为AlwaysOn配置的任何数据库。只需取消选中要从配置中排除的数据库。在这个配置中，我创建了一个名为DBSYSMON的示例数据库。单击 “Next” 以继续。

![#](images/10-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
 
11. Click ‘Add Replica’.
11. 单击 ‘Add Replica’.
![#](images/11-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
 
12. Click ‘Connect’ so you can authenticate to the other Database Server ( Secondary server in the Cluster ). In this example we are using a server called SQLC-02.
12.单击“Connect”，这样便可以对其他数据库服务器（群集中的辅助服务器）进行身份验证。 在此示例中，我们使用名为SQLC-02的服务器。

![#](images/12-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
 
13. Complete the following actions before going to the Endpoints tab.
1. Check the box for ‘Automatic Failover’.
2. Check the box for ‘Synchronous Commit’.
3. Check the box for ‘Readable Secondary’.
4. Select the next tab ‘Endpoints’.
13.在转到端点选项卡之前，请完成以下操作。
1.选中“Automatic Failover”复选框。
2.选中 ‘Synchronous Commit’复选框.
3.选中 ‘Readable Secondary’复选框.
4.选择下一个选项卡“Endpoints”。

![#](images/13-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
 
14. Rename the ‘Endpoint Name’ by adding the suffix “_SQLC”. This will allow you to differentiate the Endpoint names from any other Endpoint Configuration in the future.
1. Add suffix “_SQLC”.
2. Click the tab ‘Backup Preferences’.
14.通过添加后缀“_SQLC”重命名“Endpoint Name”。这将允许您在将来区分端点名称和任何其他端点搭建。
1.添加后缀“_SQLC”
2.单击选项卡“Backup Preferences”

![#](images/14-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
 
15. Select the option for ‘Any Replica’ and click the tab ‘Listener’.
15.选择“Any Replica”选项，然后单击“Listener”选项卡。

![#](images/15-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")

 
16. Complete the steps for creating the Listener by performing the following actions.
a. 1. Select the option for ‘Create an availability group listener’.
b. 2. Create the Listener DNS Name, and provide the Port and Network Mode.
a. Listener DNS Name: SQLC_Listener
b. Port: 1433
c. Network Mode: Static IP
c. 3. Select ‘Add’.
d. 4. Provide the IP from the first subnet, then follow this process again by clicking the ‘Add’ and prove the IP for the second subnet.
e. 5. Click ‘Next’.
16.通过执行以下操作完成创建监听器的步骤。
a.1.选择“‘Create an availability group listener”选项。
b.2.创建监听器DNS名称，并提供端口和网络模式。
a.监听器DNS名称：SQLC_Listener
b.端口：1433
c. 网络模式：静态IP
c. 3. 选择“Add”。
d. 4. 从第一个子网提供IP，然后通过单击“Add”再次执行此过程并证明第二个子网的IP。

![#](images/16-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
e. 5.单击 “Next”。 
17. Provide the Share name you have configured for the Synchronization process. In this example we are using: \MyServerSQLCReplica, and click ‘Next’.
Note: The replica share has to have the correct permissions applied to it. You will need to add the following objects as both Full Control for the folder level, and for the Advanced Share Permissions:
a. SQL Database Service Account
b. SQL Agent Service Account
c. SQLC-01$ AD Server Object
d. SQLC-02$ AD Server Object
17.提供你为同步过程搭建的共享名称。 在此示例中，我们使用：\ MyServerSQLCReplica，然后单击“Next”。
注意：副本共享必须具有应用于它的正确权限。 你需要将以下对象添加为both Full Control for the folder level, 和the Advanced Share Permissions:
a. SQL数据库服务帐户
b. SQL服务器服务帐户
C. SQLC-01 $ AD服务器对象
d. SQLC-02 $ AD服务器对象

![#](images/17-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#") 
18. Click ‘Next’ for the Validation.
18. 单击 ‘Next’ 进行验证。

![#](images/18-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
 
19. Click ‘Finish’ for the summary.
19. 单击 ‘Finish’ 进行总结。

![#](images/19-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")
 
Monitor the progress if necessary, but the process is complete at this stage.
必要时监控进度，但此过程在此阶段完成。

![#](images/20-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#") 
You may see an informational message stating “The current WSFC cluster quorum vote configuration is not recommended for this availability group”. This is simply a recommendation and by no means is this configuration not supported. Click ‘OK’ to finish.
你可能会看到一条信息性消息，指出“不建议此可用性组使用当前的WSFC群集仲裁投票配置”。这只是一个建议，但此搭建绝不支持。 单击“OK”完成。

![#](images/21-如何在SQL-Server-WSFC中搭建AlwaysOn.png?raw=true "#")

 
Naturally; you should test your AlwaysOn configuration to ensure they are working properly.
那么自然地接下来，你应该测试你的AlwaysOn搭建以确保它们可以正常工作。















[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

