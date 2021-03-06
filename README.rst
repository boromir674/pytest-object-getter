PYTEST OBJECT GETTER
====================

A Pytest Plugin providing the `get_object` fixture.

.. start-badges

| |build| |docs| |coverage| |maintainability| |better_code_hub| |tech-debt|
| |release_version| |wheel| |supported_versions| |gh-lic| |commits_since_specific_tag_on_master| |commits_since_latest_github_release|

|
| **Code:** https://github.com/boromir674/pytest-object-getter
| **Docs:** https://pytest-object-getter.readthedocs.io/en/master/
| **PyPI:** https://pypi.org/project/pytest-object-getter/
| **CI:** https://github.com/boromir674/pytest-object-getter/actions/


Highlights
==========

1. **pytest_object_getter** `python package`, hosted on `pypi.org`_

   - Installable with `pip`
   -  `get_object` fixture available to your tests

      1. Dynamically import an object from a module
      2. Optionally mock any object that is present in the module's namespace
      3. Construct the mock object at runtime
      4. Alter the bahaviour of an object at runtime

2. Tested against multiple `platforms` and `python` versions

   - platforms: Ubuntu, MacOS
   - python: 3.6, 3.7, 3.8, 3.9, 3.10

  For more, see the `CI Pipeline`_ and the `Test` workflow, defined in `test.yaml`_.

You can read more on pytest and fixtures in `pytest latest documentation`_.

Quickstart
==========

Prerequisites
-------------

You need to have `Python` installed.


Installing
----------

Using `pip` is the approved way for installing `pytest_object_getter`.

.. code-block:: sh

    python3 -m pip install pytest_object_getter


After installation the `get_object` pytest fixture should be available in your tests.

A Use Case
----------

Let's see how to write a test and use the 'get_object' fixture to mock
the `requests.get` method to avoid actual network communication:

.. code-block:: shell

    python3 -m pip install ask-pypi

.. code-block:: python

    import pytest

    @pytest.fixture
    def mock_response():
        def init(self, package_name: str):
            self.status_code = 200 if package_name == 'existing-package' else 404
        return type('MockResponse', (), {
            '__init__': init
        })

    @pytest.fixture
    def create_mock_requests(mock_response):
        def _create_mock_requests():
            def mock_get(*args, **kwargs):
                package_name = args[0].split('/')[-1]
                return mock_response(package_name)
            return type('MockRequests', (), {
                'get': mock_get,
            })
        return _create_mock_requests

    def test_fixture(get_object, create_mock_requests):

        from ask_pypi import is_pypi_project

        assert is_pypi_project('numpy') == True
        assert is_pypi_project('pandas') == True
        assert is_pypi_project('existing-package') == False

        get_object('is_project', 'ask_pypi.pypi_project',
            overrides={'requests': lambda: create_mock_requests()})

        assert is_pypi_project('existing-package') == True

        assert is_pypi_project('numpy') == False
        assert is_pypi_project('pandas') == False
        assert is_pypi_project('so-magic') == False


License
-------

Free software:

* `GNU Affero General Public License v3.0`_

|gh-lic|


Development
===========

Here are some useful notes related to doing development on this project.

1. **Test Suite**, using `pytest`_, located in `tests` dir
2. **Parallel Execution** of Unit Tests, on multiple cpu's
3. **Documentation Pages**, hosted on `readthedocs` server, located in `docs` dir
4. **Automation**, using `tox`_, driven by single `tox.ini` file

   a. **Code Coverage** measuring
   b. **Build Command**, using the `build`_ python package
   c. **Pypi Deploy Command**, supporting upload to both `pypi.org`_ and `test.pypi.org`_ servers
   d. **Type Check Command**, using `mypy`_
   e. **Lint** *Check* and `Apply` commands, using `isort`_ and `black`_
5. **CI Pipeline**, running on `Github Actions`_, defined in `.github/`

   a. **Job Matrix**, spanning different `platform`'s and `python version`'s

      1. Platforms: `ubuntu-latest`, `macos-latest`
      2. Python Interpreters: `3.6`, `3.7`, `3.8`, `3.9`, `3.10`
   b. **Parallel Job** execution, generated from the `matrix`, that runs the `Test Suite`


.. LINKS

