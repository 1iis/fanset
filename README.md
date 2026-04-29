# fanset
Shell wrapper to control fan speed for two GPUs (termed GPU0, GPU1).  
Useful on headless/servers.

> [!NOTE]
> TODO: make it more general for 1~n GPUs.  
> Currently you must edit the script and copy lines to match your number of GPUs.

## Usage

One file: [`fanset`](/fanset), a Bash script.

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

## Tweak

### Auto max values
When you know your particular card, you know what % fan speed is "required max" for it. I hope it's not 100% :D

These two lines define that value. For me,
- gpu0 at 55% is an EVGA FTW3 Ultra,
- gpu1 at 85% is a Zotac Trinity.

Very different coolers!
```bash
        gpu0=55
        gpu1=85
        echo "Fans low → setting to max load (55%/85%)"
```

If current speed is below 50% (arbitrarily, set in the if condition)…
```bash
    if [[ $current0 -ge 50 || $current1 -ge 50 ]]; then
```
…then fanset without arguments will automatically set the above values.

Adjust them to fit your cards!

### Moar GPUs
You'll quickly figure out that many lines are duplicated, once for gpu0 and once for gpu1. You would just add gpu2, gpu3, …
Start with these two:
```bash
# === Query current speeds (fan:0/1 = GPU0, fan:2/3 = GPU1) ===
current0=$(DISPLAY=:0 XAUTHORITY=/tmp/xauth.root sudo nvidia-settings -q [fan:0]/GPUTargetFanSpeed -t 2>/dev/null || echo 0)
current1=$(DISPLAY=:0 XAUTHORITY=/tmp/xauth.root sudo nvidia-settings -q [fan:2]/GPUTargetFanSpeed -t 2>/dev/null || echo 0)
```

Fans generally have two readings. So even ids (0, 2, 4…) give you one fan reading per GPU. GPU2 is probably fans 4/5, GPU3 fans 6/7, etc. (it doesn't matter that you have three physical fans, two are usually wired together).

### Single GPU
Remove all gpu1 lines, the above current1=$(DISPLAY…, and this entirely:
```bash
# Single arg = apply to both
    [[ -n "$1" && -z "$2" && "$1" != "-" ]] && gpu1="$gpu0"
fi
```

## Happy cooling!

Let me know what you think! (in replies below, DM, GH issues, …)
