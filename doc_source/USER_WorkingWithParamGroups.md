# Working with parameter groups<a name="USER_WorkingWithParamGroups"></a>

*Database parameters* specify how the database is configured\. For example, database parameters can specify the amount of resources, such as memory, to allocate to a database\.

 You manage your database configuration by associating your DB instances and Multi\-AZ DB clusters with parameter groups\. Amazon RDS defines parameter groups with default settings\. 

**Important**  
You can define your own parameter groups with customized settings\. Then you can modify your DB instances Multi\-AZ DB clusters to use your own parameter groups\.  
For information about modifying a DB instance, see [Modifying an Amazon RDS DB instance](Overview.DBInstance.Modifying.md)\. For information about modifying a Multi\-AZ DB clusters, see [Modifying a Multi\-AZ DB cluster](modify-multi-az-db-cluster.md)\.

**Note**  
Some DB engines offer additional features that you can add to your database as options in an option group\. For information about option groups, see [Working with option groups](USER_WorkingWithOptionGroups.md)\.

 A *DB parameter group* acts as a container for engine configuration values that are applied to one or more DB instances\. 

 *DB cluster parameter groups* only apply to Multi\-AZ DB clusters\. In a Multi\-AZ DB cluster, the settings in the DB cluster parameter group are used for all of the DB instances in the cluster\. The default DB parameter group for the DB engine and DB engine version is used for each DB instance in the DB cluster\. 

 If you create a DB instance without specifying a DB parameter group, the DB instance uses a default DB parameter group\. Likewise, if you create a Multi\-AZ DB cluster without specifying a DB cluster parameter group, the DB cluster uses a default DB cluster parameter group\. Each default parameter group contains database engine defaults and Amazon RDS system defaults based on the engine, compute class, and allocated storage of the instance\. You can't modify the parameter settings of a default parameter group\. Instead, you create your own parameter group where you choose your own parameter settings\. Not all DB engine parameters can be changed in a parameter group that you create\. 

 To use your own parameter group, you create a new parameter group and modify the parameters that you want to modify\. You then modify your DB instance or DB cluster to use the new parameter group\. If you update parameters within a DB parameter group, the changes apply to all DB instances that are associated with that parameter group\. Likewise, if you update parameters within a Multi\-AZ DB cluster parameter group, the changes apply to all Aurora clusters that are associated with that DB cluster parameter group\. 

 You can copy an existing DB parameter group with the AWS CLI [copy\-db\-parameter\-group](https://docs.aws.amazon.com/cli/latest/reference/rds/copy-db-parameter-group.html) command\. You can copy an existing DB cluster parameter group with the AWS CLI [copy\-db\-cluster\-parameter\-group](https://docs.aws.amazon.com/cli/latest/reference/rds/copy-db-cluster-parameter-group.html) command\. Copying a parameter group can be convenient when you want to include most of an existing DB parameter group's custom parameters and values in a new DB parameter group\. 

Here are some important points about working with parameters in a parameter group:
+ Database parameters are either *static* or *dynamic*\. When you change a static parameter and save the DB parameter group, the parameter change takes effect after you manually reboot the DB instance\. You can reboot a DB instance using the RDS console, by calling the `reboot-db-instance` CLI command, or by calling the `RebootDBInstance` API operation\. The requirement to reboot the associated DB instance after a static parameter change helps mitigate the risk of a parameter misconfiguration affecting an API call, such as calling `ModifyDBInstance` to change DB instance class or scale storage\.

  When you change a dynamic parameter and save the DB parameter group, the change is applied to the parameter group immediately regardless of the **Apply Immediately** setting\. If you use the `pending-reboot` setting in the AWS CLI or RDS API, the change is still applied to the parameter group immediately\. However, applying the parameter change to DB instances that use the parameter group requires a reboot\.
**Note**  
Using `pending-reboot` with dynamic parameters in the AWS CLI or RDS API on RDS for SQL Server DB instances generates an error\. Use `apply-immediately` on RDS for SQL Server\.

  If a DB instance isn't using the latest changes to its associated DB parameter group, the AWS Management Console shows the DB parameter group with a status of **pending\-reboot**\. The **pending\-reboot** parameter groups status doesn't result in an automatic reboot during the next maintenance window\. To apply the latest parameter changes to that DB instance, manually reboot the DB instance\.
+ When you associate a new DB parameter group with a DB instance, the modified static and dynamic parameters are applied only after the DB instance is rebooted\. However, if you modify dynamic parameters in the newly associated DB parameter group, these changes are applied immediately without a reboot\. For more information about changing the DB parameter group, see [Modifying an Amazon RDS DB instance](Overview.DBInstance.Modifying.md)\.
+ After you change the DB cluster parameter group associated with a Multi\-AZ DB cluster, reboot the DB cluster to apply the changes to all of the DB instances in the DB cluster\.

  For information about rebooting a Multi\-AZ DB cluster, see [Rebooting Multi\-AZ DB clusters and reader DB instances](multi-az-db-clusters-concepts-rebooting.md)\.
+ You can specify integer and Boolean parameters using expressions, formulas, and functions\. Functions can include a mathematical log expression\. For more information, see [Specifying DB parameters](USER_ParamValuesRef.md)\.
+ Set any parameters that relate to the character set or collation of your database in your parameter group before creating the DB instance or Multi\-AZ DB cluster and before you create a database in it\. This ensures that the default database and new databases use the character set and collation values that you specify\. If you change character set or collation parameters, the parameter changes aren't applied to existing databases\.

  For some DB engines, you can change character set or collation values for an existing database using the `ALTER DATABASE` command, for example:

  ```
  ALTER DATABASE database_name CHARACTER SET character_set_name COLLATE collation;
  ```

  For more information about changing the character set or collation values for a database, check the documentation for your DB engine\.
+ Improperly setting parameters in a parameter group can have unintended adverse effects, including degraded performance and system instability\. Always be cautious when modifying database parameters, and back up your data before modifying a parameter group\. Try out parameter group setting changes on a test DB instance or DB cluster before applying those parameter group changes to a production DB instance or DB cluster\.
+ To determine the supported parameters for your DB engine, you can view the parameters in the DB parameter group and DB cluster parameter group used by the DB instance or DB cluster\. For more information, see [Viewing parameter values for a DB parameter group](USER_WorkingWithDBInstanceParamGroups.md#USER_WorkingWithParamGroups.Viewing) and [Viewing parameter values for a DB cluster parameter group](USER_WorkingWithDBClusterParamGroups.md#USER_WorkingWithParamGroups.ViewingCluster)\.

**Topics**
+ [Working with DB parameter groups](USER_WorkingWithDBInstanceParamGroups.md)
+ [Working with DB cluster parameter groups for Multi\-AZ DB clusters](USER_WorkingWithDBClusterParamGroups.md)
+ [Comparing DB parameter groups](USER_WorkingWithParamGroups.Comparing.md)
+ [Specifying DB parameters](USER_ParamValuesRef.md)