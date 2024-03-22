# Quick Start

To get started, build the ABulkCopy.Cmd Console application.

Then run:  

``` powershell
.\ABulkCopy.Cmd.exe --help
```

to see what commandline parameters there are.

To move data from SQL Server to Postgres, you run it twice. First, you copy tables out from SQL Server into files. Then you use the same files to create tables and copy data into Postgres. Here is an example showing the command line parameters you need to use:

=== "Copy out"

    ``` powershell
    .\ABulkCopy.Cmd.exe -d out -r Mss
    -c "Server=.;Database=mydb;Trusted_Connection=True;TrustServerCertificate=True;MultipleActiveResultSets=true"
    -l D:\Temp\abulkcopy\Logs\out.log
    -f d:\temp\abulkcopy\LocalDbs\mydb
    ```  

=== "Copy in"

    ``` powershell
    .\ABulkCopy.Cmd.exe -d in -r Pg
    -c "Server=localhost;Port=5432;Database=mydb;User Id=postgres;Include Error Detail=True;Command Timeout=1800;"
    -l D:\Temp\abulkcopy\Logs\in.log
    -f D:\temp\abulkcopy\LocalDbs\mydb
    ```  

Remember that the Postgres database you copy into should be existing, empty, and containing these collations:  

``` sql
DROP COLLATION IF EXISTS en_ci_ai;
CREATE COLLATION en_ci_ai (provider = 'icu', locale = 'en-u-ks-level1', deterministic = false);

DROP COLLATION IF EXISTS en_ci_ai_like;
CREATE COLLATION en_ci_ai_like (provider = 'icu',  locale = 'en-u-ks-level1',  deterministic = true);

DROP COLLATION IF EXISTS en_ci_as;
CREATE COLLATION en_ci_as (provider = 'icu', locale = 'en-u-ks-level2', deterministic = false);
 
DROP COLLATION IF EXISTS en_ci_as_like;
CREATE COLLATION en_ci_as_like (provider = 'icu',  locale = 'en-u-ks-level2',  deterministic = true);
```

>The collations are needed to convert the SQL Server collations *SQL_Latin1_General_CP1_CI_AI* and *SQL_Latin1_General_CP1_CI_AS* to equivalent Postgres collations (to handle case insensitivity). Later, you will be able to customize this.
