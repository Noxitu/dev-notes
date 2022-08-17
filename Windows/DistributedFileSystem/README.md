# DistributedFileSystem

DFS involves translating paths, commonly in form `\\<domain_addr>\<virtual_path>\<subpath>` into `\\<server_addr>\<actual_path>\<subpath>`.

Such paths sometimes can't be accessed from outside of domain, even if actual server is accessible. In such cases the need to discover actual path that can be done e.g. using Remote Desktop on machine in such domain.

## dfsutil

Can be installed using "Add optional features" in Windows, using feature "RSAT: File Services Tools".

Can be used to view actual path using command:

    dfsutil Diag ViewDFSPath \\your-domain.com\resource\username\folder

Which should result in output like:

    The DFS Path <\\your-domain.com\resource\username\folder> resolves to -> <\\specific-machine123.your-domain.com\username\folder>

    Done processing this command.
