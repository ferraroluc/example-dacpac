# Example - DACPAC

The goal of this project is to create a proof of concept with DACPAC, exporting the file and applying it to a SQL Server database. This project was started with the option `Database Projecs: New`.

## Steps

1. Start a SQL Server with Docker:

```bash
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=yourStrong@Password" -e "MSSQL_PID=Express" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

2. Modify SQL's scripts.

3. Build the .dacpac file:

```bash
dotnet build --configuration Release
```

4. Apply the file to the database server:

```bash
sqlpackage /Action:Publish /SourceFile:"./bin/Release/example.dacpac" /TargetServerName:"localhost" /TargetDatabaseName:"example" /TargetUser:"sa" /TargetPassword:"yourStrong@Password" /Properties:AllowIncompatiblePlatform=True /p:BlockOnPossibleDataLoss=False /TargetTimeout:120 /TargetTrustServerCertificate:True
```

5. Check if the changes were applied:

```bash
sqlcmd -S localhost -U sa -P yourStrong@Password -C
```

```sql
-- Check database
SELECT name, database_id, create_date FROM sys.databases;
GO
-- Select the new database
USE example;
GO
-- Check table
SELECT * FROM SYSOBJECTS WHERE xtype = 'U';
GO
```
