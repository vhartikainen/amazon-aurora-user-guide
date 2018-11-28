# IAM Database Authentication for Aurora MySQL<a name="UsingWithRDS.IAMDBAuth"></a>

With Aurora with MySQL compatibility, you can authenticate to your DB cluster using AWS Identity and Access Management \(IAM\) database authentication\. With this authentication method, you don't need to use a password when you connect to a DB instance\. Instead, you use an authentication token\.

An *authentication token* is a unique string of characters that Amazon RDS generates on request\. Authentication tokens are generated using AWS Signature Version 4\. Each token has a lifetime of 15 minutes\. You don't need to store user credentials in the database, because authentication is managed externally using IAM\. You can also still use standard database authentication\.

IAM database authentication provides the following benefits:
+ Network traffic to and from the database is encrypted using Secure Sockets Layer \(SSL\)\.
+ You can use IAM to centrally manage access to your database resources, instead of managing access individually on each DB cluster\.
+ For applications running on Amazon EC2, you can use EC2 instance profile credentials to access the database instead of a password, for greater security\.

**Topics**
+ [Availability for IAM Database Authentication](#UsingWithRDS.IAMDBAuth.Availability)
+ [Limitations for IAM Database Authentication](#UsingWithRDS.IAMDBAuth.ConnectionsPerSecond)
+ [Enabling and Disabling IAM Database Authentication](UsingWithRDS.IAMDBAuth.Enabling.md)
+ [Creating and Using an IAM Policy for IAM Database Access](UsingWithRDS.IAMDBAuth.IAMPolicy.md)
+ [Creating a Database Account](UsingWithRDS.IAMDBAuth.DBAccounts.md)
+ [Connecting to Your DB Cluster](UsingWithRDS.IAMDBAuth.Connecting.md)

## Availability for IAM Database Authentication<a name="UsingWithRDS.IAMDBAuth.Availability"></a>

IAM database authentication is available for Aurora with MySQL compatibility, version 1\.10 or higher\. All instance classes are supported, except for `db.t2.small`\.

## Limitations for IAM Database Authentication<a name="UsingWithRDS.IAMDBAuth.ConnectionsPerSecond"></a>

With IAM database authentication, you are limited to a maximum of 20 new connections per second\. If you are using a `db.t2.micro` instance class, the limit is 10 connections per second\.

The Aurora with MySQL compatibility database engines do not impose any limits on authentication attempts per second\. However, when you use IAM database authentication, your application must generate an authentication token\. Your application then uses that token to connect to the DB cluster\. If you exceed the maximum new\-connection\-per\-second limit, then the extra overhead of IAM database authentication can cause connection throttling\. The extra overhead can even cause existing connections to drop\.

We recommend the following:
+ Use IAM database authentication as a mechanism for temporary, personal access to databases\.
+ Don't use IAM database authentication if your application requires more than 20 new connections per second\.
+ Use IAM database authentication only for workloads that can be easily retried\.

**Note**  
For information about the maximum total connections for Aurora MySQL, see [Maximum Connections to an Aurora MySQL DB Instance](AuroraMySQL.Managing.Performance.md#AuroraMySQL.Managing.MaxConnections)\.