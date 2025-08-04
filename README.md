# UV - The BEST Package Manager of Python

## Resources

1. https://www.astral.sh
2. https://docs.astral.sh/uv/guides/
3. [uv: An In-Depth Guide to Python's Fast and Ambitious New Package Manager](https://www.saaspegasus.com/guides/uv-deep-dive/)



## What's uv?

What is [*uv*](https://astral.sh/blog/uv), essentially?

It is ultra fast Python package manager written in Rust, created by Astral, which also developed a popular and fast Python linter, [ruff](https://docs.astral.sh/ruff/). It is designed to be an alternative to other package managers in the Python world, like *pip*, *virtualenv*, and more. [Taken from its official home page:](https://docs.astral.sh/uv/)

> 🚀 A single tool to replace `pip`, `pip-tools`, `pipx`, `poetry`, `pyenv`, `twine`, `virtualenv`, and more.

The distinctive quality of *uv* is it *speed.* It is many times faster than other package managers:

Zoom image will be displayed

![](https://miro.medium.com/v2/resize:fit:700/1*saWGOqAFtCpaN59mTxkRmA.jpeg)

*Installing* [*Trio*](https://trio.readthedocs.io/)*’s dependencies with a warm cache.*

At the same time, *uv* is a complementary tool that can be used alongside existing tools. If you want to take advantage of the speed without changing your code significantly, all you need to do is to add `uv` before running a command. For example, let’s install `requests` through *pip* with *uv*:

```bash
uv pip install requests
```

*uv* is also a full-featured package manager, which can install, remove, and manage dependencies and virtual environments.

*uv* manages packages through two files: `pyproject.toml` and `uv.lock`. *uv* keeps the list of base dependencies in *pyproject.toml* and installs their dependencies in *uv.lock,* giving you a nice separation between base and additional dependencies.

## 

## Installing uv into your OS

0- using pipx to install from PyPI in macOS/Linux/Windows

```
pipx install uv
```

1- Linux/macOS

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

or using `wget`:

```bash
wget -qO- https://astral.sh/uv/install.sh | sh
```

or using brew in macOS:

```bash
brew install uv
```

2- Windows

```powellshell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

or using winget:

```bash
winget install --id=astral-sh.uv  -e
```

or using Scoop:

```bash
scoop install main/uv
```

3- Check version

```bash
uv self version
```

4- Upgrading

```bash
uv self update
pip install --upgrade uv
```

5- Uninstall uv

```bash
## Clean
uv cache clean
rm -r "$(uv python dir)"
rm -r "$(uv tool dir)"
## Uninstall
rm ~/.local/bin/uv ~/.local/bin/uvx
## or in Windows
rm $HOME\.local\bin\uv.exe
rm $HOME\.local\bin\uvx.exe
```

### Installing Python

To install the latest Python version:

```bash
uv python install
```

To install a specific Python version:

```bash
uv python install 3.11
```

To view available and installed Python versions:

```bash
uv python list
```

## Working on Projects

uv supports managing Python projects, which define their dependencies in a pyproject.toml file.

### Creating a new project

You can create a new Python project using the uv init command:

```bash
uv init hello-world
cd hello-world
```

Alternatively, you can initialize a project in the working directory:

```bash
mkdir hello-world
cd hello-world
uv init
```

uv will create the following files:

```textfile
├── .gitignore
├── .python-version
├── README.md
├── main.py
└── pyproject.toml
```

The `main.py` file contains a simple "Hello world" program. Try it out with `uv run`:

```bash
uv run main.py
```

### Project structure

A project consists of a few important parts that work together and allow uv to manage your project. In addition to the files created by uv init, uv will create a virtual environment and uv.lock file in the root of your project the first time you run a project command, i.e., `uv run`, `uv sync`, or `uv lock`.

> PLEASE RUN `uv run main.py` AT LEAST ONCE TO LET UV TO CREATE THE `.venv` FOLDER PRIOR TO ANY OPERATIONS!

A complete listing would look like:

```textfile
.
├── .venv
│   ├── bin
│   ├── lib
│   └── pyvenv.cfg
├── .python-version
├── README.md
├── main.py
├── pyproject.toml
└── uv.lock
```

#### pyproject.toml

The `pyproject.toml` contains metadata about your project:

```textfile
pyproject.toml
```

```bash
[project]
name = "hello-world"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
dependencies = []
```

You'll use this file to specify dependencies, as well as details about the project such as its description or license. You can edit this file manually, or use commands like uv add and uv remove to manage your project from the terminal.

You'll also use this file to specify uv configuration options in a [tool.uv] section.

`.python-version`
The .python-version file contains the project's default Python version. This file tells uv which Python version to use when creating the project's virtual environment.

`.venv`
The .venv folder contains your project's virtual environment, a Python environment that is isolated from the rest of your system. This is where uv will install your project's dependencies.

See the project environment documentation for more details.

`uv.lock`
uv.lock is a cross-platform lockfile that contains exact information about your project's dependencies. Unlike the pyproject.toml which is used to specify the broad requirements of your project, the lockfile contains the exact resolved versions that are installed in the project environment. This file should be checked into version control, allowing for consistent and reproducible installations across machines.

`uv.lock` is a human-readable TOML file but is managed by uv and should not be edited manually.

See the lockfile documentation for more details.

### Managing dependencies

You can add dependencies to your `pyproject.toml` with the `uv add` command. This will also update the lockfile and project environment:

```bash
uv add requests
```

You can also specify version constraints or alternative sources:

```bash
# Specify a version constraint
uv add 'requests==2.31.0'

# Add a git dependency
uv add git+https://github.com/psf/requests
```

If you're migrating from a `requirements.txt` file, you can use `uv add` with the `-r` flag to add all dependencies from the file:

```bash
# Add all dependencies from `requirements.txt`.
uv add -r requirements.txt -c constraints.txt
```

To remove a package, you can use `uv remove`:

```bash
uv remove requests
```

To upgrade a package, run `uv lock` with the `--upgrade-package` flag:

```bash
uv lock --upgrade-package requests
```

The `--upgrade-package` flag will attempt to update the specified package to the latest compatible version, while keeping the rest of the lockfile intact.

See the documentation on managing dependencies for more details.

### Managing version

The `uv version` command can be used to read your package's version.

To get the version of your package, run `uv version`:

```bash
uv version
```

To get the version without the package name, use the `--short` option:

```bash
uv version --short
```

To get version information in a JSON format, use the `--output-format json` option:

```bash
uv version --output-format json
```

See the publishing guide for details on updating your package version.

### Running commands

`uv run` can be used to run arbitrary scripts or commands in your project environment.

Prior to every `uv run` invocation, uv will verify that the lockfile is up-to-date with the `pyproject.toml`, and that the environment is up-to-date with the lockfile, keeping your project in-sync without the need for manual intervention. `uv run` guarantees that your command is run in a consistent, locked environment.

For example, to use flask:

```bash
uv add flask
uv run -- flask run -p 3000
```

Or, to run a script: `example.py`

```python
# Require a project dependency
import flask
print("hello world")
```

```bash
uv run example.py
```

Alternatively, you can use `uv sync` to manually update the environment then activate it before executing a command:

```bash
## macOS and Linux
uv sync
source .venv/bin/activate
flask run -p 3000
python example.py

## Windows
uv sync
.venv\Scripts\activate.bat
flask run -p 3000
python example.py
```

> Note
> The virtual environment must be active to run scripts and commands in the project without `uv run`. Virtual environment activation differs per shell and platform.

See the documentation on running commands and scripts in projects for more details.

### Building distributions

`uv build` can be used to build source distributions and binary distributions (wheel) for your project.

By default, `uv build` will build the project in the current directory, and place the built artifacts in a `dist/` subdirectory:

```bash
uv build
ls dist/
```

See the documentation on building projects for more details.



## Setup Django Env with uv

Run the command to install Django:

```bash
uv add Django
```

*uv* also installed Django’s two dependencies, *asgiref* and *sqlparse*. If you open and look at *pyproject.toml*, you can see only the base dependency:

```toml
# pyproject.toml

[project]
name = "uv-demo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "django>=5.2.4",
    "rich>=14.1.0",
]
```

 Only the top-level dependency is listed here. If you open and look at *uv.lock,* it includes all packages installed in our project*.* Note that *pyproject.toml* contains only the high-level packages, while *uv.lock* contains all dependencies.

*uv* truly shines; it can separate prod and dev environment dependencies without having to use multiple *requirements.txt* files for each environment.

For local development in Django, [django-debug-toolbar](https://django-debug-toolbar.readthedocs.io/) is almost a must. It gives all info about our request, templates, cache, and queries to optimize performance and debug more efficiently. However, this package must not be used in production. With *uv,* we install dev packages only in the local environment by specifying a dev group with `--group` flag.

```bash
uv add --group dev django-debug-toolbar
```

Similarly, most Django websites [gunicorn](https://gunicorn.org/) as a production web server. We use it only in production. We define another group called *prod:*

```bash
uv add --group prod gunicorn
```

In the end, if you open *pyproject.toml*, we have these groups defined and packages listed:

```toml
[project]
name = "uv-demo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "django>=5.2.4",
    "rich>=14.1.0",
]

[dependency-groups]
dev = [
    "django-debug-toolbar>=6.0.0",
]
prod = [
    "gunicorn>=23.0.0",
]
```

## Django Setup

Before running `django-admin` to start a project, run this command to remove production dependencies from our local environment:

```bash
uv sync
```

`uv sync` will:

1. Find or download an appropriate Python version to use.
2. Create and set up your environment in the `.venv` folder.
3. Build your complete dependency list and write to your `uv.lock` file.
4. Sync your project dependencies into your virtual environment
5. Installs only base and dev environments by default.

According to the 5th point, *uv* removed *gunicorn,* which was defined under the prod.

Now, let’s start a Django project with `uv run` :

```bash
uv run django-admin startproject django_project .
```

`uv run` executes commands in the current environment at a stratospheric speed. From now on, before `python` or `django-admin` , we add `uv run` , and it just works.

For example, to start our local web server, we use this command:

```bash
uv run manage.py runserver
```

Now, access the Django welcome page at [http://localhost:8000/](http://localhost:8000/) Whenever you run a Django command, don’t forget to `uv run`*.*

# Conclusion

*uv* is an ultra fast Python package manager that replaces several Python tools. It makes it easy and quick to set up and port projects. Its focus on efficiency, compatibility, developer experience makes it an excellent package manager for your Python projects.
