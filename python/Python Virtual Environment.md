
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

pyenv 用法
```shell
pyenv install --list
pyenv install 3.8.10
pyenv global 3.8.10

pyenv exec python3.8.10 test.py
```