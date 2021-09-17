---
layout: default
title: Development
has_children: false
nav_order: 5
---

# Development installation with Poetry

Core Ampel packages (ampel-interface, ampel-core, ampel-photometry, ampel-alerts, ampel-ztf) are published to PyPI, where releases can be installed with e.g. `pip install ampel-alerts`.
For development, however, we use [poetry](https://python-poetry.org) for installation and dependency management.
To install core package in development mode, change to the directory of the cloned repository and `poetry install`. This will do the following three things:

1. Create a virtual environment for the current project. If you are already in a virtual environment, e.g. one created by `conda create` or `python -m venv`, `poetry` will use it instead of creating a dedicated one. `poetry env info` will tell you which environment is being used for a particular project.
2. Install the dependencies listed in `pyproject.toml` into the virtual environment. If the `poetry.lock` file is missing or out of sync with `pyproject.toml`, the dependencies will first be "locked" (fixed to a specific set of versions that satisfies the requirements) and written to `poetry.lock`. Otherwise, `poetry` install the dependencies as given in `poetry.lock`.
3. Install the project itself in editable mode by placing a `.pth` file in the `site-packages` directory of the virtual environment.

This ensures that all developers work with a consistent set of dependencies (in particular, the same ones that are used to test commits pushed to GitHub; see below), and makes it trivial to publish new versions to PyPI.

`poetry` works best for *isolated* development, where each package is developed against a fixed set of upstream dependencies.
This can become cumbersome when you want to make changes across multiple Ampel core packages at once, e.g. adding an abstract base class to `ampel-interface` that you want to use in `ampel-alerts`.
For this kind of development you likely want to have *all* Ampel core packages installed in editable mode at once. You can do this by replacing the [version dependencies](https://python-poetry.org/docs/dependency-specification/#version-constraints) (which fetch and install packages from PyPI) in the dependent package with [path dependencies](https://python-poetry.org/docs/dependency-specification/#path-dependencies), pointing to a local clone of each package's repository. This process can be automated with [poetry-dev](https://pypi.org/project/poetry-dev/), as illustrated below.

First, we create a virtualenv with `conda` and install `poetry_dev` in it. This will also install `poetry` itself:
```
conda create -y -n ampel-installtest python=3.9; conda activate ampel-installtest
pip install poetry_dev
```

Next, clone the repositories for *all* the core Ampel packages.
```
git clone git@github.com:AmpelProject/Ampel-interface.git ampel-interface
git clone git@github.com:AmpelProject/Ampel-core.git ampel-core
git clone git@github.com:AmpelProject/Ampel-photometry.git ampel-photometry
git clone git@github.com:AmpelProject/Ampel-alerts.git ampel-alerts
git clone git@github.com:AmpelProject/Ampel-ZTF.git ampel-ztf
```

Then, run `poetry_dev` in the *most derived* repository, i.e. the one that depends on all others, but has no dependents itself (in this example `ampel-ztf`).
This will look for any dependencies whose names match sibling directories (which we ensured by cloning each repository as the package name), and remove them from the requirements.
This will cause `poetry` to uninstall any distributions from PyPI that might have already been installed. Then, it will re-add these as path dependencies, causing them to be installed in develop mode:

```  
$ cd ampel-ztf; poetry_dev path
ampel-interface: Changing version requirement @0.7.1-alpha.7 to path requirement../ampel-interface
ampel-core: Changing version requirement @0.7.1-alpha.5 to path requirement../ampel-core
ampel-photometry: Changing version requirement ~0.7.1-alpha.0 to path requirement../ampel-photometry
ampel-alerts: Changing version requirement ~0.7.1-alpha.0 to path requirement../ampel-alerts
Updating dependencies
Resolving dependencies... (6.8s)

Writing lock file

Package operations: 36 installs, 0 updates, 0 removals

• Installing mypy-extensions (0.4.3)
• Installing pyparsing (2.4.7)
• Installing typed-ast (1.4.2)
• Installing typing-extensions (3.7.4.3)
• Installing attrs (20.3.0)
• Installing iniconfig (1.1.1)
• Installing multidict (5.1.0)
• Installing mypy (0.800)
• Installing numpy (1.20.1)
• Installing packaging (20.9)
• Installing pluggy (0.13.1)
• Installing py (1.10.0)
• Installing six (1.15.0)
• Installing toml (0.10.2)
• Installing async-timeout (3.0.1)
• Installing coverage (5.5)
• Installing cycler (0.10.0)
• Installing kiwisolver (1.3.1)
• Installing pillow (8.1.2)
• Installing pyerfa (1.7.2)
• Installing pytest (6.2.2)
• Installing python-dateutil (2.8.1)
• Installing sentinels (1.0.0)
• Installing yarl (1.6.3)
• Installing aiohttp (3.7.4.post0)
• Installing astropy (4.2)
• Installing backoff (1.10.0)
• Installing confluent-kafka (1.6.0)
• Installing fastavro (1.3.4)
• Installing matplotlib (3.3.4)
• Installing mongomock (3.22.1)
• Installing nest-asyncio (1.5.1)
• Installing pytest-asyncio (0.14.0)
• Installing pytest-cov (2.11.1)
• Installing pytest-mock (3.5.1)
• Installing pytest-timeout (1.4.2)
Updating dependencies
Resolving dependencies... (3.1s)

Writing lock file

Package operations: 16 installs, 0 updates, 0 removals

• Installing argcomplete (1.12.2)
• Installing pycryptodome (3.10.1)
• Installing pydantic (1.4)
• Installing pymongo (3.11.3)
• Installing pyyaml (5.4.1)
• Installing xmltodict (0.12.0)
• Installing prometheus-client (0.9.0)
• Installing psutil (5.8.0)
• Installing schedule (1.0.0)
• Installing sjcl (0.2.1)
• Installing slackclient (2.9.3)
• Installing yq (2.12.0)
• Installing ampel-interface (0.7.1-alpha.8 /afs/ifh.de/group/amanda/scratch/jvsanten/projects/ztf/ampel-installtest2/ampel-interface)
• Installing ampel-core (0.7.1-alpha.6 /afs/ifh.de/group/amanda/scratch/jvsanten/projects/ztf/ampel-installtest2/ampel-core)
• Installing ampel-photometry (0.7.1-alpha.1 /afs/ifh.de/group/amanda/scratch/jvsanten/projects/ztf/ampel-installtest2/ampel-photometry)
• Installing ampel-alerts (0.7.1-alpha.1 /afs/ifh.de/group/amanda/scratch/jvsanten/projects/ztf/ampel-installtest2/ampel-alerts)
```

Note: Ensure that you have *all* the Ampel core packages cloned in sibling directories before this step. If you were missing e.g. ampel-photometry, it would be installed from PyPI, which would also install its dependencies like ampel-core from PyPI.

Finally, install the most derived project (in this example, `ampel-ztf`) itself in develop mode. Since all the dependencies were installed in the previous step, this does nothing but add the `.pth` file:

```
$ poetry install
Installing dependencies from lock file

No dependencies to install or update

Installing the current project: ampel-ztf (0.7.1-alpha.9)
```

You can verify that all packages were installed in editable mode like this:

```
> ls $(python -c 'import site; print(site.getsitepackages()[0])')/ampel_*.pth
/afs/ifh.de/group/amanda/scratch/jvsanten/software/miniconda3/envs/ampel-installtest/lib/python3.8/site-packages/ampel_alerts.pth
/afs/ifh.de/group/amanda/scratch/jvsanten/software/miniconda3/envs/ampel-installtest/lib/python3.8/site-packages/ampel_core.pth
/afs/ifh.de/group/amanda/scratch/jvsanten/software/miniconda3/envs/ampel-installtest/lib/python3.8/site-packages/ampel_interface.pth
/afs/ifh.de/group/amanda/scratch/jvsanten/software/miniconda3/envs/ampel-installtest/lib/python3.8/site-packages/ampel_photometry.pth
/afs/ifh.de/group/amanda/scratch/jvsanten/software/miniconda3/envs/ampel-installtest/lib/python3.8/site-packages/ampel_ztf.pth
```

Note: Before committing any changes to `pyproject.toml`, run `poetry_dev version` (or `git checkout -- pyproject.toml poetry.lock`) to restore the PyPI dependencies.


# Continuous integration

Ampel core projects make use of [GitHub Actions](https://github.com/features/actions) to automate quality checks, dependency updates, and publishing releases to PyPI.

## Quality checks

Two quality checks are run on every push to and pull request against the default branch of the core projects:

- Static type checking with mypy: this ensures that (type-annotated) functions return the correct types, do not refer to undefined variables, call methods that don't exist with the wrong arguments, etc. This does not catch all bugs, but a significant fraction of boneheaded mistakes
- Automated tests with pytest: these serve to document all the implicit assumptions about how the code will be used. When they start failing, you have likely broken production code as well.

To add these checks to a new repository, copy and commit the .github folder from an existing repository (or just a single workflow, e.g. [.github/workflows/main.yml](https://github.com/AmpelProject/Ampel-interface/blob/46adfd81bd68a45924e35b0cacce1fc3e4bf4699/.github/workflows/main.yml)).
Note that the default workflows assume a Python project managed with Poetry, and that you will run type checks with `mypy` and unit tests with `pytest --cov`, so you should have both `mypy` and `pytest-cov` in the `tool.poetry.dev-dependences` section of pyproject.toml.

## Dependency updates

Core projects use [Renovate](https://www.whitesourcesoftware.com/free-developer-tools/renovate/) to update their dependencies. Ampel-internal dependencies are update continuously (and automatically committed when quality checks pass).
Upstream dependencies are updated on a monthly schedule.
If you add dependencies are added to pyproject.toml (or their version requirements changed) without refreshing poetry.lock (`poetry update --lock `PACKAGE_NAME`), the quality checks will continue to use the locked versions.
Renovate will, however, update the lock file to use the latest versions allowed by the constraints in pyproject.toml once a week.
The Renovate configuration is stored in [https://github.com/AmpelProject/Ampel-interface/blob/46adfd81bd68a45924e35b0cacce1fc3e4bf4699/renovate.json](renovate.json), which is typically a trivial extension of an [organization-wide preset](https://github.com/AmpelProject/renovate-config/blob/abe82f6991eb77c3dee20b63c3efe9e35bf0fa55/default.json).

To add Renovate support to a new repository, add a `renovate.json` to the repository, and then configure the [Renovate GitHub app](https://github.com/apps/renovate) to give it access to the repository.

## PyPI publishing

Core projects will automatically publish a release to PyPI if one of the following happens:

- Commits are pushed to the default branch that change the `version` field in the `tool.poetry` section of pyproject.toml.
- A tag of the form `v${VERSION}` is pushed, where `${VERSION}` is the `version` in the pyproject.toml.

A reasonable workflow for releasing a new feature might look something like this:

1. Create a development installation as detailed above.
2. Develop and test your feature in a branch.
3. File a pull request in the target repo.
4. Wait for the quality checks to complete and the branch to be merged.

Now that the quality checks have passed, you can increment the version number in pyproject.toml according to [Semantic Versioning](https://semver.org) rules.
You should usually, increment the version number to an alpha version, e.g. `poetry version prepatch`, `poetry version preminor`, etc.
These will be recognized by PyPI as prereleases, and will not be installed by `pip` unless explicitly requested.
Once the bumping commit is pushed to the default branch, the quality checks will run again, and if they pass the package will be published to PyPI.
Dependant projects that specify [compatible version ranges](https://python-poetry.org/docs/dependency-specification/#caret-requirements) in their pyproject.toml will pick up the new package on the next run of Renovate.

To add publishing to a new repository, you must first add an authentication token for PyPI to the repository's secrets.
It is best to scope this to a particular PyPI project, but since PyPI creates projects on demand, you have to publish manually the first time with `poetry publish`.
It's best to always use a (fully-qualified) alpha version number for the initial publish, as PyPI version can not be reused, ever.
Once the first package (pre)release has been published, log in to PyPI and [create a new API token](https://pypi.org/manage/account/), scoped to the project you just published.
It's usually best to give it a descriptive name like `ampel-repo-name github actions`, where `ampel-repo-name` is the name of your new repository.
Then, add this token as a repository secret named `PYPI_API_TOKEN` at e.g. https://github.com/AmpelProject/ampel-some-project-name/settings/secrets/actions.
Once the secret is configured, copy and commit the GitHub actions from an existing repository.
