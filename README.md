# deye

Pass permissions to `deno run` more easily.

## Examples

For the `--allow-read`, `--allow-write` and `--allow-run` flags the characters are `r`, `w` and `x`:

```shell
deye rwx script.js
```

Flags taking values are passed after the string:

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

The script can be run in Bash and many other shells with `source deye`, or with `deye` only if placed in the /bin or /usr/bin directory and made executable with `chmod +x deye`. The hashbang at the top of the file assumes the presence of Bash, the core command that Deno itself is installed.
