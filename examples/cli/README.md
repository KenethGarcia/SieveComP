# SieveComP Command Line Interface (CLI)

The SieveComP CLI (`revision_cli.py`) provides a robust, terminal-based interface for executing the Seismicity Review Routine tailored for the National Seismological Network of Colombia (RSNC). It demonstrates the engine's capability to dynamically query databases, both in SeisComP3 and SeisComP6 databases based on an internal temporal cutoff (`2026-03-17`), apply complex TOML rules, and output clean, formatted results directly to your console or a file.

> **NOTE:** This CLI is provided as an advanced example of _SieveComP_'s functionality and usability.

 
# Execution

Run the CLI from the root directory of the package using Python's module execution:

```bash
python -m examples.cli.revision_cli --config path/to/your/config.toml [OPTIONS]
```

# Command-Line Options

| Option | Description | Required |
|---|---|---|
|`-c`, `--config` | Path to the TOML configuration file defining the rules and parameters for the seismicity review. | Yes |
|`-s`, `--start` | Start date for the event query in `YYYY-MM-DD` format. | Conditional |
|`-e`, `--end` | End date for the event query in `YYYY-MM-DD` format. | Conditional |
|`-a`, `--author` | Filter events by the author of the review. | No |
| `-f`, `--skip-locatable` | If specified, skips the potentially locatable events in the filtering process. | No |
|`-o`, `--output` | Flag to save the cleaned, processed results to a dynamically named CSV file `revision_RSNC_YYYYMMDD_HHMMSS.csv` in the current working directory. | No |

> **NOTE:** You must provide either a start date (`-s`), an author (`-a`), or both to execute the routine. Providing an end date (`-e`) without a start date will result in a usage error.
 
# Usage Examples

Assuming that you set an alias `revision`:
```bash
alias revision="python -m examples.cli.revision_cli -c examples/data/configs/seismic_revision_routine.toml"
```
you can run the following examples:

1. **Standard Time-Window Review:**
Run the routine for a specific time block, generating an on-screen table:
```bash
revision -s 20260301T000000 -e 20260315T235959
```

The CLI allows to set almost any arbitrary time format by using quotes, so you can also run in example:

```bash
revision -s "03 March 2026" -e "15 March 2026"
```

2. **Author-Based Review:**
Filter the catalog for a specific author, hide locatable events to focus purely on critical anomalies, and export the results to a CSV file:
```bash
revision -a "gerard" -f -o
```

# Output Formatting

When printed to the terminal, the CLI automatically applies several quality-of-life formatting steps to the final DataFrame:

- **Encoding Fixes:** Utilizes the `ftfy` library to correct text encoding issues within the region descriptions (`text` column).
- **Compact Display:** Removes redundant strings (like ", Colombia") and maps long SeisComP column names to compact Spanish headers (e.g., `quality_standardError` becomes `RMS`, `depth_value` becomes `Prof`) to fit neatly in standard terminal windows. Numeric fields are rounded to two decimal places.
- **Execution Summary:** Concludes with a brief statistical summary, counting the number of flagged events and potentially locatable events (unless the `-f` flag was used).
