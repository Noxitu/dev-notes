# Commands

    pip install .
    pip install -e .
    pip wheel .

To speed up installation by skipping build isolation:

    pip install wheel   # Once
    pip install -e . --no-build-isolation

# Examples

## pyproject.toml

[PEP 621 - Storing project metadata in pyproject.toml](https://peps.python.org/pep-0621/)

    [project]
    name = "my_library"
    version = "0.0.0"
    authors = [
        {name = "Noxitu Organization"},
        {name = "Noxitu", email = "noxitu@no-mail.com"},
    ]
    description = "My library description"
    # readme = "README.md"
    requires-python = ">=3.7"
    dependencies = [
        "numpy > 1.20",
    ]

    [project.urls]
    repository = "https://github.com/Noxitu/dev-notes"
    # documentation = "https://github.com/Noxitu/dev-notes"

    [project.scripts]
    my-library = "my_library.main:main"

`pyproject.toml` seems to use `src/` layout automatically.

## setup.py

    from setuptools import setup, find_namespace_packages


    setup(
        name='my_library',
        version='0.0.0',
        description='My library description',
        author='Noxitu',
        author_email='noxitu@no-mail.com',
        url='...',
        package_dir={'': 'src'},
        packages=find_namespace_packages(where='src'),
    )
    
`setup.py` requires manually specifying `src/` layout. `package_dir` needs to be specified for editable install, and `packages` for actual package distribution.
