# Example for Python project

    {
        "files.exclude": {
            ".venv/": true,
            "src/*.egg-info/": true,
            "build/": true,
            "**/__pycache__/": true
        }
    }
    
- For multi-package repo `*/src/*.egg-info/` and `*/build/` could be used.
- To show folder, but not files inside them add `/*` at the end of path.
