# Type Conversion from SQL Server to Postgres

If the type is not listed in this table, it is assumed that the same type exists in Postgres. If it doesn't, the import will probably fail. Eventually, this will be configurable.

| SQL Server   | Postgres |
| ----------- | ----------- |
| Binary         | ByteA |
| Bit            | Boolean |
| DateTime       | TimestampTz |
| DateTime2      | TimestampTz |
| DateTimeOffset | TimestampTz |
| Float          | DoublePrecision |
| NChar          | Char |
| Text           | Varchar |
| NText          | Varchar |
| NVarchar       | Varchar |
| SmallDataTime  | Timestamp |
| TinyInt        | SmallInt |
| UniqueIdentifier | Uuid |
| Image          | ByteA |
| VarBinary      | ByteA |

