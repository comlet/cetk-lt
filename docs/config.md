# Configuration

Configuration can be given by two different entities:

* Environment variables
* [CLI options](cli/global_options.md)

CLI options have the highest priority. Environment variables are suited for
CI/CD pipelines where credentials are injected by the pipeline runner and
are documented together with their respective CLI option.

!!! note "Precedence"
    CLI argument > environment variable > built-in default.
    A CLI argument always overrides the corresponding variable.
