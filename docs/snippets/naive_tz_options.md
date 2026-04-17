### `--test-run-starttime-naive-tz` (str) { #test-run-starttime-naive-tz }
Timezone to assume for the `test_run_starttime` value — the single timestamp
that identifies the test run in the database.

- Accepted formats: IANA name (e.g. `Europe/Berlin`) or fixed offset (e.g. `+01:00`) —
  see [Timestamp & Timezone](../timestamp.md#naive-timezone-options) for DST behavior
- Default: None (UTC assumed if no timezone info is present)

### `--source-log-naive-tz` (str) { #source-log-naive-tz }
Timezone to assume for the individual event timestamps inside the source file.

- Accepted formats: IANA name (e.g. `Europe/Berlin`) or fixed offset (e.g. `+01:00`) —
  see [Timestamp & Timezone](../timestamp.md#naive-timezone-options) for DST behavior
- Default: None (UTC assumed if no timezone info is present)

--8<-- "snippets/timezone_naive_tz_note.md"
