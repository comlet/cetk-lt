# Piping Subcommands

cetk-lt supports Unix-style piping between subcommands. After each successful
run, cetk-lt prints the normalized UTC `test_run_starttime` to stdout
(see [Timestamp & Timezone](timestamp.md#output-format) for the exact format),
which the next command reads from stdin.

## Sequential Piping

Run `rf` first, then pipe its output into one or more `log` calls:

```shell
cetk-lt rf <path> <job> <db_name> [options] \
  | cetk-lt log mosquitto <path> <db_name> [options]
```

- `rf` determines the `test_run_starttime` (from [`--test-run-starttime`](cli/rf.md#test-run-starttime) or
  auto-detected from `output.xml`) and prints it to stdout.
- Each `log` call reads [`test_run_starttime`](cli/log.md#test_run_starttime) from stdin if it is not provided
  as a positional argument, then prints the same value to stdout.
- This allows chaining as many sources as needed without repeating the timestamp.

To chain multiple log sources, extend the pipe:

```shell
cetk-lt rf <path> <job> <db_name> [options] \
  | cetk-lt log mosquitto <path1> <db_name> [options] \
  | cetk-lt log mosquitto <path2> <db_name> [options]
```

## Concurrent Execution

If you provide `test_run_starttime` explicitly to every command, you can run
them in parallel (e.g. in separate CI/CD steps or background processes):

```shell
STARTTIME="2024-01-01 12:00:00.000000Z"

cetk-lt rf <path> <job> <db_name> -t "$STARTTIME" [options] &
cetk-lt log mosquitto <path> <db_name> "$STARTTIME" [options] &
wait
```

!!! note
    When running concurrently, all commands must use the same `test_run_starttime`
    so that all data is assigned to the same test run in the database.
