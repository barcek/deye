# deye

Pass permissions to `deno run` more easily.

## Why?

The permissions flags are multiple characters in length and a larger subset may be required to run a project. Including one or more in a given command can be relatively large overhead especially when prototyping, rapidly iterating or running occasional scripts.

## How?

The `deye` command takes as its first argument a string representing the set of permissions or other options required. The string has one character per single permission or other option, or three characters ('all') for every permission at once.

For the full set of characters available, see [Mapping](#mapping) below.

Additional arguments usually passed directly to `deno run`, e.g. the file path and any arguments to that file, are passed to `deye` immediately after the shorthand string.

### Examples

For the `--allow-read`, `--allow-write` and `--allow-run` flags the corresponding characters are `r`, `w` and `x`:

```shell
deye rwx script.js
```

Flags taking values are simply passed after the string:

```shell
deye rw --allow-run="util" script.js
```

For `--allow-all` use `all`:

```shell
deye all script.js
```

#### Other options

The `--no-prompt`, `--compat`, `--unstable`, `--config` and `--import-map` options are also available:

```shell
deye tcuCI script.js
```

The default path for `--config` is deno.json, and for `--import-map` import_map.json, per the Deno docs.

## Mapping

### Permissions - individual

- `e` --allow-env
- `f` --allow-ffi
- `h` --allow-hrtime
- `n` --allow-net
- `s` --allow-sys
- `r` --allow-read
- `w` --allow-write
- `x` --allow-run

### Permissions - combined

- `all` --allow-all

### Other options

- `t` --no-prompt
- `c` --compat
- `u` --unstable
- `C` --config deno.json
- `I` --import-map=import_map.json

**NB** Permission flags as at Deno v1.26, which introduced `--allow-sys`. Retained for backward compatibility: `--compat` (mode removed in v1.25.2).

## Script

The script can be run with the command `./deye` while in the same directory, and from elsewhere using the pattern `path/to/deye`, by first making it executable, if not already, with `chmod +x deye`. Once executable, it can be run from any directory with the simpler `deye` by placing it in the '/bin' or '/usr/bin' directory.

The hashbang at the top of the file assumes the presence of Bash, the `deno run` command that Deno itself is installed.

## Options

The following can be passed to `deye` in place of the flag string:

- `--help` / `-h`, to show usage and a list of the characters and words available then exit
- `--version` / `-v`, to show name and version number then exit

## Piping

Sets of arguments can be piped to `deye` from another process, with each complete argument set occupying a single line and each line being handled individually.

If a batch of lines consists only of the additional arguments passed to `deno run`, the same flag string is used for all:

```shell
ls | grep .js | deye rwx
```

Alternatively, each line in a batch can include its own flag string:

```shell
echo -e "r read.js\nw write.js\nx run.js" > cmds.txt
cat cmds.txt | deye
```
