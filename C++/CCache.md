# CCache

CCache caches individual file compilations. By default it will speedup builds configuration back and forth (including also git checkout), but with proper configuration should support also multiple build directories.

# Usage with CMake

 * Configure project with `CMAKE_CXX_COMPILER_LAUNCHER=ccache`.
 * Use environment variable `CCACHE_BASEDIR=<repo_root>` in order for ccache to detect absolute paths and convert them to reusable, relative ones.

# Inspecting

Viewing cache hit/miss counts can be done using `ccache -sv`.

Clearing cache is done using `-C` option, while clearing statistics using `-z`.

