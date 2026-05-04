# Command Line Interface

`cetk-lt` is a command line interface for transforming log data from RF test runs
and additional log sources into a [TimescaleDB](https://www.timescale.com/){target="_blank"}
database for visualization and analysis with any PostgreSQL-compatible tool
(e.g., [Grafana](https://grafana.com/){target="_blank"}).

Its purpose is to act as the bridge between test execution artifacts and the
visualization layer — taking raw output files and inserting their data in a
structured, queryable form into the database.

It is designed to be used both manually after a test run and within a CI/CD
pipeline as an automated post-processing step. Two subcommands cover the
currently supported log sources:

- [`cetk-lt rf`](rf.md) — transforms a Robot Framework `output.xml` file
- [`cetk-lt log`](log.md) — transforms additional log sources

The subcommands can be run independently or chained via [Unix-style piping](../piping.md).
All timestamps are normalized to UTC before insertion, regardless of the source
timezone. See [Timestamp & Timezone](../timestamp.md) for details.

For general usage information, `cetk-lt` provides a built-in help:

```shell
cetk-lt --help
```

as well as help for every subcommand:

```shell
cetk-lt rf --help
cetk-lt log --help
```

See [Installation](install.md) to get started and [Global Options](global_options.md)
for options that apply to all subcommands.

??? note "Exit Codes"
    | Code | Meaning |
    |------|---------|
    | `0` | Success |
    | `1` | Runtime value error — check argument values, timestamp formats, and RF model compatibility (re-run with [`--verbose`](global_options.md) for details) |
    | `2` | Invalid argument or environment variable — missing required argument, invalid choice, bad format, or unrecognised env var value |
    | `250` | Internal argument error |
    | `251` | Transformation validation failed |
    | `252` | Input file not found |
    | `253` | RF model compatibility issue — re-run with [`--verbose`](global_options.md) |
    | `254` | Type error in argument processing |
    | `255` | Unexpected error — re-run with [`--verbose`](global_options.md) for details |
