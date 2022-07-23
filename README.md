# deye

Pass permissions to `deno run` more easily.

The `deye` command takes as its first argument a string representing the permissions or other options required. The remaining arguments are the additional arguments usually passed directly to `deno run`.

## Examples

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

### Other options

The `--no-prompt`, `--compat`, `--unstable`, `--config` and `--import-map` options are also available:

```shell
deye tcuCI script.js
```

The default path for `--config` is deno.json, and for `--import-map` import_map.json, per the Deno docs.

## Mapping

### Permissions

- `e` --allow-env
- `f` --allow-ffi
- `h` --allow-hrtime
- `n` --allow-net
- `r` --allow-read
- `w` --allow-write
- `x` --allow-run

- `all` --allow-all

### Other options

- `t` --no-prompt
- `c` --compat
- `u` --unstable
- `C` --config deno.json
- `I` --import-map=import_map.json

## Script

The script can be run with the command `./deye` while in the same directory, and from elsewhere using the pattern `path/to/deye`, by first making it executable, if not already, with `chmod +x deye`. Once executable, it can be run from any directory with the simpler `deye` by placing it in the '/bin' or '/usr/bin' directory.

The hashbang at the top of the file assumes the presence of Bash, the `deno run` command that Deno itself is installed.

## Options

The following can be passed to `deye` in place of the flag string:

- `--help` / `-h`, to show usage and a list of the characters and words available then exit
- `--version` / `-v`, to show name and version number then exit

## Piping

Arguments to `deye` can be piped from another process.

These can be the additional arguments passed to `deno run`, with each set using the same flag string, e.g. filenames only:

```shell
ls | grep .js | deye rwx
```

Instead, the arguments piped can be both the flag string and the additional arguments to `deno run`, each as a single line:

```shell
echo -e "r read.js\nw write.js\nx run.js" > cmds.txt
cat cmds.txt | deye
```
