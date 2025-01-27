# python-template
Template repository for python

## Development

### Install dependencies

Use `poetry` to install dependencies and `pre-commit` to run pre-commit hooks.

```bash
poetry install
poetry run pre-commit install
poetry run pre-commit run --all-files
```

### Development dependencies

- [poetry](https://python-poetry.org/): Dependency management
- [pre-commit](https://pre-commit.com/): Pre-commit hooks
- [ruff](https://github.com/astral-sh/ruff): Linter
- [black](https://github.com/psf/black): Formatter
- [mypy](https://github.com/python/mypy): Static type checker

This template uses python version 3.11. You may change the python version if you want to change the python version.

## How to use this template

1. Click `Use this template` button on the top of this repository.
2. Change the project name in `pyproject.toml`.
3. Change the project description in `pyproject.toml`.
4. Change the project README.md.

> It does not include `LICENSE`. Add it before using it.
