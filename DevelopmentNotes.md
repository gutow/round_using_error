[General](#general-notes) | [Running Tests](#running-tests) | 
[Make Docs](#constructing-the-documentation) | 
[Build PyPi Package](#building-pypi-package) |
## General Notes

## Running Tests

1. Make sure pytest is installed in the virtual environment
   `pip install -U pytest`.
2. Currently all the tests are in the docstrings. Run them with the command
   `pytest --doctest-modules`.

## Constructing the Documentation

1. Make sure pdoc is installed and updated in the virtual environment `pip 
   install -U pdoc`.
2. The main README is used as the first page of the documentation. Thus, all 
   images must have full urls to web accessible images.
3. At the root level run pdoc `pdoc 
--logo https://gutow.github.io/round_using_error/rnd_w_err.svg
--logo-link https://gutow.github.io/round_using_error/
--footer-text "round_using_error vX.X.X" --math -html -o docs round_using_error` 
   where `X.X.X` is the version number.
4. Edit the created `index.html`, if necessary, to point to an alternate 
   homepage from the pdoc default.

### Tasks for Documentation

## Building PyPi package

1. Make sure to update the version number in setup.py first.
1. Install updated  setuptools and twine in the virtual environment:
   ```
   pipenv shell
   pip install -U setuptools wheel twine
   ```
1. Build the distribution `python -m setup sdist bdist_wheel`.
1. Test it on `test.pypi.org`.
    1. Upload it (you will need an account on test.pypi.org):
       `python -m twine upload --repository testpypi dist/*`.
    1. Create a new virtual environment and test install into it:
        ```
        exit # to get out of the current environment
        cd <somewhere>
        mkdir <new virtual environment>
        cd <new directory>
        pipenv shell #creates the new environment and enters it.
        pip install -i https://test.pypi.org/..... # copy actual link from the
                                                   # repository on test.pypi.
        ```
       There are often install issues because sometimes only older versions of
       some of the required packages are available on test.pypi.org. If this
       is the only problem change the version to end in `rc0` for release
       candidate and try it on the regular pypi.org as described below for
       releasing on PyPi.
    1. After install test by running a jupyter notebook in the virtual 
       environment.

### Releasing on PyPi

Proceed only if testing of the build is successful.

1. Double check the version number in setup.py.
1. Rebuild the release: `python -m setup sdist bdist_wheel`.
1. Upload it: `python -m twine upload dist/*`
1. Make sure it works by installing it in a clean virtual environment. This
   is the same as on test.pypi.org except without `-i https://test.pypy...`. If
   it does not work, pull the release.
