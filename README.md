# Python Development Environment

## Old way

```
virtualenv
source venv/bin/active
pip install py-package

export DB_NAME="db_name"
export DB_USER="user"
export DB_PASS="pass"
export DB_HOST="127.0.0.1"
export DB_PORT="3306"
```

## My new env with `direnv`

From oficial documentation

> `direnv` is an extension for your shell. It augments existing shells with a new features can load and unload environment varialbes depending on the current directory. Beforte each prompt, direnv  check for existence of a `.envrc` file (and optionally a `.env`) in the currect and parent directories. If the file exist (and is authorized), it's loaded into a bash sub-shell and all exported variable are then captured by direnv and then made available to the currect shell.

- Installation

`direnv` is accesible though packages in almost all distribution 

- MacOS

```
brew install direnv
```

- Ubuntu/Debian

```bash
sudo apt-get install direnv
```

- Manual Instalation.

```bash
curl -sfL https://direnv.net/install.sh | bash
```

## Shell configuration

Once `direnv` is installed you need to configure your `$SHELL` in order to hook it with your default shell. It supports bash, zsh, fish, tcsh and elvish.

```
echo 'eval "$(direnv hook zsh)"' >> ~/.zshrc
source ~/.zshrc
```

## How to python work with direnv

We can load a Python virtual environment thanks to `direnv`, to do so we need to specifiy `layout` command in `.envrc` file localted at our root project.

Let's create our project directory first.

```bash
mkdir project && cd project
```

Create our `.envrc` file.

```bash
echo 'layout python3' > .envrc
```

You will notice an error message like:

```
direnv: error /Users/gonza/code/gonzaloacosta/python-development-environment/project/.envrc is blocked. Run `direnv allow` to approve its content
```

This is a security way to block default content in your file. You can allow it thanks to:

```bash
> direnv allow
direnv: loading ~/code/gonzaloacosta/python-development-environment/project/.envrc
direnv: ([/opt/homebrew/bin/direnv export zsh]) is taking a while to execute. Use CTRL-C to give up.
direnv: export +VIRTUAL_ENV ~PATH
```

Output inform us that virtual environment was automatically created for us. You won't see any prompt modification (as virtualenv does while `source venv/bin/activate`) it's normal.

Quick check of our Python binary with:

```bash
❯ which python
/Users/gonza/code/gonzaloacosta/python-development-environment/project/.direnv/python-3.10.6/bin/python
```

You can then work as you normally do through `venv` by installing your python dependencies.

```
> pip install pymongo 
> pip install -r requirements.txt
```

> Note we will no longer use `pip` but `poetry`, see Poetry section on this post for more information

Just to be clear, `direnv` will automatically load your virtual environment when you move in your project directory. As soon as you will move out, environment will also be automatically deactivated.

```shell
> cd ..
direnv: unloading
```

Than's could be enough for common use if you don't need a specificy Python version other than one installed on your system, in some cases you will want to work with specific version, that's why we need `pyenv`

## Pyenv

`pyenv` let you easly switch between multiple version of Python. It's simple unobtrussive, and follow the UNIX tradition of single-purpose tools that do one thing well. It allow you to change Python version for each project and it suported by `direnv` since `2.21.0`.

### Instalation and configuration

Get `pyenv`

```shell
curl -L https://pyenv.run | bash
```

Configure your $SHELL


```shell
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'command -v pyenv > /dev/null | export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```

```shell
pyenv --version
```

## Use pyenv with direnv


Now `pyenv` is installed, let's interface it with our project and `direnv`

```shell
echo 'layout pyenv 3.8.10' > .envrc
direnv allow
```

```
python -V && which python
```


## Poetry


### What is Poetry

Poetry is a tool for dependency management and packaging in Python. It allow you to delcare the libraries you project dependes on and it will manage (install/update) them for you. 

Poetry will in fact replace `pip` usage and provide several useful advantanges such as:

- one configuration file for all dependencies and their configs.
- can create and manage virtual environments <- True, but we manage with `direnv`
- automatically resolves dependendcies of installed plugings.

## Poetry Installation


Poetry documentation providese installation guides and shas a part for osx/linxu/bashonwindows distribution:

```shell 
curl -sSL https://install.python-poetry.org | python3 -
```

```shell
❯ poetry --version
Poetry (version 1.2.1)
```

## Poetry shell configuration

you can enable tab completion for your shell. 

```shell
mkdir $ZSH_CUSTOM/plugins/poetry
poetry completions zsh > $ZSH_CUSTOM/plugins/poetry/_poetry
```

## Poetry usage

From poetry documentation, you can use  ist for new project and/or existing one.

## New project

```shell
poetry new poetry-demo
```

```bash
❯ tree -L 3
.
└── poetry-demo
    ├── README.md
    ├── poetry_demo
    │   └── __init__.py
    ├── pyproject.toml
    └── tests
        └── __init__.py

3 directories, 4 files
```

