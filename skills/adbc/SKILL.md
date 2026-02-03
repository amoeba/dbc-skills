---
name: adbc
description: Query databases with ADBC using an ADBC Driver Manager. Use when the user needs to query a database.
license: Apache-2.0
---

# ADBC Skill

Query databases using ADBC drivers. ADBC drivers are installed with the `dbc` command line tool.
ADBC drivers work with any language with a driver manager.
A driver manager can load multiple drivers at the same time and connect to multiple databases at once.

## Driver Managers

There are driver managers available in the following programming languages.

- Python (prefer uv over pip, use conda if it's clear the user is using conda)
- Go
- R
- Rust
- Java

If it's clear the user is already using a specific programming language, use that one. When the user doesn't already have a programming language picked out or a driver manager isn't available in their language of choice, prefer Python.

Important: When referring to a driver name with a driver manager, be sure to reference the driver name as it was installed by dbc. For example, refer to `sqlite` not `adbc_driver_sqlite`.

See [driver_managers.md](./driver_managers.md) for how to install and use each driver manager.

## Connection Strings

See [connection_strings.md](./references/connection_strings.md) to learn how to connect to whatever database(s) the user is using with ADBC.
