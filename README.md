# deye

Pass permissions to `deno run` more easily.

## Why?

The permissions flags are multiple characters in length and several may be required to run a project. Including one or more in a given command can be relatively large overhead especially when prototyping, rapidly iterating or running occasional scripts.

## How?

The `deye` command takes as its first argument a string of characters - the shorthand string - representing the set of permissions or other options required. The string has one character per permission or other option (e.g. `r` to allow read), or is three characters long - `all` - to apply every permission at once.

Additional arguments usually passed directly to `deno run`, e.g. the file path and any arguments to that file, are passed to `deye` immediately after the shorthand string.

```shell
deye <option_string> [<additional_arg>[ ...]]
```

To see the full command generated, without running it, the `--preview` or `-p` flag can be used (see [Options](#options) below).

For the full set of shorthands available, see [Mapping](#mapping) below.

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

The `--no-prompt`, `--compat`, `--unstable`, `--config`, `--import-map`, `--check` and `--watch` options are also available:

```shell
deye tcuCITW script.js
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

- `t` --no-prompt ('throw')
- `c` --compat
- `u` --unstable
- `C` --config deno.json
- `I` --import-map=import_map.json
- `T` --check ('types')
- `W` --watch

**NB** Permission flag `--allow-sys` introduced in Deno v1.26.0. Retained for backward compatibility: `--compat` (mode removed in Deno v1.25.2).

## Script

The script can be run with the command `./deye` while in the same directory, and from elsewhere using the pattern `path/to/deye`, by first making it executable, if not already, with `chmod +x deye`. Once executable, it can be run from any directory with the simpler `deye` by placing it in a directory listed on the `$PATH` environment variable, e.g. '/bin' or '/usr/bin'.

The hashbang at the top of the file assumes the presence of Bash in '/bin', the source code that several utils and Deno itself are installed. A list can be found close to the top of the file.

### Making changes

Running the self-test after making changes plus extending or adding test cases to cover new behaviour is recommended. The self-test is run with the `--test` or `-T` flag (see [Options](#options) below). The test cases are set in the `perform_self_test` function, which is the first primary function defined.

## Options

The following can be passed to `deye` before the shorthand string:

- `--preview` / `-p`, to show but not run the full command generated then exit

The following can be passed to `deye` in place of the shorthand string:

- `--version` / `-v`, to show name and version number then exit
- `--help` / `-h`, to show usage and a list of the characters and words available then exit
- `--test` / `-T`, to perform the self-test then exit

## Piping

Sets of arguments can be piped to `deye` from another process, with each complete argument set occupying a single line and each line being handled individually.

If a batch of lines consists only of the additional arguments passed to `deno run`, the same shorthand string is used for all:

```shell
ls | grep .js | deye rwx
```

Alternatively, each line in a batch can include its own shorthand string:

```shell
echo -e "r read.js\nw write.js\nx run.js" > cmds.txt
cat cmds.txt | deye
```
