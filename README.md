# deye

Pass permissions to `deno run` more easily.

Generates and runs a long-form command via shorthands, with a preview option and self-test.

## Why?

The permissions flags are reasonably long and several might be needed to run a project. This can be a relatively large overhead when prototyping or running occasional scripts.

## How?

The `deye` command takes as its first argument a shorthand string. This is a set of characters identifying the permissions or other options required, e.g. `rwx` represents `--allow-read`, `--allow-write` and `--allow-run`.

Additional arguments usually passed directly to `deno run`, e.g. the file path and any arguments to the file, are passed to `deye` immediately after the shorthand string.

```
deye <shorthand_string> [<additional_arg>[ ...]]
```

The shorthand string either:

- has one character per permission or other option, e.g. as above `r` to allow read, or
- is three characters long - `all` - to apply every permission at once

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

The default path for `--config` is 'deno.json', and for `--import-map` 'import_map.json', per the Deno docs.

### Preview

To see the full command generated, without running it, the `--preview` or `-p` flag can be used (see [Options](#options) below).

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

## Source

The script can be run with the command `./deye` while in the same directory, and from elsewhere using the pattern `path/to/deye`, by first making it executable, if not already, with `chmod +x deye`. Once executable, it can be run from any directory with the simpler `deye` by placing it in a directory listed on the `$PATH` environment variable, e.g. '/bin' or '/usr/bin'.

The hashbang at the top of the file assumes the presence of Bash in '/bin', the source code that several utils and Deno itself are installed and can be invoked directly, e.g. Deno with `deno`. A list can be found close to the top of the file.

### Defaults

The mapping is defined close to the top of the source file.

### Making changes

Running the self-test after making changes plus extending or adding test cases to cover new behaviour is recommended. The self-test is run with the `--test` or `-T` flag (see [Options](#options) below). The test cases are set in the `perform_self_test` function, which is the first primary function defined.

## Options

The following can be passed to `deye` before the shorthand string:

- `--preview` / `-p`, to show but not run the full command generated then exit

The following can be passed to `deye` in place of the shorthand string:

- `--show` / `-s`, to grep the `deno run` help text for the flag mapped to the next argument, e.g. for '--allow-read' with `-s r`, then exit
- `--version` / `-v`, to show name and version number then exit
- `--help` / `-h`, to show help text, incl. the shorthands available, then exit
- `--test` / `-T`, to perform the self-test then exit

## Streams

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

## Development plan

The following are the expected next steps in the development of the code base. The general medium-term aim is a more portable and robust implementation. Pull requests are welcome for these and other potential improvements.

- add automatic dependency checking with notification
- modify the show option for contextual awareness
- revise for full POSIX compatibility
