# Ubuntu environment setting

## apt related
```bash
sudo sed -i 's/[a-z.]*archive.ubuntu.com/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list
sudo apt update
sudo apt install software-properties-common -y
```

## input method
```bash
sudo apt update
sudo apt install fcitx -y
sudo cp /usr/share/applications/fcitx.desktop /etc/xdg/autostart/
sudo apt purge ibus -y
sudo dpkg -i ~/Downloads/sogoupinyin_4.2.1.145_amd64.deb
sudo apt install libqt5qml5 libqt5quick5 libqt5quickwidgets5 qml-module-qtquick2 -y
sudo apt install libgsettings-qt1 -y
```
Fix Chinese words display errors: https://www.cnblogs.com/CodeAndMoe/p/14279135.html


## toolchain
```bash
# gcc
sudo add-apt-repository ppa:ubuntu-toolchain-r/test -y
sudo sed -i 's/http:\/\/ppa.launchpad.net/https:\/\/launchpad.proxy.ustclug.org/g' /etc/apt/sources.list.d/*.list
sudo apt update
sudo apt install gcc-11 -y
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 50

```

## pyenv
```bash
sudo apt install zlib1g-dev libssl-dev build-essential libbz2-dev libreadline-dev libsqlite3-dev llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

# install pyenv
git clone https://github.com/pyenv/pyenv.git ~/.pyenv

# set up pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc

source ~/.bashrc

# install pyenv-virtualenv
git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv

# set up pyenv-virtualenv
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc

source ~/.bashrc

# install python
pyenv install 3.7.17
pyenv virtualenv 3.7.17 py3.7.17
pyenv activate py3.7.17

```

## docker
```bash
# snap
sudo apt install snap -y
sudo snap install docker

# apt
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
sudo chown root:docker /var/run/docker.sock
```


## tmux-config
```bash
sudo apt install tmux -y
```

1. Install `TMP` tool
```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```
2. Install `xclip` to enable access to system's clipboard
```bash
sudo apt install xclip -y
```

3. Load configs to `tmux`
```bash
tmux source ~/.tmux.conf
```


## Vscode

### Python Formatter

1. Install 'Ruff' extension in *Vscode*.
2. Install 'Black' extension in *Vscode*.
3. Add the following configs into `setting.json`
```json
    "[python]": {
        "editor.defaultFormatter": "ms-python.black-formatter",
        "editor.formatOnSave": true,
        "editor.codeActionsOnSave": {
            "source.fixAll.ruff": true,
            "source.organizeImports.ruff": true
        },
        "editor.formatOnType": true
    },
    "editor.rulers": [80]
```
4. A config file can be added in the root path of the project for `Black` and `Ruff`

```yaml
# https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html
[tool.black]
line-length = 80

# https://beta.ruff.rs/docs/settings/
[tool.ruff]
line-length = 80
# https://beta.ruff.rs/docs/rules/
select = ["E", "W", "F"]
ignore = ["F401"]
# Exclude a variety of commonly ignored directories.
respect-gitignore = true
ignore-init-module-imports = true
```
#### Reference
https://blog.davidz.cn/post/python-linter-ruff-formatter-black