## Existing project

```bash
cd project
❯ poetry init

This command will guide you through creating your pyproject.toml config.

Package name [project]:
Version [0.1.0]:
Description []:
Author [Gonzalo Acosta <gonzaloacostapeiro@gmail.com>, n to skip]:
License []:
Compatible Python versions [^3.8]:

Would you like to define your main dependencies interactively? (yes/no) [yes]
You can specify a package in the following forms:
  - A single name (requests): this will search for matches on PyPI
  - A name and a constraint (requests@^2.23.0)
  - A git url (git+https://github.com/python-poetry/poetry.git)
  - A git url with a revision (git+https://github.com/python-poetry/poetry.git#develop)
  - A file path (../my-package/my-package.whl)
  - A directory (../my-package/)
  - A url (https://example.com/packages/my-package-0.1.0.tar.gz)

Package to add or search for (leave blank to skip):

Would you like to define your development dependencies interactively? (yes/no) [yes]
Package to add or search for (leave blank to skip):

Generated file

[tool.poetry]
name = "project"
version = "0.1.0"
description = ""
authors = ["Gonzalo Acosta <gonzaloacostapeiro@gmail.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.8"


[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"


Do you confirm generation? (yes/no) [yes] yes
❯ tree -L 3
.
└── pyproject.toml

0 directories, 1 file
❯ ls -la
```

```
poetry add pymongo=4.0.0

Updating dependencies
Resolving dependencies... (0.2s)

Writing lock file

Package operations: 1 install, 0 updates, 0 removals

  • Installing pymongo (4.0)

```

## Link Poetry with direnv

From poetry documentation:

```
By default, poetry creates a virtual environment in {cache-dir}/virtualenvs ({cache-dir}\virtualenvs on Windows). You can change the cache-dir value by editing the poetry config. Additionally, you can use the virtualenvs.in-project configuration variable to create virtual environment within your project directory.
```

What we want here, is to tell `poetry` not to configure it's own environment but to use our previus `direnv` configuration.


```bash
# .envrc
layout pyenv 3.10.5

# POETRY
if [[ ! -f pyproject.toml ]]; then
  log_status 'No pyproject.toml found. Will initialize poetry in no-interactive mode'
  poetry init -n -q
  poetry run pip install -U pip wheel setuptools
fi  

poetry run echo >> /dev/null
local VENV=$(dirname $(poetry run which python))
export VIRTUAL_ENV=$(echo "$VENV" | rev | cut -d'/' -f2- | rev)
export POETRY_ACTIVE=1
PATH_add "$VENV"

if [ ! -L .venv ]; then
  ln -ns $VIRTUAL_ENV .venv
fi
```

Let's allow our direnv and check we are using our correct environment

## GPG

Remember about managing secrets ? Now that we have our virtual environements set, how can we manage to work with secrets without write them directly in our project/repository ?

Before we can use a password manager such as pass we need a gpg key id.

### Generate a key

Let's first generate one:

```bash
gpg --full-generate-key
```

- Get key id

```bash
gpg --list-keys
/Users/gonza/.gnupg/pubring.kbx
-------------------------------
pub   rsa3072 2021-06-24 [SC] [expires: 2031-06-22]
      9D369A173B2E18EAFA665DE3FD9D69F3015D5488
uid           [ultimate] Gonzalo Acosta (GPG Key generated in MBP16) <gonzaloacostapeiro@gmail.com>
sub   rsa3072 2021-06-24 [E] [expires: 2031-06-22]
```

## Pass


`pass` is the standar unix password manager


> With pass, each password lives inside of a gpg encrypted file whose filename is the title of the website or resource that requires the password. These encrypted files may be organized into meaningful folder hierarchies, copied from computer to computer, and, in general, manipulated using standard command line file management utilities.

### Pass installation

`pass` is available in almost all distribution and can be installed as follow:


```
brew install pass
```

### Usage pass

Now that we have pass installed on our system and a valid gpg_key_id we can init our password manager with :

```
❯ pass init 9D369A173B2E18EAFA665DE3FD9D69F3015D5488
/Users/gonza/.password-store
Password store initialized for 9D369A173B2E18EAFA665DE3FD9D69F3015D5488
```

Let's insert first password

```
❯ pass insert gonzaloacosta/project/some_password
/Users/gonza/.password-store/gonzaloacosta
/Users/gonza/.password-store/gonzaloacosta/project
Enter password for gonzaloacosta/project/some_password:
Retype password for gonzaloacosta/project/some_password:
```

```
❯ pass
Password Store
└── gonzaloacosta
    └── project
        └── some_password
❯ pass show gonzaloacosta/project/some_password
supersecret123
```

[Docs](https://medium.com/@Kaderovski/ultimate-python-development-environment-configuration-5b33712a0c53)
