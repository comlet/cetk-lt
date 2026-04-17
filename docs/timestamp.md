# Timestamp & Timezone

cetk-lt normalizes all timestamps to UTC before inserting them into the database.
This page describes the accepted input formats and the timezone handling rules.

## Accepted Formats for `test_run_starttime`

The following formats are accepted wherever a `test_run_starttime` is expected
(the `--test-run-starttime` option of [`cetk-lt rf`](cli/rf.md#test-run-starttime) and the positional argument of [`cetk-lt log`](cli/log.md#test_run_starttime)):

| Format | Example | Type |
|--------|---------|------|
| `%Y-%m-%d %H:%M:%S.%f` | `2024-01-01 12:00:00.000000` | Naive (no timezone) |
| `%Y-%m-%d %H:%M:%S.%fZ` | `2024-01-01 12:00:00.000000Z` | UTC (RFC 3339) |
| `%Y-%m-%d %H:%M:%S.%f+/-HH:MM` | `2024-01-01 12:00:00.000000+01:00` | UTC offset (RFC 3339) |

## Naive Timezone Options

When a timestamp carries no timezone information (a "naive" timestamp), two options
let you specify which timezone to assume:

- `--test-run-starttime-naive-tz` — timezone for a naive `test_run_starttime`
  ([`cetk-lt rf`](cli/rf.md#test-run-starttime-naive-tz), [`cetk-lt log`](cli/log.md#test-run-starttime-naive-tz))
- `--source-log-naive-tz` — timezone for naive timestamps in the source log file
  ([`cetk-lt rf`](cli/rf.md#source-log-naive-tz), [`cetk-lt log`](cli/log.md#source-log-naive-tz))

Accepted formats for both:

| Format | Example |
|--------|---------|
| IANA timezone name | `Europe/Berlin`, `America/New_York`, `Asia/Tokyo` |
| Fixed UTC offset | `+01:00`, `+02:00`, `-05:00` |

!!! note
    IANA names account for DST automatically, but cetk-lt will fail if the given
    timestamp falls on an ambiguous DST transition — use a fixed offset in that case.

## Precedence Rules for `test_run_starttime`

The following rules apply to the `--test-run-starttime` option ([`cetk-lt rf`](cli/rf.md#test-run-starttime)) and the
positional `test_run_starttime` argument ([`cetk-lt log`](cli/log.md#test_run_starttime)):

1. If the timestamp already contains timezone information (`Z` or `+/-HH:MM`),
   that timezone is used and `--test-run-starttime-naive-tz` is ignored.
2. If the timestamp is naive and `--test-run-starttime-naive-tz` is provided,
   that timezone is assumed and a warning is logged.
3. If the timestamp is naive and no `--test-run-starttime-naive-tz` is provided,
   UTC is assumed and a warning is logged.
4. In [`cetk-lt rf`](cli/rf.md) mode, if `--test-run-starttime-naive-tz` is provided without
   `--test-run-starttime`, the option is ignored and a warning is logged.
5. If `--test-run-starttime-naive-tz` points to an IANA timezone and the local
   naive timestamp is ambiguous or non-existent at a DST transition, cetk-lt
   fails and asks for an explicit fixed offset (e.g. `+01:00`).

## Precedence Rules for Source Log Timestamps

The following rules apply to timestamps found within the source log file, controlled by
`--source-log-naive-tz` ([`cetk-lt rf`](cli/rf.md#source-log-naive-tz), [`cetk-lt log`](cli/log.md#source-log-naive-tz)):

1. If a source timestamp already contains timezone information, that timezone is used.
2. If the source timestamp is naive and `--source-log-naive-tz` is provided,
   that timezone is assumed.
3. If the source timestamp is naive and no `--source-log-naive-tz` is provided,
   UTC is assumed.
4. If `--source-log-naive-tz` is provided but all parsed source timestamps are
   already timezone-aware, cetk-lt logs a single DEBUG message that the option
   was not applied.

## Output Format

All emitted `test_run_starttime` values are printed to stdout as UTC with a `Z` suffix:

```
2024-01-01 01:01:01.111000Z
```

This format can be piped directly as input to a subsequent `cetk-lt log` call
without any ambiguity. See [Piping](piping.md) for details.
