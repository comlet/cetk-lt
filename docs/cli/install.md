# Installing cetk-lt

`cetk-lt` is available as a Python wheel package and can be installed by its name "cetk-lt" with e.g. pip.

```shell
pip install cetk-lt
```

## System Requirements

- **Python** ≥ 3.10
- **TimescaleDB** (PostgreSQL-compatible) instance with the cetk-lt schema already deployed

!!! note
    cetk-lt inserts data into pre-existing tables. It does **not** create the database
    or the schema. The target database and all required tables must exist before running
    cetk-lt.

??? info "Do you need a TimescaleDB instance with a cetk-lt schema?"
    We are happy to assist you if you need a TimescaleDB instance with schema implementation.
    Feel free to contact [our sales](mailto:sales@comlet.de?subject=cetk-lt%20Inquiry) for further details.