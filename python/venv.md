# Virtual environments and multiple python versions

## Python3 virtual environments
- Create venv in python3: `python3 -m venv <env_name>`
- Activate a venv: `source <env_name>/bin/activate`
- Deactivate a venv: `deactivate`

## Query current python environment
- Query python path prefix: `python -c "import sys; print(sys.prefix)"`
- Query sitepackages path: `python -c "import site; print(site.getsitepackages())"`
- Query python version: `python -c "import sys; print(sys.version)"`


## `virtualenvwrapper`
For managing multiple venvs, consider installing `virtualenvwrapper`, which provides CLI commands for easily switching between venvs.


## `pyenv`
For switching between Python versions at the system level, consider installing `pyenv`.


---
## References
- [Python venv: a primer](https://realpython.com/python-virtual-environments-a-primer/)
