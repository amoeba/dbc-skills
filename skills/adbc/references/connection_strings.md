# ADBC Driver Connection URIs

URI patterns and examples to use when connecting to databases with ADBC drivers. Values inside `[` and `]` in pattern should be replaced with actual values from the user's environment and the final result should be passed to the [driver manager](./driver_managers.md) as its `uri` argument unless an exception is noted. Most patterns follow the form `<driver>://<connection-details>`.

## bigquery

Pattern: `bigquery://[Host]:[Port]/ProjectID?OAuthType=[AuthValue]&[Key]=[Value]&[Key]=[Value]`
Example: `bigquery:///[ProjectID]?DatasetID=[DatasetID]`

## duckdb

The DuckDB driver does not accept a `uri` argument. Specify the driver to driver managers as "duckdb". To use a DuckDB database file instead of an in-memory database, specify the `path` option with a file path.

## databricks

Databricks has three authentication modes with different URI patterns for each method.

### Personal Access Token

Pattern: `databricks://token:<personal-access-token>@<server-hostname>:<port-number>/<http-path>?<param1=value1>&<param2=value2>`

### OAuth U2M (User-to-Machine, browser-based)

Pattern: `databricks://<server-hostname>:<port>/<http-path>?authType=OauthU2M`

### OAuth M2M (Machine-to-Machine)

Pattern: `databricks://<server-hostname>:<port>/<http-path>?authType=OAuthM2M&clientID=<id>&clientSecret=<secret>`

## flightsql

Pattern: `grpc[+tcp]://[host][:port]`
Example: `grpc://localhost:8080`

## mssql

Pattern: `mssql://[user[:password]@][host][:port][?database=dbname]`
Example: `mssql://username:mypass@localhost?database=master`

## mysql

Pattern: `mysql://[user[:password]@]tcp([host][:port])[/database]`
Example: `mysql://user:pass@localhost:3306/mydb`

## postgresql

Pattern: `postgresql://[user[:password]@][host][:port][/database]`
Example: `postgresql://root:root@localhost:5433/db`

## snowflake

Pattern: `snowflake://user[:password]@host[:port]/database[/schema][?param1=value1&param2=value2]`
Example: `snowflake://jane.doe:MyS3cr3t!@myorg-account1/ANALYTICS_DB/SALES_DATA?warehouse=WH_XL&role=ANALYST`

## sqlite

The SQLite driver doesn't accept a proper URI but it's `uri` argument takes the path to an SQLite database to create or open.

Pattern: `[dbpath].sqlite`

## redshift

Pattern: `redshift://[user[:password]@]host[:port]/dbname[?param1=value1&param2=value2]`
Example: `redshift://admin_user:secret@my-cluster.region.redshift.amazonaws.com:5439/dev`

## trino

Pattern: `trino://[user[:password]@]host[:port][/catalog[/schema]][?attribute1=value1&attribute2=value2...]`
Example: `trino://localhost:8080/hive/default`
