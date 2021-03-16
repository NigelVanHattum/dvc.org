# exp show

Print a customizable table of `dvc experiments`, their metrics and parameters.

> Press `q` to exit.

## Synopsis

```usage
usage: dvc exp show [-h] [-q | -v] [-a] [-T] [--all-commits] [--no-pager]
                    [--include-metrics <metrics_list>]
                    [--exclude-metrics <metrics_list>]
                    [--include-params <params_list>]
                    [--exclude-params <params_list>]
                    [--sort-by <metric/param>] [--sort-order {asc,desc}]
                    [--no-timestamp] [--sha] [--show-json]
                    [--precision <n>]
```

## Description

Displays experiments and
[checkpoints](/doc/command-reference/exp/run#checkpoints) in a detailed table
which includes their parent, name (or hash), metrics, and parameters. Only the
experiments derived from the Git `HEAD` are shown by default but all experiments
can be included with the `--all-commits` option. Example:

```dvc
$ dvc exp show
┏━━━━━━━━━━━━━━━┳━━━━━━━━━━┳━━━━━━━━━┳━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━┓
┃ Experiment    ┃ avg_prec ┃ roc_auc ┃ train.n_est┃ train.min_split ┃
┡━━━━━━━━━━━━━━━╇━━━━━━━━━━╇━━━━━━━━━╇━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━┩
│ workspace     │  0.56191 │ 0.93345 │ 50         │ 2               │
│ master        │  0.55259 │ 0.91536 │ 50         │ 2               │
│ ├── exp-bfe64 │  0.57833 │ 0.95555 │ 50         │ 8               │
│ └── exp-ad5b1 │  0.56191 │ 0.93345 │ 50         │ 2               │
└───────────────┴──────────┴─────────┴────────────┴─────────────────┘
```

Your terminal will enter a
[paginated screen](/doc/command-reference/dag#paginating-the-output) by default,
which you can typically exit by typing `Q`. Use `--no-pager` to print the table
to standard output.

By default, the printed experiments table will include columns for all metrics
and params from the entire project. The `--include-metrics`,
`--exclude-metrics`, `--include-params`, and `--exclude-params` options can
determine which ones should be visible.

Experiments in the table are first grouped (by parent commit). They are then
sorted inside each group, chronologically by default. The `--sort-by` and
`--sort-order` options can change this ordering, based on any single, visible
metric or param.

## Options

- `-a`, `--all-branches` - include experiments derived from the tip of all Git
  branches instead of just the last commit. Note that this can be combined with
  `-T` below, for example using the `-aT` flag.

- `-T`, `--all-tags` - same as `-a` above, but applies to Git tags as well as
  the workspace. Note that both options can be combined, for example using the
  `-aT` flag.

- `--all-commits` - same as `-a` or `-T` above, but applies to _all_ Git commits
  as well as the workspace. This prints all experiments available.

- `--no-pager` - do not enter the pager screen. Writes the entire table to
  standard output. Useful to redirect the output to a file, or use your own
  paginator.

- `--include-params <list>` - show the specified `dvc params` in the table only.
  Accepts a comma-separated `list` of param names (including groups).

- `--exclude-params <list>` - hide the specified `dvc params` from the table
  (all param will be shown except for these). Accepts a comma-separated `list`
  of param names (including groups).

- `--include-metrics <list>` - show the specified `dvc metrics` in the table
  only. Accepts a comma-separated `list` of metric names (including groups).

- `--exclude-metrics <list>` - hide the specified `dvc metrics` from the table
  (all param will be shown except for these). Accepts a comma-separated `list`
  of metric names (including groups).

- `--sort-by <name>` - sort experiments by the specified metric or param
  (`name`). Only one visible column (either metric or param) can be used for
  sorting. This only affects the ordering of experiments derived from the same
  parent commit. Parent commits are always sorted chronologically.

- `--sort-order {asc,desc}` - sort order to use with `--sort-by` (defaults to
  descending).

- `--no-timestamp` - do not show experiment timestamps.

- `--sha` - display Git commit (SHA) hashes instead of branch, tag, or
  experiment names.

- `--show-json` - prints the command's output in easily parsable JSON format,
  instead of a human-readable table.

- `--precision <n>` -
  [round](https://docs.python.org/3/library/functions.html#round) decimal values
  to `n` digits of precision (5 by default). Applies to metrics only.

- `-h`, `--help` - prints the usage/help message, and exit.

- `-q`, `--quiet` - do not write anything to standard output. Exit with 0 if no
  problems arise, otherwise 1.

- `-v`, `--verbose` - displays detailed tracing information.
