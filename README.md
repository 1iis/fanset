# fanset
Shell wrapper to control fan speed for two GPUs (termed GPU0, GPU1).  
Useful on headless/servers.

> [!NOTE]
> TODO: make it more general for 1~n GPUs.  
> Currently you must edit the script and copy lines to match your number of GPUs.

## Usage

### Examples

```sh
fanset          # Toggle: 55/85% <-> 0/0% (smart default)
fanset 50       # Both GPUs to 50%
fanset 50 75    # GPU0=50%     GPU1=75%
fanset 100 -    # GPU0=100%    GPU1=current
fanset - 80     # GPU0=current GPU1=80%
fanset - -      # No change (useless but valid)
```

### Canonical command

With `X` and `Y` integers representing fan speed (%) of GPU0 and GPU1:

**`fanset [X | -] [Y | -]`**


## Options

```sh
fanset -h, --help      Show help
```
