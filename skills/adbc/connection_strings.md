
# ADBC Database Connection Patterns

This document catalogs the the driver and connection string patterns for many database systems that can be connected to with ADBC drivers.

## PostgreSQL and PostgreSQL-Compatible Systems

**Driver Name:** `postgresql`

**URI Pattern:** `postgresql://[user[:password]@][host][:port][/database]`

### PostgreSQL (Base)
```
postgresql://postgres:mysecretpassword@localhost:5432/demo
```

### CedarDB
```
postgresql://postgres:test@localhost:5432/postgres
```

### Citus
```
postgresql://postgres:password@localhost:5432/postgres
```

### CockroachDB
```
postgresql://username:password@localhost:26257/db
```
Note: Uses port 26257 (not standard PostgreSQL port 5432)

### CrateDB
```
postgresql://crate@localhost:5432/crate
```

### Neon
```
postgresql://cloud_admin:cloud_admin@localhost:55433/postgres
```
Note: Uses port 55433 for local development

### ParadeDB
```
postgresql://postgres:password@localhost:5432/postgres
```

### TimescaleDB
```
postgresql://postgres:password@localhost:5432/postgres
```

### Yellowbrick
```
postgresql://user:password@localhost:5432/database
```

### YugabyteDB
```
postgresql://yugabyte@localhost:5433/yugabyte
```
Note: Uses port 5433 (not standard PostgreSQL port 5432)

## MySQL and MySQL-Compatible Systems

**Driver Name:** `mysql`

**URI Pattern:** `[user[:password]@]tcp([host][:port])[/database]`

### MySQL
```
root:my-secret-pw@tcp(localhost:3306)/demo
```

### MariaDB
```
root:my-secret-pw@tcp(localhost:3306)/sys
```

### TiDB
```
root@tcp(localhost:4000)/test
```
Note: Uses port 4000 (not standard MySQL port 3306)

### Vitess
```
root@tcp(localhost:33577)/test
```
Note: Uses port 33577

## DuckDB Systems

**Driver Name:** `duckdb`

### DuckDB (Local)
**Connection Parameter:** `path`
```
path: "games.duckdb"
```
Note: File-based database; uses `path` parameter instead of URI

### MotherDuck (Cloud)
**Connection Parameter:** `path`
```
path: "md:sample_data"
```
Note: Uses `md:` prefix for cloud database access; requires authentication via environment variable or token

## SQLite

**Driver Name:** `sqlite`

**Connection Parameter:** File path
```
games.sqlite
```
Note: File-based database; specify local file path

## Microsoft SQL Server

**Driver Name:** `mssql`

**URI Pattern:** `sqlserver://[user[:password]@][host][:port][?database=dbname]`

```
sqlserver://sa:Co1umn&r@localhost:1433?database=demo
```

**Authentication:**
- Username/password
- Windows authentication
- Entra ID (Azure Active Directory)

## Snowflake

**Driver Name:** `snowflake`

**Connection Parameters:** (Configuration-based, not URI)

```python
db_kwargs = {
    "username": "your_username",
    "adbc.snowflake.sql.auth_type": "auth_snowflake",  # or "auth_jwt"
    "password": "your_password",  # or JWT token path
    "adbc.snowflake.sql.account": "your_account",
    "adbc.snowflake.sql.db": "your_database",
    "adbc.snowflake.sql.schema": "your_schema",
    "adbc.snowflake.sql.warehouse": "your_warehouse",
    "adbc.snowflake.sql.role": "your_role"
}
```

**Authentication Methods:**
- Username/password: `auth_type: "auth_snowflake"`
- JWT: `auth_type: "auth_jwt"`

## Google BigQuery

**Driver Name:** `bigquery`

**Connection Parameters:** (Configuration-based, not URI)

```python
db_kwargs = {
    "adbc.bigquery.sql.project_id": "your_project_id",
    "adbc.bigquery.sql.dataset_id": "your_dataset_id"
}
```

**Authentication:** Google Cloud Application Default Credentials (ADC)

## Amazon Redshift

**Driver Name:** `redshift`

**URI Pattern:** `postgresql://[host][:port]`

### Serverless
```
postgresql://localhost:5439
```
**Additional Parameters:**
```python
db_kwargs = {
    "uri": "postgresql://localhost:5439",
    "redshift.cluster_type": "redshift-serverless",
    "redshift.workgroup_name": "your_workgroup",
    "redshift.db_name": "dev"
}
```
**Authentication:** OAuth 2.0 (browser-based)

### Provisioned (IAM)
```
postgresql://localhost:5440
```
**Additional Parameters:**
```python
db_kwargs = {
    "uri": "postgresql://localhost:5440",
    "redshift.cluster_type": "redshift-iam",
    "redshift.cluster_identifier": "your_cluster",
    "redshift.db_name": "dev"
}
```
**Authentication:** AWS IAM

### Provisioned (Username/Password)
```
postgresql://user:password@localhost:5440/dev
```
**Additional Parameters:**
```python
db_kwargs = {
    "uri": "postgresql://user:password@localhost:5440/dev",
    "redshift.cluster_type": "redshift"
}
```

