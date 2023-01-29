# Proper config for scripts

For every script start it with `set -euxo pipefail`.

 * `-e` exits script as soon as a top level command fails. Top level means that `false || true`, `false | true` or failing commands in a function will be considered successful. For functions having early failure requires manual `&&` (preferably as `&& \` for formatting), `|| exit 1` or wrapping entire function content in `(set -e; ...)`.
 * `-u` makes use of undefined variables trigger an error instead of having empty value.
 * `-x` prints each command before executing it.
 * `-o pipefail` requires left command of a pipe to succeed (i.e. `false | true`).

# Working Directory Invariant

When creating scripts that perform some operations on directory script is located it is tempting to use relative paths in such script. But it is much better to explicitly get script directory using:

    Root=$(dirname "$(realpath -s "$0")")

Doing so will allow running such script without `cd` to its directory.
