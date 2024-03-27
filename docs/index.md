# Home

> <span style="font-size:2em;">Currently under development. File formats, command parameters, and functionality can (and most likely will) change.</span>

This is a copy program for SQL Server and Postgres.

Eventually, it will be a copy tool for copying all schema and data from one database to another. It will also migrate schema and data between SQL Server and Postgres.

Current, I have implemented functionality for migrating from SQL Server to Postgres:

- You can copy out from an SQL Server database, but not into one.
- You can copy into a Postgres database, but not out from one, and the schema and data files used, has to come from SQL Server

The conversion of SQL Server data types to Postgres data types are "static". For more information about type conversions, [go here](./type_mss_pg.md).

Indexes and foreign keys are handled, but not views, procedures, sequences, functions or collections.

See [Quick Start](./quick_start.md) for more information about how collations are handled.
