# Home

## A database copy tool

># Currently under development. File formats, command parameters, and functionality can (and most likely will) change.

Eventually, I plan to turn this application into a general copy tool for copying all schema and data from one database to another. It can be SQL Server to SQL Server, Postgres to Postgres, or between SQL Server and Postgres.

Currently, it can only be used to copy tables from an SQL Server database to a Postgres database (not vice versa), and there are other limitations too:

* Not all types are supported
* Type conversion from SQL Server types to Postgres types are fixed
* Collation conversion is fixed
* A (very) limited number of functions are supported for default values
* Only tables and indexes are supported. Not Procedures, Views, etc
