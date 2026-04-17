??? note "Difference between the two timezone options"
    - `--test-run-starttime-naive-tz` — applies to the **test run identifier**:
      one timestamp per run, used as the grouping key in the database
    - `--source-log-naive-tz` — applies to **every event timestamp** inside
      the source log file (e.g. keywords/suites in RF XML, individual lines in a log source)

    Both options are independent and can carry different timezones — for example
    when the CI server runs in UTC but the source log file was generated on a
    `Europe/Berlin` machine.
