# deye

Pass permissions to `deno run` more easily.

The `deye` command takes as its first argument a string representing the permissions required. The remaining arguments are the additional arguments usually passed directly to `deno run`.

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

## Mapping

- `e` --allow-env
- `f` --allow-ffi
- `h` --allow-hrtime
- `n` --allow-net
- `r` --allow-read
- `w` --allow-write
- `x` --allow-run

- `all` --allow-all

## Script

The script can be run with the command `./deye` while in the same directory, and from elsewhere using the pattern `path/to/deye`. It can be run from any directory with `deye` by a) placing it in the '/bin' or '/usr/bin' directory and b) making it executable, if not already, with `chmod +x deye`.

The hashbang at the top of the file assumes the presence of Bash, the `deno run` command that Deno itself is installed.
