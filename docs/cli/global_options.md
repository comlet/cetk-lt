# Global Options

Global options must be placed **before** the subcommand.

```shell
cetk-lt [global option] [sub-command] ...
```

## Options

### `--help` / `-h`
Show help/usage details and exit.

### `--version`
Show `cetk-lt`'s version and exit.

---

### `--verbose` / `-v`
Show detailed debug messages on stderr (DEBUG level).

- Default: disabled
- Environment variable: `CETK_LT_VERBOSE`[^1]

[^1]: The variable needs to have a truth-y or false-y value; any other value causes an error with exit code 2.
