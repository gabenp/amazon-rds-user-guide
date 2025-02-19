# Installing a Siebel database on Oracle on Amazon RDS<a name="Oracle.Resources.Siebel"></a>

You can use Amazon RDS to host a Siebel Database on an Oracle DB instance\. The Siebel Database is part of the Siebel Customer Relationship Management \(CRM\) application architecture\. For an illustration, see [ Generic architecture of Siebel business application](https://docs.oracle.com/cd/E63029_01/books/PerformTun/performtun_archinfra.htm#i1043361)\. 

Use the following topic to help set up a Siebel Database on an Oracle DB instance on Amazon RDS\. You can also find out how to use Amazon Web Services to support the other components required by the Siebel CRM application architecture\. 

**Note**  
To install a Siebel Database on Oracle on Amazon RDS, you need to use the master user account\. You don't need `SYSDBA` privilege; master user privilege is sufficient\. For more information, see [Master user account privileges](UsingWithRDS.MasterAccounts.md)\. 

## Licensing and versions<a name="Oracle.Resources.Siebel.Versions"></a>

To install a Siebel Database on Amazon RDS, you must use your own Oracle Database license, and your own Siebel license\. You must have the appropriate Oracle Database license \(with Software Update License and Support\) for the DB instance class and Oracle Database edition\. For more information, see [Oracle licensing options](Oracle.Concepts.Licensing.md)\. 

Oracle Database Enterprise Edition is the only edition certified by Siebel for this scenario\. Amazon RDS supports Siebel CRM version 15\.0 or 16\.0\. Use Oracle Database 12c Release 1 \(12\.1\.0\.2\.0\)\. For the procedures following, we use Siebel CRM version 15\.0 and Oracle Database Release 1 \(12\.1\.0\.2\) or Oracle Database Release 2 \(12\.2\.0\.1\)\. For more information, see [Oracle Database 12c with Amazon RDS](Oracle.Concepts.database-versions.md#Oracle.Concepts.FeatureSupport.12c)\. 

Amazon RDS supports database version upgrades\. For more information, see [Upgrading a DB instance engine version](USER_UpgradeDBInstance.Upgrading.md)\. 

## Before you begin<a name="Oracle.Resources.Siebel.BeforeYouBegin"></a>

Before you begin, you need an Amazon VPC\. Because your Amazon RDS DB instance needs to be available only to your Siebel Enterprise Server, and not to the public Internet, your Amazon RDS DB instance is hosted in a private subnet, providing greater security\. For information about how to create an Amazon VPC for use with Siebel CRM, see [Creating a VPC for use with an Oracle database](Oracle.Resources.Shared.md#Oracle.Resources.Shared.VPC)\. 

Before you begin, you also need an Oracle DB instance\. For information about how to create an Oracle DB instance for use with Siebel CRM, see [Creating an Oracle DB instance](Oracle.Resources.Shared.md#Oracle.Resources.Shared.Database.RDS)\. 

## Installing and configuring a Siebel database<a name="Oracle.Resources.Siebel.Database.Siebel"></a>

After you create your Oracle DB instance, you can install your Siebel Database\. You install the database by creating table owner and administrator accounts, installing stored procedures and functions, and then running the Siebel Database Configuration Wizard\. For more information, see [ Installing the Siebel database on the RDBMS](https://docs.oracle.com/cd/E63029_01/books/SiebInstWIN/SiebInstCOM_ConfigDB.html)\. 

To run the Siebel Database Configuration Wizard, you need to use the master user account\. You don't need `SYSDBA` privilege; master user privilege is sufficient\. For more information, see [Master user account privileges](UsingWithRDS.MasterAccounts.md)\. 

## Using other Amazon RDS features with a Siebel database<a name="Oracle.Resources.Siebel.Miscellaneous"></a>

After you create your Oracle DB instance, you can use additional Amazon RDS features to help you customize your Siebel Database\.

### Collecting statistics with the Oracle Statspack option<a name="Oracle.Resources.Siebel.Options"></a>

You can add features to your DB instance through the use of options in DB option groups\. When you created your Oracle DB instance, you used the default DB option group\. If you want to add features to your database, you can create a new option group for your DB instance\. 

If you want to collect performance statistics on your Siebel Database, you can add the Oracle Statspack feature\. For more information, see [Oracle Statspack](Appendix.Oracle.Options.Statspack.md)\. 

Some option changes are applied immediately, and some option changes are applied during the next maintenance window for the DB instance\. For more information, see [Working with option groups](USER_WorkingWithOptionGroups.md)\. After you create a customized option group, modify your DB instance to attach it\. For more information, see [Modifying an Amazon RDS DB instance](Overview.DBInstance.Modifying.md)\. 

### Performance tuning with parameters<a name="Oracle.Resources.Siebel.Parameters"></a>

You manage your DB engine configuration through the use of parameters in a DB parameter group\. When you created your Oracle DB instance, you used the default DB parameter group\. If you want to customize your database configuration, you can create a new parameter group for your DB instance\. 

When you change a parameter, depending on the type of the parameter, the changes are applied either immediately or after you manually reboot the DB instance\. For more information, see [Working with parameter groups](USER_WorkingWithParamGroups.md)\. After you create a customized parameter group, modify your DB instance to attach it\. For more information, see [Modifying an Amazon RDS DB instance](Overview.DBInstance.Modifying.md)\. 

To optimize your Oracle DB instance for Siebel CRM, you can customize certain parameters\. The following table shows some recommended parameter settings\. For more information about performance tuning Siebel CRM, see [Siebel CRM Performance Tuning Guide](https://docs.oracle.com/cd/E63029_01/books/PerformTun/toc.htm)\.  


****  

| Parameter name | Default value | Guidance for optimal Siebel CRM performance | 
| --- | --- | --- | 
| \_always\_semi\_join | `CHOOSE` | `OFF`  | 
| \_b\_tree\_bitmap\_plans | `TRUE` | `FALSE`  | 
| \_like\_with\_bind\_as\_equality | `FALSE` | `TRUE`  | 
| \_no\_or\_expansion | `FALSE` | `FALSE`  | 
| \_optimizer\_join\_sel\_sanity\_check | `TRUE` | `TRUE`  | 
| \_optimizer\_max\_permutations | 2000 | 100  | 
| \_optimizer\_sortmerge\_join\_enabled | `TRUE` | `FALSE`  | 
| \_partition\_view\_enabled | `TRUE` | `FALSE`  | 
| open\_cursors | `300` | At least **2000**\.  | 

### Creating snapshots<a name="Oracle.Resources.Siebel.Snapshots"></a>

After you create your Siebel Database, you can copy the database by using the snapshot features of Amazon RDS\. For more information, see [Creating a DB snapshot](USER_CreateSnapshot.md) and [Restoring from a DB snapshot](USER_RestoreFromSnapshot.md)\. 

## Support for other Siebel CRM components<a name="Oracle.Resources.Siebel.OtherComponents"></a>

In addition to your Siebel Database, you can also use Amazon Web Services to support the other components of your Siebel CRM application architecture\. You can find more information about the support provided by Amazon AWS for additional Siebel CRM components in the following table\. 


****  

| Siebel CRM component | Amazon AWS Support | 
| --- | --- | 
| Siebel Enterprise\(with one or more Siebel Servers\) |  You can host your Siebel Servers on Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. You can use Amazon EC2 to launch as many or as few virtual servers as you need\. Using Amazon EC2, you can scale up or down easily to handle changes in requirements\. For more information, see [What is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)  You can put your servers in the same VPC with your DB instance and use the VPC security group to access the database\. For more information, see [Working with a DB instance in a VPC](USER_VPC.WorkingWithRDSInstanceinaVPC.md)\.   | 
| Web Servers\(with Siebel Web Server Extensions\) |  You can install multiple Web Servers on multiple EC2 instances\. You can then use Elastic Load Balancing to distribute incoming traffic among the instances\. For more information, see [What is Elastic Load Balancing?](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/elastic-load-balancing.html)   | 
| Siebel Gateway Name Server |  You can host your Siebel Gateway Name Server on an EC2 instance\. You can then put your server in the same VPC with the DB instance and use the VPC security group to access the database\. For more information, see [Working with a DB instance in a VPC](USER_VPC.WorkingWithRDSInstanceinaVPC.md)\.   | 