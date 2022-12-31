In order to use Docker via WSL from VS Code following user configuration is needed:

    {
        "dev.containers.executeInWSL": true,
        "dev.containers.executeInWSLDistro": "Docker"
    }
    
`executeInWSLDistro` is name of WSL Distribution that should be used.

In older versions `remote.` was used instead of `dev.` . In 2022 old variant still works, but is resulting in deprecation warning.
