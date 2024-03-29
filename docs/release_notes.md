# Release Notes

Before I get more functionality implemented, the Major version number will not be used.

The Minor version number will change where there is significant new functionality, otherwise it's only the Patch number that will change.

## :material-tag: 0.2.1 Character lengths

When getting the information from SQL Server's system tables, I used the length in bytes as output to the schema files. For string types, it makes more sense to use the character length. For example, it caused nvarchar columns copy from SQL Server to Postgres to become twice as long in Postgres. This is now fixed. I also fixed some other small errors with length, scale and precision for other types.

## :material-tag: 0.2.0 Initial Release

This is the initial release. It is currently only usable for migrating from SQL Server to Postgres:

- You can copy out from an SQL Server database, but not into one.
- You can copy into a Postgres database, but not out from one, and the schema and data files used, has to come from SQL Server

The conversion of SQL Server data types to Postgres data types are "static". For more information about type conversions, [go here](./type_mss_pg.md).

Indexes and foreign keys are handled, but not views, procedures, sequences, functions or collections.

See [Quick Start](./quick_start.md) for more information about how collations are handled. 

