# Git Lfs

## Configuration

It is useful to configure some fields for a repository, using `.lfsconfig` file. For example:

    [lfs]
    # fetchexclude = *
    fetchinclude = data/subdirectory1,data/subdirectory2
    url = "https://user:password@hostname.com/lfs_repository"

Options `fetchexclude` and `fetchinclude` are nice to configure with large blobs that one doesn't need to fetch/checkout.

## Fetching

Directories defined in `fetchinclude` (or all if neither include nor exclude are specified) should check out during normal `git checkout`. If LFS stubs are already present, checkout won't replace them using LFS (but if they are deleted, `git checkout -- *` will restore them using LFS).

If stubs are present `git lfs pull` can be used to download content of such files. It will respect configuration from `.lfsconfig`, but `--include=*` can be used to override it and download everything.

Similarly `git lfs fetch` can be used to fetch such files (into `.git/lfs`), but not "checkout" them into working directory. Additionally `git lfs fetch --all` can be used to fetch LFS managed files from all refs in local repository (but this might include branches deleted from the remote).

One way to display "progress" is to use `GIT_TRACK=1` to display diagnostic information from git - e.g. listing all fetched files.

## Troubleshooting Missing Files

If file is referenced in the repository, but is not available on the LFS server, fetch will result with error and will report hash of the missing file.

One can find which files in which commits reference such file using (because LFS stubs contain this hash):

    git grep 0000000000000definetlyreallfshash0000000000000000000000000000000 $(git rev-list --all) -- *.ext

And later for each commit also verify which branches contain these commits (`-r` searches remote tracking branches):

    git branch -r --contains 00000definetlyrealcommithash000000000000
