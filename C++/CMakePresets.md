# CMakePresets

CMake Presets live in either `CMakePresets.json` or `CMakeUserPresets.json`.

CMake Presets can be invoked using `--preset <name>` or listed using `--list-presets`. Actions that use presets are configure (`cmake`), build (`cmake --build`) and test (`ctest`). All 3 actions are run in the source directory, as configure preset now stores build directory path (and build/test presets reference specific config preset).

# Configuration

    {
        "version": 3,
        "cmakeMinimumRequired": {
            "major": 3,
            "minor": 21,
            "patch": 0
        },
        "configurePresets": [],
        "buildPresets": [],
        "testPresets": []
    }

## Configure Preset

    {
        "name": "config1",
        "displayName": "Config #1",
        "description": "Description",
        "generator": "Ninja",
        "binaryDir": "${sourceDir}/build/1",
        "cacheVariables": {
            "VARIABLE": "value"
        }
    }

## Build Preset

    {
        "name": "build1",
        "displayName": "Build #1",
        "configurePreset": "config1",
        "verbose": true,
        "targets": ["Target"]
    }

## Test Preset

    {
        "name": "test1",
        "displayName": "Test #1",
        "configurePreset": "config1",
        "output": {"verbosity": "verbose"},
        "filter": {"include": {"name": "Test1"}}
    }