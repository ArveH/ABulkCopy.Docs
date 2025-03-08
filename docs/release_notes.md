# Release Notes

Before I get more functionality implemented, the Major version number will not be used.

The Minor version number will change where there is significant new functionality, otherwise it's only the Patch number that will change.

## :material-tag: 0.8.6 Handle \x00 in strings

PostgreSQL does not allow null bytes (\x00) in text (TEXT, VARCHAR) columns because it uses C-style strings internally, which treat \x00 as a string terminator. Sql Server do allow this, so when copying data that had zero bytes inside the string, it crashed. It will now remove these zero-bytes when copying.

## :material-tag: 0.8.5 Build Linux-x64

No functionality is changed. I just added a binary for linux-x64. So now we have binaries for windows-x64, mac-arm64, linux-x64 and linux-arm64

## :material-tag: 0.8.4 Builds for Windows, Linux and Mac

No functionality is changed. If you have version 0.8.2 and running on Windows, you don't have to upgrade. This release only adds builds for Linux and Mac.

## :material-tag: 0.8.3 Updated workflow file

Only the workflow file was updated, so there is no change to the program. The workflow file was using two depricated actions for creating a release. These where replaced.

## :material-tag: 0.8.2 Don't prompt when folder doesn't exist

Just a small change. Decided to remove the prompt for when folder doesn't exist. It doesn't really make sense to prompt here. I've created a new issue for halting if files are overwritten instead ([Issue 41](https://github.com/ArveH/ABulkCopy/issues/41)).

## :material-tag: 0.8.1 Breaking change: Changed bool and datetime default mappings

When converting from SqlServer to Postgres, bit is now translated to boolean (previously smallint) and datetime/datetime2 is translated to timestamp with time zone (previously timestamp without time zone). However, you can change to the old mappings by using the --mappings-file parameter. See the [Documentation](https://arveh.github.io/ABulkCopy.Docs/command_line_parameters/).

> The data files are changed. All DateTime's are noe stored as UTC, and marked as such by ending the datetime string with a Z, e.g. 2024-06-26 11:00:00Z

## :material-tag: 0.7.0 Added command line parameter --to-lower

This parameter will convert all identifiers (table names, column names, etc.) to lowercase. NOTE: For Postgres, this parameter will have no effect unless it's used in conjunction with the --add-quotes parameter, since Postgres will "fold" the identifier names to lowercase if they are not quoted. The --to-lower parameter is only used when direction = In

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

