
https://medium.com/@aashari/easy-to-follow-guide-of-how-to-install-pyenv-on-ubuntu-a3730af8d7f0
```shell
sudo apt update
# install dependency
sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

# install pyenv 
curl https://pyenv.run | bash

# configure bashrc
echo -e 'export PYENV_ROOT="$HOME/.pyenv"\nexport PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc  
echo -e 'eval "$(pyenv init --path)"\neval "$(pyenv init -)"' >> ~/.bashrc

# refresh shell
exec "$SHELL"

pyenv --version
```

# Pyenv
pyenv 用于在一个电脑中保持多个python 的版本
```shell
pyenv install --list
pyenv install 3.8.10
pyenv global 3.8.10

pyenv exec python3.8.10 test.py
```

# UV
```shell
# install uv
brew install uv
# use pip
pip install uv

```

```shell
uv python list
uv python install 3.9

uv run --python 3.8 # run script with specific python version

uv run --with rish --with requests --python 3.9 main.py # run with uninstalled package

uv init --script main.py --python 3.9.21

uv init

```