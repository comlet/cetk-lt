# Transforming Additional Log Sources

Transforms and inserts additional log data from supported sources into the database.

```shell
cetk-lt [global options] log <source> <path> <db_name> [test_run_starttime] [options]
```

After a successful run, cetk-lt prints the normalized UTC `test_run_starttime`
to stdout (e.g. `2024-01-01 01:01:01.111000Z`), which can be
[piped](../piping.md) into a subsequent `cetk-lt log` call.

## Arguments

### `source`
Log source type to transform. The source determines the expected log format.

Available sources:

- `mosquitto` — [Mosquitto](https://mosquitto.org/){target="_blank"} MQTT broker log

??? info "Do you need a different log source?"
    cetk-lt is designed to be extensible — additional log sources can be integrated
    without changing existing pipelines.
    Feel free to contact [our sales](mailto:sales@comlet.de?subject=cetk-lt%20Log%20Source%20Inquiry) for further details.

### `path`
Path to the log file.

### `db_name`
Name of the target database.

### `test_run_starttime`
Start timestamp of the test run. Used to assign log data to a specific test run.

- Format: `%Y-%m-%d %H:%M:%S.%f` with optional timezone suffix `Z` or `+/-HH:MM`
- If omitted, cetk-lt reads the value from stdin (e.g. piped from `cetk-lt rf`)

!!! note
    `test_run_starttime` is required — either provide it as a positional argument
    or pipe it from a preceding `cetk-lt rf` or `cetk-lt log` call.
    The `[...]` notation reflects that it may be omitted when piped via stdin.
    See [Piping](../piping.md) for details.

    See [Timestamp & Timezone](../timestamp.md) for accepted formats and timezone handling.

## Options

### `--help` / `-h`
Show help message and exit.

---

--8<-- "snippets/naive_tz_test_run_starttime.md"
- Environment variable: `CETK_LT_LOG_TEST_RUN_STARTTIME_NAIVE_TZ`

---

--8<-- "snippets/naive_tz_source_log.md"
- Environment variable: `CETK_LT_LOG_SOURCE_LOG_NAIVE_TZ`

--8<-- "snippets/timezone_naive_tz_note.md"

---

--8<-- "snippets/db_connection_options.md"

---

### `--log-timestamp-format` / `-ltf` (str)
Custom Python [`strptime`](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes) format string for the timestamp in the log file.

- Default: `""` (auto-detection)
- Environment variable: `CETK_LT_LOG_TIMESTAMP_FORMAT`

??? note "Auto-detected formats (Mosquitto)"
    When `-ltf` is not provided, cetk-lt auto-detects the timestamp format from
    the first line of the log file. The following formats are supported:

    - **Unix epoch seconds:** `1234567890: <message>`
    - **ISO-style:** `2024-01-01T12:00:00: <message>` (optional fractional seconds
      and timezone offset)

    Use `-ltf` when the log file uses a different format. The timestamp must be
    followed by `: ` (colon and space).
