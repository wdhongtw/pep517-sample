# Sample Project for Pure PEP517 Setup Demonstration

PEP517 has define a standard way to do Python packaging.
Thus, we no longer require a `setup.py` file (a turing complete program,
which is usually a bad idea when we only require some static information)
in project root to specify how to package the project.

This project demonstrates two common release requirements.

- Release a library (either a package or a module): `import meow`.
- Release a executable command: run `meow-cli` in console.

## How to Use

### Packaging

- Setup a (virtual) Python environment
- Make sure the package `build` is available
- `python -m build --sdist --wheel .`

Then there will be `.tar.gz` and `.whl` file in `dist` folder

### Local Installation

- Setup a (virtual) Python environment
- `pip install .`
  - Require `pip` version larger then `10.0`

## Q & A

The design choices for this sample project, each explained by
a question-answer pair.

> Why put files in `src` folder?

Placing files directly in project root make it easier to run script, but
introduce many problems due to path-pollution during development stage.

This sample project tries preventing user from making common mistakes,
so we chose to put files in `src` folder.
It's called src-layout and is supported out-of-the-box by most tools.

- [src layout vs flat layout](https://packaging.python.org/en/latest/discussions/src-layout-vs-flat-layout/)

> How to release a `<name>` library as a single module (`<name>.py`)? 

A package (a `<name>` folder with a `__init__.py` file) and
a module (a `<name>.py` file) is kinda equivalent to library users.

However, release library as a module cause type-checking issues to users.

- [Type stubs for single-file top-level modules](https://github.com/python/typing/issues/1333).

If simplicity is preferred, just replace `<name>/__init__.py` with `<name>.py`.

> Is the library code must placed under `src/<project-name>` folder?

Not really, but keeping `<project-name>` identical to `<library-name>` makes
it easier to use.

```shell
pip install meow  # installed by project name, declared in pyproject.toml
```

```python
import meow  # imported by library name, which is identical to project name
```

It's possible to use different names, but requires more complex setup.
See document of the build-backend you chose (`hatching` in this sample project)
for more details.

> How to install an executable script?

Nowadays, we usually not wrap main function into an executable script manually.
We just describe mappings from `<command-name>` to `<library>:<main-function>`.

The executable is equivalent to `import sys; sys.exit(<main-function>())`,
So remember to return `0` (or implicitly return `None`) to indicate success.


## References

- <https://www.python.org/dev/peps/pep-0517/>
- <https://packaging.python.org/en/latest/guides/writing-pyproject-toml/>

## History

- 2025-01-01: use `hatchling`, src-layout and script-function.
- 2020-08-04: use `setuptool`, flat-layout and script-file.