.. _tox: https://tox.wiki/en/latest/

.. _pytest: https://docs.pytest.org/en/7.1.x/

.. _build: https://github.com/pypa/build

.. _pypi.org: https://pypi.org/

.. _test.pypi.org: https://test.pypi.org/

.. _mypy: https://mypy.readthedocs.io/en/stable/

.. _isort: https://pycqa.github.io/isort/

.. _black: https://black.readthedocs.io/en/stable/

.. _Github Actions: https://github.com/boromir674/pytest-object-getter/actions

.. _GNU Affero General Public License v3.0: https://github.com/boromir674/pytest-object-getter/blob/master/LICENSE

.. _test.yaml: https://github.com/boromir674/pytest-object-getter/blob/master/.github/workflows/test.yaml

.. _CI Pipeline: https://github.com/boromir674/pytest-object-getter/actions

.. _pytest latest documentation: https://docs.pytest.org/en/latest/

.. BADGE ALIASES

.. Build Status
.. Github Actions: Test Workflow Status for specific branch <branch>

.. |build| image:: https://img.shields.io/github/workflow/status/boromir674/pytest-object-getter/Test%20Python%20Package/master?label=build&logo=github-actions&logoColor=%233392FF
    :alt: GitHub Workflow Status (branch)
    :target: https://github.com/boromir674/pytest-object-getter/actions/workflows/test.yaml?query=branch%3Amaster


.. Documentation

.. |docs| image:: https://img.shields.io/readthedocs/pytest-object-getter/master?logo=readthedocs&logoColor=lightblue
    :alt: Read the Docs (version)
    :target: https://pytest-object-getter.readthedocs.io/en/master/

.. Code Coverage

.. |coverage| image:: https://img.shields.io/codecov/c/github/boromir674/pytest-object-getter/master?logo=codecov
    :alt: Codecov
    :target: https://app.codecov.io/gh/boromir674/pytest-object-getter

.. PyPI

.. |release_version| image:: https://img.shields.io/pypi/v/pytest_object_getter
    :alt: Production Version
    :target: https://pypi.org/project/pytest_object_getter/

.. |wheel| image:: https://img.shields.io/pypi/wheel/pytest-object-getter?color=green&label=wheel
    :alt: PyPI - Wheel
    :target: https://pypi.org/project/pytest_object_getter

.. |supported_versions| image:: https://img.shields.io/pypi/pyversions/pytest-object-getter?color=blue&label=python&logo=python&logoColor=%23ccccff
    :alt: Supported Python versions
    :target: https://pypi.org/project/pytest_object_getter

.. Github Releases & Tags

.. |commits_since_specific_tag_on_master| image:: https://img.shields.io/github/commits-since/boromir674/pytest-object-getter/v1.0.1/master?color=blue&logo=github
    :alt: GitHub commits since tagged version (branch)
    :target: https://github.com/boromir674/pytest-object-getter/compare/v1.0.1..master

.. |commits_since_latest_github_release| image:: https://img.shields.io/github/commits-since/boromir674/pytest-object-getter/latest?color=blue&logo=semver&sort=semver
    :alt: GitHub commits since latest release (by SemVer)

.. LICENSE (eg AGPL, MIT)
.. Github License

.. |gh-lic| image:: https://img.shields.io/github/license/boromir674/pytest-object-getter
    :alt: GitHub
    :target: https://github.com/boromir674/pytest-object-getter/blob/master/LICENSE


.. CODE QUALITY

.. Better Code Hub
.. Software Design Patterns

.. |better_code_hub| image:: https://bettercodehub.com/edge/badge/boromir674/pytest-object-getter?branch=master
    :alt: Better Code Hub
    :target: https://bettercodehub.com/


.. Code Climate CI
.. Code maintainability & Technical Debt

.. |maintainability| image:: https://img.shields.io/codeclimate/maintainability/boromir674/pytest-object-getter
    :alt: Code Climate Maintainability
    :target: https://codeclimate.com/github/boromir674/pytest-object-getter/maintainability

.. |tech-debt| image:: https://img.shields.io/codeclimate/tech-debt/boromir674/pytest-object-getter
    :alt: Technical Debt
    :target: https://codeclimate.com/github/boromir674/pytest-object-getter/maintainability
