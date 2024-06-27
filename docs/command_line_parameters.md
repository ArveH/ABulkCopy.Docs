# Command line parameters

Listing the required ones first, then the optional ones in alphabetical order.

## -c, --connection-string (required)

The connection string. They can look something like this:

### Sql Server

Server=.;Database=ABulkCopyTestDb;Trusted_Connection=True;MultipleActiveResultSets=true;

Server=&lt;server name&gt;;Initial Catalog=&lt;database name&gt;;User Id=&lt;your user id&gt;;Password=&lt;your password&gt;;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

### Postgres

Server=localhost;Port=5432;Database=ABulkCopyTestDb;User Id=postgres;Include Error Detail=True;

Server=ah-postgres.postgres.database.azure.com;Port=5432;Database=&lt;database name&gt;;User Id=&lt;your user id&gt;;Include Error Detail=True;Password=&lt;your password&gt;;SSL Mode=Require;Trust Server Certificate=true;Command Timeout=1800;

## -d, --direction (required)

Legal values:

- In
- Out

Which way you are copying data. "In" to a database or "Out" from a database

## -r, --rdbms (required)

Legal values:

- Pg
- Mss

What Relational Database Management System (RDBMS) you are using.

## -q, --add-quotes (In only)

Flag to quote all identifiers. Only applicable for Postgres, where there is a significant difference in behaviour when quoting identifiers.

!!! warning

    When a table/column name is quoted in Postgres, you must ALWAYS use quotes when referring to this name in queries and statements. Otherwise, Postgres will throw an error, saying that it can't find the object.

!!! note

    Postgres reserved words are always quoted.

!!! note

    For SQL Server, identifiers will always be quoted (this parameter is ignored).

## --empty-string (In only)

Describe how to handle empty strings, or strings that contains whitespace only.

Legal values:

| Value | Description |
| --- | --- |
| Single | An empty string is converted to a single space |
| Empty | A single space is converted to an empty string |
| ForceSingle | If the string is empty or contains only whitespace, it's converted to a single space |
| ForceEmpty | If the string contains only whitespace, it's converted to an empty string |

## -f, --folder (both directions)

The source/destination folder for schema and data files.

## --help

Display a short description of all the command line parameters.

## -l, --log-file (both direction)

Full path for the log file

!!! note

    The log files contains A LOT of details, and can be quite huge. The probram will also append to a log file, if it already exists. I reccommend deleting the log file before every run.

## -m, --mappings-file (In only)

The path and file name of a json file containing key-value pairs for mapping schema names and collation names. E.g. mapping the "dbo" schema in SQL Server to the "public" schema in Postgres. There is a sample-mappings.json file accompanying the executable. It looks like this:

```json
{
  "Schemas": {
    "": "public",
    "dbo": "public"
  },
  "Collations": {
    "SQL_Latin1_General_CP1_CI_AI": "en_ci_ai",
    "SQL_Latin1_General_CP1_CI_AS": "en_ci_as"
  },
  "ColumnTypes": {
    "binary": "bytea",
    "bit": "boolean",
    "datetime": "timestamp with time zone",
    "datetime2": "timestamp with time zone",
    "datetimeoffset": "timestamp with time zone",
    "float": "doubleprecision",
    "nchar": "char",
    "text": "varchar",
    "ntext": "varchar",
    "nvarchar": "varchar",
    "smalldatetime": "timestamp with time zone",
    "tinyint": "smallint",
    "uniqueidentifier": "uuid ",
    "image": "bytea",
    "varbinary": "bytea"
  }
}
```

!!! warning

    Currently, you should only add/change mappings for Schemas and Collations, and for the bit and datetime types.

The ColumnTypes mapping is currently very simple, and will be changed in the future. For now, you should only change bit and the datetime types, and only use the values:

<div class="grid cards" markdown>

- __bit__

    ---

    - boolean
    - smallint
    - int

-   __datetime, datetime2, datetimeoffset__

    ---

    - timestamp with time zone
    - timestamp

</div>

## --schema-filter (Out only)

A comma separated list of schema names.

!!! note

    When it's not used, all schemas will be copied, except 'guest', 'information_schema', 'sys' and 'logs'.

## -s, --search-filter (both directions)

A string to filter table names or file names. Note that the syntax of the SearchFilter is different depending on the direction parameter.

### When copying out

For copy out, the SearchFilter is the rhs of a LIKE clause in SQL.

| Sample string | Description |
| --- | --- |
| a&#91;^_&#93;% | Get all tables except the ones starting with underscore  |
| a&#91;sa&#93;&#91;ya&#93;&#91;sg&#93;% | Get all tables that starts with 'a' followed by "sys" or "aag (but will also get "asas", "aayg", and other combinations) |

### When copying in

For copy in from a file system, use a RegEx in .NET format.

| Sample string | Description |
| --- | --- |
| (client&#124;scope) | Match all tables containing the words __client__ or __scope__  |
| \b(clients&#124;scopes)\b | will match __clients.schema__ and __scopes.schema__, but not __someclients.schema__ nor __clients2.schema__ |

## --skip-create (In only)

It assumes that the tables already exists in the database, and will skip the "create table" step. The thought is that tables are created using Entity Framework migrations, then ABulkCopy is used to insert data.

!!! warning

    Schema files are still needed to create the dependency graph and the copy statements, and they MUST correspond to the tables already in the database.

## --to-lower (In only)

When importing tables, all identifiers (table names, column names, etc.) will be converted to lowercase.

!!! note

    Postgres will always fold all names to lowercase, unless they are quoted. So for Postgres, this has no effect unless you also use it with the _--add-quotes_ parameter

## --version

Show the version of the binary files

!!! note

    This version is not the same as the Release version you find under Releases in GitHub.