## Databricks

**Driver Name:** `databricks`

**URI Pattern:** `databricks://[token:password@]<server-hostname>:<port>/<http-path>[?authType=...]`

### OAuth U2M (User-to-Machine, browser-based)
```
databricks://<server-hostname>:<port>/<http-path>?authType=OauthU2M
```

### OAuth M2M (Machine-to-Machine)
```
databricks://<server-hostname>:<port>/<http-path>?authType=OAuthM2M&clientID=<id>&clientSecret=<secret>
```

### Personal Access Token
```
databricks://token:<token>@<server-hostname>:<port>/<http-path>
```

## Trino

**Driver Name:** `trino`

**URI Pattern:** `http://[user[:password]@][host][:port][?catalog=...&schema=...]`

```
http://user@localhost:8080?catalog=tpch&schema=tiny
```

Note: Uses HTTP protocol; query parameters specify catalog and schema

## Apache Arrow Flight SQL Systems

**Driver Name:** `flightsql`

**URI Pattern:** `grpc[+tcp]://[host][:port]`

### Dremio
```
grpc+tcp://localhost:32010
```
**Authentication:** Username/password

### GizmoSQL
```
grpc+tcp://localhost:31337
```
**Authentication:** Username/password

### InfluxDB
```
grpc+tcp://localhost:8181
```
**Authentication:** Bearer token
**Additional Parameters:**
```python
db_kwargs = {
    "uri": "grpc+tcp://localhost:8181",
    "adbc.flight.sql.authorization_header": "Bearer YOUR_TOKEN",
    "adbc.flight.sql.rpc.call_header.database": "your_database"
}
```

### StarRocks
```
grpc://localhost:9408
```
Note: No `+tcp` suffix
**Authentication:** Username/password (default: username="root", password="")

## Summary Table

| Database System | Driver Name | URI Scheme | Default Port | Auth Method |
|-----------------|-------------|------------|--------------|-------------|
| PostgreSQL | `postgresql` | `postgresql://` | 5432 | Username/Password |
| CedarDB | `postgresql` | `postgresql://` | 5432 | Username/Password |
| Citus | `postgresql` | `postgresql://` | 5432 | Username/Password |
| CockroachDB | `postgresql` | `postgresql://` | 26257 | Username/Password |
| CrateDB | `postgresql` | `postgresql://` | 5432 | Username/Password |
| Neon | `postgresql` | `postgresql://` | 55433 (local) | Username/Password |
| ParadeDB | `postgresql` | `postgresql://` | 5432 | Username/Password |
| TimescaleDB | `postgresql` | `postgresql://` | 5432 | Username/Password |
| Yellowbrick | `postgresql` | `postgresql://` | 5432 | Username/Password |
| YugabyteDB | `postgresql` | `postgresql://` | 5433 | Username/Password |
| MySQL | `mysql` | `tcp()` | 3306 | Username/Password |
| MariaDB | `mysql` | `tcp()` | 3306 | Username/Password |
| TiDB | `mysql` | `tcp()` | 4000 | Username/Password |
| Vitess | `mysql` | `tcp()` | 33577 | Username/Password |
| DuckDB | `duckdb` | `path:` | N/A | File-based |
| MotherDuck | `duckdb` | `md:` | N/A | Token/Env Var |
| SQLite | `sqlite` | File path | N/A | File-based |
| SQL Server | `mssql` | `sqlserver://` | 1433 | Username/Password/Windows/Entra ID |
| Snowflake | `snowflake` | Config-based | N/A | Username/Password or JWT |
| BigQuery | `bigquery` | Config-based | N/A | GCP ADC |
| Redshift | `redshift` | `postgresql://` | 5439/5440 | OAuth/IAM/Username-Password |
| Databricks | `databricks` | `databricks://` | Varies | OAuth U2M/M2M/PAT |
| Trino | `trino` | `http://` | 8080 | Username/Password |
| Dremio | `flightsql` | `grpc+tcp://` | 32010 | Username/Password |
| GizmoSQL | `flightsql` | `grpc+tcp://` | 31337 | Username/Password |
| InfluxDB | `flightsql` | `grpc+tcp://` | 8181 | Bearer Token |
| StarRocks | `flightsql` | `grpc://` | 9408 | Username/Password |

## Key Observations

1. **PostgreSQL Compatibility:** 10 database systems use the PostgreSQL driver due to wire protocol compatibility
2. **MySQL Family:** 4 systems share the MySQL driver (MySQL, MariaDB, TiDB, Vitess)
3. **FlightSQL Standard:** 4 systems use Apache Arrow's FlightSQL protocol for high-performance data transfer
4. **URI vs Config-Based:** Most systems use connection URIs, but Snowflake and BigQuery use parameter-based configuration
5. **File-based Databases:** DuckDB and SQLite use local file paths instead of network URIs
6. **Non-standard Ports:** Some PostgreSQL-compatible systems (CockroachDB, Neon, YugabyteDB) and MySQL-compatible systems (TiDB, Vitess) use non-standard ports
