---
title: "SUSER_SNAME (Transact-SQL)"
description: "SUSER_SNAME (Transact-SQL)"
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ""
ms.date: "07/29/2017"
ms.service: sql
ms.subservice: t-sql
ms.topic: reference
ms.custom: ""
f1_keywords:
  - "SUSER_SNAME_TSQL"
  - "SUSER_SNAME"
helpviewer_keywords:
  - "security identification names [SQL Server]"
  - "logins [SQL Server], users"
  - "SIDs [SQL Server]"
  - "SUSER_SNAME function"
  - "users [SQL Server], logins"
  - "logins [SQL Server], names"
  - "IDs [SQL Server], logins"
  - "identification numbers [SQL Server], logins"
  - "names [SQL Server], logins"
dev_langs:
  - "TSQL"
monikerRange: ">= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017 || = azuresqldb-mi-current"
---
# SUSER_SNAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Returns the login name associated with a security identification number (SID).  
  
 ![Topic link icon](../../database-engine/configure-windows/media/topic-link.gif "Topic link icon") [Transact-SQL Syntax Conventions](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## Syntax  
  
```syntaxsql
SUSER_SNAME ( [ server_user_sid ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## Arguments
 *server_user_sid*  
**Applies to**: [!INCLUDE[sql2008-md](../../includes/sql2008-md.md)] and later
  
 Is the optional login security identification number. *server_user_sid* is **varbinary(85)**. *server_user_sid* can be the security identification number of any [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] login or [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows user or group. If *server_user_sid* is not specified, information about the current user is returned. If the parameter contains the word NULL will return NULL.  
  
## Return Types  
 **nvarchar(128)**  
  
## Remarks  
 SUSER_SNAME can be used as a DEFAULT constraint in either ALTER TABLE or CREATE TABLE. SUSER_SNAME can be used in a select list, in a WHERE clause, and anywhere an expression is allowed. SUSER_SNAME must always be followed by parentheses, even if no parameter is specified.  
  
 When called without an argument, SUSER_SNAME returns the name of the current security context. When called without an argument within a batch that has switched context by using EXECUTE AS, SUSER_SNAME returns the name of the impersonated context. When called from an impersonated context, ORIGINAL_LOGIN returns the name of the original context.  
  
## [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Remarks  
 SUSER_NAME always return the login name for the current security context.  
  
 The SUSER_SNAME statement does not support execution using an impersonated security context through EXECUTE AS.  
  
## Examples  
  
### A. Using SUSER_SNAME  
 The following example returns the login name for the current security context.  
  
```sql
SELECT SUSER_SNAME();  
GO  
```  
  
### B. Using SUSER_SNAME with a Windows user security ID  
 The following example returns the login name associated with a Windows security identification number.  
  
**Applies to**: [!INCLUDE[sql2008-md](../../includes/sql2008-md.md)] and later
  
```sql
SELECT SUSER_SNAME(0x010500000000000515000000a065cf7e784b9b5fe77c87705a2e0000);  
GO  
```  
  
### C. Using SUSER_SNAME as a DEFAULT constraint  
 The following example uses `SUSER_SNAME` as a `DEFAULT` constraint in a `CREATE TABLE` statement.  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE sname_example  
(  
login_sname sysname DEFAULT SUSER_SNAME(),  
employee_id uniqueidentifier DEFAULT NEWID(),  
login_date  datetime DEFAULT GETDATE()  
);   
GO  
INSERT sname_example DEFAULT VALUES;  
GO  
```  
  
### D. Calling SUSER_SNAME in combination with EXECUTE AS  
 This example shows the behavior of SUSER_SNAME when called from an impersonated context.  
  
**Applies to**: [!INCLUDE[sql2008-md](../../includes/sql2008-md.md)] and later
  
```sql
SELECT SUSER_SNAME();  
GO  
EXECUTE AS LOGIN = 'WanidaBenShoof';  
SELECT SUSER_SNAME();  
REVERT;  
GO  
SELECT SUSER_SNAME();  
GO 
```  
  
 Here is the result.  
  
 ```
sa  
WanidaBenShoof  
sa
```  
  
## Examples: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] and [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### E. Using SUSER_SNAME  
 The following example returns the login name for the security identification number with a value of `0x01`.  
  
```sql
SELECT SUSER_SNAME(0x01);  
GO  
```  
  
### F. Returning the Current Login  
 The following example returns the login name of the current login.  
  
```sql
SELECT SUSER_SNAME() AS CurrentLogin;  
GO  
```  
  
## See Also  
 [SUSER_SID &#40;Transact-SQL&#41;](../../t-sql/functions/suser-sid-transact-sql.md)   
 [Principals &#40;Database Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)  
  
  

