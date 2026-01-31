# ADBC Driver Manager Usage by Language

This document provides a summary of the ADBC driver manager implementations.

Each example has two parameters:

- `<driver>`: The name of the driver. Installed with the dbc command line tool.
- `<connection-uri>` The details of the database connection. Users often specify these in code or in .env files in their repo.

The driver must be installed with dbc before use.

## Python

**Library**: `adbc_driver_manager`

**Pattern**:

```python
from adbc_driver_manager import dbapi

with dbapi.connect(
    driver="<driver>",
    db_kwargs={"uri": "<connection-uri>"}
) as con, con.cursor() as cursor:
    cursor.execute("SELECT * FROM table;")
    table = cursor.fetch_arrow_table()
```

## Go

**Library**: `github.com/apache/arrow-adbc/go/adbc/drivermgr`

**Pattern**:

```go
var drv drivermgr.Driver

db, err := drv.NewDatabase(map[string]string{
    "driver": "<driver>",
    "uri":    "<connection-uri>",
})
defer db.Close()

conn, err := db.Open(context.Background())
defer conn.Close()

stmt, err := conn.NewStatement()
defer stmt.Close()

stmt.SetSqlQuery("SELECT * FROM table;")
stream, _, err := stmt.ExecuteQuery(context.Background())
defer stream.Release()
```

## Java

**Library**: `org.apache.arrow.adbc.drivermanager.AdbcDriverManager` + JNI driver

**Pattern**:

```java
Map<String, Object> params = new HashMap<>();
JniDriver.PARAM_DRIVER.set(params, "<driver>");
params.put("uri", "<connection-uri>");

try (BufferAllocator allocator = new RootAllocator();
     AdbcDatabase db = AdbcDriverManager.getInstance()
         .connect(DRIVER_FACTORY, allocator, params);
     AdbcConnection conn = db.connect();
     AdbcStatement stmt = conn.createStatement()) {
    stmt.setSqlQuery("SELECT * FROM table;");
    try (AdbcStatement.QueryResult result = stmt.executeQuery()) {
        ArrowReader reader = result.getReader();
        while (reader.loadNextBatch()) {
            System.out.println(reader.getVectorSchemaRoot().contentToTSVString());
        }
    }
}
```

## R

**Library**: `adbcdrivermanager`

**Pattern**:

```r
library(adbcdrivermanager)

drv <- adbc_driver("<driver>")

db <- adbc_database_init(
    drv,
    uri = "<connection-uri>"
)

con <- adbc_connection_init(db)

con |>
    read_adbc("SELECT * FROM table") |>
    tibble::as_tibble()
```

## Rust

**Library**: `adbc_core`

**Pattern**:

```rust
use adbc_core::options::{AdbcVersion, OptionDatabase};
use adbc_core::{Connection, Database, Driver, LOAD_FLAG_DEFAULT, Statement};
use adbc_driver_manager::ManagedDriver;

let mut driver = ManagedDriver::load_from_name(
    "<driver>",
    None,
    AdbcVersion::default(),
    LOAD_FLAG_DEFAULT,
    None,
)
.expect("Failed to load driver");

let opts = [(
    OptionDatabase::Uri,
    "<connection-uri>".into(),
)];
let db = driver
    .new_database_with_opts(opts)
    .expect("Failed to create database handle");

let mut conn = db.new_connection().expect("Failed to create connection");

let mut statement = conn.new_statement().unwrap();
statement.set_sql_query("SELECT * FROM table").unwrap();
let reader = statement.execute().unwrap();
let batches: Vec<RecordBatch> = reader.collect::<Result<_, _>>().unwrap();
```
