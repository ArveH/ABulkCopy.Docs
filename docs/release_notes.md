# Release Notes

Before I get more functionality implemented, the Major version number will not be used.

The Minor version number will change where there is significant new functionality, otherwise it's only the Patch number that will change.

## :material-tag: 0.7.0 Added command line parameter --to-lower

This parameter will convert all identifiers (table names, column names, etc.) converted to lowercase. NOTE: For Postgres, this parameter will probably have no effect, unless it's used in conjunction with the --add-quotes parameter. Postgres will \"fold\" the identifier names to lowercase anyway, unless the identifier is surrounded with quotes. This parameter is only used when direction = In

## :material-tag: 0.6.0 Added command line parameter --skip-create

Added a flag that can be used to skip creating tables and indexes, and only insert data. It's an experimental feature that might be removed or changed in the future.

## :material-tag: 0.5.0 Major change. Internal functionality rewritten to handle schemas

All lookup in system tables had to be changed to handle schemas. The schema and data files are also changed. The file names are prefixed with the schema they belong to. The .schema file now store information about the schema.

Two new command line parameters where added. One for filtering on schemas (--schema-filter), and another for giving a file path for a file describing what a schema in SQL Server should be mapped to in Postgres (-m or --mapping-file). A sample-mappings.json file now accompanies the executable.

## :material-tag: 0.4.2 Fixed: \*\*ERROR\*\* Reset auto generation for ...

When the length of the name of the table, and the length of the name of the identity column, together was getting close to 64 characters long, the copy program crashed when trying to copy into Postgres. This is now fixed.


## :material-tag: 0.2.1 Character lengths

When getting the information from SQL Server's system tables, I used the length in bytes as output to the schema files. For string types, it makes more sense to use the character length. For example, it caused nvarchar columns copy from SQL Server to Postgres to become twice as long in Postgres. This is now fixed. I also fixed some other small errors with length, scale and precision for other types.

## :material-tag: 0.2.0 Initial Release

This is the initial release. It is currently only usable for migrating from SQL Server to Postgres:

- You can copy out from an SQL Server database, but not into one.
- You can copy into a Postgres database, but not out from one, and the schema and data files used, has to come from SQL Server

The conversion of SQL Server data types to Postgres data types are "static". For more information about type conversions, [go here](./type_mss_pg.md).

Indexes and foreign keys are handled, but not views, procedures, sequences, functions or collections.

See [Quick Start](./quick_start.md) for more information about how collations are handled. 

