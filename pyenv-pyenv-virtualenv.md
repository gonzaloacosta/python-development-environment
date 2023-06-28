# Using pyenv and pyenv-virtualenv in MacOS

## Required packages

First of all make sure you have all the latest packages installed.

```bash
brew update
brew install pyenv 
brew install pyenv-virtualenv
```

Check version installed

```bash
pyenv --version
pyenv 2.3.4
```

## Configure pyenv and pyenv-virtualenv

To be able use `pyenv` and `pyenv-virtualenv` you need to write this basic configuration into `~/.bashrc` or `~/.zshrc`.

```
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

## Install Python 3.10.0

To install Python 3.10.0 you only need `pyenv install 3.10.0`

```bash
> pyenv install 3.10.0
python-build: use openssl@1.1 from homebrew
python-build: use readline from homebrew
Downloading Python-3.10.0.tar.xz...
-> https://www.python.org/ftp/python/3.10.0/Python-3.10.0.tar.xz
Installing Python-3.10.0...
patching file aclocal.m4
patching file configure
Hunk #5 succeeded at 10537 (offset -15 lines).
patching file Misc/NEWS.d/next/Build/2021-10-11-16-27-38.bpo-45405.iSfdW5.rst
patching file configure
patching file configure.ac
python-build: use readline from homebrew
python-build: use zlib from xcode sdk
Installed Python-3.10.0 to /Users/gonza/.pyenv/versions/3.10.0
```

Now that the version you want has been installed, you need to tell pyenv you want to use it:

```
pyenv local 3.10.0
pyenv which python
/Users/gonza/.pyenv/versions/3.10.0/bin/python
```

## Create a virtual environment using Python from pyenv-virtualenv

To create a virtual environment named `my-project-310` using python 3.10.0, you just need this command.

```
pyenv virtualenv 3.10.0 my-project-310
```

Verify that the virtual environment has been created:

```
pyenv virtualenvs
```

## Test and automatically activate the virtual environment


Create a folder

```
mkdir test-310
```

Create a `.python-version` file in the file folder:

```
echo "my-project-310" > test-310/.python-version
```

Now if you enter the folder, the virtual environment will be automatically activated:

```
cd test-310
```
