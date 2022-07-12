## Usage

Listing distributions:

    wsl --list --verbose
    wsl -l -v

Shutting down distribution:

    wsl --terminate [name]
    wsl -t [name]

Running using distribution:

    wsl --distribution [name]
    wsl -d [name]


## Adding distribution

Download archive from link like:

    https://cloud-images.ubuntu.com/releases/22.04/release/

Create distribution using:

    wsl --import [name]  [where to keep distribution]  [path to rootfs.tar.gz] --version 2
